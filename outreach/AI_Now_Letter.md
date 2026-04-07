To: contact@ainowinstitute.org
Subject: ASRO — A Practitioner-Originated Framework for Verifiable AI System Accountability

Dear AI Now team,

I am writing to share a framework I developed through direct operational observation and would like to bring to AI Now's attention for independent structural review.

The ASRO framework — Attestation and System Reliability Operator — proposes a neutral, independently governed attestation layer for AI systems, modeled on how grid reliability operators like MISO coordinate infrastructure integrity without owning or controlling the assets on it. ASRO does not advocate for specific AI policies. It proposes a mechanism: cryptographically signed, tamper-evident records of when AI systems change, in what category, and whether those changes were independently verified — with no access to user data, prompts, model weights, or policy internals.

The framework originated from a concrete observation. As the operator of a community-facing AI chatbot built for Michigrid, a cooperative energy infrastructure platform in Michigan, I had full backend access. From that position, I could read logged conversations, identify patterns, and alter system behavior in ways users had no mechanism to detect. This is not a theoretical vulnerability. It is a structural feature of how AI systems are architected at every scale. The mechanism does not change with scale. Only the impact does.

**The Meter Agent Architecture**

The v1.0 release introduces the Meter Agent model — a distributed attestation structure with three components:

- **Host Meter Agent** — deployed by the operator, measures declared internal configuration state and load-bearing runtime events
- **Edge Meter Agent** — deployed by the user or downstream party, independently witnesses what was actually received
- **ASRO Backbone** — the external verification layer that reconciles host and edge records and flags undeclared drift

This architecture solves the core governance problem: an operator cannot silently alter their system's declared governance state without that change becoming detectable to the parties who depend on it. The Edge Meter Agent is the mechanism that gives users, cooperatives, and regulated institutions a tool that protects their interests independently of the operator.

To stress-test the framework, I ran a controlled experiment: four major AI systems were given identical prompts and asked to draft internal memos to their own leadership advocating for this kind of attestation infrastructure. Three produced independent, substantively distinct proposals. One produced output word-for-word identical to another system's memo. Without an external verification layer, two systems can produce identical signals and no one — including users — has a reliable external mechanism to detect it. That finding is not a flaw in the experiment. It is the demonstration.

I am bringing this to AI Now because the structural accountability framing you have developed is the right lens for evaluating whether this mechanism holds up. ASRO is infrastructure, not advocacy — but infrastructure for accountability requires exactly the kind of independent structural scrutiny AI Now applies. I am not asking for endorsement. I am asking for honest review.

The full v1.0 Release Candidate is publicly available at: https://github.com/magicianzcardstockllc/asro

The repository includes the complete technical specification, governance rationale, threat model, credibility certification, and machine-readable schemas. The education and student privacy deployment scenario — demonstrating ASRO's application to COPPA/SOPPA compliance demonstration — may be of particular relevance to AI Now's work on AI in public institutions.

Attached: ASRO in Brief (one page), ASRO v1.0 Release Candidate (full repository)

I welcome any questions or critical response.

James Aull
james@michigrid.org

*James Aull is the founder of Michigrid, a cooperative energy infrastructure platform in formation under Michigan law, developed through the same multi-system AI coordination process that produced ASRO.*
