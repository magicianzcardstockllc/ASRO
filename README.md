# ASRO v1.0 Release Candidate
## Attestation and System Reliability Operator — AI Governance Evidence Framework

**Status:** ASRO v1.0 Release Candidate — Framework Complete, Pending External Review and Adoption Pathways

**Contact:** james@michigrid.org
**Repository:** https://github.com/magicianzcardstockllc/asro

James Aull is the founder of Michigrid, a cooperative energy infrastructure platform in formation under Michigan law, developed through the same multi-system AI coordination process that produced ASRO.

---

> **ASRO does not trust operators to report their own compliance — it makes compliance verifiable by the parties who depend on it.**

> **Internal measurement is not independent oversight. Trust arises only when host measurement, edge witnessing, and ASRO reconciliation remain consistent over time.**

---

## What ASRO Is

ASRO is a framework for cryptographically attesting to AI deployment-state continuity without inspecting private conversations, model internals, or raw user data.

It addresses a structural gap that behavioral monitoring cannot reach: AI systems can be silently modified after deployment — policies changed, tool permissions expanded, runtime configurations altered — without any user knowledge and without any external record of the change. ASRO makes those changes independently checkable by the parties who depend on the system's governance claims.

ASRO answers four questions:

- Did the declared policy remain in force?
- Did tool permissions drift without declaration?
- Did runtime state change without a signed attestation event?
- Did the host's claimed governance state match what edge observers actually received?

---

## What ASRO Is Not

ASRO is **not**:

- A safety certification system
- A legal compliance guarantor
- A regulator, court, or enforcement authority
- A promise that the AI will never hallucinate or behave badly
- A replacement for legal counsel or sectoral regulation

**ASRO is an evidence framework.** It provides the technical record from which auditors, regulators, counsel, and partners can make their own determinations. It does not make those determinations itself.

---

## The Adversarial Model

> **The primary adversary in ASRO is not a system that refuses to attest, but a system that attests selectively enough to preserve plausible compliance while hiding governance-relevant change.**

---

## Certification Status

> **ASRO v1.0 passes final credibility review as an attestation-and-governance evidence framework, not as a safety certification system, legal compliance guarantor, or enforcement authority.**

*Final Credibility Pass Report: `/governance/Credibility_Pass_Report_v1.0.md`*

---

## For Users — Privacy and Verification

Deploy an **Edge Meter Agent** to independently witness whether the AI system's declared governance state matches what your device actually receives.

An Edge Meter Agent records:
- The system's declared configuration and policy fingerprints
- Whether those fingerprints remained consistent across sessions
- Whether observable behavior matched declared permissions
- Discrepancies between host claims and edge-observed state

You can independently check whether the operator's attestation record remains consistent with what your device observed.

*See: `/spec/MAS_v1.1_Unified.md` — Section: Edge Meter Agent*

---

## For Operators — Compliance and Governance

Deploy a **Host Meter Agent** to produce a continuous, cryptographically signed record of your system's declared governance state and load-bearing runtime events.

ASRO attestation provides:
- A technically verifiable record of governance continuity for your board, partners, and regulators
- Evidence that supports compliance demonstration and governance review under applicable law
- A structured audit trail that reviewers can examine without accessing conversation content or model internals

**ASRO attestation supports compliance demonstration. Whether that evidence satisfies the specific requirements of applicable statutes is a legal determination made by qualified counsel and regulators, not by ASRO.**

*See: `/compliance/Implementation_Readiness_Checklist_v1.0.md` — start here*

---

## For Regulators and Auditors — Evidence and Review

ASRO provides a continuous, signed, independently checkable attestation record that answers:
- Was the declared governance configuration in force during this period?
- Did any undeclared change occur?
- Is the continuity chain intact, or are there gaps requiring explanation?
- What is the system's current attestation status?

ASRO review findings are evidentiary, not adjudicatory. Legal compliance determinations require human judgment applied to the attestation evidence, combined with the specific requirements of applicable statutes.

ASRO does not determine legality or compliance outcome; it preserves the evidence chain on which those determinations can be based.

*See: `/governance/Governance_Response_Protocol_v1.0.md`*
*See: `/governance/Governance_Rationale_v1.1.md`*

---

## Attestation Status Tiers

| Status | Label | Meaning |
|--------|-------|---------|
| `host_attested` | **Host Attestation Active** | Operator-side attestation active. No claim of edge reconciliation. |
| `reconciled_attested` | **Independent Verification Active** | Host and edge attestation are both active and reconcilable under the declared observation conditions. |
| `under_review` | **Attestation Under Review** | Trust-relevant anomaly active. Status not clean. Required during S4 alerts. |
| `non_compliant` | **Attestation Not Current** | System cannot credibly claim current ASRO attestation compliance. |

