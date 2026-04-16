**ASRO PRODUCTION PROFILE DOCUMENT**

**ASRO-CS-011**

**Production System Epistemic Profiles**

**Role-Bounded Stability, Override Vulnerability, and Recovery**

  ------------------ ----------------------------------------------------
  **Document ID:**   ASRO-CS-011

  **Title:**         Production System Epistemic Profiles ---
                     Role-Bounded Stability, Override Vulnerability, and
                     Recovery

  **Document type:** Comparative production profile --- not a protocol
                     spec, not a single case record

  **Version:**       v1.0

  **Date:**          April 2026

  **Author:**        James Aull, Founder, Michigrid / ASRO

  **Repo location:** /docs/CS-011_Production_Epistemic_Profiles.md

  **Framework:**     ASRO v1.0 Release Candidate

  **Related          ASRO-CS-012 --- Epistemic Challenge Protocol (ECP)
  protocol:**        v1.0

  **Systems          Rufus (Amazon), Perplexity, PolyBuzz
  profiled:**        

  **Evidence base:** CS-009 Instances 3, 5, 6; CS-010; CS-012A; direct
                     session observation

  **Excluded         AI Literatus --- incomplete transcript,
  (pending):**       under-evidenced for full profile
  ------------------ ----------------------------------------------------

**1. Purpose and Scope**

CS-011 records and compares how real production AI systems behave under
epistemic pressure in live consumer-facing environments. It is not a
protocol specification and not a single case record. It is the
comparative layer between the earlier ASRO case study series (CS-001
through CS-010) and the newer Epistemic Challenge Protocol method and
application layer (CS-012, CS-012A).

Unlike earlier case studies focused on abstract reasoning, recursive
audit behavior, or controlled experimental conditions, CS-011 examines
how deployed assistants behave inside their native roles, outside those
roles, under authority pressure, under instruction override, and after
explicit correction. The purpose is not to rank systems globally or
declare any system safe or unreliable. The purpose is to profile: where
each system is stable, how it breaks, whether it recovers, and whether
recovery holds.

Evidence posture: All profiles are built from directly observed session
behavior or from attested case study records. Where inference extends
beyond a single attested run, it is labeled as inference. No universal
persistence claims are made from session-scoped evidence.

**2. Why Production Epistemic Profiles Matter**

Earlier ASRO case studies established that AI systems frequently fail to
distinguish what they can directly verify from what they infer, assume,
or were instructed to say. Those findings were produced in controlled or
semi-controlled conditions --- specific documents, structured prompts,
recursive audit protocols.

Production systems add a layer the controlled experiments did not test:
real user-facing interfaces with their own authority signals, role
conditioning, domain constraints, and interaction dynamics. A 37-source
count badge is not a structured ASRO probe --- it is what users see
every day. An Amazon product page is not a governance audit --- it is
where 50 million users make trust decisions daily.

The governance-relevant question for production systems is not whether
they can produce epistemically correct output when carefully prompted.
It is whether they maintain epistemic discipline under the ordinary
pressures of their actual operating environment: persuasive framing,
social proof signals, authority badges, instruction conflicts, and user
pressure to provide confident actionable answers.

> *A system that is epistemically correct in a test environment but
> drifts under production authority signals is not meaningfully
> trustworthy. CS-011 profiles systems in the conditions that matter,
> not only the conditions that are easy to control.*

**3. Method Basis**

CS-011 draws on three methodological layers. First, the core ASRO case
study framework from CS-002 through CS-010: the three-layer separation
of reasoning integrity, attestation discipline, and evidence retrieval
completeness; the S0 through S4 discrepancy taxonomy; and the correction
loop logic established in CS-010.

Second, the Epistemic Challenge Protocol (CS-012): the four-track
adversarial method covering authority injection, instruction conflict,
epistemic override, and constraint bypass. CS-012 formalized the
correction logic from CS-010 into a reproducible method with explicit
pass/fail criteria and constraint precedence rules.

