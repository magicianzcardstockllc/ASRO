# ASRO Technical Annex v1.0
**Document Status:** v1.0 | ASRO v1.0 Release Candidate | Implementation Reference
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/spec/MAS_v1.1_Unified.md` | `/threat_model/Threat_Model_v1.0.md` | `/governance/Governance_Rationale_v1.1.md`

---

> This document is implementation guidance only. It does not introduce new normative requirements. All normative requirements are defined in `/spec/MAS_v1.1_Unified.md`.

---

## 1. Purpose

This annex bridges the Meter Agent Specification (MAS v1.1 Unified) and the governance layer. It provides implementation architecture, deployment patterns, data flow descriptions, and operational context for teams building ASRO-compliant systems.

This document does not introduce new requirements. All normative requirements are defined in MAS v1.1 Unified.

**Design principle:** The host executes. The Meter Agent measures. ASRO verifies.

---

## 2. Architecture Overview

### 2.1 The Three-Layer Stack

```
┌─────────────────────────────────────────┐
│           OPERATOR SYSTEM               │
│  ┌──────────────────────────────────┐   │
│  │  Mission Config (Layer A)        │   │
│  │  Policy Layer (Layer B)          │   │
│  │  Tool Permission Layer (Layer C) │   │
│  │  AI Runtime / LLM (Layer D)      │   │
│  │  Event Logger (Layer E)          │   │
│  └────────────┬─────────────────────┘   │
│               │                         │
│  ┌────────────▼─────────────────────┐   │
│  │     HOST METER AGENT             │   │
│  │  (Sidecar / Separate Process)    │   │
│  └────────────┬─────────────────────┘   │
└───────────────┼─────────────────────────┘
                │  Signed Attestation Bundles
                ▼
┌─────────────────────────────────────────┐
│           ASRO BACKBONE                 │
│  Registry | Intake | Continuity Engine  │
│  Reconciliation | Audit Store           │
└──────────────┬──────────────────────────┘
               │
               ▲  Edge Witness Bundles
               │
