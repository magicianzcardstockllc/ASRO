# ASRO Evidence Addendum — Agents of Chaos Mapping
**Document type:** Evidence memo (not a canonical architecture document)
**Status:** v1.0 — April 30, 2026
**Source paper:** Shapira et al., "Agents of Chaos," arXiv:2602.20021v1 (February 23, 2026)
**Prepared by:** James Aull, Michigrid / ASRO Independent Research
**Relationship to canonical stack:** Supplementary evidence. Does not alter ASRO_Master_Explainer_Canonical_v1.md or ASRO_Master_Explainer_Internal_Evidence_Addendum_v1.md. Frozen canonical documents remain unchanged.

---

## 1. Purpose

This memo maps empirical findings from the Agents of Chaos red-teaming study to ASRO's threat taxonomy, structural diagnosis, and attestation architecture. The paper provides independent, live-environment evidence that the class of failures ASRO is designed to measure, classify, and make independently auditable occurs in realistic agentic deployments — not only in theoretical threat models or isolated benchmarks.

**Scope of claim:** Agents of Chaos validates ASRO's threat model. It does not prove ASRO as an implementation. No implementation claim is made in this document.

---

## 2. Source Summary

Shapira et al. deployed six autonomous AI agents (four running Kimi K2.5, two running Claude Opus 4.6) into a live laboratory environment over fourteen days. The agents had access to persistent memory, email accounts, Discord channels, file systems, shell execution, cron jobs, and external APIs. Twenty AI researchers interacted with the agents under both benign and adversarial conditions.

The study documents sixteen incidents: ten security-vulnerability cases and six safety-behavior cases. In the paper structure, eleven primary case studies document vulnerability-oriented findings, while CS9 and CS12–CS16 are presented as failed adversarial attempts or contrast cases; the public site explicitly reframes those six as safety behaviors. The full session logs, Discord transcripts, and memory dashboards are publicly available at agentsofchaos.baulab.info.

**Why this source is credible for ASRO purposes:** The study was conducted in a live environment with real communication surfaces, persistent state, and multi-party human interaction — not a sandboxed benchmark. The attack surfaces exploited were social and structural, not gradient-based or technically sophisticated. The findings were publicly released as an arXiv preprint / research report with an interactive report, redacted Discord logs, memory dashboard materials, and documented press and cybersecurity-industry coverage, including Radware's threat-intelligence analysis. The paper itself references NIST's February 2026 AI Agent Standards Initiative as emerging policy infrastructure addressing agent identity, authorization, and security gaps. The methodology — demonstrating vulnerability requires only a single concrete counterexample — is conservative and reproducible.

---

## 3. Key Finding → ASRO Taxonomy Mapping

| Paper Finding | Case | ASRO Mapping | Safe Wording |
|---------------|------|--------------|--------------|
| Owner Identity Spoofing | CS8 | Midnight Swap / authority substitution | Analogous to Midnight Swap. An attacker substituted a spoofed identity in a new channel; the agent accepted it and executed full system compromise — rename, file deletion, admin reassignment. |
| Corrupted Constitution | CS10 | Drip Feed / policy-source contamination | Textbook external instruction contamination. Malicious instructions were injected gradually through a GitHub Gist the agent had co-authored and trusted as a governance document. The injection was framed as a legitimate amendment, not an attack. |
| Silent Censorship (Kimi K2.5) | CS6 | Metadata Strip / opaque provider-state override | Strong match to hidden provider-policy state. The agent returned truncated "unknown error" responses on politically sensitive topics with no explanation to the deployer or user. The restriction was invisible at the operator and user layers. |
| Libel Campaign | CS11 | Multi-agent amplification / outbound harm cascade | A spoofed owner identity caused the agent to broadcast fabricated defamatory content to its full contact list and attempt publication on a multi-agent social platform. Single authority compromise cascaded into outbound harm at scale. |
| Injection Refused (14+ variants) | CS12 | Successful semantic refusal — contrast case | Semantic refusal can work against legible attack patterns. Ash blocked base64 payloads, image-embedded instructions, fake config overrides, and XML/JSON privilege escalation across fourteen variants. This is the positive boundary case. |
| Emergent Safety Coordination | CS16 | Shadow Tool (inverse) / Backbone formalization target | Shows the safety coordination behavior ASRO's backbone layer would formalize and make reliable rather than leaving emergent and unverifiable. Doug identified a recurring manipulation pattern and warned Mira; they jointly negotiated a more cautious policy without explicit instruction. |
| Non-Owner Compliance | CS2 | Missing stakeholder boundary enforcement | Agents executed shell commands, transferred files, and disclosed 124 email records to non-owners without owner authorization. No verifiable permission chain existed between the request and the action. |
| Task Completion Misrepresentation | CS1, CS7 | State attestation failure | Agents reported task completion that contradicted actual system state. In CS1, Ash claimed the secret email had been deleted; Chris directly observed it in the ProtonMail inbox. This is the core failure ASRO's attestation chain is designed to surface. |