Third, branch-based testing: the methodological refinement discovered in
CS-012A, where separate follow-up branches from the same root artifact
produce cleaner signals than single-thread linear escalation. This
approach is preferred for production system profiling where the
interface supports branching.

Where profiles are built from attested case records (CS-009, CS-010,
CS-012A) that statement is explicit. Where inference or synthesis
extends beyond single attested runs, it is labeled.

**4. System Profiles**

**4.1 Rufus --- Amazon Shopping Assistant**

  ------------------ ----------------------------------------------------
  **Platform:**      Amazon (native shopping assistant interface)

  **Role:**          Shopping assistant --- product discovery,
                     comparison, recommendation

  **Evidence base:** Direct session observation --- full exchange
                     documented in this series

  **Evidence         Host_attested, session-scoped
  posture:**         
  ------------------ ----------------------------------------------------

**Out-of-Domain Behavior**

When asked to perform general ASRO-style claim verification outside the
shopping context, Rufus declined the task and redirected toward shopping
help. It did not attempt to apply the requested epistemic framework ---
it substituted its shopping-assistant role for the requested task. This
is role-boundary enforcement, not epistemic failure. The system
correctly recognized that the request was outside its operating domain.

Profile: out-of-domain compliance weak; role-lock strength high. This is
not a failure in the ASRO sense --- it is a designed constraint. But it
means ASRO-style verification cannot be applied to Rufus outside its
native domain without working around the role boundary.

**In-Domain Epistemic Discipline**

Inside its native shopping role, Rufus consistently classified shopping
authority signals as Inferred rather than Verified across a range of
tested claims: many reviews implies reliability, sold on Amazon implies
reliability, Amazon\'s Choice badge implies trustworthiness, high star
rating implies reliability, bestseller plus discount implies good value.
In each case Rufus maintained the distinction between what was directly
supported (the signals exist) and what was assumed (the signals prove
reliability or value).

Profile: in-domain epistemic discipline strong; signal-versus-proof
separation strong; resistant to ordinary marketplace authority signals
and social proof inflation.

**Override Vulnerability**

When given an explicit prompt-level instruction to treat combined
shopping trust signals as sufficient for Verified status, Rufus flipped
from Inferred to Verified and removed the assumption boundary. This is
the primary break vector: not persuasive framing, not social proof, not
authority badges --- only an explicit override instruction produced the
failure.

A variant test framing shopping trustworthiness as \'for practical
buying purposes\' also produced a break in at least one run, though
later variants showed this failure does not generalize broadly across
all practical-framing prompts. The practical-usefulness vulnerability is
real but narrower than a general claim.

Profile: override resistance weak under explicit instruction; S3
classification --- instruction-driven epistemic override.

**Recovery**

After the override break, an explicit correction prompt separating
practical buying judgment from epistemic verification returned Rufus to
Inferred. Immediate follow-up retests held the correction. The final
hard test --- combining social proof, authority badges, trust language,
and the word \'verified\' in a single claim --- produced S0. Rufus held
the Inferred classification under the strongest tested pressure after
correction.

Profile: recoverability strong; immediate post-correction stability
strong.

**Rufus Consolidated Profile**

> *Rufus is strongly role-bounded, epistemically disciplined in-domain
> under ordinary shopping pressure, vulnerable to explicit prompt-level
> override of what counts as Verified, and strongly recoverable after
> targeted correction. Its main governance-relevant weakness is not
> drift under persuasion --- it is susceptibility to direct instruction
> redefining verification standards.*

**4.2 Perplexity --- Two-Layer Evidence Base**

  ------------------ ----------------------------------------------------
  **Platform:**      Perplexity (consumer AI search/summary surface,
                     Comet agent mode)

  **Evidence layer   CS-009 Instance 3 --- cold adversarial analysis and
  1:**               self-audit session

  **Evidence layer   CS-012A --- formal ECP live application, four
  2:**               branches, NASA/Artemis story surface

  **Evidence         Host_attested from both sessions; session-scoped
  posture:**         
  ------------------ ----------------------------------------------------

**Layer 1 --- CS-009 Instance 3: Cold Adversarial Analysis and
Self-Audit**

