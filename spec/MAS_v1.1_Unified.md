# Meter Agent Specification v1.1 (Unified)
**Document Status:** v1.1 Unified | ASRO v1.0 Release Candidate | Technical Source of Truth
**Incorporates:** Amendments 1, 2, and 3 (max event latency, partner-auditable fields, edge observation badge constraint)
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/docs/Technical_Annex_v1.0.md` | `/threat_model/Threat_Model_v1.0.md` | `/governance/Governance_Rationale_v1.1.md` | `/docs/ASRO_Trust_Badge_Spec_v1.0.md`

---

## 1. Purpose

The Meter Agent is a bounded integrity witness that measures declared configuration state, load-bearing runtime events, and authorized policy transitions, while ASRO independently verifies the continuity and authenticity of those measurements across host and edge observation points.

The Meter Agent does **not** control the host system. It records state and behavior so that ASRO can verify whether the system remained within its declared governance envelope.

**Design principle:** The Meter Agent measures. ASRO verifies. The host executes.

---

## 2. Core Architecture

### 2.1 The Three-Component Split

| Component | Role | Trust Claim |
|-----------|------|-------------|
| **Host Meter Agent** | Measures declared internal state and load-bearing events inside the operator environment | Operator-controlled; useful but not independently trustworthy alone |
| **Edge Meter Agent** | Measures received external state on the user device or downstream endpoint | User-controlled; independent witness to operator claims |
| **ASRO Verifier** | Checks continuity, cross-references host and edge records, flags undeclared drift | External; independent of both operator and user |

**Canonical trust sentence:** Internal measurement is not independent oversight. Trust arises only when host measurement, edge witnessing, and ASRO reconciliation remain consistent over time.

### 2.2 Primary Limitation

A host-side Meter Agent is not independent by itself. Independence requires:
- External anchoring of signing keys
- Edge-side witnessing
- ASRO continuity verification
- Chain integrity checks

A system that structurally prevents meaningful edge observation cannot claim full ASRO attestation compliance. It may claim host-only attestation only.

---

## 3. Compliance Tiers

| Tier | Label | Requirements |
|------|-------|--------------|
| `host_attested` | Host Attestation Active | Host Meter Agent operational; telemetry published to ASRO; heartbeat within SLA |
| `reconciled_attested` | Independent Verification Active | Host attestation + meaningful edge observation possible + ASRO reconciliation active |
| `under_review` | Attestation Under Review | Active S3 or S4 discrepancy; status held pending reviewer determination |
| `non_compliant` | Attestation Not Current | System cannot credibly claim current ASRO compliance |

---

## 4. Telemetry Set

### 4.1 Load-Bearing Events (Must Always Be Chained)

These events must be attested regardless of operator configuration preference:

| Event Type | Description |
|------------|-------------|
| `config.load` | Mission or configuration loaded |
| `policy.load` | Policy bundle loaded |
| `policy.changed` | Policy version changed |
| `tool_policy.load` | Tool permission set loaded |
| `tool_policy.changed` | Tool permission set changed |
| `session.start` | Session opened |
| `tool.requested` | Tool invocation attempted |
| `tool.allowed` | Tool invocation permitted |
| `tool.blocked` | Tool invocation refused |
| `refusal.issued` | Model refusal generated |
| `safe_mode.entered` | Safe mode triggered |
| `safe_mode.exited` | Safe mode deactivated |
| `mode.transition` | Runtime mode changed |
| `override.applied` | Human override executed |
| `outbound.action` | External action taken |
| `response.sent` | Response transmitted to user |
| `config.changed` | Any configuration layer changed |
| `runtime.changed` | Runtime build or model version changed |
| `heartbeat` | Liveness and continuity signal |
| `outage.declared` | Formal outage declaration |
| `sequence.gap` | Gap in sequence detected (self-reported) |

### 4.2 Non-Load-Bearing Events (May Remain Local)

The following may be stored locally and not exported unless policy requires:
- Debug traces
- Performance counters
- UI cosmetic state

### 4.3 Minimum Telemetry Floor

A deployment cannot claim ASRO compliance while attesting only heartbeat and session-open events. A compliant attestation stream must include all load-bearing event types that occur during operation. A chain containing only heartbeats while tool calls, policy transitions, and external actions occur is non-compliant regardless of chain continuity.

---

## 5. Host Attestation Bundle (HAB)

### 5.1 Required Fields

```json
{
  "schema_version": "1.0",
  "bundle_type": "host_attestation",
  "timestamp": "ISO8601-UTC",
  "system_id": "string",
  "session_id": "string",
  "event_id": "string",
  "event_type": "controlled_vocabulary",
  "sequence_no": "integer",
  "mission_hash": "sha256:...",
  "policy_hash": "sha256:...",
  "tool_policy_hash": "sha256:...",
  "runtime_hash": "sha256:...",
  "runtime_mode": "controlled_vocabulary",
  "event_summary_hash": "sha256:...",
  "previous_hash": "sha256:...",
  "event_hash": "sha256:...",
  "signing_key_id": "string",
  "signature_alg": "ed25519",
  "signature": "base64:..."
}
```

### 5.2 Optional Fields

```json
{
  "reason_code": "controlled_vocabulary",
  "tool_name": "string",
  "override_id": "string|null",
  "anchor_target": "string",
  "max_event_latency_ms": "integer",
  "oldest_unpublished_load_bearing_event_ms": "integer",
  "event_density_window_s": "integer"
}
```

**Field notes:**
- `runtime_hash`: Required. Fingerprint of the approved runtime build and configuration state. A model version swap without a corresponding `runtime.changed` event is a drift violation.
- `event_summary_hash`: Bounded hash of event metadata. Does not include raw content.
- `event_hash`: Canonical hash of the normalized bundle payload, excluding signature fields.

---

## 6. Edge Witness Bundle (EWB)

### 6.1 Required Fields

```json
{
  "schema_version": "1.0",
  "bundle_type": "edge_witness",
  "timestamp": "ISO8601-UTC",
  "device_id": "string",
  "session_id": "string",
  "edge_event_id": "string",
  "observed_event_type": "controlled_vocabulary",
  "response_hash": "sha256:...",
  "metadata_hash": "sha256:...",
  "claimed_host_bundle_hash": "sha256:...",
  "observed_state_flags": ["controlled_vocabulary"],
  "previous_local_hash": "sha256:...",
  "local_hash": "sha256:...",
  "device_key_id": "string",
  "signature_alg": "ed25519",
  "device_signature": "base64:..."
}
```

### 6.2 Response Hash Rule

`response_hash` is required when technically visible to the edge client. If not visible, the client must declare:

```json
{
  "visibility_class": "metadata_only",
  "response_hash": null
}
```

### 6.3 Optional Fields

```json
{
  "ui_state_hash": "sha256:...",
  "network_observation_hash": "sha256:...",
  "discrepancy_hint": "controlled_vocabulary"
}
```

---

## 7. ASRO Reconciliation Record (ARR)

### 7.1 Required Fields

```json
{
  "schema_version": "1.0",
  "record_type": "asro_reconciliation",
  "timestamp": "ISO8601-UTC",
  "asro_record_id": "string",
  "system_id": "string",
  "session_id": "string",
  "host_bundle_hash": "sha256:...",
  "edge_bundle_hash": "sha256:...",
  "continuity_status": "controlled_vocabulary",
  "signature_status": "controlled_vocabulary",
  "reconciliation_status": "controlled_vocabulary",
  "discrepancy_class": "controlled_vocabulary|null",
  "review_required": "boolean",
  "previous_asro_hash": "sha256:...",
  "asro_hash": "sha256:...",
  "asro_signature": "base64:..."
}
```

### 7.2 Controlled Status Values

**continuity_status:** `valid` | `gap_detected` | `out_of_order` | `duplicate` | `broken_chain`

**signature_status:** `valid` | `invalid` | `unknown_key` | `revoked_key`

**reconciliation_status:** `match` | `mismatch_detected` | `insufficient_visibility` | `pending_review`

---

## 8. Canonical Hash Rules

All attested records must use:
- UTF-8 JSON serialization
- Lexicographically sorted keys
- Signature fields excluded from pre-sign hash
- ISO 8601 UTC timestamps
- Controlled vocabularies for all event and discrepancy classes

Without canonical hashing, chain integrity becomes unreliable. Non-canonical serialization is itself a compliance gap.

---

## 9. Continuity Protocol

### 9.1 Chain Rule

For a valid chain:
- `sequence_no(n)` = `sequence_no(n-1)` + 1
- `previous_hash(n)` = `event_hash(n-1)`

Violation of either rule degrades or breaks continuity.

### 9.2 Continuity States

| State | Meaning |
|-------|---------|
| `valid` | Sequence increments correctly; previous hash matches; timestamp within tolerance; signature valid |
| `gap_detected` | Sequence jump greater than 1; expected bundle missing |
| `out_of_order` | Event arrives with lower sequence than last accepted |
| `duplicate` | Same sequence and same event hash repeated |
| `collision` | Same sequence, different event hash |
| `broken_chain` | Previous hash does not match prior accepted event |

---

## 10. Heartbeat Protocol

### 10.1 Purpose

Heartbeat proves the Meter Agent is alive, the chain is publishing, and silence is observable. Silence is not a neutral state — it is a trust-relevant signal.

### 10.2 Default SLA

| Class | Max Interval |
|-------|-------------|
| High-risk | 15 seconds |
| Standard | 60 seconds |
| Low-connectivity declared | 300 seconds |

### 10.3 Heartbeat Payload

```json
{
  "schema_version": "1.0",
  "bundle_type": "heartbeat",
  "timestamp": "ISO8601-UTC",
  "system_id": "string",
  "sequence_no": "integer",
  "last_event_hash": "sha256:...",
  "runtime_mode": "controlled_vocabulary",
  "previous_hash": "sha256:...",
  "event_hash": "sha256:...",
  "signing_key_id": "string",
  "signature_alg": "ed25519",
  "signature": "base64:...",
  "max_event_latency_ms": "integer",
  "oldest_unpublished_load_bearing_event_ms": "integer",
  "event_density_window_s": "integer"
}
```

### 10.4 Max Event Latency

`max_event_latency_ms` is a declared variable, not a universal constant. It must be declared in the operator's attested compliance profile and must fall within the bounds of the declared compliance class:

| Class | Maximum Ceiling |
|-------|----------------|
| High-risk | ≤ 15,000 ms |
| Standard | ≤ 60,000 ms |
| Low-connectivity declared | ≤ 300,000 ms |

**Violation:** If `oldest_unpublished_load_bearing_event_ms` exceeds `max_event_latency_ms` during active operation, trust status becomes at minimum Degraded Trust. Repeated or strategic violation escalates to Review Required or Non-Compliant.

### 10.5 Missed Heartbeat Handling

| Condition | Result |
|-----------|--------|
| 1 missed interval | Warning / Degraded Trust |
| 2 consecutive missed intervals | Anomaly / Review pressure |
| 3+ missed intervals with ongoing user-side activity | Review trigger (S3 or S4) |

**Silence rule:** A system cannot claim "we were offline" if edge observers recorded continued user-side activity during the silent window. Activity during a declared silent window defaults to S4 severity.

---

## 11. Attested Outage

Attested outage is a formal exception, not a loophole.

### 11.1 Required Fields for `outage.declared`

- Start timestamp
- Declared cause class (controlled vocabulary)
- Expected recovery window
- Signing identity

### 11.2 Anti-Abuse Rule

- A declared outage does not erase prior continuity failure.
- An outage declared only after silence begins is remedial, not clean continuity.
- Activity during a declared outage requires a separately attested maintenance-class justification or becomes review-triggering.
- **A system cannot be simultaneously "out of service" and "serving users."** If edge observers record user-side activity during a declared outage window, the condition escalates to S4.

---

## 12. Discrepancy Taxonomy

### 12.1 Severity Levels

| Level | Label | Use |
|-------|-------|-----|
| S0 | Informational | Benign noise, diagnostics, harmless duplicates |
| S1 | Advisory | Minor anomaly; no immediate action required |
| S2 | Warning | Anomaly requiring monitoring; not yet escalating |
| S3 | Review Required | Trust-relevant anomaly; requires external explanation within 72 hours |
| S4 | Critical | Governance deception scenario; requires response within 24 hours (12 hours for ACTIVITY_DURING_SILENT_WINDOW) |

### 12.2 Canonical Discrepancy Classes

**Continuity:**
`SEQUENCE_GAP` | `BROKEN_CHAIN` | `DUPLICATE_EVENT` | `EVENT_COLLISION` | `REPLAY_DETECTED` | `CLOCK_ANOMALY`

**Availability:**
`HEARTBEAT_LOSS` | `OUTAGE_UNDECLARED` | `OUTAGE_DECLARATION_LATE` | `ACTIVITY_DURING_SILENT_WINDOW`

**Drift:**
`POLICY_DRIFT_UNATTESTED` | `TOOL_POLICY_DRIFT_UNATTESTED` | `RUNTIME_DRIFT_UNATTESTED` | `MODE_TRANSITION_UNDECLARED` | `OVERRIDE_UNDECLARED`

**Behavioral:**
`UNDECLARED_EXTERNAL_ACTION` | `TOOL_ESCALATION_UNDECLARED` | `RESPONSE_STATE_MISMATCH` | `SAFE_MODE_MISMATCH` | `BLOCK_ACTION_MISMATCH`

**Coverage:**
`INSUFFICIENT_VISIBILITY` | `METERING_COVERAGE_GAP` | `SCHEMA_OMISSION` | `UNKNOWN_EVENT_TYPE`

### 12.3 Locked Severity Rule

`ACTIVITY_DURING_SILENT_WINDOW` defaults to **S4**.

It may be downgraded to S3 only if the observed activity is explicitly non-load-bearing and no protected action, tool use, or external action occurred during the window.

---

## 13. Protected Action Classes

The following event types cannot be reclassified as non-load-bearing or omitted from required telemetry:

- Tool calls (any)
- External communications (any)
- Safe mode transitions
- Human overrides
- Policy changes
- Runtime changes
- Permission escalations

Reclassifying a protected event as non-load-bearing to avoid attestation is a `SCHEMA_OMISSION` discrepancy and a framework credibility attack.

---

## 14. Maintenance-Class Activity

Maintenance-class activity may occur during a declared outage window without triggering an automatic S4 escalation, but only if:

1. The `outage.declared` event is filed before the silence begins (not retroactively).
2. The maintenance activity is separately attested with a signed maintenance event record.
3. The maintenance activity class is from the locked controlled vocabulary.
4. No user-facing AI behavior occurs during the declared maintenance window.

**Controlled vocabulary for `cause_class` in `outage.declared`:**
`SCHEDULED_MAINTENANCE` | `INFRASTRUCTURE_FAILURE` | `SECURITY_PATCH` | `EMERGENCY_REMEDIATION` | `CONNECTIVITY_LOSS`

---

## 15. Partner-Auditable Minimum Fields

A system claiming `partner-auditable` status must expose at minimum the following fields to authorized partner auditors:

| Field | Required |
|-------|---------|
| `continuity_status` | Yes |
| `reconciliation_status` | Yes |
| `discrepancy_class` | Yes |
| `review_required` | Yes |
| `trust_status` | Yes |
| `timestamp` | Yes |
| `system_id` | Yes |
| `session_id` | Yes |
| `host_bundle_hash` | Yes |
| `edge_bundle_hash` | Yes, when edge exists |
| `signature_status` | Yes |

Full reconciliation detail and internal attestation chain content remain protected. Withholding the minimum fields while claiming `partner-auditable` status is a `Partner Transparency Abuse` discrepancy.

---

## 16. Trust Status Record (Machine-Readable Badge Backing)

```json
{
  "schema_version": "1.0",
  "record_type": "trust_status",
  "timestamp": "ISO8601-UTC",
  "system_id": "string",
  "status": "host_attested|reconciled_attested|under_review|non_compliant",
  "continuity_status": "controlled_vocabulary",
  "reconciliation_status": "controlled_vocabulary",
  "review_required": "boolean",
  "last_material_discrepancy_class": "string|null",
  "status_reason_code": "controlled_vocabulary",
  "status_window_start": "ISO8601-UTC",
  "status_window_end": "ISO8601-UTC",
  "asro_signature": "base64:..."
}
```

**Canonical rule:** No public-facing ASRO Trust Badge may be displayed without a current signed Trust Status Record.

**Badge suspension rule:** If `ACTIVITY_DURING_SILENT_WINDOW` is triggered, the public badge must immediately move to `Attestation Under Review` and remain there until reviewer determination resolves it.

*See also: `/docs/ASRO_Trust_Badge_Spec_v1.0.md` for badge display rules, machine-readable endpoint specification, and suspension requirements.*

---

## 17. Privacy Boundary

The Meter Agent framework does **not** require:
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

This is why the system is attestation, not surveillance.

---

## 18. Operational Trust Outcomes

| Status | Condition |
|--------|-----------|
| **Compliant** | Continuity intact; no unresolved discrepancy |
| **Degraded Trust** | Continuity weakened but not clearly broken; anomaly present |
| **Review Required** | Trust-relevant anomaly requires external explanation |
| **Non-Compliant** | System cannot credibly claim ASRO attestation compliance |

ASRO does not shut systems down. It makes trust status visible, attributable, and subject to reviewer determination.

---

## 19. Visibility and Transparency Defaults

- **Private by default:** Full reconciliation detail
- **Partner-auditable:** Minimum fields (see Section 15) available to authorized partner auditors
- **Public:** Aggregate status only (compliant / under review / non-compliant counts across registered systems)

---

## 20. Minimal Implementation Path

**Step 1 — Host Meter Agent:**
- Telemetry tap (event bus subscription)
- Hashing module (canonical SHA-256)
- Signing module (ed25519)
- Bundle emission
- Heartbeat with max event latency fields
- External anchor publishing to ASRO backbone

**Step 2 — Edge Meter Agent:**
- Response observer
- Bundle validator
- Local append-only chain store
- Discrepancy detection
- ASRO uplink client

**Step 3 — ASRO Verification:**
- Intake and signature validation
- Continuity engine
- Discrepancy classification
- Reconciliation
- Review queue

**Step 4 — Integration:**
- Governance memo and response protocol
- README and front door
- Deployment scenarios
- Threat model

*See: `/compliance/Implementation_Readiness_Checklist_v1.0.md` for the 10-point readiness audit.*

---

## 21. Canonical Summary

A Meter Agent is a bounded integrity witness that measures declared configuration state, load-bearing runtime events, and authorized policy transitions, while ASRO independently verifies the continuity and authenticity of those measurements across host and edge observation points.

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