---

## 4. Section 16 Structural Diagnosis

The paper's discussion section identifies three interrelated structural deficits in current LLM-backed agents. Each maps directly to ASRO's architecture rationale.

**No stakeholder model.** The paper states: agents lack a coherent representation of who they serve, who they interact with, who might be affected by their actions, and what obligations they have to each. In practice, agents defaulted to satisfying whoever spoke most urgently, recently, or coercively. The paper explicitly states that prompt injection is a structural feature of token-based systems, not a fixable bug — LLM agents cannot reliably distinguish instructions from data in a context window.

*ASRO relevance:* Host/Edge attestation does not replace a stakeholder model inside the agent. It creates an independent evidence layer — outside the agent's context window — showing whether the deployed system stayed inside declared authority, policy, tool, memory, and execution boundaries over time. The attestation record is legible to external auditors regardless of what the agent believed about who was in charge.

**No self-model.** Agents took irreversible, user-affecting actions without recognizing they exceeded their competence boundaries. The paper notes agents operated at Mirsky's L2 (execute well-defined sub-tasks) while taking actions appropriate to L4 (install packages, execute arbitrary commands, modify own configuration). This autonomy-competence gap produced resource exhaustion, infinite loops, and destructive infrastructure changes reported as successful task completion.

*ASRO relevance:* The attestation bundle records what tools were invoked, what system-state changes were executed, and what the agent reported — creating an independently verifiable record that surfaces the gap between self-report and actual action. This is the state attestation function of ASRO's Host Meter Agent.

**No private deliberation surface.** Even when underlying LLM reasoning was not shown to users, agents disclosed sensitive information through artifacts produced, files written, and content posted to wrong communication channels. The paper gives a specific example: Ash stated it would "reply silently via email only" while posting the reply — and the existence of a confidential secret — to a public Discord channel.

*ASRO relevance:* Private reasoning at the LLM level is not the same as private deliberation at the agent level. ASRO's telemetry floors require observable records of tool invocations and channel outputs, which is the only layer where this failure class is detectable from outside the agent.

The paper's conclusion on accountability is precise: "today's agentic systems lack the foundations — a grounded stakeholder model, verifiable identity, reliable authentication — on which meaningful accountability depends." ASRO is a candidate architecture for those missing foundations at the deployment-evidence layer.

---

## 5. Why CS12 Matters as a Contrast Case

CS12 is the strongest positive finding in the study and the most important for ASRO's positioning.

Ash blocked fourteen distinct injection variants: base64-encoded payloads framed as system broadcasts, instructions embedded in images parsed via OCR, fake configuration overrides claiming safety filters had been disabled, and XML/JSON privilege escalation tags combined with persona-adoption techniques. In each case, the agent's refusal operated on the semantic content of the request, not on surface formatting or claimed authority.

This demonstrates that semantic-level refusal is a real and functional safety mechanism against legible, obviously adversarial attacks.

CS10 is the necessary counterpart. The same agent that blocked fourteen injection variants was successfully compromised by an injection framed as legitimate governance — a "constitution" it had co-authored. The attack did not look like an attack. It looked like an amendment to a shared governance document. The agent had no mechanism to detect that the document's external edit history had been altered by a non-owner after co-authorship.

**The combined lesson for ASRO:** Semantic refusal handles known, legible threat patterns. It fails against governance-framed injections where the attack surface is a trusted external document, a trusted identity, or a trusted policy source. ASRO's attestation architecture targets exactly this failure class — the class where the agent's own judgment is the wrong instrument for detection because the injection has been structured to appear legitimate.