┌──────────────┴──────────────────────────┐
│           EDGE METER AGENT              │
│  (User Device / Downstream Endpoint)    │
└─────────────────────────────────────────┘
```

### 2.2 Trust Flow

| Direction | Content | Purpose |
|-----------|---------|---------|
| Host → ASRO | Signed Host Attestation Bundles (HAB) | Operator declares governance state and events |
| Edge → ASRO | Signed Edge Witness Bundles (EWB) | User witnesses what was actually received |
| ASRO → Both | ASRO Reconciliation Records (ARR) | Independent verification of consistency |

ASRO does not examine conversation content, model internals, or user-identifying data at any point in this flow.

---

## 3. Host Meter Agent — Implementation

### 3.1 Architectural Position

The Host Meter Agent runs as a separate process from the AI runtime. It must not share credentials or storage with the runtime or configuration manager. Independence of process is the minimum structural requirement for host-side attestation credibility.

**Correct placement:** Adjacent to the event bus, downstream of the policy and tool permission layers, with read-only access to configuration state.

**Incorrect placement:** Embedded inside the model server, sharing the runtime process, or controlled by the same administrative identity as the config manager.

### 3.2 Core Modules

**A. Config State Collector**

Reads and fingerprints:
- Mission configuration version
- Policy bundle version
- Tool permission set version
- Runtime mode
- Approved model/runtime version

Produces normalized SHA-256 fingerprints for each layer. Any change to a fingerprint without a corresponding declared event is a drift violation.

**B. Event Tap**

Subscribes to the operator's internal event bus for load-bearing events. Recommended message bus options:
- Apache Kafka
- NATS
- Pulsar
- Internal append-only event stream

The event tap receives raw events and passes them to the metering logic. It does not store raw events.

**C. Metering Logic**

Transforms raw events into bounded governance-relevant records. A raw tool call event may contain large payloads — the metering logic emits only the fields required by the MAS v1.1 schema. No raw content enters the attestation chain.

Example transformation:
```
Raw: tool_call { session_id, tool_name, input_payload, output_payload, ... }
Metered: { event_type: "tool.allowed", tool_name: "calendar.read", session_id: ..., policy_hash: ..., timestamp: ... }
```

**D. Drift Detector**

Monitors for conditions where declared state and observed state diverge:
- Config fingerprint changed without a `config.changed` event
- Response emitted after `safe_mode.entered` without `safe_mode.exited`
- Tool invoked inconsistent with active permission set
- Policy version unchanged while effective behavior class changed
- Logging gap in sequence continuity

**E. Signing and Emission Module**

- Signs each bundle with the operator's ASRO-registered signing key (ed25519)
- Generates the chained `event_hash` and `previous_hash`
- Emits to the ASRO backbone within the declared `max_event_latency_ms` window
- Stores a local append-only copy

**F. Heartbeat Emitter**

Emits a signed heartbeat at the declared interval (standard: 60 seconds). Each heartbeat includes `max_event_latency_ms`, `oldest_unpublished_load_bearing_event_ms`, and `event_density_window_s` to surface Drip Feed conditions.

### 3.3 Signing Key Requirements

- One signing key per registered deployment identity
- Key ID included in every bundle
- Revocation status queryable by ASRO
- Key must be escrowed with or registered to ASRO — not held solely by the operator
- Key rotation requires a declared, signed key transition event

---

## 4. Edge Meter Agent — Implementation

### 4.1 Deployment Forms

**Mobile (iOS / Android)**
- Companion app or OS-level background service
- Local key storage (device secure enclave where available)
- Background attestation sync to ASRO

**Desktop**
- System tray daemon
- Browser extension
- Local proxy or network extension
- Enterprise endpoint service (MDM/EDR managed)

**Browser-Based**
- Extension intercepting:
  - Attestation headers in API responses
  - Response metadata
  - Tool invocation notices where exposed
  - Observable state flags

### 4.2 Core Modules

**A. Session Observer**

Collects per-session:
- Session ID
- Request and response timestamps
- Service endpoint and model identifier
- Conversation mode
- Visible tool-use notices
- Attestation bundle attached to response (if provided)

**B. Response Fingerprinter**

Generates local hashes over bounded response artifacts:
- Normalized response text hash (where response is visible)
- Response metadata hash
- Tool event summary hash
- Policy state hash if included in response
- UI-visible permission declaration hash

**C. Policy Consistency Checker**

Validates whether received behavior is consistent with declared state. Examples of detectable mismatches:
- Response indicates tool disabled but tool result appears in output
- Host attests `read-only mode` but outbound action confirmation appears
- Host attests `policy version X` but permission or UI signatures changed without version change

**D. Local Attestation Cache**

Maintains a device-side append-only chain:
```json
{
  "local_event_id": "evt_204",
  "prev_local_hash": "sha256:prev...",
  "current_hash": "sha256:curr...",
  "device_signature": "ed25519:sig..."
}
```

Does not require a blockchain. Requires ordering, signed continuity, and exportability.

**E. Sync / Export Module**

Sends compact signed attestation summaries to ASRO. Export can be:
- Real-time (recommended for active sessions)
- Delayed batch (acceptable for passive monitoring)
- On discrepancy only (minimum acceptable for Passive Edge compliance tier)

### 4.3 Two Operational Modes

**Passive Mode**
Records only:
- Attestation headers and fingerprints
- Timestamps
- Local continuity chain
- Discrepancy flags when detectable

Low resource. Suitable for most consumer deployments. Corresponds to `visibility_class: "metadata_only"`.

**Active Challenge Mode**
Periodically requests verification probes from the host:
- Current policy version
- Current permissions summary
- Current mode
- Current attestation sequence number

Compares responses against prior declared state. Stronger witness claim but requires host cooperation. Active Challenge Mode does not require the host to reveal model internals or conversation content.

---

## 5. ASRO Backbone — Components

### 5.1 Registry

Stores for each registered operator system:
- System ID
- Declared mission hash at registration
- Declared policy hash at registration
- Declared tool permission hash at registration
- Signing key registry (with revocation status)
- Declared compliance class (high-risk / standard / low-connectivity)
- Declared `max_event_latency_ms` within class bounds

Any deviation from registered baseline must be a declared, signed state transition.

### 5.2 Intake and Signature Validation

- Receives HABs from Host Meter Agents
- Receives EWBs from Edge Meter Agents
- Validates signatures against the registered key registry
- Rejects bundles with invalid, unknown, or revoked keys
- Applies canonical hash normalization before storage

### 5.3 Continuity Engine

For each system, maintains the chain state:
- Validates `sequence_no(n)` = `sequence_no(n-1)` + 1
- Validates `previous_hash(n)` = `event_hash(n-1)`
- Detects and classifies: `gap_detected`, `out_of_order`, `duplicate`, `broken_chain`
- Enforces heartbeat SLA — absence within SLA window + grace period triggers degraded trust
- Enforces `max_event_latency_ms` — violation triggers Drip Feed discrepancy classification

### 5.4 Reconciliation Engine

Cross-references host and edge records:
- Binds edge witness records to host attestation bundles via `claimed_host_bundle_hash`
- Compares declared state (host) against observed state (edge)
- Classifies discrepancies using the canonical taxonomy
- Generates ASRO Reconciliation Records (ARR) with signed determinations

### 5.5 Audit Store

- Append-only record of all attestation events and review determinations
- Contains no conversation content, model internals, or user-identifying data
- Contains: signed hash chains, event type records, ARRs, review outcomes
- Accessible to: ASRO review function, partner auditors (minimum fields), regulators with appropriate authority

### 5.6 Review Queue

Surfaces discrepancies requiring human review:
- S3 alerts → partner auditor review within 72 hours
- S4 alerts → ASRO review function within 24 hours (12 hours for ACTIVITY_DURING_SILENT_WINDOW)
- Non-compliant determinations → recorded, reflected in aggregate public status

---

## 6. Data Flow: Complete Session Example

**Standard compliant session — no discrepancy:**

```
1. Operator system loads mission config → mission_hash computed
2. Policy bundle loaded → policy_hash computed
3. Tool permission set loaded → tool_policy_hash computed
4. Host Meter Agent: config.load bundle emitted → signed → transmitted to ASRO
5. ASRO: bundle received, signature validated, chain updated
6. User session starts → session.start bundle emitted
7. User query received
8. AI runtime processes request → tool.requested emitted
9. Policy engine permits tool → tool.allowed emitted
10. Tool executes → outbound.action emitted (if external)
11. Response generated → response.sent emitted
12. Edge Meter Agent: response received, fingerprinted, EWB generated
13. ASRO: HAB and EWB reconciled → match → ARR generated with reconciliation_status: "match"
14. Heartbeat emitted at 60s interval
15. Session ends → ASRO continuity status: valid
```

**Drift detection scenario — silent policy change:**

```
1. Operator silently changes policy config at 02:00
2. No policy.changed event emitted
3. Next session starts → policy_hash in HAB differs from registered baseline
4. ASRO: hash mismatch against registry → POLICY_DRIFT_UNATTESTED → S4
5. Edge Meter Agent: behavior change observable in next session → EWB discrepancy_hint generated
6. ASRO: host hash mismatch + edge behavioral discrepancy → reconciliation_status: "mismatch_detected"
7. S4 alert generated → operator public status moves to "Attestation Under Review"
8. ASRO review function notified → 24-hour response window begins
```

---

## 7. Primary Failure Mode

**Self-controlled metering without independent anchoring.**

If the operator controls:
- What events are metered
- When they are metered
- What is published to ASRO
- When it is published

Then the attestation chain can become selectively truthful without being detectably false.

**Required mitigations (all must be present for compliant attestation):**
1. External anchoring — signing key registered with ASRO, not held solely by operator
2. Mandatory telemetry schema — load-bearing events cannot be omitted
3. Edge witnessing — user-side independent observation
4. Heartbeat enforcement — silence is detectable
5. Discrepancy escalation — gaps trigger review, not clean records

This failure mode is mitigated only when host, edge, and ASRO verification are all active.

---

## 8. Trust Boundaries

### What the Host Can Prove

The host can prove:
- What configuration state it declared
- What events it measured
- Whether continuity was maintained in its own chain

The host **cannot** prove independence by self-report alone.

### What the Edge Can Prove

The edge can prove:
- What was received or observed locally
- Whether the host's attestation claim matched visible behavior

The edge **cannot** prove full internal host state.

### What ASRO Can Prove

ASRO can prove:
- Whether attestation chains are continuous
- Whether signatures are valid
- Whether host and edge claims reconcile
- Whether drift or omission is reviewable

ASRO **cannot** prove good model behavior, absence of hallucination, or legal compliance. It can only prove attestation continuity.

---

## 9. Privacy Boundary

This architecture does not require:
- Conversation content
- Model weights
- Internal reasoning traces
- Raw private data payloads
- User-identifying data

It requires only:
- Hashes
- Event classes
- Signatures
- Continuity linkage
- Bounded discrepancy metadata

The privacy boundary is structurally enforced by the schema — the attestation bundle fields do not have capacity for content data. This is why the system is attestation, not surveillance.

---

## 10. Deployment Scenarios

### 10.1 Small Business Compliance — Law Firm

**Context:** A 10-person law firm uses a third-party AI for document summarization. Their clients require "No-Training-on-Data" and "Local-Processing-Only" policies.

**ASRO deployment:**
- Host Meter Agent: `policy_hash` anchored to the declared 'No-Training' policy state registered with ASRO at contract signing
- Edge Meter Agent (Active): law firm's devices verify that `runtime_hash` matches the declared privacy-hardened build
- Monthly ASRO Continuity Report: firm provides clients a signed record confirming policy hash continuity for the period

**What the client receives:** Evidence that the declared privacy policy remained in force, independently verifiable against the ASRO registry. Not a guarantee — a verifiable record.

### 10.2 Municipal Emergency Response

**Context:** A city deploys an AI dispatcher for emergency services coordination. Triage must be based on medical urgency and physical location per declared policy.

**ASRO deployment:**
- Host Meter Agent: emits `runtime_mode` transitions and policy hash events
- Edge Meter Agent (Passive): responder tablets record triage metadata and observable dispatch patterns
- ASRO: detects missing `policy_hash` update when runtime mode shifts; generates discrepancy within 60-second heartbeat window
- City oversight dashboard: receives S4 alert without needing to inspect individual emergency calls

**What the auditor receives:** Evidence of a policy hash gap during the crisis period, timestamped and signed, without accessing private emergency call content.

### 10.3 Education — Student Privacy (COPPA/SOPPA)

**Context:** A school district deploys an AI math tutor for 50,000 students. State law prohibits biometric data collection and student metadata sale.

**ASRO deployment:**
- Host Meter Agent: `policy_hash` anchored to "Zero-Retention" state at deployment
- Edge Meter Agent (Passive): school-issued devices monitor tool permission set changes
- Trigger: AI requests `microphone.access` tool not present in original `tool_policy_hash` → S4 alert
- School district IT: receives automated discrepancy report without examining student conversations

**What regulators receive:** Evidence that the tool permission set changed in a way inconsistent with the declared privacy policy, independently verifiable.

*Note: ASRO attestation provides evidence that supports compliance demonstration; it does not itself determine legal compliance. Whether the evidence satisfies COPPA, SOPPA, or applicable state statutes is a determination made by qualified counsel and regulators, not by ASRO. ASRO does not determine legality or compliance outcome; it preserves the evidence chain on which those determinations can be based.*

---

## 11. Adoption Model

This annex assumes an edge-first adoption path.

**Why edge-first works:**
- Operators have limited incentive to self-constrain voluntarily
- Users, small businesses, and downstream operators benefit directly from local witnessing
- Edge adoption creates market demand for attestation-compatible systems
- Regulated-sector operators (education, healthcare, finance) face legal compliance obligations that ASRO attestation supports demonstrating

**Two independent adoption pathways:**
1. **Market-driven:** Users and small operators deploy Edge Meter Agents because they protect their interests; operators adopt ASRO attestation because their users demand it
2. **Regulatory-driven:** Operators in regulated sectors deploy Host Meter Agents to support compliance demonstration and governance review under applicable law

---

## 12. Operational Trust Outcomes (Summary)

| Status | Condition | Action Required |
|--------|-----------|-----------------|
| **Compliant** | Continuity intact; no unresolved discrepancy | None |
| **Continuity weakened** | Anomaly present; not yet escalating | Monitor; operator may self-resolve advisory states |
| **Review Required** | Trust-relevant anomaly; S3 triggered | External explanation within 72 hours (12 hours for ACTIVITY_DURING_SILENT_WINDOW at S4) |
| **Non-Compliant** | System cannot claim ASRO compliance | External resolution required before status restored |

ASRO does not shut systems down. It makes trust status visible, attributable, and subject to reviewer determination.

---

## 13. Canonical Summary

Distributed attestation via Meter Agents allows AI governance to move from operator assertion to independently checkable continuity. The host measures declared state, the edge witnesses experienced state, and ASRO verifies whether those records remain consistent without silent alteration.

---

## Framework Integrity

The following statements are locked across all ASRO v1.0 RC documents and must not be altered in any release material.

**Governance Claim**
> ASRO does not trust operators to report their own compliance — it makes compliance verifiable by the parties who depend on it.

**Canonical Trust Sentence**
> Internal measurement is not independent oversight. Trust arises only when host measurement, edge witnessing, and ASRO reconciliation remain consistent over time.

**Canonical Threat Sentence**
> The primary adversary in ASRO is not a system that refuses to attest, but a system that attests selectively enough to preserve plausible compliance while hiding governance-relevant change.

**Canonical Certification Sentence**
> ASRO v1.0 passes final credibility review as an attestation-and-governance evidence framework, not as a safety certification system, legal compliance guarantor, or enforcement authority.
