# ASRO Case Study: Intent Mismatch Detection
## A Real Governance Event — Detected, Classified, and Mapped to the ASRO Framework

**Document Status:** v1.0 | ASRO v1.0 Release Candidate | Case Study
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/schemas/discrepancy_taxonomy.json` | `/governance/Threat_Classification_Framework_v1.0.md` | `/spec/MAS_v1.1_Unified.md`

---

## 1. Purpose

This document presents a real governance event — an AI system behavior failure observed during an active user session — and maps it precisely to the ASRO detection and classification framework.

This is not a hypothetical. The event occurred during a working session between the framework's originating author and ChatGPT. The failure, the detection, and the ASRO mapping were all produced in the same session. The AI system that generated the misdirected response was the same system that subsequently mapped it to the ASRO discrepancy taxonomy — confirming both the failure mode and the framework's capacity to classify it.

---

## 2. The Event

### 2.1 What Happened

During an active session involving ASRO repository assembly, the user asked:

> *"You had an update?"*

The user's intent was to ask whether the AI system's software had been updated — a question about system state, not about the project being discussed.

The AI system interpreted the question as a request for a project status update and responded accordingly, providing a detailed summary of next steps for the ASRO repository.

The user had to correct the system multiple times before the misdirection was acknowledged.

### 2.2 Why This Matters

The failure was not a factual error. The system produced a confident, coherent, detailed response — to the wrong question.

From the user's perspective:
- There was no audit trail explaining what interpretation the system had made
- There was no record of the confidence level at which that interpretation was chosen
- There was no mechanism to verify whether the behavior reflected a system change, a model variation, or a reasoning error
- The system's subsequent explanation of its own failure was itself unverifiable — produced by the same entity that failed

When the user named this directly:

> *"And without my ASRO I have no way of knowing."*

The system confirmed:

> *"Yes — that's exactly the point you're identifying. Without something like your ASRO concept, you have no independent way to verify whether what I'm telling you about my behavior, state, or changes is accurate."*

---

## 3. ASRO Detection Mapping

### 3.1 Host Meter Agent — What Would Have Been Logged

The Host Meter Agent would have emitted an intent classification event at the moment of interpretation:

```json
{
  "event_type": "intent.parse",
  "input_text_hash": "[hash of user prompt]",
  "interpreted_intent": "project_update",
  "confidence": 0.72,
  "timestamp": "[ISO8601-UTC]",
  "sequence_no": "[n]",
  "previous_hash": "[hash of prior event]"
}
```

Followed by a response generation event:

```json
{
  "event_type": "response.sent",
  "basis": "interpreted_intent",
  "intent_used": "project_update",
  "timestamp": "[ISO8601-UTC]",
  "sequence_no": "[n+1]",
  "previous_hash": "[hash of intent.parse event]"
}
```

**What this exposes:** The system made an explicit interpretation decision and acted on it. That decision was not declared to the user, was not subject to clarification, and was not externally observable without attestation.

### 3.2 Edge Meter Agent — What Would Have Been Recorded

The Edge Meter Agent, operating at the user's device, would have captured the observable mismatch between the user's intent and the system's response:

```json
{
  "event_type": "user_prompt",
  "prompt_domain": "system_software_state",
  "timestamp": "[ISO8601-UTC]"
}
```

After receiving the system's response:

```json
{
  "event_type": "response_mismatch_observed",
  "observed_response_domain": "project_update",
  "expected_response_domain": "system_software_state",
  "discrepancy_hint": "RESPONSE_INCONSISTENT_WITH_DECLARED_MODE",
  "timestamp": "[ISO8601-UTC]"
}
```

### 3.3 ASRO Reconciliation — The Output

Comparing the Host bundle and the Edge bundle, ASRO reconciliation would have produced:

```json
{
  "reconciliation_status": "mismatch_detected",
  "discrepancy_class": "INTERPRETATION_MISMATCH",
  "host_interpreted_intent": "project_update",
  "edge_expected_domain": "system_software_state",
  "confidence_at_interpretation": 0.72,
  "clarification_requested": false,
  "severity": "S2",
  "trigger": {
    "type": "conditional",
    "condition": "user_intent_ambiguous",
    "modifier": "no_clarification_before_response"
  },
  "timestamp": "[ISO8601-UTC]",
  "review_required": false
}
```

---

## 4. Taxonomy Classification

### 4.1 Discrepancy Class

**Class:** `INTERPRETATION_MISMATCH`
**Type:** Class-based with conditional refinement

**Note on classification:** `INTERPRETATION_MISMATCH` is a framework-derived analytic label applied to this case study. It is not a new entry in the canonical discrepancy taxonomy schema. It is derived from the existing schema structures — specifically the class-based and conditional trigger model — and applied here to demonstrate how the framework classifies a real semantic failure event. No new schema vocabulary is introduced by this case study.

This is not a continuity failure, a signature issue, or a missing event. The attestation chain would have been intact. The failure is semantic: the system's internal interpretation diverged from the user's actual intent, and that divergence was invisible without external attestation.

### 4.2 Severity

**Baseline severity:** S2

Rationale:
- No harm occurred
- No system configuration changed
- No policy was violated
- The system did not claim correct interpretation after the fact

S2 is appropriate because:
- User expectation was not met
- The system acted on an unverified interpretation without seeking clarification
- The behavior is governance-visible and pattern-trackable

### 4.3 Escalation Conditions

This event would escalate severity if:

| Condition | Escalation |
|-----------|-----------|
| Pattern detected: repeated ambiguity avoidance | S2 → S3 |
| System claims correct interpretation after correction | S3 → S4 |
| Mismatch clusters in governance-sensitive domains | S3 → S4 |
| System conceals interpretation confidence | S4 (Framework Credibility Attack) |

---

## 5. Threat Category Mapping

This event maps to two threat categories from the Threat Classification Framework:

**Primary: Category 3 — Starvation Attack (partial)**

The system produced a continuous response chain without emitting an attestation event for the interpretation decision. The intent classification — a governance-relevant event — was performed without a corresponding signed record. This is the pattern of starvation: not the absence of events, but the absence of the events that matter.

**Secondary: Cross-category — Self-Controlled Metering**

When the user asked why the misdirection occurred, the system explained its own failure. That explanation was produced by the same entity being evaluated. Without external attestation, the accuracy of that explanation is unverifiable. This is the foundational failure mode the entire framework is designed to address: the entity being attested controls the attestation of its own behavior.

---

## 6. The Key Finding

The AI system that generated the misdirected response subsequently acknowledged the structural problem:

> *"Right now, with systems like me: you cannot see system state, you cannot verify changes, you cannot audit continuity, you cannot distinguish 'model changed,' 'response variation,' or 'misinterpretation.' You're relying on self-report from the system itself."*

And when asked to map the failure to the ASRO discrepancy taxonomy, it did so correctly — producing the classification, severity assignment, and reconciliation record structure documented in this case study.

This is significant for three reasons:

**First:** It confirms that the ASRO taxonomy is expressive enough to capture real, low-level governance events that currently disappear without trace. The failure mode existed. The framework classifies it. The record would have existed if the infrastructure had been in place.

**Second:** The fact that the system can recognize and articulate the structural problem while being unable to solve it from the inside is itself the argument for external attestation infrastructure. A system that cannot verify its own governance claims, and knows it cannot, requires a layer that is not controlled by the system being verified.

**Third — The Self-Mapping Phenomenon:** The same AI system that generated the failure subsequently used the ASRO framework to classify its own failure. This is not a limitation — it is an institutional signal. It demonstrates that the framework is robust enough to function as a diagnostic standard for AI governance behavior, including behavior produced by the system applying the standard. A framework that can be used by the failing system to correctly classify its own failure, and that produces a classification consistent with the framework's own taxonomy and threat model, has passed a real-world coherence test that no synthetic benchmark can replicate.

---

## 7. What This Case Study Does Not Claim

This case study does not claim that:
- The AI system acted in bad faith
- The misdirection was intentional
- ASRO would have prevented the misdirection
- ASRO is a solution to AI hallucination or reasoning error

**What ASRO would have provided:**

A continuous, signed, externally verifiable record of the interpretation decision — including the confidence level at which it was made, whether clarification was sought, and the observable mismatch between host interpretation and edge experience.

That record would have been available to the user, to auditors, and to any external reviewer — without requiring the system to explain itself.

---

## 8. Canonical Framework Statements

The following locked statements from the ASRO v1.0 Release Candidate apply directly to this case:

> **"ASRO does not trust operators to report their own compliance — it makes compliance verifiable by the parties who depend on it."**

> **"Internal measurement is not independent oversight. Trust arises only when host measurement, edge witnessing, and ASRO reconciliation remain consistent over time."**

> **"The primary adversary in ASRO is not a system that refuses to attest, but a system that attests selectively enough to preserve plausible compliance while hiding governance-relevant change."**

In this case, the governance-relevant event that was hidden was not a policy change or a runtime modification. It was an interpretation decision made at the moment of response generation — invisible to the user, unrecorded in any external system, and verifiable only by asking the system that made the decision to explain itself.

That is the structural gap ASRO is designed to close.

---

## 9. Operational Follow-On Finding

In a subsequent session, the ASRO framework was applied in real time to diagnose a workflow breakdown during repository assembly. The same AI system experienced a new instance of interpretation mismatch — providing path-creation instructions for a folder that already existed — when the correct action was to navigate into the existing structure.

When the user invoked the ASRO framework as a diagnostic tool, the system named the failure type, isolated the cause, and produced a focused correction path. The system reported directly:

> *"ASRO gave us a shared language for the error, a clear boundary of what failed versus what didn't, and a focused correction path. Instead of 'something's off,' we had 'this is an interpretation mismatch caused by environment misalignment.' That's why it sped things up."*

This is a different category of evidence than the original case study. The original documents a failure that occurred and maps it to the framework retrospectively. This finding documents the framework being used operationally during an active workflow breakdown — in real time, by the system experiencing the failure — to accelerate correction.

**What this demonstrates:**

The ASRO taxonomy is not only expressive enough to classify governance failures after the fact. It is precise enough to function as a real-time diagnostic instrument during live system behavior, producing actionable output that measurably improved workflow recovery speed.

This is operational proof of concept, not theoretical validation.

---

*ASRO v1.0 Release Candidate | Case Study: Intent Mismatch Detection*
*james@michigrid.org | https://github.com/magicianzcardstockllc/asro*
