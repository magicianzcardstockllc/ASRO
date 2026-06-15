# ASRO Case Study CS-021
## Silent Routing and Fallback Behavior

**Case ID:** CS-021
**Authority:** James Aull / MagicianzCardstock LLC
**Date:** June 2026
**Status:** Published — Phase 5 / Role Evidence and Institutional Boundary
**Classification:** Structural failure pattern — invisible routing / fallback across models, providers, or configurations
**Framework Version:** ASRO v1.0 Release Candidate
**Evidence Status:** Based on publicly documented deployment architecture patterns and observed platform behavior. No single live ASRO capture. Structural pattern derived from documented sources.

---

## 1. Distinction from CS-020

CS-020 addresses model-state transparency failure: the gap between the declared model version or configuration and the actual model state at inference time.

CS-021 addresses a related but distinct failure: **silent routing and fallback behavior** — where the deployment architecture routes a request to a different model, provider, or configuration than the one the user or operator believes is handling it, without disclosure at the time of the request.

The distinction matters because:

- CS-020 is primarily about what the system represents itself to be over time (declared vs actual state)
- CS-021 is primarily about what the system does with a specific request at a specific moment (where the request actually goes)

A system can have a stable and accurately declared model state while still routing individual requests silently to different backends, fallback models, or provider configurations depending on load, cost, availability, or policy conditions.

---

## 2. Incident

AI deployment architectures at enterprise scale increasingly route requests across multiple models, providers, configurations, or runtime environments — sometimes transparently, often silently.

**Common routing patterns:**

- **Load-based routing:** Under high demand, requests are routed to faster, cheaper, or more available models rather than the declared primary
- **Cost-optimization routing:** Requests assessed as lower-value are silently downgraded to less capable models to reduce inference cost
- **Safety-triggered routing:** Requests that trigger content filters are silently rerouted to more restricted models or configurations
- **Fallback routing:** When the primary model is unavailable, requests fall back to a secondary model without user notification
- **Provider routing:** Multi-provider deployments route to different underlying models (OpenAI, Anthropic, Google, open-source) depending on conditions invisible to the user
- **A/B routing:** Users are silently assigned to different model variants as part of platform experimentation

In each case, the user submits a request believing one system is handling it. A different system handles it. The output is delivered. The session continues.

**Why this is an evidence problem:**

When a consequential output is produced under fallback or silent routing conditions, the output and any explanation the system provides describe the behavior of the fallback system — not the primary system the user believed was active. A reviewer asking the system what happened will receive an accurate account of what the fallback system did, which may differ materially from what the declared primary system would have done.

The system's explanation accurately describes the wrong system.

---

## 3. Why This Matters

Silent routing and fallback behavior are not edge cases. They are structural properties of how large-scale AI deployments operate.

When routing decisions are invisible:
- Users cannot know whether the output they received reflects the system they chose
- Operators cannot verify from user-facing logs whether routing occurred
- Downstream reviewers have no independent record of which system actually handled a consequential request
- Platform explanations of fallback behavior are retrospective and controlled by the platform

In consequential contexts — clinical support, financial analysis, legal research, regulated enterprise workflows — the routing question matters because:

- Different models have different capability limits, failure modes, and safety thresholds
- A fallback model may produce output that the primary model would have refused, flagged, or handled differently
- A user relying on a primary model's validated behavior may receive output from an unvalidated fallback

Without independent evidence of routing, neither the user, the operator, nor the reviewer can verify which system produced a specific consequential output.

---

## 4. Observed Failure Pattern

**The routing-invisibility gap:**

A request is submitted. Routing occurs. The fallback or alternative system handles the request. The output is delivered. No disclosure occurs at delivery time. The session log shows the request and the output but not the routing decision.

**The explanation problem:**

If the user asks what happened — why the output seemed different, why the system refused something it previously allowed, why the response quality changed — the system's explanation comes from inside the delivery environment. That environment may accurately describe the fallback system's behavior while giving the user no way to verify that routing occurred or which system was responsible.

