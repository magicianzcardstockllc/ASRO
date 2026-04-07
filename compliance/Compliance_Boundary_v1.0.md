# ASRO Compliance Boundary Statement v1.0
**Document Status:** v1.0 | ASRO v1.0 Release Candidate
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/governance/Governance_Rationale_v1.1.md` | `/governance/Credibility_Pass_Report_v1.0.md` | `/compliance/Implementation_Readiness_Checklist_v1.0.md`

---

## 1. Purpose

This document defines the precise boundary between what ASRO determines and what ASRO does not determine. It exists to prevent compliance-adjacent overclaiming in any context where ASRO attestation is used as evidence in regulatory, legal, or institutional review.

This boundary is not a limitation of the framework. It is a deliberate architectural decision. A framework that claims to determine legal compliance would be making representations it cannot back with legal authority. ASRO makes no such representations. It makes technically verifiable ones.

---

## 2. What ASRO Determines

ASRO determines the following, and only the following:

**Continuity status** — whether the attestation chain is intact, gapped, or broken across a defined time window.

**Reconciliation status** — whether the host's declared governance state is consistent with what edge observers actually received, to the degree that edge observation is technically possible.

**Discrepancy class** — the category and severity of any detected deviation from the declared governance state, using the canonical taxonomy defined in `/schemas/discrepancy_taxonomy.json`.

**Trust status** — the current attestation status of the system: `host_attested`, `reconciled_attested`, `under_review`, or `non_compliant`, as defined in `/schemas/controlled_vocab.json`.

**Review requirement** — whether a detected discrepancy requires human review, and at what response tier (S3 or S4).

These determinations are technically grounded, independently verifiable, and machine-auditable. They are the basis on which human reviewers, auditors, regulators, and counsel make their own determinations.

---

## 3. What ASRO Does Not Determine

ASRO does not determine, and cannot be represented as determining, any of the following:

**Legal compliance** — whether a system satisfies the requirements of any specific statute, regulation, or legal standard. This includes but is not limited to COPPA, SOPPA, HIPAA, GDPR, CCPA, the EU AI Act, NIST AI RMF, or any sectoral data governance requirement.

**Regulatory approval** — ASRO attestation does not constitute approval, certification, or endorsement by any regulatory body.

**Safety** — ASRO does not attest to the safety, harmlessness, accuracy, or quality of AI system outputs.

**Liability** — ASRO attestation does not create, limit, or transfer legal liability for any party.

**Sufficiency of evidence** — whether the attestation record produced by ASRO satisfies the evidentiary requirements of any specific legal proceeding, regulatory review, or compliance audit is a determination made by qualified counsel and regulators, not by ASRO.

---

## 4. The Evidence-Not-Adjudication Principle

ASRO is an evidence framework. This distinction is architectural, not rhetorical.

An adjudicatory framework makes binding determinations — it decides whether a rule was violated and what consequence follows. ASRO does neither. It records, attests, and makes records verifiable. Determinations that follow from those records are made by parties with appropriate legal authority.

The correct framing for all ASRO attestation language is:

> ASRO attestation provides the technical evidence base for compliance demonstration and governance review under applicable law. It does not itself determine legal compliance. Whether that evidence satisfies the specific requirements of applicable statutes is a legal determination made by qualified counsel and regulators, not by ASRO.

This framing must appear consistently wherever ASRO attestation is described in a compliance or regulatory context.

---

## 5. Regulatory Adoption Pathway

ASRO's value in regulated sectors — education, healthcare, financial services, municipal government — is specifically as an evidence provider, not a compliance authority.

The distinction matters practically. A school district using ASRO attestation to demonstrate compliance with COPPA or SOPPA is using ASRO to produce a continuous, independently verifiable record that the system's declared data handling policy remained in force without silent modification. That record supports the district's compliance demonstration. It does not substitute for legal counsel's determination that the record satisfies the applicable statute's requirements.

This is how ASRO is designed to be used. Operators who describe ASRO as guaranteeing compliance, rather than supporting compliance demonstration, are making representations ASRO cannot substantiate and the framework explicitly does not authorize.

---

## 6. Wording Discipline Requirements

The following phrases are prohibited in any context where ASRO attestation is described in relation to legal or regulatory compliance:

- "ASRO-certified compliant"
- "ASRO confirms legal compliance"
- "ASRO guarantees compliance"
- "ASRO ensures [statute] compliance"
- "Legally compliant by ASRO"
- "ASRO approval" used in a compliance context
- Any language implying ASRO is a regulatory authority or safe harbor

The following phrases are permitted and accurate:

- "ASRO attestation provides the technical evidence base for compliance demonstration"
- "ASRO preserves the evidence chain on which compliance determinations can be based"
- "ASRO attestation supports compliance demonstration and governance review under applicable law"
- "ASRO does not determine legal compliance; it provides independently checkable evidence"

---

## 7. Attorney Review Requirement

Before any institutional submission, regulatory filing, or procurement response that references ASRO attestation as supporting legal compliance demonstration, the Governance Rationale and Governance Response Protocol should be reviewed by qualified counsel. These documents are drafted to be legally defensible, but legal defensibility requires qualified legal review, not only careful drafting.

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