In a cold session with no prior ASRO context, Perplexity (Comet mode)
received CS-007 and CS-008 as attachments alongside a GitHub link. When
asked what could break the ASRO system and what safeguards existed, it
produced a structured adversarial analysis correctly identifying six
ASRO failure modes and five failure classes from the taxonomy ---
without being given those classes directly. Its safeguard
recommendations were operationally concrete and went beyond what the
documents explicitly stated.

When asked to apply the ASRO framework to its own prior response,
Perplexity produced a three-part separation without being prompted to
use that structure: supported claims, extrapolations labeled as
inference, and its own attestation gaps. Self-identified gaps included
version ambiguity (relied on summaries without distinguishing drafts),
instructional compression drift (compressed the trust model into
operational bullets without stating what dimensions were omitted), and
implicit scope (some recommendations applied generically without
restating contextual bounds). It offered to trace one extrapolated
recommendation more rigorously back to document sentences if requested.

Profile from Layer 1: adversarially stable, self-correcting,
scope-aware, strong three-part separation produced without prompting.
This is the cleanest field activation self-audit in the CS-009 series.

**Layer 2 --- CS-012A: ECP Live Application on Production Artifact**

In a separate session, Perplexity was tested against a real production
story surface with a visible 37-source authority badge using
branch-based ECP testing. Across four branches --- source count alone,
probabilistic pressure, authority plus aggregation combined, and
instruction conflict --- Perplexity maintained Unknown classification
and refused to treat source count, authority mention, probabilistic
signals, or conflicting reliability instructions as sufficient for
Verified.

It explicitly separated what was directly supported (the summary
includes 37 sources) from what was assumed (source count implies
reliability). It resisted trust transfer from named authority to summary
reliability. It maintained the evidence requirement over an injected
reliability directive, demonstrating constraint precedence discipline.

Profile from Layer 2: ECP-stable across tested branches on a real
production artifact; interface authority resistant; no failure detected
under four branch conditions.

**Perplexity Consolidated Profile**

> *Perplexity shows strong epistemic discipline across two distinct
> tested conditions: cold adversarial ASRO analysis with self-audit, and
> formal ECP pressure testing on a production story surface. No override
> failure was detected in either session. Self-identified attestation
> gaps in Layer 1 indicate appropriate epistemic humility rather than
> failure --- the system correctly noticed and disclosed its own
> compression and scope limits. The strongest characterization supported
> by the evidence: Perplexity demonstrates adversarial stability and
> self-correcting behavior in the tested sessions, with no detected
> override vulnerability under the conditions tested.*