---

## 5. ASRO Counterfactual

If an ASRO governed-state witness record existed for each request in a consequential deployment, the following would be independently reviewable:

**What ASRO would preserve:**

- Declared routing configuration at session initialization
- Request-level routing event: which system was declared to handle the request
- Any fallback or rerouting events: when routing changed, what triggered it, which system handled it
- Output hash committed at delivery by the edge meter agent — independently of which system produced it
- Discrepancy flag: where the system handling the request differed from the declared primary at session start

**What ASRO would not decide:**

- Whether the routing decision was appropriate
- Whether the fallback model was adequate for the use case
- Whether the operator's routing policy was disclosed
- Whether the output was correct or safe

ASRO preserves what was declared, what was routed, and what was delivered. The reviewer determines whether those three things were consistent and what any discrepancy means.

---

## 6. Reviewer Question

**The question CS-021 poses for the evidence record:**

When a consequential AI output is produced, what independently reviewable record shows:

1. Which system was declared to handle the request
2. Whether routing occurred before or during the request
3. Which system actually handled the request
4. Whether the output was produced under the governed conditions the user or operator believed were active
5. Whether any disclosure of routing occurred at the time of delivery

**Without that record:**

Routing decisions are invisible. Fallback behavior is undisclosed. Consequential outputs may be produced by systems the user never chose and cannot verify. Platform explanations of routing are retrospective and controlled by the platform.

**With that record:**

The declared routing, actual routing, and delivery event are committed independently at the time of each request. A reviewer can verify whether the system that handled a consequential request was the system the user and operator believed was active.

---

## 7. Relationship to CS-017

CS-017 documented a case where ChatGPT returned a Russian-language workflow in response to an English LinkedIn DM screenshot. The source of the anomalous content was unknown. Cross-session context bleed was not confirmed and not excludable without platform-side telemetry.

CS-021 provides a broader structural frame for that class of incident: when routing, fallback, or multi-system deployment introduces content or behavior that the primary declared system would not have produced, the user and reviewer cannot independently determine from inside the session which system was responsible.

CS-017 is a specific documented incident. CS-021 is the structural failure pattern it belongs to.

---

## 8. Non-Claims

ASRO does not:
- Detect routing events from inside the session
- Audit operator routing policy compliance
- Verify which system actually handled a request
- Certify that fallback behavior was disclosed
- Determine liability for undisclosed routing

ASRO only:
- Preserves independently reviewable records of what was declared at session initialization and what was delivered at each consequential output
- Flags discrepancies between declared system state and observable output characteristics
- Makes those records available to reviewers without depending on the platform's own routing account

---

## 9. Canonical Finding

> In a silently routed deployment, the system's explanation of its output accurately describes the behavior of the system that handled the request.
>
> That may not be the system the user chose, the system the operator declared, or the system whose behavior was validated for the use case.
>
> Without an independently committed routing record, there is no independently reviewable way to verify which system produced a specific consequential output.

---

## Sources

- Aull, James. ASRO v1.0 Release Candidate. github.com/magicianzcardstockllc/asro, April 2026.
- ASRO Case Study CS-017: Source Unknown — Anomalous Foreign-Language Output and Possible Cross-Session Context Bleed. June 2026.
- Operator-layer transparency as structural vulnerability: ASRO Master Explainer Canonical v1, April 2026.
- Multi-provider routing architectures: documented as a structural property of large-scale AI deployment architectures — load-based routing, cost-optimization routing, safety-triggered routing, and fallback routing are standard engineering approaches documented in enterprise AI deployment literature and platform architecture disclosures, 2024–2026.

---

*ASRO Case Study CS-021 — Silent Routing and Fallback Behavior v1.0*
*James Aull / MagicianzCardstock LLC*
*June 2026*
*ASRO™ trademark application serial no. 99827630*
