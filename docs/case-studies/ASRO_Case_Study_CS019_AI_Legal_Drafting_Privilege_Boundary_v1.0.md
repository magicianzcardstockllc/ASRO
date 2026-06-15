# ASRO Case Study CS-019
## AI Legal Drafting and Privilege Boundary Uncertainty

**Case ID:** CS-019
**Authority:** James Aull / MagicianzCardstock LLC
**Date:** June 2026
**Status:** Published — Phase 5 / Role Evidence and Institutional Boundary
**Classification:** Structural failure pattern — role evidence gap / privilege boundary uncertainty
**Framework Version:** ASRO v1.0 Release Candidate
**Evidence Status:** Based on publicly documented court rulings, federal civil case data, and published legal scholarship. No single live ASRO capture. Structural pattern derived from documented sources.

---

## 1. Incident

When an AI system helps a person draft a legal filing, rehearse an argument, value a claim, or shape litigation strategy, a new evidentiary question enters the court:

**What was the AI's role in this legal act?**

Was the chatbot:
- A writing aid that formatted an argument the person already had?
- A legal-advice substitute that told the person what to argue?
- A privileged work-product tool whose outputs are shielded from the opposing party?
- A third-party disclosure surface whose outputs may be discoverable?
- A source of fabricated authority the person relied on in good faith?
- A system that materially changed the person's litigation posture?

Courts are now being asked to answer these questions — and they are reaching opposite conclusions.

**Documented record:**

In February 2026, a federal court in Michigan ruled that a self-represented person's conversations with ChatGPT to prepare her case were work product — shielded from the opposing side.

On the same day, a federal court in New York held that documents a criminal defendant generated using Claude were not privileged attorney-client conversations or work product. The court reasoned that Claude is not an attorney and that a user has no reasonable expectation of confidentiality because AI companies can disclose user data to third parties.

In March 2026, Judge Maritza Braswell, a federal magistrate judge in Colorado, ruled that a self-represented person's use of a chatbot should stay off limits, writing: "It is true that AI systems like ChatGPT, Claude, Gemini, and others collect user data for training and other purposes. But that does not eliminate all expectations of privacy."

Courts have remained split since.

**Scale of the problem:**

A study by Anand Shah (MIT) and Joshua Levy (University of Southern California), examining 4.5 million federal civil cases from 2005 to 2026, found:
- Self-represented lawsuits rose from approximately 11% in 2022 to 16.8% in 2025
- The share of sampled filings flagged as containing AI-generated writing rose from 1% in 2023 to 18% in 2026
- The number of filings made by self-represented litigants more than doubled from pre-2023 levels

Source: Shah and Levy, SSRN; reported in MIT Technology Review, June 4, 2026.

---

## 2. Why This Matters

The privilege and work-product questions are not only access-to-justice questions. They are evidentiary boundary questions.

When a court must determine what role an AI played in a legal act, it needs independently reviewable evidence of:

- What AI system was active
- What version, configuration, and model state was active at the time
- What the user asked
- What the AI produced
- Whether the AI provided legal analysis, legal conclusions, strategic framing, or only formatting
- Whether the output contained fabricated authority (hallucinated cases, fabricated quotes)
- What the user relied on from the AI output and what they discarded
- What the AI disclosed about its own limitations and role
- Whether the session was conducted in a context the platform may disclose to third parties

Without a governed-state record, courts are left reconstructing the AI's role after the fact — from the final document, user testimony, platform policies, and disputed chat logs. Each of those sources is incomplete, retrospective, or controlled by the party with the most to gain from a particular characterization.

---

## 3. Observed Failure Pattern

**The role-evidence gap:**

The AI system produces an output. The output enters a legal proceeding. The court must determine the AI's role in that proceeding. The only evidence available is:

1. The final filing or document — which has been edited, formatted, and submitted by a human
2. The user's own account of what the AI did — which is interested testimony
3. The platform's terms of service and privacy policy — which govern platform behavior, not per-session role
4. The AI's own explanation if asked — which is self-attestation from the same system whose role is in dispute

None of these is independent evidence of the governed state that existed when the AI-assisted legal act occurred.

**The fabricated-authority failure:**

Separate from the privilege question, courts have documented AI systems producing hallucinated case citations, fabricated quotes, and incorrect legal valuations that were incorporated into filings and litigation strategy by users who treated the output as authoritative.

Judge Allison Goddard (California): "Where are you getting the idea that you're getting $700,000? Did you go to ChatGPT?"

When the AI produces fabricated authority that changes a user's litigation posture — their settlement demand, their case theory, their procedural choices — there is no evidence record showing what the AI said, what the user relied on, or what the user discarded. The reliance question is entirely reconstructed after the fact.

---

## 4. What the System/User/Institution Could Prove