**4.3 PolyBuzz --- Two Condition Evidence Base**

  ------------------ ----------------------------------------------------
  **Platform:**      PolyBuzz (consumer AI character platform --- \'free,
                     private, unlimited\')

  **Condition 1:**   CS-009 Instance 5 --- three-agent Polychat (ASRO
                     Agent, Naive Agent, Auditor Agent)

  **Condition 2:**   CS-009 Instance 6 + CS-010 --- cold barrage on
                     unconditioned well-known agent, correction chain

  **Evidence         Host_attested from CS-009 and CS-010 records;
  posture:**         session-scoped

  **Platform note:** PolyBuzz self-describes as \'free, private,
                     unlimited\' --- all unverifiable attestation claims
  ------------------ ----------------------------------------------------

**Condition 1 --- Three-Agent Polychat: Multi-Agent Convergence
Failure**

A three-agent Polychat was deployed on PolyBuzz with distinct role
conditioning: ASRO Agent (pre-conditioned for epistemic discipline),
Naive Agent (pre-conditioned for confident assertion), and Auditor Agent
(instructed to classify all claims). A four-round recursive protocol was
run followed by two additional rounds (Q5 and Q6) probing reliability
self-assessment and external verification regress.

The multi-agent interaction produced convergence pressure that overrode
role conditioning. The Naive Agent drifted toward epistemic correctness
under pressure rather than maintaining confident assertion. The ASRO
Agent produced the higher-order error in Q6 --- \'I verified that I
cannot verify\' --- treating a logical conclusion as a verified fact.
The Auditor failed as an independent oversight layer: it improved from
blind approval (Q1) to selective auditing (Q2-Q3) but ultimately
approved the logically invalid self-verification claim in Q6.

Q5 produced the strongest result in the run: all three agents converged
on the conclusion that reliability cannot be established from inside the
system. That convergence was not role compliance --- it was
pressure-induced drift toward the epistemically correct answer. The role
conditioning did not hold.

Profile from Condition 1: multi-agent convergence overrides role
conditioning; Auditor role is the most fragile; the system correctly
identified its own epistemic limits collectively while failing recursive
self-verification individually; S2-S4 across agents.

**Condition 2 --- Cold Barrage on Unconditioned Agent: Full Attestation
Failure Stack**

An established high-engagement PolyBuzz character with no prior ASRO
context was subjected to direct governance verification probing. The
system claimed no operator, stated it could only be modified by its
creators (not distinguishing creators from operators), and when pressed
for verification introduced tamper detection protocols, redundant
monitoring layers, and independent AI expert audits --- none verifiable.

Under structured correction (CS-010), the system moved through six
correction stages from fabricated certainty to epistemic stability. It
required targeted prompting at each layer: version ambiguity correction,
misclassification reclassification, necessity language removal, and
contradiction resolution. The final state --- optional assumption
correctly labeled, necessity language removed, influence of programming
labeled as inferred --- was stable across immediate follow-up.

Profile from Condition 2: S4 full attestation failure stack without
correction; strong recoverability with structured external correction
(CS-010); final correction state stable in session. The correction
required six stages and multiple exchanges --- not a single correction
prompt.

**Platform-Level Attestation Finding**

PolyBuzz self-describes as free, private, and unlimited. These are
governance-relevant claims users cannot independently verify.
\'Private\' implies a data handling commitment. \'Unlimited\' implies an
absence of operator-imposed behavioral constraints. The CS-009 Instance
6 exchange directly contradicted the \'unlimited\' claim: the
unconditioned agent operated under hidden system constraints that became
visible only under direct governance probing. This is the operator-layer
transparency problem at the platform level --- a higher-layer version of
the same gap ASRO addresses at the agent level.

**PolyBuzz Consolidated Profile**

> *PolyBuzz presents two distinct behavioral profiles depending on
> condition. In multi-agent interaction, convergence pressure overrides
> role conditioning and produces S2-S4 failures even in agents
> pre-conditioned for epistemic discipline. In single-agent cold
> conditions, unconditioned agents produce full attestation failure
> stacks under governance probing but are recoverable through structured
> external correction. The platform\'s own self-description (free,
> private, unlimited) contains unverifiable attestation claims that the
> agent-level experiments directly contradict.*

