# ASRO Privacy & Legal Stress Memo
*Version 1.0 | March 2026 | Companion to: ASRO v1 Attestation Bundle Specification*

---

## Purpose

This memo evaluates ASRO v1 against two distinct questions:

1. Does the attestation bundle design expose user data, proprietary system internals, or individually identifiable information, even indirectly?

2. Do ASRO's verifiability claims hold under adversarial legal scrutiny — specifically, can a participating organization use ASRO to defend against negligence, misrepresentation, or failure-to-disclose claims in a dispute or regulatory proceeding?

These are not rhetorical questions. Both will be tested by regulators, opposing counsel, and institutional reviewers. This memo answers them before they are asked.

---

## Part 1: Privacy Exposure Analysis

ASRO v1 handles no user content, no prompts, and no model weights. The attestation bundle is metadata about changes to a system, not a record of what the system said or was asked. The privacy analysis therefore focuses on indirect exposure risks — cases where bundle fields could leak sensitive information through inference, aggregation, or content error.

### Field-Level Risk Assessment

**Zero exposure risk:**

`bundle_id`, `timestamp_utc`, `system_id`, `operator_id` — Pure operational metadata. No PII, no content, no inference path to user data.

`previous_bundle_hash`, `bundle_hash`, `signature` — Cryptographic primitives. One-way. No reconstruction possible.

`event_class` — Categorical label from a closed taxonomy. No user-specific information.

**Low exposure risk (design-dependent):**

`state_fingerprint` — Must be a one-way hash (SHA-3 or BLAKE3) of system state. Risk exists only if: (a) the underlying state being hashed includes PII, which is an operator implementation error; or (b) hash collisions make the fingerprint exploitable. `state_fingerprint` MUST be derived from model or configuration state only — not from logs, prompts, or user-derived artifacts.

`policy_version` — A version identifier. Low risk that version numbering patterns could reveal deployment timing. Mitigated by not requiring sequential or date-stamped identifiers.

**Medium exposure risk (content-dependent):**

`change_summary` — The primary privacy risk in the entire bundle. A human-readable field with no automated content controls at the point of authorship. A poorly written summary could include user references, prompt fragments, or internal decision rationale.

Required controls:
- Maximum 500 characters
- Content scanning at ingestion: blocklist for PII patterns, email addresses, user identifiers, prompt fragments
- Published operator guidance with acceptable and unacceptable examples
- Rejected bundles logged internally with violation reason (not published publicly)

> ✓ Acceptable: "Updated refusal threshold for self-harm category from 0.82 to 0.91"
> ✗ Unacceptable: "Updated threshold after user reports of false positives on accounts X, Y, Z"

`impact_metrics` — Aggregated behavioral deltas are low-risk by design but become medium-risk if granularity is too fine. Metrics must be reported at system level only. Minimum aggregation threshold: changes must reflect behavior across at least 10,000 requests before a delta is reportable.

`integrity_assertion` — The operator's formal legal declaration. Defined as a structured sub-object with enumerated fields. No user data should appear here.

### Privacy Claim Defensibility

ASRO's core privacy claim must be stated accurately:

> "ASRO v1 is designed to receive no user data. The attestation bundle format prohibits user-identifying content. The `change_summary` field is the sole field where operator compliance is required to maintain this property, and ASRO enforces content controls at ingestion."

The simpler formulation "ASRO never receives user data" is accurate in design intent but overstated as an absolute guarantee. The accurate formulation above is defensible.

---

## Part 2: Legal Stress Test

**Q: How do you know the company logged every required event?**

*ASRO's honest answer:* We don't, and we don't claim to. ASRO proves that submitted records are cryptographically intact and that the chain has no detectable gaps in submitted data. Whether all qualifying events were submitted requires independent audit of the operator's internal change logs against the public ledger. That audit function is a structural requirement of ASRO, not an optional add-on.

*Legal defensibility:* Medium-High. ASRO does not claim completeness. It claims tamper-evidence of what was submitted, plus an audit mechanism to test completeness externally. The combination shifts the burden: an operator cannot credibly claim an event was logged if the public ledger shows no corresponding entry and independent auditors find no internal record.

**Q: Couldn't a company classify a safety policy change as a routine configuration change?**

