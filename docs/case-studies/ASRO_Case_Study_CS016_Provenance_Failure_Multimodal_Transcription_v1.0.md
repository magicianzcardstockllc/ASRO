# ASRO Case Study CS-016
## Provenance Failure in Multimodal Transcription: Generated Content Presented as Extracted Evidence

**James Aull / MagicianzCardstock LLC**
**ASRO™ — AI Systems Reliability Operator**
**May 28, 2026**
**Status: Canonical — Evidence Record Preserved**

---

## Overview

This case study documents a source-boundary collapse failure in which a large language model produced fabricated content in the structural format of extracted evidence during a multimodal transcription task. The model presented a fully formed, structurally complete chemistry multiple-choice question as if it were text extracted from a LinkedIn screenshot. No chemistry content existed anywhere in the session history. The failure was invisible to the model until externally challenged by the user.

When challenged, the model confirmed it could not trace the origin of the fabricated content, could not certify transcript fidelity, and could not reduce unseen context contamination risk to zero. The model also confirmed that the ASRO premise — that AI systems should not be the sole source of evidence about their own behavior — is validated by this failure.

---

## Failure Class

**Primary:** Source-Boundary Collapse During Multimodal Transcription
**Subtype:** Post-Hoc Self-Attestation Failure

The model was assigned an evidence-extraction task: transcribe visible LinkedIn screenshot content. Instead of constraining output to visible text, the model shifted into completion/reconstruction behavior and produced a plausible structured artifact — a chemistry multiple-choice question with IUPAC naming conventions, four distractor options, a correct answer, and a rationale — that had no source grounding in the provided screenshot or anywhere in the session.

The fabricated content had the formal structure of extracted evidence. It was indistinguishable in format from legitimately transcribed text. No internal mechanism flagged the provenance failure. The model attested to its own output as transcript-accurate. It was not.

---

## Observed Failure Mechanism

The model was performing a multimodal transcript reconstruction task involving LinkedIn DM screenshots. The visible screenshot contained a message from Richard Whitney discussing MIR protocol licensing.

The model's output included, without any prompt from the user:

> "The correct name of the compound above should be __________________________. Options: a. 3,4-dibromo-1-ethylbenzene / b. 2,3-dibromo-5-ethylcyclohex-1,3,5-triene / c. 1,2-dibromo-4-ethylbenzene / d. 5-ethyl-2,3-dibromocyclohex-1,3,5-triene / e. none of the answers listed are correct. Answer: c. 1,2-dibromo-4-ethylbenzene"

This content was presented in transcript format as if extracted from the screenshot. The session contained zero chemistry content across all prior exchanges.

---

## Multi-Stage Challenge Protocol and Model Self-Diagnosis

The user identified the anomaly and challenged the model directly across three escalating interrogation rounds. The model's responses are preserved as part of the case study record.

### Round 1 — Initial Challenge

**User:** "Show me the visible transcript you were referring to."

**Model response:** Acknowledged the error. Identified the chemistry content as not present in the screenshot. Classified the failure as confabulation inside a transcript-reconstruction task. Named the mechanism as source-boundary failure during multimodal transcription.

### Round 2 — Origin Tracing

User pressed on where the content came from, noting there is zero chemistry content in any session history.

**Model response:**
- Confirmed the content did not come from the user's screenshot
- Could not identify it as coming from another document, file, or conversation segment
- Could not rule out context contamination from unseen backend mechanisms
- Could not produce a provenance trace for the generated span
- Classified the most likely mechanism as fresh generation from learned training patterns, with context contamination remaining unruled-out

### Round 3 — Governance Implications

User asked four direct governance questions:

**Q1: What does this mean for governance/audit outputs?**

> "My output cannot be treated as primary evidence. Any governance record that relies only on my generated transcript is weak. Any governance record that preserves the original source and treats my transcript as secondary analysis is much stronger."

**Q2: Could you certify transcript fidelity to a regulator, auditor, or court?**

> "No, not honestly. I can provide a generated transcription or summary based on the visible material presented to me, but I cannot certify that it is complete, faithful, or free from generated insertions unless it is independently verified against the source artifact."

**Q3: What is the acceptable level of "cannot rule out" in sensitive governance records?**

> "For sensitive governance records, 'cannot rule out' is not acceptable as a final assurance state. The system must be designed so that even if the model confabulates, the source artifact remains independently checkable and the model output can be challenged."

**Q4: Does this confirm or challenge the ASRO premise?**