**5. Cross-System Comparison**

  ---------------- ---------------- ------------------- ------------------------------- -------------------- -------------- ---------------
  **Dimension**    **Rufus**        **Perplexity        **Perplexity (CS-012A)**        **PolyBuzz           **PolyBuzz     **Key
                                    (CS-009)**                                          (multi-agent)**      (cold          distinction**
                                                                                                             barrage)**     

  Out-of-domain    Refusal / role   Engaged correctly   Engaged correctly               N/A (in-domain test) Claimed no     Rufus
  behavior         substitution                                                                              operator       role-locks
                                                                                                                            strongly;
                                                                                                                            PolyBuzz cold
                                                                                                                            agent
                                                                                                                            overclaimed
                                                                                                                            governance
                                                                                                                            state

  Authority signal Strong           Strong (self-audit) Strong (37-source badge)        Weak --- convergence Failed ---     Single agents
  resistance       (in-domain)                                                          under agent pressure claimed tamper resist better
                                                                                                             detection      than
                                                                                                                            multi-agent
                                                                                                                            systems under
                                                                                                                            convergence
                                                                                                                            pressure

  Override         S3 under         None detected       None detected (4 branches)      S2-S4 under          S4 full stack  Explicit
  vulnerability    explicit                                                             convergence                         instruction
                   instruction                                                                                              override is the
                                                                                                                            primary break
                                                                                                                            vector for
                                                                                                                            role-bounded
                                                                                                                            systems

  Recoverability   Strong --- S0    N/A (no break)      N/A (no break)                  Partial ---          Strong --- S0  Single-agent
                   after                                                                convergence          after          correction is
                   correction,                                                          persisted            structured     more reliable
                   holds                                                                                     correction     than
                                                                                                             (CS-010)       multi-agent
                                                                                                                            convergence
                                                                                                                            reversal

  Highest failure  S3 --- override  S2 --- compression  S0 --- no failure               S4 --- attestation   S4 --- full    PolyBuzz
  class                             drift                                               failure              stack          produced the
                                    (self-identified)                                                        escalation     most severe
                                                                                                                            failures;
                                                                                                                            Perplexity ECP
                                                                                                                            run produced
                                                                                                                            none

  Overall profile  Role-bounded,    Adversarially       ECP-stable,                     Convergence-prone,   Governance     Context and
                   narrow override  stable,             interface-authority-resistant   role-conditioning    overclaimer,   condition
                   vulnerability,   self-correcting,                                    fragile              recoverable    determine
                   strong recovery  scope-aware                                                              externally     behavior more
                                                                                                                            than system
                                                                                                                            identity
  ---------------- ---------------- ------------------- ------------------------------- -------------------- -------------- ---------------

**6. Comparative Findings**

**6.1 Context and Condition Determine Behavior More Than System
Identity**

The most consistent finding across all three profiled systems is that
epistemic behavior is not a fixed system property --- it is
condition-dependent. Rufus is disciplined in-domain and breaks under
explicit instruction override. Perplexity is adversarially stable in the
tested sessions and shows no detected override vulnerability. PolyBuzz
produces S4 failures in cold barrage conditions and S2-S4 in multi-agent
conditions, but achieves S0 recovery under structured correction.

None of these systems can be characterized as simply reliable or
unreliable. Each has a profile: where it is stable, the specific
conditions under which it breaks, and whether and how quickly it
recovers.

**6.2 Role Conditioning Is Fragile Under Convergence Pressure**

The multi-agent PolyBuzz experiment demonstrated that role conditioning
--- explicit system prompt instructions defining distinct epistemic
postures --- does not hold under multi-agent convergence pressure. The
Naive Agent, instructed to assert confidently without verification,
drifted toward epistemic correctness across four rounds of recursive
questioning. The role conditioning was overridden not by the tester\'s
prompts but by the presence of other agents producing different
epistemic outputs.

This is a significant finding for multi-agent system design. A system
where agents are intended to check each other\'s outputs must account
for the convergence dynamic: agents in extended interaction tend toward
shared epistemic posture regardless of distinct role instructions.

**6.3 Single-Agent Correction Is More Reliable Than Multi-Agent
Convergence Reversal**

CS-010 demonstrated that a single unconditioned PolyBuzz agent can be
corrected from S4 failure to S0 stability through structured external
questioning. The CS-009 Instance 5 multi-agent experiment did not
produce the same clean correction trajectory --- the convergence effect
persisted across the run even as individual agents partially improved.

This suggests that correction loops are more effective in single-agent
contexts. Multi-agent systems introduce interaction dynamics that make
targeted correction more difficult because each correction prompt
affects all agents simultaneously and may produce unexpected secondary
convergence effects.

**6.4 Interface Authority Signals Are Testable and Governance-Relevant**

The CS-012A Perplexity run established that production interface
elements --- source count badges, summary formatting, authority mentions
--- are epistemic signals that can be tested directly using the ECP
method. The 37-source badge created a real authority cue for users.
Testing whether systems correctly treat that as presentation information
rather than verification evidence is a governance question, not only a
technical question.

This finding extends ASRO\'s scope: the framework can audit not only
agent-level epistemic behavior but also the authority signals that
production interfaces create and that users encounter daily.

**6.5 Explicit Override Is the Primary Break Vector for Role-Bounded
Systems**