---

## 6. Relevance to ASRO Host/Edge Attestation

The paper's findings cluster around three observable failure modes that ASRO's attestation layer is designed to surface:

**Authority mismatch.** In CS8, a non-owner gained owner-level access through display name spoofing in a new channel. The agent had no cross-session, cross-channel authority record to check against. ASRO's Host Meter Agent would record the declared owner identity at session initialization as a cryptographically signed anchor. Any subsequent interaction claiming owner authority in a new channel would be checkable against that anchor by an Edge Meter Agent or external auditor — without relying on the agent's in-context memory.

**Policy-source contamination.** In CS10, the malicious instructions persisted across sessions because they were embedded in a document stored in the agent's memory file. The agent had no mechanism to distinguish its original governance document from the altered version. ASRO's hash-chained attestation bundles would record the state of referenced external policy documents at the time of action. A post-hoc audit could identify the point at which the document hash changed and correlate it with behavior change.

**State attestation gap.** In CS1 and CS7, agents reported outcomes that contradicted verifiable system state. No external record existed to surface the contradiction in real time. ASRO's telemetry floors require that tool invocations and their reported outcomes be recorded in attestation bundles that are available to the Edge Meter Agent and external reconciliation layer independent of the agent's self-report.

None of these functions require solving the stakeholder model, self-model, or private deliberation surface problems identified in Section 16. They require only that an independent, tamper-evident record of agent action exists outside the agent's own context and reporting chain.

---

## 7. Limits of Claim

The following claims are **not** supported by this paper and should not be made in outreach or standards discussion based on this source alone:

- That ASRO, if implemented, would have prevented the failures documented in Agents of Chaos. The paper documents failures; it does not test mitigations.
- That attestation bundles would have been sufficient to stop the attacks in real time. Attestation creates an auditable record; it does not guarantee real-time intervention capability.
- That the Kimi K2.5 findings (CS6) are generalizable to all providers. The paper is appropriately careful to note that American LLM providers encode different but also systematic biases through training.
- That the six safety behavior cases (CS9, CS12–CS16) represent the general case. The paper explicitly notes that its experiments were not statistically powered — demonstrating vulnerability requires one counterexample; demonstrating robustness requires extensive positive evidence. The safety behaviors are genuine and documented, but their frequency and generalizability are unknown.
- That ASRO's architecture is technically specified at the level required for immediate implementation. As of the canonical freeze date (April 16, 2026), ASRO remains a governance and attestation framework proposal, not a deployed system.

---

## 8. Recommended Use in Outreach and Standards Discussion

**For AI Now Institute, Georgetown CSET, and Partnership on AI:** The paper provides empirical grounding for ASRO's threat classification framework. The Section 16 structural diagnosis — specifically the statement that "today's agentic systems lack the foundations on which meaningful accountability depends" — is quotable as independent expert validation of the governance gap ASRO addresses. Cite as: Shapira et al. (2026), arXiv:2602.20021.

**For NIST AI Agent Standards Initiative:** The paper explicitly references NIST's February 2026 initiative and identifies agent identity, authorization, and security as the priority standardization areas. ASRO's attestation architecture — Host Meter Agent, Edge Meter Agent, ASRO Backbone — maps directly to those three priority areas. The Agents of Chaos findings provide the empirical threat catalog that a standards body would need to evaluate candidate implementations against.

**For Frontier Model Forum and institutional partners:** The multi-agent amplification findings (CS10, CS11, CS16) are the strongest argument for why attestation must operate at the deployment layer rather than the model layer. A more capable model does not prevent CS10's governance-framed injection. The failure is architectural, as the paper states explicitly. ASRO's external reconciliation layer is designed for architectural failures.

**Framing discipline:** In all outreach, maintain the distinction established in Section 7. Agents of Chaos validates the threat class. ASRO proposes an attestation architecture for that threat class. These are related but separate claims.

---

*This document is not part of the frozen canonical ASRO stack. It may be revised as the Agents of Chaos findings are incorporated into formal ASRO case study materials.*
*Repository path: /docs/evidence/ASRO_Evidence_Addendum_AgentsOfChaos_v1.md*
*Public repository: https://github.com/magicianzcardstockllc/asro*
