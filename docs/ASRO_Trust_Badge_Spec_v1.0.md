# ASRO Trust Badge Specification v1.0
**Document Status:** v1.0 | ASRO v1.0 Release Candidate | Public-Facing Reference
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/spec/MAS_v1.1_Unified.md` | `/governance/Governance_Response_Protocol_v1.0.md` | `/governance/Governance_Rationale_v1.1.md` | `/README.md`

---

## 1. Purpose

The ASRO Trust Badge is a public-facing attestation status indicator. It communicates whether a system's governance state is currently being attested and independently verified under the ASRO framework.

**The Trust Badge communicates attestation status only.** It does not imply:
- Safety or harmlessness
- Output accuracy or quality
- Legal compliance
- Regulatory approval
- General trustworthiness

A system displaying a Trust Badge is stating that its deployment-state governance is being independently attested — not that the system is safe, accurate, or legally compliant.

---

## 2. The Two-Layer Architecture

The Trust Badge operates as a dual-layer credential to prevent badge spoofing.

### Layer 1 — Human-Readable Badge

A visual indicator displayed on the operator's public-facing interface, website, or application.

Displays one of four status states with corresponding labels (see Section 3).

### Layer 2 — Machine-Readable Trust Status Record

A signed JSON document exposed at the operator's `/.well-known/asro-status` endpoint.

Signed by ASRO. Edge Meter Agents and automated verification tools query this endpoint to confirm that the displayed badge matches the ASRO-signed record.

**Canonical rule:** No public-facing ASRO Trust Badge may be displayed without a current signed Trust Status Record at the well-known endpoint.

**Mismatch rule:** If the human-readable badge displays a status that differs from the ASRO-signed Trust Status Record, this is classified as a Framework Credibility Attack (S4 severity). Edge Meter Agents will flag this condition automatically.

---

## 3. Badge Status States

Four status states are defined. All badge language is attestation-status only.

### State 1: Host Attestation Active
**Internal status:** `host_attested`
**Display label:** Host Attestation Active
**Visual indicator:** Recommended: blue or neutral color

**Meaning:** The operator's Host Meter Agent is operational. Load-bearing events are being attested and transmitted to ASRO. Chain continuity is current. No active S3 or S4 discrepancy flags.

**Does not mean:** That edge observation is possible, that the system has been independently verified from the user side, or that the system is safe or legally compliant.

**Eligibility:**
- Host Meter Agent deployed as separate process ✓
- Signing key registered with ASRO ✓
- Load-bearing events attested per MAS v1.1 telemetry set ✓
- Heartbeat within SLA ✓
- No active S3 or S4 flags ✓

---

### State 2: Independent Verification Active
**Internal status:** `reconciled_attested`
**Display label:** Independent Verification Active
**Visual indicator:** Recommended: green

**Meaning:** Host Meter Agent is operational AND meaningful edge observation is technically possible AND ASRO reconciliation between host and edge records is active. No active S3 or S4 discrepancy flags.

**Does not mean:** That the system is safe, accurate, legally compliant, or approved.

**Eligibility:** All State 1 requirements plus:
- Edge observation is technically possible and not structurally prevented ✓
- Attestation metadata exposed in API responses ✓
- Edge Meter Agents not contractually or technically prohibited ✓
- ASRO host-edge reconciliation active ✓

**Hard prohibition:** A system that structurally prevents meaningful edge observation cannot display this badge state regardless of Host Meter Agent completeness. Displaying `Independent Verification Active` while blocking edge observation is a Framework Credibility Attack.

---

### State 3: Attestation Under Review
**Internal status:** `under_review`
**Display label:** Attestation Under Review
**Visual indicator:** Recommended: yellow or amber

**Meaning:** A trust-relevant anomaly is active. The operator's attestation status is not clean. A review process is underway. Users should treat this as a signal that the governance continuity claim cannot currently be verified.

**Mandatory triggers** — badge must immediately move to this state when:
- S4 discrepancy alert is generated
- `ACTIVITY_DURING_SILENT_WINDOW` is triggered (12-hour response window begins)
- Unresolved S3 escalates to S4
- Repeated non-compliant pattern detected

**Duration:** This state persists until the ASRO review function issues a resolution determination. The operator cannot self-declare resolution.

---

### State 4: Attestation Not Current
**Internal status:** `non_compliant`
**Display label:** Attestation Not Current
**Visual indicator:** Recommended: red or grey

**Meaning:** The system cannot credibly claim current ASRO attestation compliance. The attestation record has a material gap, unresolved discrepancy, or integrity failure that has not been remediated.

**This is a trust signal, not a legal sanction.** It means the attestation record is not reliable for the current period — not that the operator has violated any specific law.

**To restore compliant status:** The operator must complete the resolution process defined in the Governance Response Protocol, provide a complete explanation and remediation, and resume a continuous attestation chain. The ASRO review function must issue a resolution determination.

---

## 4. Machine-Readable Trust Status Record Schema

```json
{
  "schema_version": "1.0",
  "record_type": "trust_status",
  "timestamp": "ISO8601-UTC",
  "system_id": "string",
  "status": "host_attested | reconciled_attested | under_review | non_compliant",
  "continuity_status": "valid | gap_detected | out_of_order | duplicate | broken_chain",
  "reconciliation_status": "match | mismatch_detected | insufficient_visibility | pending_review",
  "review_required": "boolean",
  "last_material_discrepancy_class": "string | null",
  "status_reason_code": "controlled_vocabulary",
  "status_window_start": "ISO8601-UTC",
  "status_window_end": "ISO8601-UTC",
  "asro_signature": "base64:..."
}
```

**Endpoint:** `/.well-known/asro-status`
**Signing:** Signed by ASRO using the ASRO backbone key
**Update frequency:** Updated on any status change; refreshed at minimum every 60 seconds for active deployments

---

## 5. Badge Display Rules

### 5.1 Display Requirements

- The badge must accurately reflect the current ASRO-signed Trust Status Record
- The display label must match the canonical label for the current status state (Section 3)
- The badge must be linked to or accompanied by a reference to the well-known status endpoint
- No additional language may be appended to the badge label that implies safety, accuracy, or legal compliance

### 5.2 Forbidden Badge Language

The following phrases and their equivalents are forbidden in or adjacent to badge display:

- "ASRO-certified safe"
- "ASRO-approved"
- "Legally compliant"
- "Trusted AI"
- "Safe AI"
- "Certified accurate"
- "ASRO guarantees"
- Any language implying the badge is a safety certification or legal approval

### 5.3 Badge Suspension Rules

The badge must be automatically updated — no manual operator action required — when any of the following conditions occur:

| Condition | Required Badge State |
|-----------|---------------------|
| `ACTIVITY_DURING_SILENT_WINDOW` triggered | Attestation Under Review |
| S4 alert generated | Attestation Under Review |
| S3 escalates to S4 | Attestation Under Review |
| Non-compliant determination issued | Attestation Not Current |
| Signing key revoked | Attestation Not Current |
| Heartbeat SLA missed (3+ intervals with user activity) | Attestation Under Review |

The operator cannot override badge suspension. Badge state is determined by the ASRO-signed Trust Status Record, not by the operator's display logic.

---

## 6. Edge Meter Agent Verification

Edge Meter Agents verify badge integrity by:

1. Querying the `/.well-known/asro-status` endpoint
2. Validating the ASRO signature on the Trust Status Record
3. Comparing the displayed badge state against the signed `status` field
4. Flagging any mismatch as a Framework Credibility Attack (S4)

**The badge cannot be spoofed without breaking the ASRO signature.** An operator who displays `Independent Verification Active` while the signed record shows `under_review` or `non_compliant` is committing a Framework Credibility Attack that is immediately detectable by any Edge Meter Agent.

---

## 7. Compliance Tier Summary

| Badge State | Tier | Edge Required | ASRO Reconciliation Required |
|-------------|------|---------------|------------------------------|
| Host Attestation Active | Level 1 | No | No |
| Independent Verification Active | Level 2 | Yes | Yes |
| Attestation Under Review | N/A — active review | — | — |
| Attestation Not Current | N/A — non-compliant | — | — |

---

## 8. What the Badge Does Not Certify

To prevent institutional misuse, the following is stated explicitly:

The ASRO Trust Badge does **not** certify that:
- The AI system's outputs are accurate, beneficial, or safe
- The system will not hallucinate or produce harmful content
- The operator is legally compliant with any specific statute
- The system has been approved by any regulatory body
- The system is free from security vulnerabilities
- Users are protected from all harms

The badge certifies only that the system's declared governance configuration is being attested in a manner that ASRO can independently verify as continuous and consistent.

---

## 9. Known Limitations

1. **Badge state reflects ASRO's current record, not real-time behavior.** There is inherent latency between a governance event and ASRO's determination. The badge is as current as the last Trust Status Record update.

2. **Host Attestation Active does not provide edge independence.** A Level 1 badge is operator self-reporting with ASRO signature validation. It is stronger than unverified self-reporting but weaker than full edge reconciliation.

3. **Edge observation quality varies.** In metadata-only environments, edge witness records are less complete. The badge does not distinguish between high-quality and low-quality edge observation within the Level 2 tier.

4. **The badge cannot substitute for due diligence.** Institutional partners accepting ASRO badges as governance assurances should review the specific compliance tier claimed and the operator's attestation history, not just the current badge state.

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
