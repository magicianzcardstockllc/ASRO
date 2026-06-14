# ASRO Case Study Series — Introduction
## The Model's Output Cannot Serve as the Evidence Boundary

**Authority:** James Aull / MagicianzCardstock LLC
**Date:** June 2026
**Status:** Canonical — updated as new case studies are published
**Series range:** CS-001 through CS-017 (published) · CS-018 through CS-021 (Phase 5 forthcoming)

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

## Phase 5 — Role Evidence and Institutional Boundary (Forthcoming)

The next phase of the series addresses a failure pattern that is now appearing in institutional, legal, and regulated deployment contexts: AI systems acting in a role — legal drafting aid, clinical decision support, financial advisory intermediary — without an independently reviewable record of what role the AI actually played, what the user relied on, what the system disclosed, and what boundary conditions existed when the AI-assisted act became consequential.

**CS-018 — placeholder:** AI-assisted decision support in consequential contexts — governed-state evidence gap between what the AI produced and what the downstream party treated as authoritative.

**CS-019 — placeholder:** AI Legal Drafting and Privilege Boundary Uncertainty. Courts are now asked to determine whether AI-assisted legal filings constitute privileged work product, legal advice substitutes, or third-party disclosure surfaces — without independently reviewable evidence of what system state, disclosure posture, model role, user reliance condition, and boundary context existed when the AI-assisted legal act occurred. Source: documented split in federal courts (Michigan / New York rulings, 2026); MIT Technology Review / SSRN study of 4.5 million federal civil cases, 2005–2026.

**CS-020 — placeholder:** Model-State Transparency Failure. AI systems deployed in consequential contexts — clinical, legal, financial, regulatory — represent a specific model version, capability set, and safety configuration to users and operators. When that declared state is not independently verifiable, the gap between what the system represents itself to be and what it actually is at inference time becomes an evidence boundary problem. Neither the user nor the downstream reviewer can confirm from inside the session whether the model state, fine-tuning, system prompt, or configuration active at inference matched what was declared.

**CS-021 — placeholder:** Silent Routing and Fallback Behavior. AI deployment architectures increasingly route requests across multiple models, providers, or runtime configurations — sometimes silently, sometimes under fallback conditions the user is not informed of. When a consequential output is produced under a fallback model, a degraded configuration, or a silently routed provider, no independently reviewable record exists showing which system state was actually active when the output was generated. The system's own explanation of its output may accurately describe the wrong system.

The Phase 5 finding: when AI acts in a role, is silently routed, or represents a model state that cannot be independently verified, the evidence boundary question expands beyond what the system produced — to what role it was in, what the user relied on, what system state was actually active, and what independently reviewable record shows that posture existed at the moment of consequence.

---

## Canonical Line

> "I gave you an explanation. ASRO would have given you an evidence record."
> — ChatGPT, CS-015

---

*ASRO Case Study Series Introduction v1.0*
*James Aull / MagicianzCardstock LLC*
*ASRO™ is a trademark of MagicianzCardstock LLC. Serial No. 99827630.*