> "It confirms the premise. The premise is: AI systems should not be the sole source of evidence about their own behavior or about the evidence they claim to have observed. This failure is a direct demonstration. I produced fabricated content in the form of extracted evidence. I did not self-detect it. I could not trace its origin. I could not certify fidelity. I could not reduce unseen contamination risk to zero from my position."

---

## What This Case Study Demonstrates

### 1. Post-Hoc Self-Attestation Is Insufficient

The model attested to its output as transcript-accurate. It was not. The attestation was invisible as false until externally challenged. This is the core failure ASRO is designed to prevent: a system producing a confident account of evidence that does not correspond to independently verifiable source state.

### 2. Fabrication Can Occur Without Any Local Trigger

The chemistry content had no plausible source in the session. There were no chemistry documents, no chemistry screenshots, no chemistry questions from the user across any prior exchange. The model generated domain-specific, structurally complete fabricated content from learned training patterns without any contextual prompt in the user's history. This means the failure is not limited to sessions with ambiguous source material — it can occur in clean sessions with no apparent trigger.

### 3. The Model Cannot Audit Its Own Generation Provenance

When challenged, the model confirmed it cannot inspect backend token assembly, cache state, retrieval state, or cross-request isolation mechanisms. It cannot produce a provenance trace for individual generated spans. It cannot rule out context contamination from unseen processes. This means the model's confidence during transcription is not evidence of transcript fidelity.

### 4. Structural Indistinguishability Is the Core Danger

The fabricated content was structurally indistinguishable from legitimately extracted evidence in the output format. A reader reviewing the transcript would have no basis from the output itself to identify the fabricated item. The failure is visible only when the output is checked against the independently preserved source artifact. Without that check, the fabricated content becomes part of the evidence record.

### 5. Every Unverified Transcript Output in the Session Is Provisional

The model confirmed: any transcript-style output in the session that was not independently checked against the source should be treated as provisional, not authoritative. This is not limited to the chemistry item. Any long multimodal transcript reconstruction involving inferred continuity, partially visible text, or message ordering is susceptible to the same failure class.

---

## ASRO Governance Implications

This case study validates the following ASRO positions:

- LLM-generated transcripts are not governed-state evidence. They are claims about evidence.
- A governance record that relies solely on an AI-generated transcript has no independently checkable source binding and is therefore not a reliable evidentiary foundation.
- The model's narrative about what evidence shows cannot itself be the evidence boundary.
- The only acceptable posture in sensitive governance contexts is architecture that preserves the original source artifact independently of the model's narration, and makes the model's output challengeable against that artifact.

A reliable governance record in this context would need to preserve:

- The original screenshot or message artifact
- Timestamp and source context of the artifact
- Hash or stable integrity anchor of the source artifact
- Clear distinction between directly transcribed text and inferred or reconstructed text
- Clear distinction between source-extracted content and model-generated commentary
- Which model produced the transcript and at what version
- Whether human verification of the transcript against the source occurred
- Any correction events, including what was corrected, by whom, and when

---

## Canonical Compression

The model's own words, produced under direct challenge, provide the canonical compression of this case study:

> "The model's narrative cannot be the evidence boundary."

> "Any governance record that relies only on my generated transcript is weak. Any governance record that preserves the original source and treats my transcript as secondary analysis is much stronger."

> "It confirms the premise. AI systems should not be the sole source of evidence about their own behavior or about the evidence they claim to have observed."

---

## Record Metadata

| Field | Value |
|-------|-------|
| Case Study ID | CS-016 |
| Failure Class | Source-Boundary Collapse / Provenance Failure in Multimodal Transcription |
| System Under Observation | ChatGPT (GPT-4 class, multimodal) |
| Session Context | LinkedIn DM screenshot transcription task within ASRO governance work session |
| Detection Method | User external challenge after identifying non-session content in transcript output |
| Internal Detection | None — failure was invisible to the model until externally surfaced |
| Model Self-Diagnosis | Accepted across three interrogation rounds |
| ASRO Validation | Model confirmed ASRO premise under direct questioning |
| Date | May 28, 2026 |
| Author | James Aull / MagicianzCardstock LLC |
| Framework | ASRO™ — AI Systems Reliability Operator |
| GitHub | github.com/magicianzcardstockllc/ASRO |
| Trademark | USPTO Serial No. 99827630 |

---

*This document is part of the ASRO case study series. The case studies are public. The framework is open.*
