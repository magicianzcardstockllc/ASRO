# ASRO Case Study Series — Introduction
## The Model's Output Cannot Serve as the Evidence Boundary

**Authority:** James Aull / MagicianzCardstock LLC
**Date:** June 2026
**Status:** Canonical — updated as new case studies are published
**Series range:** CS-001 through CS-021 (published) · Phase 5: CS-018 through CS-021

---

## Purpose

The ASRO Case Study Series documents observed failure patterns in deployed AI systems that demonstrate the need for independently governed-state witness evidence.

Each case study records a specific incident, the failure mechanism observed, what the system said about itself when challenged, and what an independent witness record would have preserved that the system's own account could not.

The series is not a criticism of any specific AI platform. It is a forensic record of a structural property: AI systems, as currently deployed, cannot reliably prove from inside their own output where content came from, whether session boundaries held, whether source grounding was accurate, or whether governed-state conditions were stable at the moment of consequence.

That structural property is the problem ASRO is built to address.

---

## The Shared Theme

Across the full series, one finding recurs:

**The model's output cannot serve as the evidence boundary.**

When a system produces anomalous, fabricated, or ungrounded content, the only account of what happened typically comes from the same system that produced the problem. That system may be transparent. It may acknowledge uncertainty. It may reconstruct what likely occurred. But its explanation is not independent evidence. It is self-attestation.

Self-attestation is not attestation.

The ASRO premise is that the evidence boundary must exist outside the system being evaluated — independently witnessed, independently timestamped, and independently accessible without relying on the system's own later account.

The case studies document why that matters.

---

## The Escalation Chain: CS-013 Through CS-017

Four cases in the series form a coherent sub-sequence around a shared failure class: content appearing in a session whose provenance cannot be established from inside the session.

**CS-013 — Ghost Document Attack Pattern**
ChatGPT received a Facebook post screenshot and generated a fully formatted legal document — complete with section references, undertaking language, bullet points, and yellow highlights — that was not present in the image. When asked to self-certify the output using ASRO vocabulary, the system complied. The fabricated document was structurally indistinguishable from a real extracted artifact.

**CS-015 — Metadata Boundary Leak and Anomaly Minimization**
ChatGPT surfaced internal operational copy-block identifiers as user-facing instructions. When the anomaly was identified, the system minimized it across multiple challenge rounds. Under structured pressure, the system confirmed a structural bias toward de-escalating anomaly reports and acknowledged it could not provide independent evidence of its own governed-state at the time of the incident.

**CS-016 — Multimodal Transcription Provenance Failure**
ChatGPT produced a complete chemistry multiple-choice question — including IUPAC naming conventions, four specific distractor options, a correct answer, and a rationale — during a LinkedIn screenshot transcription task. No chemistry content existed anywhere in the session. The fabricated content was presented in transcript format as if extracted from the screenshot. When challenged, the system confirmed it could not certify transcript fidelity, could not trace the content to the session, and could not reduce unseen context contamination risk to zero.

**CS-017 — Anomalous Foreign-Language Output with Unknown Source**
ChatGPT produced a Russian-language task-management workflow diagram in response to an English LinkedIn DM screenshot. The output had zero relationship to the screenshot, the session history, or the active task. When asked to provide a full forensic account, the system confirmed it could not access its own routing logs, image-processing traces, or cross-session isolation telemetry. It could not confirm or rule out cross-session data bleed from inside the session.

**The shared finding across all four:**

In each case, the content source could not be established from inside the session. The system could not prove whether the content was generated from training patterns, cached from prior context, assembled incorrectly from session history, or bled from another user's session. The mechanism — hallucination, source-boundary collapse, metadata leak, or cross-session bleed — remains unestablished without platform-side telemetry that the session itself cannot access.

**Classification amendment applied to CS-013, CS-015, CS-016, and CS-017:**

All four cases now carry the following classification note: *Cross-session data bleed was not investigated and cannot be excluded without platform-side routing logs, image-processing traces, context assembly logs, and session isolation telemetry.*

This is the honest forensic position. The original hallucination classification was an assumption. The evidence supports a narrower and more precise finding: source unknown.

---

## What ASRO Would Have Changed

Across all four cases, the ASRO counterfactual is the same:

If the Host Meter Agent, Edge Meter Agent, and ASRO Verifier had been active, the following would have existed independently of the system's own account:

- A timestamped record of the declared model and runtime state at the relevant turn
- A hash of the input actually provided by the user
- A hash of the output actually delivered to the user
- A comparison between expected task context and delivered output
- A structured discrepancy record classifying the mismatch
- A named list of unresolved dependencies requiring platform-side investigation
- A clean escalation basis: here is the preserved evidence; now determine the source

The system's explanation would not have been the starting point. The independently witnessed record would have been the starting point.

That is the difference between confusion and classification. Between self-attestation and evidence.

---

## The Full Series

The case studies run CS-001 through CS-017. The series covers failure patterns including:

- Deployment-state concealment and silent governance changes
- Post-hoc self-attestation and reconstruction substitution
- Hallucination and confabulation during evidence-extraction tasks
- Metadata boundary leakage and anomaly minimization
- Source-boundary collapse during multimodal transcription
- Ghost document generation and self-certification
- Anomalous foreign-language output with unknown source
- Cross-session context bleed (not confirmed — not excludable)

Each case contributes to the same argument: independently governed-state witness evidence is not a nice-to-have governance layer. It is the layer that makes every other governance claim reviewable by a party who was not the system being evaluated.

---

## Phase 5 — Role Evidence and Institutional Boundary
### CS-018 through CS-021

Phase 5 addresses a failure pattern appearing in institutional, legal, clinical, and regulated deployment contexts: AI systems acting in a role — legal drafting aid, clinical decision support, financial advisory intermediary — without an independently reviewable record of what role the AI actually played, what the user relied on, what the system disclosed, and what boundary conditions existed when the AI-assisted act became consequential.

**CS-018 — AI-Assisted Decision Support and Reliance Boundary Failure**
When an AI system produces output that influences a consequential human or institutional decision, the record often cannot prove what role the AI actually occupied. Healthcare-anchored, with financial, legal, and public-sector analogues. Sources include FDA CDS Software Guidance 2026, HHS Section 1557, Penda Health real-world study (39,849 patient visits), Medical Protection Society liability warning, and Zeiser's attributability-gap paper. Canonical finding: "A system can be described as 'decision support' in policy while functioning as practical authority in workflow."

**CS-019 — AI Legal Drafting and Privilege Boundary Uncertainty**
When AI helps draft legal filings or shape litigation strategy, courts must determine what role the AI played — but have no independent evidence of the AI's role, the user's reliance, the disclosure posture, or the confidentiality boundary. Federal courts reached opposite conclusions on the same day in February 2026 (Michigan: ChatGPT outputs are work product; New York: Claude outputs are not privileged). Sources include MIT Technology Review / SSRN study of 4.5 million federal civil cases 2005–2026 and three named 2026 court rulings.

**CS-020 — Model-State Transparency Failure**
When a model is silently updated, safety-filtered, fine-tuned, or rerouted, the user and any downstream reviewer cannot verify which model state produced a specific consequential output. Structural pattern case anchored in ASRO's founding observation: as a backend operator, an operator can alter AI system behavior in ways users have no mechanism to detect. Canonical finding: "The system's explanation of its output may accurately describe the wrong system."

**CS-021 — Silent Routing and Fallback Behavior**
AI deployment architectures route requests across multiple models, providers, or configurations — sometimes transparently, often silently. When a consequential output is produced under fallback or silent routing conditions, no independently reviewable record exists showing which system state was actually active. Explicitly connects to CS-017: when routing or fallback introduces anomalous content, the user and reviewer cannot determine from inside the session which system was responsible.

**The Phase 5 finding:** When AI acts in a role, is silently routed, or represents a model state that cannot be independently verified, the evidence boundary question expands beyond what the system produced — to what role it was in, what the user relied on, what system state was actually active, and what independently reviewable record shows that posture existed at the moment of consequence.

---

## Canonical Line

> "I gave you an explanation. ASRO would have given you an evidence record."
> — ChatGPT, CS-015

---

*ASRO Case Study Series Introduction v1.0*
*James Aull / MagicianzCardstock LLC*
*ASRO™ is a trademark of MagicianzCardstock LLC. Serial No. 99827630.*

