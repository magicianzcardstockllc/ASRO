# ASRO Governance Response Protocol v1.0
**Document Status:** v1.0 | ASRO v1.0 Release Candidate | Operational Governance Reference
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/governance/Governance_Rationale_v1.1.md` | `/spec/MAS_v1.1_Unified.md` | `/governance/Threat_Classification_Framework_v1.0.md`

---

## 1. Purpose

A governance framework that produces alerts without defining what authorized reviewers must do with them is not a governance framework — it is a monitoring system. The Governance Response Protocol converts ASRO's attestation chain into an accountability instrument by specifying the actions, timelines, and authorities that give flagged discrepancies operational consequence.

This protocol defines:
- Reviewer roles and their authorities
- Response procedures for S3 and S4 discrepancy events
- What reviewers examine and what they do not examine
- The enforcement boundary of ASRO review findings

---

## 2. Foundational Principle

ASRO review findings are evidentiary, not adjudicatory.

A determination of non-compliant attestation status is a finding that the operator's deployment-state continuity record cannot be verified as intact — it is not a finding of legal violation. Authorized reviewers produce findings that regulatory bodies, auditors, and legal counsel can act upon. They do not issue legal rulings, impose penalties, or compel operator behavior. Any legal consequence of a non-compliant ASRO determination flows from the application of external law to the attestation evidence — not from ASRO's authority over the operator.

ASRO does not determine legality or compliance outcome; it preserves the evidence chain on which those determinations can be based.

---

## 3. Reviewer Roles

The protocol recognizes three reviewer classes. Each has defined authority and defined limits.

### 3.1 Internal Compliance Reviewer

**Who:** The operator's designated internal compliance function.

**Authority:**
- Resolve S1 (Advisory) states by providing attested explanation
- Resolve S2 (Warning) states by providing attested explanation and remediation notice
- File attested outage declarations

**Does not have authority to:**
- Resolve S3 or S4 states unilaterally
- Declare their own system compliant after a review-required flag
- Override ASRO continuity determinations

**Accountability:** Internal compliance reviewers are the operator's designated point of contact for ASRO notifications. Their attested responses become part of the audit record.

---

### 3.2 Partner Auditor

**Who:** An external party designated by ASRO or by contractual arrangement between operator and downstream users.

**Authority:**
- Review partner-auditable records (minimum field set per MAS v1.1, Section 15)
- Request additional information from the operator
- Accept operator explanations for S3 states and close them as resolved
- Escalate S3 states to S4 if the operator does not provide substantive response within the defined window

**Does not have authority to:**
- Issue legal rulings or impose penalties
- Access full reconciliation detail beyond the minimum partner-auditable fields
- Override ASRO review function determinations on S4 states

---

### 3.3 ASRO Review Function

**Who:** ASRO's own designated review capacity or an independently appointed auditor operating under ASRO governance.

**Authority:**
- Review all non-private records in the audit store
- Determine whether non-compliant status is warranted
- Publish aggregate findings (not individual record content)
- Issue formal review determinations for S4 states

**Does not have authority to:**
- Shut down or modify an operator's AI system
- Issue legal rulings or impose penalties
- Compel operator behavior beyond making non-compliance visible

---

## 4. What Reviewers Examine

ASRO review is conducted entirely on the attestation chain and the operator's signed responses. Reviewers examine:

- Whether the continuity record is intact
- Whether declared states match observed states across host and edge records
- Whether gaps in the chain are explained or unexplained
- Whether the operator's response to a flagged condition is consistent with the declared governance framework
- Whether review-required conditions have been resolved within the defined window

**What reviewers do NOT examine:**
- Conversation content
- Model internals or reasoning
- User-identifying data
- Raw prompt or response logs
- Proprietary system design details not relevant to attestation integrity

The privacy boundary holds through the review process. No review procedure requires access to content.

---

## 5. Response Procedures

### 5.1 S3 — Review Required

**Triggered by:** Broken chain, conflicting sequence, undeclared gap, repeated schema omissions, declared outage overlapping edge-observed activity.

**Required response timeline:** 72 hours from alert generation.

**Step 1 — Operator notification.**
ASRO notifies the operator's registered compliance contact of the S3 alert, including:
- The discrepancy class
- The session or time window affected
- The specific continuity failure

**Step 2 — Operator response.**
The operator must provide one of three responses within 72 hours:
- (a) An attested explanation of the gap with supporting evidence
- (b) An attested outage declaration with signed cause class and recovery window, if not already filed
- (c) An attested remediation notice describing the technical failure and the fix implemented

**Step 3 — Partner auditor determination.**
The partner auditor reviews the operator's response against the attestation record:
- If the response is accepted: S3 state is resolved; operator returns to compliant or weakened-continuity status as appropriate
- If the response is insufficient or not provided within 72 hours: state escalates to S4

**Step 4 — Record.**
The resolution or escalation is recorded in the audit store as a signed ASRO review record containing:
- The discrepancy class
- The operator response (or notation of no response)
- The reviewer determination
- The resulting trust status

---

### 5.2 S4 — Critical

**Triggered by:** Activity during a silent window, framework credibility attack indicators, unresolved S3 escalation, repeated non-compliant patterns, revoked or invalid signing identity, structural prevention of edge observation.

**Required response timeline:** 24 hours from alert generation.
**Special subcase — ACTIVITY_DURING_SILENT_WINDOW:** 12 hours from alert generation. This is the highest-severity governance deception scenario and requires the shortest response window.

**Step 1 — Operator notification and status change.**
ASRO notifies the operator and simultaneously changes their public aggregate status to "Attestation Under Review." This is not punitive — it is the transparency mechanism. Users running Edge Meter Agents against that system see the status change immediately.

**Step 2 — Operator response.**
The operator must provide a substantive response within the applicable window explaining the condition that produced the S4 alert. Silence or a non-substantive response within the window results in escalation to non-compliant status.

**Step 3 — ASRO review function engagement.**
An S4 alert requires ASRO review function involvement — not only partner auditor review. The ASRO review function examines:
- The continuity record for the affected period
- The operator's signed response
- Any available edge witness records from the same period

**Step 4 — Determination.**
The ASRO review function issues one of three determinations:
- **(a) Resolved:** The operator has provided a satisfactory explanation and the chain is restored
- **(b) Degraded:** The explanation is partial; trust status remains weakened pending further review
- **(c) Non-compliant:** The operator cannot credibly claim ASRO attestation compliance for the affected period

**Step 5 — Record and publication.**
The determination is recorded in the audit store and reflected in the operator's aggregate public status. Non-compliant determinations are visible to:
- All Edge Meter Agents querying that system's `/.well-known/asro-status` endpoint
- ASRO's partner auditor network
- Regulatory contacts where applicable

---

## 6. Non-Compliant Status

A system in non-compliant status cannot claim ASRO attestation compliance. Non-compliant status:

- Is a trust signal, not a legal sanction
- Is visible to Edge Meter Agents and partner auditors
- Remains until the operator demonstrates resolution through the review process

**To restore compliant status, the operator must:**
1. Provide a complete, attested explanation of the condition that caused non-compliance
2. Demonstrate that the root cause has been remediated
3. Resume a complete, continuous attestation chain with no unexplained gaps
4. Receive a determination of resolution from the ASRO review function

**What ASRO cannot do:** ASRO cannot compel an operator to remediate, cannot shut down their system, and cannot impose penalties. Legal enforcement, if applicable, requires external regulatory action based on evidence that ASRO surfaces but does not adjudicate.

---

## 7. Attested Outage During Review

If an operator declares an attested outage during an active S3 or S4 review period:

- The outage declaration does not suspend the review clock
- The outage cause class must be from the locked controlled vocabulary
- If edge observers recorded activity during the declared outage window, the outage declaration does not close the S4 condition — it becomes additional evidence for the review
- Retroactive outage declarations filed after the review window begins are treated as remedial, not as clean continuity

---

## 8. Aggregate Public Status

ASRO publishes aggregate status — not individual record content.

Public-facing status indicators:
- **Attestation Active:** System is in compliant status with no active S3 or S4 flags
- **Attestation Under Review:** Active S3 or S4 condition; operator response pending or under review
- **Attestation Not Current:** Non-compliant determination; system cannot credibly claim current ASRO compliance

Aggregate status is signed by ASRO and exposed via the operator's well-known status endpoint. A mismatch between the displayed badge and the ASRO-signed status record is itself a Framework Credibility Attack condition.

---

## 9. Governance Boundary Summary

**ASRO review is evidentiary.** Reviewers assess attestation chain integrity and produce signed findings.

**ASRO is not an enforcement authority.** Non-compliant status is a trust signal. Legal sanctions require external action.

**The privacy boundary holds throughout.** No review procedure requires access to conversation content, model internals, or user-identifying data.

**Response timelines are binding.** 72 hours for S3; 24 hours for S4; 12 hours for ACTIVITY_DURING_SILENT_WINDOW. Failure to respond within the window is itself a trust-relevant event.

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
