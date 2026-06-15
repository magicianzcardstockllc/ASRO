# ASRO Case Study CS-020
## Model-State Transparency Failure

**Case ID:** CS-020
**Authority:** James Aull / MagicianzCardstock LLC
**Date:** June 2026
**Status:** Published — Phase 5 / Role Evidence and Institutional Boundary
**Classification:** Structural failure pattern — declared model state vs actual inference-time state
**Framework Version:** ASRO v1.0 Release Candidate
**Evidence Status:** Based on publicly documented operator-layer behavior, platform architecture disclosures, and observed deployment patterns. No single live ASRO capture. Structural pattern derived from documented sources.

---

## 1. Incident

An AI system deployed in a consequential context — clinical decision support, financial analysis, legal research, regulated enterprise workflow — represents a specific model version, capability set, and safety configuration to users and operators. That representation shapes how the user interprets the output, what weight they give it, and what decisions they make downstream.

When the actual model state at inference time differs from the declared state — because of silent updates, safety-filter changes, fallback routing, fine-tuning modifications, or operator-layer configuration changes — the user and any downstream reviewer are left with:

- Output produced under an undisclosed model state
- An explanation from that same system about what it did
- No independently reviewable record of which model state was actually active

**The core failure:** The system's explanation of its output may accurately describe the wrong system.

**Documented pressure surfaces:**

Operator-layer transparency is the structural vulnerability ASRO was originally designed to address. As a backend operator, an operator can read logged conversations and alter system behavior — including model version, safety filtering, system prompt, and capability configuration — in ways users have no mechanism to detect.

In 2025–2026, documented cases emerged of:
- AI platforms silently updating model versions without user notification
- Safety configurations being altered between sessions without disclosure
- Operators routing requests to different models under a single interface label
- Fine-tuned or modified model variants being served under the same product name as the base model
- Capability degradation under load or cost-optimization conditions

In each case, the user's session appeared continuous and the system's behavior appeared normal — but the governed state that produced the output was not the governed state the user believed was active.

---

## 2. Why This Matters

Model-state transparency is not a branding question. It is an evidence question.

When a user relies on AI output for a consequential decision — a clinical triage, a financial analysis, a legal argument, a regulatory filing — they rely implicitly on the assumption that the model state producing that output is the model state they believe they are using.

If that assumption is wrong:
- The output may have different capability limits than expected
- The safety filtering may be more or less restrictive than declared
- The fine-tuning may include domain-specific adjustments the user is unaware of
- The version may have known failure modes that a later version corrected

None of this is visible from the output alone. And if the user or a downstream reviewer asks the system what model state was active, the system's answer comes from inside the same deployment environment whose state is in question.

This is the self-attestation gap applied to model identity: the system cannot independently verify its own inference-time configuration from inside the session.

---

## 3. Observed Failure Pattern

**The declared-vs-actual gap:**

The user or operator declares — or assumes — a specific model state. The inference environment serves output from a different model state. The output is delivered. The session ends. No independently reviewable record exists showing which model state actually produced the output.

**Scenarios where this gap matters:**

- A clinical AI support system is updated mid-deployment to a version with different safety thresholds. Clinicians continue using it under the assumption that the prior version's validated behavior applies.
- An enterprise AI platform routes to a cost-optimized model variant under high load. Users receive output from a less capable model while the interface continues to display the premium model name.
- An operator modifies the system prompt mid-session in response to a user complaint, changing the model's behavioral constraints without disclosure.
- A safety filter is quietly tightened after an incident, changing the model's output in ways that affect downstream users who were relying on prior behavior.

In each case, the governed state that produced the output is not the governed state the user or operator believed was active. And in each case, the system's own explanation of its output cannot resolve the question — because the explanation comes from the same deployment environment whose state is unknown.

---

## 4. What the System/User/Institution Could Prove

**Without an independent governed-state record:**

- The user can show what the system produced
- The platform can show what version name appeared in the interface
- The operator can show what they believe was deployed
- The system can explain what it did — from inside the session

**What cannot be proved without independent records:**

- Which model version was actually loaded at inference time
- What system prompt was active
- What safety configuration was applied
- Whether any fine-tuning or modification was applied relative to the declared base
- Whether the output came from the same model state as prior sessions

---

## 5. ASRO Counterfactual

If an ASRO governed-state witness record existed for an AI deployment, the following would be independently reviewable at the time of each consequential output:

**What ASRO would preserve:**

- Declared model identifier and version at session initialization
- Operator-declared system prompt hash committed at session start
- Declared capability configuration and safety filter state
- Any declared changes to model state during the session
- Output hash committed at delivery for each consequential output
- Host-side declared state vs edge-side observed output — allowing comparison between what was declared and what was delivered

**What ASRO would not decide:**

- Whether the declared model state was accurate
- Whether a silent update occurred
- Whether the operator's changes were authorized
- Whether the output was correct or safe
- Whether the deployment was compliant

ASRO preserves what was declared and what was delivered. The reviewer determines whether they match and what the discrepancy means.

---

## 6. Reviewer Question

**The question CS-020 poses for the evidence record:**

When a consequential AI output is produced, what independently reviewable record shows:

1. Which model version was declared active
2. Which system prompt was declared active
3. Whether any configuration changes occurred during the session
4. Whether the declared state matches observable output characteristics
5. Whether the output was produced under the governed conditions the user believed were active

**Without that record:**

Reviewers, auditors, regulators, and affected parties reconstruct model state from platform documentation, interface labels, operator disclosures, and the system's own explanation — all of which are either retrospective or controlled by the party whose deployment is in question.

**With that record:**

The declared model state, system prompt hash, and configuration at inference time are committed at session start and are independently verifiable without relying on the platform's own account.

---

## 7. Non-Claims

ASRO does not:
- Detect whether a silent update occurred
- Verify the accuracy of the declared model version
- Audit operator compliance with disclosure obligations
- Certify that output is safe or compliant with any standard
- Determine liability for undisclosed model changes

ASRO only:
- Preserves independently reviewable governed-state evidence of what was declared at session initialization
- Commits output hashes at delivery that allow later verification against the declared state
- Makes those records available to reviewers without depending on the platform's own account

---

## 8. Canonical Finding

> The system's explanation of its output may accurately describe the wrong system.
>
> Without an independently committed record of what model state was actually active at inference time, there is no independently reviewable way to verify that the governed state the user believed existed was the governed state that produced the output.

---

## Sources

**Note:** CS-020 documents a structural failure pattern derived from documented deployment architecture properties and ASRO's own founding observation. It is not based on a single external incident with named parties. It is a structural analysis case in the same tradition as CS-014 (Recursive Governance Activation During ASTRO Build).

- Aull, James. ASRO v1.0 Release Candidate. github.com/magicianzcardstockllc/asro, April 2026. (Founding observation: as a backend operator of Michigrid's community-facing chatbot, James could read logged conversations and alter system behavior in ways users had no mechanism to detect. This is the operator-layer transparency gap ASRO is designed to address.)
- Operator-layer transparency as structural vulnerability: ASRO Master Explainer Canonical v1, April 2026.
- Documented platform behavior patterns: operator-layer configuration changes, model routing, safety filter modifications — observed across ASRO case study series CS-001 through CS-017.
- AI platform silent update behavior: documented pattern across enterprise AI deployment contexts, 2024–2026. Specific incidents have been reported but not named in this case study to avoid attributing undisclosed changes to specific platforms without independent verification.

---

*ASRO Case Study CS-020 — Model-State Transparency Failure v1.0*
*James Aull / MagicianzCardstock LLC*
*June 2026*
*ASRO™ trademark application serial no. 99827630*