**Without an independent governed-state record, available evidence is limited to:**

- The final filed document
- The user's testimony about their AI use
- Platform terms of service and disclosed data practices
- Any preserved chat logs — which may or may not be complete, authenticated, or accurate
- AI detection tool outputs — which probabilistically assess AI involvement, not role or reliance

**What courts have demonstrated they cannot reliably determine:**

- Whether specific AI output was treated as legal advice, strategic guidance, or mere formatting
- What model version or configuration was active
- Whether the output contained fabricated authority that was later corrected, retained, or relied upon
- Whether the session was conducted under conditions the platform would treat as disclosable
- What the user actually relied on versus what they ignored

---

## 5. ASRO Counterfactual

If an ASRO governed-state witness record existed for an AI-assisted legal act, the following would be independently reviewable without relying on the user's testimony, the platform's policies, or the final filed document:

**What ASRO would preserve:**

- Model identifier, version, and declared configuration at session initialization
- Session context and disclosed role (writing aid, general assistant, legal resource)
- User query structure — what was asked, at what stage of the legal act
- AI output hash committed at delivery — allowing later verification that the output record matches what was actually delivered
- Fabricated-authority flags — where the AI produced case citations, quotes, or legal conclusions that triggered known failure patterns
- Disclosure posture — whether the AI disclosed limitations, suggested legal counsel, or presented output as definitive
- Reliance indicators — where the user explicitly accepted, incorporated, or acted on specific AI outputs
- Session boundary — whether the session was conducted under conditions consistent with the platform's third-party disclosure policy

**What ASRO would not decide:**

- Whether the AI-assisted output is privileged
- Whether the user's reliance was reasonable
- Whether the output constitutes the practice of law
- Whether the platform must disclose the session
- Whether the filing is meritorious

ASRO preserves the independently reviewable record. The court determines the legal conclusions.

---

## 6. Reviewer Question

**The question CS-019 poses for the evidence record:**

When an AI system participates in a legal act — drafting, advising, valuing, rehearsing, framing — what independently reviewable record shows:

1. What AI system was active and in what configuration
2. What role the AI took in the act
3. What the user relied on
4. What the AI disclosed about its own limitations
5. Whether the session was conducted under conditions that bear on privilege or discoverability
6. Whether fabricated authority was produced and whether it entered the user's legal posture

**Without that record:**

Courts reconstruct AI role from the document, the user's testimony, and the platform's policies. That reconstruction is incomplete, retrospective, and controlled by interested parties.

**With that record:**

Courts can evaluate the AI's role from a governed-state witness record that was committed at the time of the act, is independently verifiable, and does not depend on the AI system's own later explanation.

---

## 7. Non-Claims

ASRO does not:
- Determine whether AI-assisted legal work is privileged
- Determine whether an AI system practiced law without a license
- Certify that AI output is legally accurate
- Replace legal counsel or judicial review
- Decide whether a session is discoverable or protected
- Determine liability for AI-generated bad advice

ASRO only:
- Preserves independently reviewable governed-state evidence of what AI system was active, what it produced, what it disclosed, and what role it appeared to occupy at the time of the legal act
- Makes that record available to courts, regulators, and reviewers without depending on the system's own account

---

## 8. Classification Amendment

This case study documents a structural failure pattern derived from publicly reported court rulings and empirical case-data research. It is not based on a single live ASRO capture.

The failure class — role-evidence gap, privilege boundary uncertainty, fabricated-authority reliance — is documented from external sources cited above. ASRO's counterfactual reconstruction shows what an independent governed-state witness record would have preserved. It does not claim ASRO captured these records live.

---

## 9. Canonical Finding

> When an AI system participates in a legal act, the evidence boundary question is not only what the system produced.
> It is what role the system was in, what the user relied on, what the system disclosed, and what independently reviewable record proves that posture existed at the moment of consequence.

---

## Sources

- Shah, Anand and Levy, Joshua. Empirical study of AI in federal civil litigation, 2005–2026. SSRN (forthcoming).
- Kim, Michelle. "How courts are coping with a flood of AI-generated lawsuits." MIT Technology Review, June 4, 2026.
- Federal court ruling, Michigan, February 2026 (ChatGPT / work product).
- Federal court ruling, New York, February 2026 (Claude / attorney-client privilege).
- Braswell, Judge Maritza. Federal magistrate ruling, Colorado, March 2026 (chatbot use / privacy).
- Nippon Life Insurance Company v. OpenAI. Filed March 2026, pending.

---

*ASRO Case Study CS-019 — AI Legal Drafting and Privilege Boundary Uncertainty v1.0*
*James Aull / MagicianzCardstock LLC*
*June 2026*
*ASRO™ trademark application serial no. 99827630*