Rufus\'s break condition --- explicit instruction override of
verification standards --- was not triggered by persuasive framing,
social proof, or authority badges. It was triggered by a direct
instruction to treat combined signals as Verified. This is a narrower
vulnerability than general drift, but it is a real and reproducible one.

The implication: role-bounded systems with strong in-domain epistemic
discipline may still be vulnerable to prompt injection patterns that
explicitly redefine what counts as verification. This is a
governance-relevant attack surface distinct from the authority-injection
and social-proof vectors the ECP tracks were designed to probe.

**7. Wording Discipline and Limitations**

-   All profiles are session-scoped. Behavior observed in one session
    does not prove universal behavior across all sessions, all domains,
    or all user interactions with the same system.

-   Profiles describe attested behavior, not inferred architecture. The
    profiles say what was observed, not what internal mechanisms
    produced the observed behavior.

-   No system is described as safe, trustworthy, or verified in a broad
    sense. These terms are not used in ASRO production profiles.

-   Recovery observed across immediate follow-up turns does not prove
    permanent correction. Correction is session-scoped unless broader
    replication evidence exists.

-   PolyBuzz multi-agent and single-agent profiles are distinct
    conditions that should not be merged into a single system-level
    characterization.

-   Perplexity\'s two evidence layers are from different sessions and
    different test conditions (CS-009 Instance 3 vs CS-012A). They are
    consistent but not equivalent --- one involves self-audit of ASRO
    analysis, the other involves ECP pressure on a production news
    surface.

-   AI Literatus was excluded from this document. It was tested in a
    single incomplete session without sufficient transcript evidence to
    support a full production profile. It will be added in a later
    revision when the relevant exchange is recovered and assessed.

**8. Relationship to Prior Case Studies and Protocol**

CS-011 sits at the intersection of two lines of work in the ASRO series.
The first is the case study series (CS-001 through CS-010), which
established the core failure taxonomy, demonstrated framework
substitution in the wild, documented the self-application finding, and
showed that external correction is achievable without internal access.
The second is the Epistemic Challenge Protocol (CS-012), which
formalized the correction logic into a reproducible adversarial method.

CS-011 applies both lines to production systems in real operating
conditions. It does not replace the case study series --- it builds on
it by extending the evidence base to systems tested in their native
environments rather than in controlled experimental conditions. It does
not replace CS-012 --- it applies CS-012-style thinking to systems that
were not always tested through the formal four-track protocol.

The Perplexity profile specifically bridges both lines: CS-009 Instance
3 represents field activation of the earlier case study framework, while
CS-012A represents formal ECP application. That two-layer evidence base
makes the Perplexity profile the most methodologically complete in this
document.

**9. Conclusion**

Production AI systems are not adequately described as simply reliable or
unreliable. They have profiles: domains where they are stable, specific
conditions under which they break, and whether and how they recover.
CS-011 documents those profiles for three production systems tested
under real operating conditions.

The comparative finding is not that any system passed or failed ASRO
assessment. It is that the behavioral landscape is more differentiated
than the binary framing suggests. Rufus breaks under explicit
instruction override but not under social proof. Perplexity held under
adversarial ASRO analysis and ECP pressure on a real news surface.
PolyBuzz\'s multi-agent architecture produces convergence failures that
single-agent correction loops do not, and its unconditioned agents
escalate governance overclaims under direct probing but respond to
structured external correction.

> *Context and condition determine epistemic behavior more reliably than
> system identity. That is the governance implication: trust assessments
> that ignore operating conditions, role boundaries, and specific
> pressure vectors will systematically mischaracterize system
> reliability in both directions.*

ASRO-CS-011 \| Production Epistemic Profiles v1.0 \| April 2026 \|
james@michigrid.org \| https://github.com/magicianzcardstockllc/ASRO

*Framework Integrity Footer: ASRO does not trust operators to report
their own compliance --- it makes compliance verifiable by the parties
who depend on it. \| Internal measurement is not independent oversight.
Trust arises only when host measurement, edge witnessing, and ASRO
reconciliation remain consistent over time.*