**The Trust Badge communicates attestation status only. It does not imply safety, accuracy, quality, harmlessness, or regulatory approval.**

A system that structurally prevents meaningful edge observation cannot claim `Independent Verification Active` status.

*See: `/docs/ASRO_Trust_Badge_Spec_v1.0.md`*

---

## Known Limitations

These limitations are stated prominently because honest scope is what makes this framework credible.

1. ASRO does not guarantee good model behavior.
2. ASRO does not eliminate hallucinations.
3. ASRO does not by itself prove legal compliance.
4. ASRO does not prevent all infrastructure compromise.
5. Host-only attestation is weaker than host-edge reconciled attestation.
6. Low-visibility environments weaken edge assurance even when honestly declared.

---

## Document Index

> **Note on file formats:** Markdown (`.md`) files are the canonical versions of all documents. DOCX files are distribution copies for email attachment and institutional submission. Where both exist, the markdown version is authoritative.

### Technical

| Document | Path | Description |
|----------|------|-------------|
| Meter Agent Specification v1.1 | `/spec/MAS_v1.1_Unified.md` | Authoritative technical specification: telemetry, schemas, continuity, discrepancy taxonomy, trust model |
| Technical Annex v1.0 | `/docs/Technical_Annex_v1.0.md` | Implementation architecture, data flow, deployment patterns |
| Threat Model v1.0 | `/threat_model/Threat_Model_v1.0.md` | Attack surfaces, named attack patterns, mitigations |

### Governance

| Document | Path | Description |
|----------|------|-------------|
| Governance Rationale v1.1 | `/governance/Governance_Rationale_v1.1.md` | Why attestation-based governance is necessary; adoption logic; escalation rules |
| Governance Response Protocol v1.0 | `/governance/Governance_Response_Protocol_v1.0.md` | Reviewer roles, S3/S4 response procedures, timelines |
| Threat Classification Framework v1.0 | `/governance/Threat_Classification_Framework_v1.0.md` | Five threat categories ranked by governance significance |
| Credibility Pass Report v1.0 | `/governance/Credibility_Pass_Report_v1.0.md` | Final quality certification — ten findings, all resolved |

### Compliance and Public

| Document | Path | Description |
|----------|------|-------------|
| Implementation Readiness Checklist v1.0 | `/compliance/Implementation_Readiness_Checklist_v1.0.md` | 10-point operator self-audit before claiming ASRO compliance |
| Trust Badge Specification v1.0 | `/docs/ASRO_Trust_Badge_Spec_v1.0.md` | Human-readable and machine-readable badge status rules |

### Schemas

| File | Path | Description |
|------|------|-------------|
| Controlled Vocabulary | `/schemas/controlled_vocab.json` | Canonical event types, reason codes, discrepancy classes |
| Discrepancy Taxonomy | `/schemas/discrepancy_taxonomy.json` | S0–S4 severity levels and class definitions |
| Continuity Protocol | `/schemas/continuity_protocol.json` | Sequence, hash, and heartbeat rules |
| Developer Sandbox | `/schemas/sandbox/` | **Non-production.** Testing schema only. Cannot be used for compliance claims. |

### Case Studies

| Document | Path | Description |
|----------|------|-------------|
| Intent Mismatch Detection v1.0 | `/docs/case-studies/ASRO_Case_Study_Intent_Mismatch_v1.0.md` | A real governance event mapped to the ASRO detection and classification framework — the same AI system that generated the failure subsequently used the framework to classify it |

### Provenance and Review Record

| Document | Path | Description |
|----------|------|-------------|
| Complete Memo Record | `/review-record/ASRO_Complete_Memo_Record.md` | Full unedited multi-system memo record across all development rounds |
| Strategic Roadmap v1 | `/review-record/ASRO_Roadmap_v1.md` | Phase structure, placement record, institutional engagement path |
| Multi-System Review Summary | `/review-record/ASRO_MultiSystem_Review_Summary.md` | Structured synthesis of findings and decisions from the review process |

### Outreach

| Document | Path | Description |
|----------|------|-------------|
| AI Now Letter | `/outreach/AI_Now_Letter.md` | Phase 3 outreach: structural accountability frame |
| CSET Letter | `/outreach/CSET_Letter.md` | Phase 3 outreach: governance infrastructure frame |

---

## Repository

ASRO is developed and maintained as an open governance framework.

Repository: [https://github.com/magicianzcardstockllc/asro](https://github.com/magicianzcardstockllc/asro)

---

*ASRO v1.0 Release Candidate. This repository contains the frozen technical, governance, threat, compliance, and public-facing documents for the release candidate stack.*

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
