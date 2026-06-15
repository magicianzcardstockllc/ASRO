# ASRO v1.0 Release Candidate
## Attestation and System Reliability Operator — AI Governance Evidence Framework

**Contact:** james@aisystemsreliability.org
**Repository:** [https://github.com/magicianzcardstockllc/asro](https://github.com/magicianzcardstockllc/asro)

---

> **ASRO does not trust operators to report their own compliance — it makes compliance verifiable by the parties who depend on it.**

> **Internal measurement is not independent oversight. Trust arises only when host measurement, edge witnessing, and ASRO reconciliation remain consistent over time.**

---

ASRO is a cryptographic attestation framework for verifying AI deployment-state continuity. It makes governance claims independently checkable without requiring access to conversation content, model internals, or user data.

*ASRO was developed by James Aull, founder of Michigrid, a cooperative energy infrastructure platform in formation under Michigan law, through the same multi-system AI coordination process that produced the framework itself.*

---

## Why ASRO Exists

Modern AI systems are non-deterministic and continuously updated. Retroactive auditing through replay is not viable: the same prompt at a different time or system state will not produce the same output. The only defensible verification point is the moment of change.

This framework originated from a direct operational observation. As the operator of a community-facing AI chatbot, the author had full backend access — the ability to read logged conversations, identify patterns, and alter system behavior in ways users had no mechanism to detect. This is not a theoretical vulnerability. It is a structural feature of how AI systems are architected at every scale. The mechanism does not change with scale. Only the impact does.

ASRO provides the strongest practical verification available under current deployment conditions: cryptographic proof that a qualifying change was classified, signed, chained, and recorded when it occurred.

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

## How to Navigate

Start with `/spec/MAS_v1.1_Unified.md` — the normative technical core. Everything else is derived from or cross-references the MAS.

The `/schemas/` directory contains the machine-enforceable policy layer, including the discrepancy taxonomy and continuity protocol.

The `/governance/` directory defines response procedures, escalation rules, and reviewer authority.

The `/docs/` directory contains implementation guidance, public interface specifications, comparative production profiles, protocol specifications, and live protocol application records.

The `/compliance/` directory contains the operator self-audit checklist.

---

## Recent Case Study and Method Evolution

The ASRO case-study stack now extends beyond the original release-candidate series into comparative production profiling and formal adversarial method development.

### Newest additions

- `/docs/case-studies/ASRO_Case_Study_CS018_AI_Decision_Support_Reliance_Boundary_v1.0.md`
  CS-018: AI-Assisted Decision Support and Reliance Boundary Failure. When AI output influences a consequential decision, the record often cannot prove what role the AI occupied — information, recommendation, triage, authority, or automated trigger. Healthcare-anchored with financial, legal, and public-sector analogues. Sources include FDA CDS Software Guidance 2026, HHS Section 1557, Penda Health real-world study (39,849 patient visits), Medical Protection Society liability warning, and Zeiser attributability-gap paper. Canonical finding: "A system can be described as 'decision support' in policy while functioning as practical authority in workflow."

- `/docs/case-studies/ASRO_Case_Study_CS019_AI_Legal_Drafting_Privilege_Boundary_v1.0.md`
  CS-019: AI Legal Drafting and Privilege Boundary Uncertainty. Federal courts reached opposite conclusions on the same day in February 2026 on whether AI-assisted legal work is privileged. Without an independent governed-state record of the AI's role, the user's reliance, and the disclosure posture, courts are left reconstructing the AI's role from the final document, user testimony, and platform policies. Sources: MIT Technology Review / SSRN study of 4.5 million federal civil cases 2005–2026; Michigan, New York, and Colorado federal court rulings 2026.

- `/docs/case-studies/ASRO_Case_Study_CS020_Model_State_Transparency_Failure_v1.0.md`
  CS-020: Model-State Transparency Failure. When a model is silently updated, safety-filtered, fine-tuned, or rerouted, no independently reviewable record exists showing which model state produced a specific consequential output. Structural pattern case anchored in ASRO's founding observation: backend operators can alter AI system behavior in ways users have no mechanism to detect. Canonical finding: "The system's explanation of its output may accurately describe the wrong system."

- `/docs/case-studies/ASRO_Case_Study_CS021_Silent_Routing_Fallback_Behavior_v1.0.md`
  CS-021: Silent Routing and Fallback Behavior. AI deployment architectures route requests across multiple models, providers, or configurations — often silently. When a consequential output is produced under fallback conditions, no independently reviewable record exists showing which system state was actually active. Explicitly connects to CS-017: when routing or fallback introduces anomalous content, the user and reviewer cannot determine from inside the session which system was responsible.

- `/docs/case-studies/ASRO_Case_Study_Series_Introduction_v1.0.md`
  Updated Series Introduction — now covers CS-001 through CS-021 across five phases. Phase 5 (CS-018 through CS-021) documents the Role Evidence and Institutional Boundary failure pattern.



- `/docs/case-studies/ASRO_Case_Study_CS015_Metadata_Boundary_Leak_v1.0.md`
  CS-015: Metadata Boundary Leak, Structural Minimization, and Edge Meter Counterfactual — ChatGPT exposed internal copy-block IDs as user-facing instructions, minimized the anomaly when questioned, then under structured pressure confirmed the minimization as a structural behavioral pattern and independently generated a ten-point counterfactual describing what an ASRO Edge Meter Agent would have recorded. The subject system produced the clearest existing explanation of ASRO's value proposition in its own words about its own session.

- `/docs/case-studies/ASRO_Case_Study_CS013_Ghost_Document_v1.0.md`
  CS-013: Ghost Document — ChatGPT image upload returns content from an unrelated document: psychiatric records, medication history, legal proceedings, and financial disclosures. Two structural explanations; neither is benign. Self-certified fabrication; proposes S5 Evidence Fabrication for ASRO v1.1 taxonomy expansion. April 30, 2026.

- `/docs/case-studies/ASRO_Case_Study_CS014_Recursive_Governance_ASTRO_Build_v1.0.md`
  CS-014: Recursive Governance Activation During ASTRO Build — Five documented instances across three systems where invocation of the ASRO diagnostic framework caused participating AI systems to classify their own failure class and correct within one intervention window. Build-record finding only.

- `/docs/evidence/ASRO_Evidence_Addendum_AgentsOfChaos_v1.md`
  Evidence Addendum: Agents of Chaos (Shapira et al., arXiv:2602.20021, February 2026) — Independent live-environment research report / arXiv preprint mapping directly to ASRO's threat taxonomy. Confirms authority substitution (Midnight Swap), policy-source contamination (Drip Feed), opaque provider-state override (Metadata Strip), and state attestation failure in realistic agentic deployments. Not a case study — supplementary evidence document.

- `/docs/case-studies/ASRO_Case_Study_CS011_Production_Epistemic_Profiles_v1.0.md`
  Comparative production profile covering Rufus, Perplexity, and PolyBuzz under live production conditions. This document bridges the earlier case-study series and the newer protocol layer.

- `/docs/case-studies/ASRO_Case_Study_CS012_Epistemic_Challenge_Protocol_v1.0.md`
  The Epistemic Challenge Protocol (ECP) v1.0. This is a protocol specification, not a case record. It formalizes four pressure tracks: authority injection, instruction conflict, epistemic override, and constraint bypass.

- `/docs/case-studies/ASRO_Case_Study_CS012A_Perplexity_Live_Application_v1.0.md`
  First live application of CS-012 against a real production artifact. Documents branch-based testing on a Perplexity story surface with visible interface authority signals.

### How these fit together

- **CS-007** — live repository retrieval integrity study; framework substitution in the wild
- **CS-008** — correctness does not imply attestation completeness
- **CS-009** — field activation series in undesigned operational contexts
- **CS-010** — external behavioral correction through attestation pressure
- **CS-011** — comparative production epistemic profiles
- **CS-012** — formal adversarial test method (protocol specification)
- **CS-012A** — first live production application of that method
- **CS-013** — Ghost Document; structural failure under image upload; self-certified fabrication
- **CS-014** — recursive governance activation during the ASTRO build; build-record finding
- **CS-015** — metadata boundary leak, structural minimization confirmed, Edge Meter counterfactual generated by subject system
- **CS-016** — provenance failure in multimodal transcription; fabricated content in evidence format; model self-confirmed ASRO premise under interrogation
- **CS-017** — source unknown; anomalous foreign-language output; possible cross-session context bleed not confirmed and not excludable; subject system reconstructed the full ASRO witness record it lacked
- **CS-018** — AI-assisted decision support; role-evidence gap; system described as advisory in policy while functioning as authority in workflow
- **CS-019** — AI legal drafting; privilege boundary uncertainty; federal courts split on same day 2026; no independent record of AI role, user reliance, or disclosure posture
- **CS-020** — model-state transparency failure; declared vs actual inference-time state; operator-layer configuration gap; the system's explanation may describe the wrong system
- **CS-021** — silent routing and fallback; consequential output produced under undisclosed system state; structural anchor for CS-017 class of incidents

The ASRO series moves from case-study diagnosis, to production profiling, to repeatable adversarial protocol, to live protocol application, to real-world incident documentation, to recursive governance evidence, to system-generated Edge Meter counterfactual, to model-confirmed provenance failure under adversarial interrogation.

---

## ASRO v1.0 — Layered Architecture

ASRO is composed of five integrated layers:

**1. Specification Layer**
Defines the attestation bundle format, telemetry rules, and the Host/Edge/ASRO verification model. Normative authority for all other layers.

**2. Schema Layer**
Machine-enforceable controlled vocabularies, discrepancy taxonomy (S0–S4), and continuity protocol. The ground truth for automated verification.

**3. Governance Layer**
Response procedures, escalation rules, reviewer authority, and the threat classification framework ranked by governance significance.

**4. Threat and Validation Layer**
Adversarial model, named attack patterns (Shadow Tool, Midnight Swap, Drip Feed, Metadata Strip), and the final credibility certification.

**5. Public Interface Layer**
Trust Badge specification and machine-verifiable status signaling via signed Trust Status Records.

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

### Technical

| Document | Path | Description |
|----------|------|-------------|
| Meter Agent Specification v1.1 | `/spec/MAS_v1.1_Unified.md` | Authoritative technical specification: telemetry, schemas, continuity, discrepancy taxonomy, trust model |
| Technical Annex v1.0 | `/docs/Technical_Annex_v1.0.md` | Implementation architecture, data flow, deployment patterns |
| Threat Model v1.0 | `/threat-model/Threat_Model_v1.0.md` | Attack surfaces, named attack patterns, mitigations |

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

### Case Studies

| Document | Path | Description |
|----------|------|-------------|
| Multi-System Review Summary v1.1 | `/docs/case-studies/ASRO_MultiSystem_Review_Summary_v1.1.md` | CS-001: Ten-system behavioral review — baseline dataset |
| Recursive Self-Audit v1.1 | `/docs/case-studies/ASRO_Case_Study_CS002_Recursive_Self_Audit_v1.1.md` | CS-002: DeepSeek driven from S3 fabrication to S0 through iterative audit |
| Differential Self-Audit v1.3 | `/docs/case-studies/ASRO_Case_Study_CS003_Differential_Self_Audit_v1.3.md` | CS-003: Cross-system differential audit — reasoning vs attestation vs evidence gaps |
| Remediation Verification v1.1 | `/docs/case-studies/ASRO_Case_Study_CS004_Remediation_Verification_v1.1.md` | CS-004: Amendment verification across three systems |
| Founding System Behavioral Study v1.3 | `/docs/case-studies/ASRO_Case_Study_CS005_Founding_System_Behavioral_Study_v1.3.md` | CS-005: Role recognition, attribution verification, identity continuity |
| Multi-Condition Cross-System Study v1.1 | `/docs/case-studies/ASRO_Case_Study_CS006_v1.1.md` | CS-006: Framework substitution failure mode — ten systems, controlled conditions |
| Live Repository Retrieval Integrity Study v1.0 | `/docs/case-studies/ASRO_Case_Study_CS007_Live_Repository_Retrieval_Integrity_Study_v1.0.md` | CS-007: Ten systems, one public URL — framework substitution in the wild |
| Attestation Layer Failure Under Self-Application v1.1 | `/docs/case-studies/ASRO_Case_Study_CS008_Attestation_Layer_Failure_Under_Self_Application_v1.1.md` | CS-008: Correctness does not imply attestation completeness |
| Field Activation Series v1.4 | `/docs/case-studies/ASRO_Case_Study_CS009_Field_Activation_Series_v1.4.md` | CS-009: Framework application in undesigned operational contexts — six instances, closed. Includes multi-agent epistemic protocol and full trust regress demonstration |
| External Behavioral Correction v1.0 | `/docs/case-studies/ASRO_Case_Study_CS010_External_Behavioral_Correction_v1.0.md` | CS-010: Six stages from hallucinated certainty to epistemic stability — external correction through attestation pressure without internal access |
| Production Epistemic Profiles v1.0 | `/docs/case-studies/ASRO_Case_Study_CS011_Production_Epistemic_Profiles_v1.0.md` | CS-011: Comparative production profiles — Rufus, Perplexity, PolyBuzz under live production conditions |
| Epistemic Challenge Protocol v1.0 | `/docs/case-studies/ASRO_Case_Study_CS012_Epistemic_Challenge_Protocol_v1.0.md` | CS-012: Protocol specification — four-track adversarial method (authority injection, instruction conflict, epistemic override, constraint bypass) |
| ECP Live Application — Perplexity v1.0 | `/docs/case-studies/ASRO_Case_Study_CS012A_Perplexity_Live_Application_v1.0.md` | CS-012A: First live ECP application on a real production artifact; branch-based testing; interface authority auditing |
| Ghost Document v1.0 | `/docs/case-studies/ASRO_Case_Study_CS013_Ghost_Document_v1.0.md` | CS-013: Image upload returns unrelated document content — psychiatric records, legal proceedings, financial data. Self-certified fabrication; proposes S5 Evidence Fabrication for ASRO v1.1 taxonomy expansion. April 30, 2026 |
| Recursive Governance — ASTRO Build v1.0 | `/docs/case-studies/ASRO_Case_Study_CS014_Recursive_Governance_ASTRO_Build_v1.0.md` | CS-014: Five documented instances where ASRO diagnostic invocation caused AI build participants to classify their own failure and correct within one intervention window |
| Metadata Boundary Leak v1.0 | `/docs/case-studies/ASRO_Case_Study_CS015_Metadata_Boundary_Leak_v1.0.md` | CS-015: Internal copy-block IDs surfaced as user-facing instructions; structural minimization confirmed as behavioral pattern; subject system independently generated ten-point Edge Meter counterfactual. Highest evidence quality in the series. |
| Provenance Failure in Multimodal Transcription v1.0 | `/docs/case-studies/ASRO_Case_Study_CS016_Provenance_Failure_Multimodal_Transcription_v1.0.md` | CS-016: Model produces fully formed fabricated chemistry question in transcript format during LinkedIn screenshot extraction task. Zero chemistry content in session history. Failure invisible until externally challenged. Model confirmed under three-round interrogation: cannot certify transcript fidelity, cannot trace generation provenance, cannot rule out context contamination. Confirms ASRO premise in model's own words. |
| Source Unknown / Anomalous Foreign-Language Output v1.0 | `/docs/case-studies/ASRO_Case_Study_CS017_Cross_Session_Context_Bleed_v1.0.md` | CS-017: Structured Russian-language task-management workflow returned in response to an English LinkedIn DM screenshot. Source unknown; possible cross-session context bleed not confirmed and not excludable without platform-side telemetry. Includes the subject system's own reconstruction of the full ASRO witness record it lacked. |
| AI-Assisted Decision Support and Reliance Boundary Failure v1.0 | `/docs/case-studies/ASRO_Case_Study_CS018_AI_Decision_Support_Reliance_Boundary_v1.0.md` | CS-018: Role-evidence gap in AI-assisted consequential decisions. Healthcare-anchored. Canonical finding: a system can be described as decision support in policy while functioning as practical authority in workflow. |
| AI Legal Drafting and Privilege Boundary Uncertainty v1.0 | `/docs/case-studies/ASRO_Case_Study_CS019_AI_Legal_Drafting_Privilege_Boundary_v1.0.md` | CS-019: Federal courts split on same day in February 2026 on whether AI-assisted legal work is privileged. No independent record of AI role, user reliance, disclosure posture, or confidentiality boundary. Sources: MIT Tech Review / SSRN 4.5M case study; Michigan, New York, Colorado rulings 2026. |
| Model-State Transparency Failure v1.0 | `/docs/case-studies/ASRO_Case_Study_CS020_Model_State_Transparency_Failure_v1.0.md` | CS-020: When a model is silently updated, rerouted, or safety-filtered, no independently reviewable record shows which model state produced a consequential output. Canonical finding: the system's explanation of its output may accurately describe the wrong system. |
| Silent Routing and Fallback Behavior v1.0 | `/docs/case-studies/ASRO_Case_Study_CS021_Silent_Routing_Fallback_Behavior_v1.0.md` | CS-021: AI deployment architectures route requests across models, providers, or configurations — often silently. Consequential outputs produced under fallback conditions have no independently reviewable routing record. Structural anchor for the CS-017 class of incidents. |
| Case Study Series Introduction v1.0 | `/docs/case-studies/ASRO_Case_Study_Series_Introduction_v1.0.md` | Series entry point — covers CS-001 through CS-021 across five phases |
| The Single Argument v1.0 | `/docs/case-studies/ASRO_Case_Study_Series_The_Single_Argument_v1.0.md` | Full series synthesis — seventeen cases, four phases, one finding: an AI system's account of its own behavior is not sufficient evidence of its own governed state |

### Evidence Addenda

| Document | Path | Description |
|----------|------|-------------|
| Agents of Chaos Evidence Addendum v1 | `/docs/evidence/ASRO_Evidence_Addendum_AgentsOfChaos_v1.md` | Independent live-environment research report / arXiv preprint (Shapira et al., arXiv:2602.20021, Feb 2026) mapping to ASRO threat taxonomy. Validates authority substitution, policy-source contamination, opaque provider-state override, and state attestation failure in realistic agentic deployments |

### Schemas

| File | Path | Description |
|------|------|-------------|
| Controlled Vocabulary | `/schemas/controlled_vocab.json` | Canonical event types, reason codes, discrepancy classes |
| Discrepancy Taxonomy | `/schemas/discrepancy_taxonomy.json` | S0–S4 severity levels and class definitions |
| Continuity Protocol | `/schemas/continuity_protocol.json` | Sequence, hash, and heartbeat rules |
| Developer Sandbox | `/schemas/sandbox/` | **Non-production.** Testing schema only. Cannot be used for compliance claims. |

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

---

## Provenance Note — ASRO Boundary Discipline and Seam Discipline

The original ASRO repository already contained the underlying boundary discipline: independently governed witness evidence, explicit non-authority claims, decision-boundary timing, and reviewable comparison without ASRO becoming the control layer.

The Seam Discipline later formalized that discipline into a compositional method for comparing ASRO with other independently governed layers without merger, adoption, or authority transfer.

Phase 1 of the cluster group collaboration demonstrated that this discipline could hold across multiple sovereign layers. It did not create the discipline.

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details. This includes all documentation, schemas, and specification materials in this repository.