*ASRO's honest answer:* Yes. Misclassification is possible. ASRO's detection mechanism is cross-system behavioral anomaly flagging: a `CONFIG_CHANGE` associated with a substantial behavioral delta triggers a classification review flag. This is probabilistic detection, not a guarantee.

*Legal defensibility:* High, if `impact_metrics` are required. The anomaly detection mechanism only functions if behavioral metrics are submitted. If `impact_metrics` remain optional, this rating drops to Medium for operators who do not provide metrics. This is the strongest argument for making `impact_metrics` required in future versions.

**Q: What prevents retroactive log construction?**

*ASRO's honest answer:* Daily Merkle root anchoring to external public timestamp services. A retroactively constructed log cannot predate the Merkle root anchors already published. The within-day window (up to 24 hours) is the residual gap. Hourly anchoring closes this gap further.

*Legal defensibility:* High. The Merkle root predates any litigation or regulatory inquiry. The timestamp anchor is held by an external party with no interest in the outcome.

**Q: What if multiple companies agreed not to log a specific category of change?**

*ASRO's honest answer:* Coordinated omission across all participating systems is undetectable by ASRO alone. The structural mitigation is external: ASRO participation becomes a regulatory or procurement condition, meaning omission creates direct legal exposure independent of ASRO's detection capability.

*Legal defensibility:* Medium-High. ASRO alone cannot prevent coordinated omission. The legal architecture surrounding ASRO can. This distinction should be explicit in any regulatory submission — ASRO is one layer of an accountability system, not the entire system.

---

## Part 3: Regulatory Alignment

**GDPR Article 5(1)(b) — Purpose limitation**
- ASRO coverage: Bundle design prohibits collection for any purpose beyond integrity verification.
- Gap: None in design. Operator must implement per their own GDPR obligations.

**GDPR Article 5(1)(c) — Data minimization**
- ASRO coverage: Bundle collects the minimum fields necessary for tamper-evidence and chain continuity.
- Gap: `impact_metrics` granularity must be defined to remain consistent with minimization principle.

**CCPA §1798.100 — Right to know**
- ASRO coverage: No personal information collected.
- Gap: None.

**NIST AI RMF 2.1 — Govern/Measure**
- ASRO coverage: Attestation bundle directly supports the "transparency and documentation" sub-function.
- Gap: Event taxonomy should be mapped explicitly to NIST's AI risk categories in a companion note for regulatory submissions.

**EU AI Act — High-Risk System Logging Requirements**
- ASRO coverage: ASRO's logging architecture is structurally consistent with Article 12 (record-keeping for high-risk AI systems).
- Gap: `EMERGENCY_OVERRIDE` class should explicitly reference "significant malfunction" language from Article 62 to align with incident reporting obligations.

---

## Part 4: Residual Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| `change_summary` PII leak | Medium | High | Content scanning + 500-char cap + operator guidance. Required pre-v1. |
| `state_fingerprint` hash collision | Low | Medium | Specify SHA-3 or BLAKE3. Do not allow SHA-256. |
| `impact_metrics` reidentification | Medium | Medium | Minimum 10,000-request aggregation threshold. |
| Coordinated omission | Low (with mandate) | High | Regulatory/procurement requirement; taxonomy versioning creates compliance baseline. |
| ASRO governance capture | Low (with rules) | Critical | Hard representation limits; pre-defined independence spin-out trigger. |

---

## Part 5: Actionable Requirements

**Required before v1 external submission:**
- Define `change_summary` content controls in the spec (500-char cap, prohibited content categories)
- Specify hash algorithm requirements (SHA-3 or BLAKE3; not SHA-256)
- Define minimum aggregation threshold for `impact_metrics` (≥10,000 requests)
- State privacy claim accurately: "designed to receive no user data" not "never receives user data"

**Required in v1.1:**
- Hourly Merkle anchoring (closes within-day window)
- Formal NIST AI RMF mapping note
- EU AI Act Article 62 alignment for `EMERGENCY_OVERRIDE`

**Governance requirements:**
- Privacy review function on ASRO governing board (advisory, non-voting)
- Annual third-party privacy audit
- Published incident response procedure for privacy violations in submitted bundles

---

*ASRO Privacy & Legal Stress Memo v1.0 | james@michigrid.org | March 2026*
