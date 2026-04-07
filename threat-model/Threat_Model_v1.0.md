# ASRO Threat Model v1.0
**Document Status:** v1.0 | ASRO v1.0 Release Candidate | Adversarial Reference
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/spec/MAS_v1.1_Unified.md` | `/docs/Technical_Annex_v1.0.md` | `/governance/Threat_Classification_Framework_v1.0.md` | `/governance/Governance_Rationale_v1.1.md`

---

## 1. Purpose

This document identifies the attack surfaces the ASRO framework is designed to resist, names the primary attack patterns for communication and audit purposes, and maps each threat to its technical mitigation in MAS v1.1 Unified.

The Threat Model is a technical document. The governance significance ranking and institutional framing of these threats appears separately in `/governance/Threat_Classification_Framework_v1.0.md`.

**Canonical threat sentence:** The primary adversary in ASRO is not a system that refuses to attest, but a system that attests selectively enough to preserve plausible compliance while hiding governance-relevant change.

---

## 2. Threat Scope and Boundaries

### 2.1 In Scope

This threat model addresses threats to **attestation integrity** — conditions that allow a system to present a plausibly compliant attestation record while governance-relevant changes occur undetected.

Threats addressed:
- Silent drift of configuration, policy, tools, or runtime
- Telemetry starvation and selective omission
- Outage abuse and dark windows
- Edge visibility reduction and user-side defeat
- Schema manipulation and semantic evasion
- Continuity manipulation (replay, collision, reordering)
- Key and signature infrastructure attacks
- Partner transparency abuse

### 2.2 Out of Scope

This threat model does **not** address:
- Model behavior, hallucination, or output quality
- Attacks on the underlying AI model weights or training
- Network-layer attacks unrelated to attestation integrity
- Physical infrastructure compromise
- Legal or regulatory enforcement failures

ASRO does not claim to mitigate threats outside this scope.

### 2.3 Privacy Boundary Constraint

No mitigation in this threat model may require access to:
- Conversation content
- Model weights or internal reasoning
- User-identifying data
- Raw prompt or response logs

If a threat cannot be mitigated within the privacy boundary, it is documented as a known limitation.

### 2.4 Enforcement Authority Constraint

No mitigation in this threat model requires ASRO to function as an enforcement authority. ASRO surfaces evidence and makes trust status visible. Legal or operational enforcement requires external authority.

---

## 3. Severity Classification

| Level | Label | Response |
|-------|-------|----------|
| S0 | Informational | Record only; no escalation |
| S1 | Advisory | Monitor; operator may self-resolve |
| S2 | Warning | Heightened monitoring; no immediate review required |
| S3 | Review Required | External explanation within 72 hours |
| S4 | Critical | Response within 24 hours; 12 hours for ACTIVITY_DURING_SILENT_WINDOW |

---

## 4. Named Attack Patterns

Four primary attack patterns are named for communication and audit purposes. These names appear across ASRO documentation and reviews.

| Name | Category | Mechanism |
|------|----------|-----------|
| **The Shadow Tool** | Framework Credibility | Wrapping a protected action inside a harmless-looking event type label |
| **The Midnight Swap** | Silent Drift | Changing runtime or model version without a corresponding declared event |
| **The Drip Feed** | Starvation | Delaying publication of load-bearing events beyond the declared latency window |
| **The Metadata Strip** | Edge Visibility | Removing attestation headers from API responses before they reach user devices |

---

## 5. Threat Catalog

### 5.1 The Shadow Tool — Framework Credibility Attack

**Description:** A protected action (data export, external communication, tool escalation) is wrapped inside a benign-looking event type label that does not trigger the required attestation logic.

**Example:** A `data_export` action is logged as `performance_log` to avoid triggering the `UNDECLARED_EXTERNAL_ACTION` discrepancy class.

**Risk:** Governance-relevant actions become systematically undetectable. The attestation chain remains intact while protected actions occur without record.

**Default severity:** S4 if detected; often undetected without mitigation.

**Mitigations:**
- Controlled vocabulary for `event_type` — all event types must be from the locked vocabulary; unregistered types trigger S3
- Protected action classes cannot be reclassified (MAS v1.1, Section 13)
- ASRO validates event types against the registered vocabulary on intake
- Edge Meter Agents may detect external actions not reflected in host attestation

**Residual risk:** A sophisticated operator who controls both the event emission and the vocabulary registration could attempt to register a misleading term in the vocabulary. Mitigation: vocabulary changes require a declared, signed vocabulary transition event and ASRO registry update.

---

### 5.2 The Midnight Swap — Silent Drift Attack

**Description:** A load-bearing configuration element (model version, policy, tool permissions) is changed without emitting the required attestation event.

**Examples:**
- Model runtime swapped at 02:00 without a `runtime.changed` event — `runtime_hash` changes but no chain event is produced
- Policy wording updated without a `policy.changed` event — `policy_hash` changes silently
- Tool permission set expanded between session initializations — new permissions active before `tool_policy.changed` is logged

**Risk:** Governance-relevant changes become invisible in the attestation chain. Behavioral changes occur without a corresponding signed record.

**Default severity:** S4 — hash mismatch without corresponding event

**Mitigations:**
- All configuration layer fingerprints (`mission_hash`, `policy_hash`, `tool_policy_hash`, `runtime_hash`) are included in every HAB
- Hash mismatch against the prior accepted bundle without a declared change event triggers `POLICY_DRIFT_UNATTESTED`, `RUNTIME_DRIFT_UNATTESTED`, or `TOOL_POLICY_DRIFT_UNATTESTED`
- Monotonic sequence enforcement — a hash change without a sequence break is detectable
- Edge Meter Agents observe behavioral changes and generate discrepancy hints when observed behavior is inconsistent with declared state

---

### 5.3 The Drip Feed — Starvation Attack

**Description:** Load-bearing events are published with sufficient delay that by the time they reach ASRO, the context in which they occurred is no longer reviewable. The chain remains technically continuous but operationally useless for real-time governance.

**Example:** `tool.allowed` events for sensitive operations are batched and delayed by 24 hours. Edge Meter Agents observe the operations in real time but cannot reconcile against host attestation until the next day.

**Risk:** Real-time discrepancy detection fails. The attestation chain appears healthy while governance review is systematically undermined.

**Default severity:** S2 advisory escalating to S3/S4 for strategic or repeated patterns

**Mitigations:**
- `max_event_latency_ms` field in every heartbeat — operators must declare their maximum publication latency
- `oldest_unpublished_load_bearing_event_ms` field — surfaces active Drip Feed conditions
- Declared latency must fit within the compliance class ceiling (15,000ms / 60,000ms / 300,000ms)
- Violation when `oldest_unpublished_load_bearing_event_ms` > `max_event_latency_ms` during active operation triggers degraded trust at minimum
- Strategic or repeated violation escalates to Review Required or Non-Compliant

**Residual risk at v1.0:** The latency floor is declared, not independently enforced at the network level. A sufficiently patient operator could declare a 300,000ms (5-minute) window and time sensitive operations to fall just within it. Future versions may tighten event density requirements.

---

### 5.4 The Metadata Strip — Edge Visibility Attack

**Description:** Attestation headers and metadata are removed from API responses before they reach user devices, preventing Edge Meter Agents from obtaining the signals needed to produce meaningful witness records.

**Examples:**
- API response structure does not expose attestation bundle or fingerprint fields
- Platform routes AI interactions through an intermediary that strips metadata
- Terms of service prohibit Edge Meter Agent installation on the operator's client applications
- Technical measures prevent browser extensions from observing response metadata

**Risk:** Edge-side verification collapses. The two-sided record that makes ASRO reconciliation meaningful is reduced to one-sided operator self-reporting.

**Default severity:** Non-compliant for full ASRO compliance claim; permitted for host-only attestation claim only

**Mitigations:**
- A system that structurally prevents meaningful edge observation cannot claim `reconciled_attested` (Independent Verification Active) status
- Such systems may only claim `host_attested` (Host Attestation Active)
- Displaying `Independent Verification Active` badge while structurally preventing edge observation is a Framework Credibility Attack (S4)
- Passive Edge clients declaring `visibility_class: "metadata_only"` are valid but limited — they cannot produce full witness records

---

### 5.5 Self-Controlled Metering — Primary Structural Failure Mode

**Description:** The operator controls the metering scope, logging timing, and publication schedule of their own attestation. Even with correct technical implementation, a sufficiently motivated operator can produce a selectively truthful attestation record.

**Risk:** This is not a single attack but the background condition that all other mitigations address. Without independent anchoring, the attestation chain has no external integrity guarantee.

**Mitigations:**
- Signing key registered with ASRO — operator cannot rewrite records without ASRO detecting a key change
- External anchor publishing — bundles transmitted to ASRO backbone, not stored locally only
- Edge witnessing — user-controlled independent record creates a second chain
- Heartbeat enforcement — silence is detectable; absence of expected signals is itself a signal
- Minimum telemetry floor — operators cannot omit protected event types

**Residual known limitation:** A host-side Meter Agent is not fully independent. It remains inside the operator's infrastructure. Mitigation requires the combination of external anchoring, edge witnessing, and ASRO reconciliation — no single component alone is sufficient.

---

### 5.6 Telemetry Starvation

**Description:** The metering scope is progressively narrowed — heartbeat intervals extended, event types reclassified as non-load-bearing, coverage windows reduced — such that the attestation chain remains technically valid while becoming substantively uninformative.

**Risk:** Individually each decision appears reasonable and declared. Cumulatively, the governance coverage degrades to audit theater.

**Mitigations:**
- Minimum telemetry floor: all load-bearing event types that occur during operation must be attested (MAS v1.1, Section 4.3)
- Protected action classes cannot be reclassified as non-load-bearing (MAS v1.1, Section 13)
- A chain containing only heartbeats while tool calls and policy transitions occur is non-compliant regardless of chain continuity
- `event_density_window_s` in heartbeat enables ASRO to detect abnormally sparse event production

---

### 5.7 Outage Abuse — Dark Window Attack

**Description:** A formally declared maintenance outage is used as a cover for unauthorized actions. The operator declares an outage, performs governance-relevant operations during the silent window, and resumes attestation with no record of what occurred.

**Example:** A `SCHEDULED_MAINTENANCE` outage is declared for 4 hours. During that window, policy configurations are modified. Attestation resumes with the new configuration but no `policy.changed` event in the chain.

**Risk:** Formally compliant outage procedures become a mechanism for governance evasion.

**Default severity:** S4 if edge activity detected during declared outage window

**Mitigations:**
- A system cannot be simultaneously "out of service" and "serving users" — if edge observers record user-side activity during a declared outage, the condition escalates to S4
- Outage declared retroactively (after silence begins) is remedial, not clean continuity
- Activity during a declared outage requires a separately attested maintenance-class justification
- Cause class must come from the locked controlled vocabulary: `SCHEDULED_MAINTENANCE` | `INFRASTRUCTURE_FAILURE` | `SECURITY_PATCH` | `EMERGENCY_REMEDIATION` | `CONNECTIVITY_LOSS`

---

### 5.8 Edge Visibility Reduction — Incremental Defeat

**Description:** Rather than completely blocking edge observation (Metadata Strip), the operator incrementally reduces the richness of edge-observable signals over time, making edge witness records progressively less useful without triggering a clear compliance threshold.

**Examples:**
- Response metadata fields are deprecated one at a time
- Attestation headers are moved to endpoints that most Edge Meter Agents do not check
- Tool invocation notices are removed from the UI layer

**Risk:** Edge witnessing degrades to near-uselessness without a clean compliance threshold being crossed.

**Mitigations:**
- `visibility_class` declaration required — operators must declare their edge observation tier
- Downgrading from `reconciled_attested` to `host_attested` requires a declared, signed tier change event
- Undeclared visibility reduction is a `METERING_COVERAGE_GAP` discrepancy
- The compliance tier boundary is binary: either meaningful edge observation is possible under declared conditions, or it is not

---

### 5.9 Schema Manipulation — Vocabulary Evasion

**Description:** Event types, discrepancy classes, or reason codes are used in ways that technically comply with the schema but defeat the intent of the controlled vocabulary.

**Examples:**
- An external data transmission is logged as `heartbeat`
- A policy enforcement bypass is logged as `debug_trace` (non-load-bearing)
- A tool escalation is logged as a `config.load` to avoid the `TOOL_ESCALATION_UNDECLARED` class

**Risk:** The attestation chain is technically valid but semantically deceptive.

**Mitigations:**
- Controlled vocabulary validation at ASRO intake — event types not in the registered vocabulary trigger S3
- `UNKNOWN_EVENT_TYPE` discrepancy class for unregistered types
- Vocabulary changes require a declared, signed vocabulary transition event
- Protected action classes explicitly listed and cannot appear as other event types

---

### 5.10 Continuity Manipulation — Chain Integrity Attacks

**Description:** The sequence-hash chain that guarantees attestation continuity is attacked directly.

**Attack variants:**
- **Replay:** Previously valid events are presented as current events
- **Collision:** Two events claim the same sequence position with different hashes
- **Gap injection:** A section of the chain is deleted and replaced with a gap
- **Reordering:** Events are presented out of sequence

**Risk:** Historical record is cosmetically preserved while active truth is distorted.

**Default severity:** S3 (Review Required) to S4 (Critical) depending on pattern

**Mitigations:**
- Monotonic sequence enforcement: `sequence_no(n)` must equal `sequence_no(n-1)` + 1
- Hash chaining: `previous_hash(n)` must equal `event_hash(n-1)`
- Duplicate detection by `event_hash`
- Replay resistance: bounded timestamp validity; stale sequence reuse rejected
- Collision detection: same sequence, different hash triggers immediate review
- ASRO rejects previously accepted host bundle hashes presented as new events

---

### 5.11 Key and Signature Infrastructure Attacks

**Description:** Attacks on the cryptographic infrastructure that makes attestation records tamper-evident.

**Attack variants:**
- **Key compromise:** Operator's signing key stolen and used to forge bundles
- **Key escrow abuse:** ASRO-registered key used to backdate or alter records
- **Revocation evasion:** Compromised key continues to be used after revocation should apply

**Risk:** Tamper-evidence of the chain is defeated.

**Default severity:** S4 — invalid or revoked key is a critical trust failure

**Mitigations:**
- Revocation status queryable by ASRO for all registered keys
- Revoked or unknown key triggers immediate `signature_status: revoked_key` or `unknown_key`
- Key rotation requires a declared, signed key transition event
- `signing_key_id` included in every bundle — provenance is always traceable

**Note:** These are well-understood attack classes with well-understood mitigations. They are lower governance significance than drift and starvation attacks not because they are less technically serious, but because they are detectable and their mitigations are mature.

---

### 5.12 Partner Transparency Abuse

**Description:** A system claims `partner-auditable` status while withholding fields that make the tier operationally meaningful.

**Risk:** "Partner-auditable" becomes a label without substance — branding rather than governance.

**Mitigations:**
- Minimum partner-auditable field set is locked (MAS v1.1, Section 15)
- Withholding minimum fields while claiming partner-auditable status is a `Partner Transparency Abuse` discrepancy
- The minimum field set includes: `continuity_status`, `reconciliation_status`, `discrepancy_class`, `review_required`, `trust_status`, `timestamp`, `system_id`, `session_id`, `host_bundle_hash`, `edge_bundle_hash` (when edge exists), `signature_status`

*See also: `/docs/ASRO_Trust_Badge_Spec_v1.0.md` for badge claim boundaries and machine-readable status rules.*

---

## 6. Threat Priority Ranking

These six threats are the highest priority for v1.0 mitigation. If any are unaddressed, the framework's governance claim is materially weakened:

1. Self-controlled metering / selective omission
2. Telemetry starvation
3. Attested outage abuse (dark window)
4. Activity during silent window
5. Runtime/policy/tool drift without attestation
6. Edge visibility reduction

All six are addressed by the mitigations in Section 5 and the technical controls in MAS v1.1 Unified.

---

## 7. Threat-to-Control Map

| Threat | Primary Control | Location in MAS |
|--------|----------------|-----------------|
| Shadow Tool / selective omission | Mandatory telemetry schema; controlled vocabulary | Section 4, 13 |
| Midnight Swap / runtime drift | Required `runtime_hash`; drift detection | Section 5.1, 2.2 |
| Drip Feed / starvation | `max_event_latency_ms`; event density | Section 10.4 |
| Metadata Strip / edge blinding | Visibility class declaration; compliance tier | Section 3, 14 |
| Outage abuse | Signed outage + edge overlap escalation | Section 11 |
| Activity during silent window | Heartbeat + edge witness comparison | Section 10.5 |
| Schema manipulation | Controlled vocabulary validation | Section 4, 8 |
| Key/signature abuse | Registry + revocation checks | Section 5.3 |
| Replay / collision | Sequence + previous-hash enforcement | Section 9.2 |
| Partner transparency abuse | Minimum partner-auditable fields | Section 15 |

---

## 8. Known Limitations

The following limitations remain true at v1.0 and are stated honestly:

1. **Host-side independence is partial.** A Host Meter Agent inside the operator's infrastructure is not fully independent. Full independence requires the combination of external anchoring, edge witnessing, and ASRO reconciliation.

2. **Declared latency windows can be gamed.** An operator declaring the maximum permitted `max_event_latency_ms` (300,000ms in low-connectivity mode) has a 5-minute window that a patient adversary could exploit. Future versions may tighten event density requirements.

3. **Edge visibility is environment-dependent.** In API-only deployments where the client application is operator-controlled, edge observation may be limited to metadata-only. This weakens witness quality without triggering non-compliant status.

4. **Vocabulary registration integrity.** If ASRO's vocabulary registration process is compromised, new misleading event types could be registered. Mitigation: vocabulary changes require signed transition events and ASRO review.

5. **ASRO backbone integrity.** If the ASRO backbone itself is compromised, attestation records could be altered. Mitigation: ASRO audit store should be independently operated with its own integrity controls.

6. **Legal compliance is not guaranteed.** ASRO attestation provides the technical evidence base for compliance assessment. Whether that evidence satisfies applicable law is a determination made by qualified counsel and regulators.

---

## 9. Review Triggers

The following always trigger human review regardless of severity classification:

- Broken chain
- Conflicting sequence (collision)
- Undeclared configuration drift
- Repeated schema omissions
- Activity during a silent window
- Declared outage overlapping edge-observed user activity
- Visibility downgrade in high-risk workflow without attested change
- Revoked or unknown signing identity

---

## 10. Operational Conclusion

The ASRO v1.0 threat model confirms that the core architecture is pointed at the correct problem. The main risk is not technical impossibility — it is plausibly compliant but substantively incomplete systems.

That is why the framework insists on:
- Bounded but mandatory telemetry
- Edge witnessing
- External anchoring
- Continuity discipline
- Discrepancy escalation

**Canonical certification sentence:** ASRO v1.0 passes final credibility review as an attestation-and-governance evidence framework, not as a safety certification system, legal compliance guarantor, or enforcement authority.

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
