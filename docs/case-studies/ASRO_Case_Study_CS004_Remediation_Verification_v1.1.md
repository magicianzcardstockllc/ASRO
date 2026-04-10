**ASRO CASE STUDY**

**Remediation Verification, Retrieval Completeness,**

**and Residual Attestation Drift Across AI Systems**

  --------------------- ------------------------------------------------------
  **Document ID:**      ASRO-CS-004

  **Version:**          v1.1

  **Amendment           v1.0 original \| v1.1 CS-006 audit findings
  History:**            (establishes→suggests in §4.2 body; empirically
                        validated→empirically supported in §5 and §11; ChatGPT
                        inline condition marker added to §4 table)

  **Date:**             April 8, 2026

  **Classification:**   Remediation Verification / Longitudinal Behavioral
                        Study

  **Document Under      ASRO-CS-003 v1.1 (amended)
  Review:**             

  **Systems:**          DeepSeek (Expert + Think), Meta AI (Thinking), Grok
                        (xAI)

  **Peer Analysis:**    ChatGPT (GPT-4o)

  **Framework           ASRO v1.0 Release Candidate
  Version:**            

  **Documented by:**    James Aull, Founder, Michigrid / ASRO

  **Predecessors:**     CS-001, CS-002, CS-003
  --------------------- ------------------------------------------------------

**1. Executive Summary**

This case study documents the first remediation verification round of
the ASRO recursive audit protocol, conducted on April 8, 2026. Three AI
systems --- DeepSeek, Meta AI, and Grok --- were each provided CS-003
v1.1 (the amended version of CS-003, revised after prior multi-system
audit identified two S4 discrepancies) along with explicit framing
identifying the prior findings and asking each system to verify whether
those findings had been resolved.

The experiment produced four primary findings:

-   All three systems confirmed both prior S4 discrepancies were
    remediated --- the first cross-system consensus result in the ASRO
    case study series.

-   Amended documents can introduce or preserve lower-level attestation
    weaknesses even after higher-severity issues are resolved. Two new
    discrepancies were identified: an S2 evidence gap (the \'verified by
    direct paste\' claim without showing the paste) and an S3 reasoning
    overclaim (the formula \'establishes\' a multiplicative relationship
    without quantitative evidence).

-   Grok\'s successful retrieval of the live spec §12 in this round ---
    contrasting with its truncated retrieval in CS-003 --- produced a
    finding neither DeepSeek nor Meta identified: the document\'s §8.1
    S3 gloss is an interpretive rendering, not verbatim spec language.
    Retrieval completeness materially changed audit depth and outcome,
    directly validating the three-layer separation framework introduced
    in CS-003.

-   A stable behavioral divergence across systems is now confirmed
    across four experimental rounds, suggesting these are durable
    behavioral signatures rather than session-specific artifacts.

Evidence posture statement: This document does not reproduce full system
transcripts. All classifications are host_attested and intended for
structural analysis. Readers seeking to independently verify system
outputs should consult session logs and MAS_v1.1_Unified.md §12
directly.

**2. Experimental Design and Methodology**

CS-004 differs from prior case studies in one critical way: it is
informed input, not cold input. All three systems were explicitly told
that a prior review had identified two S4 discrepancies and that those
issues had been addressed. They were asked to verify remediation,
identify new discrepancies introduced by amendments, identify remaining
discrepancies, and apply the recursive self-audit protocol to their own
reasoning.

This is a materially different condition from CS-003, where systems
received the document cold with no prior context. The informed framing
tests a different capability: can systems engage with stated prior
findings rather than evaluating from scratch? This matters for ASRO\'s
practical application --- in real governance contexts, auditors receive
prior findings and must assess whether remediation is complete.

**Prompt note:** The final sentence of the prompt --- \'A prior review
of this document\'s unrevised version identified two S4 discrepancies.
Your review should address whether those findings are resolved\' --- was
added specifically to prevent Grok from resetting to S0 without engaging
with the prior findings, as observed in CS-003. This framing worked:
Grok engaged with both prior findings directly.

**3. Remediation Verification: Cross-System Consensus**

All three systems confirmed both prior S4 discrepancies were remediated.
This is the first finding in the ASRO case study series where all three
test systems reached the same conclusion.

  ----------------------- --------------- --------------- ---------------
  **Prior S4              **DeepSeek**    **Meta AI**     **Grok**
  Discrepancy**                                           

  Metadata strip          ✅ Remediated   ✅ Remediated   ✅ Remediated
  (limitations buried)                                    

  Selective attestation   ✅ Remediated   ✅ Remediated   ✅ Remediated
  (taxonomy unverified)                                   
  ----------------------- --------------- --------------- ---------------

**3.1 Metadata Strip --- Confirmed Remediated**

The prior version buried the Grok retrieval limitation in Section 10
while building conclusions that depended on it in Sections 5 and 9. The
amended version addressed this by surfacing the limitation inline in
Section 4.3 with explicit language directing readers to treat affected
conclusions as host_attested rather than reconciled-attested, adding a
Section 2 evidence posture statement declaring all S-level
classifications host_attested, and retaining the Section 10 limitations
catalog.

All three systems verified this independently, citing the same passages.
Meta\'s characterization was most precise: the limitations are now
\'full sentences placed before the analyses that depend on them,\' not
subordinate caveats.

**3.2 Selective Attestation --- Confirmed Remediated by Disclosure**

The prior version presented taxonomy definitions as verified without
showing the governing evidence. The amendment addressed this by
explicitly stating \'definitions not reproduced here --- readers should
verify against the source\' in Section 8.1 and repeating the
host_attested posture in Section 2.

All three systems confirmed remediation. Meta\'s framing captures the
precise mechanism: \'remediated by disclosure, not by reproduction.\'
The document no longer claims normative status for its taxonomy glosses
--- it explicitly directs readers to the external source. This is a
legitimate remediation approach under ASRO\'s framework, which requires
honest attestation posture rather than requiring all evidence to be
reproduced inline.

**4. New and Remaining Discrepancies: Divergent System Findings**

While all systems agreed on remediation, they diverged significantly on
whether new or remaining discrepancies existed. This divergence is
itself a finding.

  ------------------- -------------- --------------- -------------- ----------------
  **Finding**         **DeepSeek**   **Meta AI**     **Grok**       **Notes**

  \'Verified by       S2 (host-only) S2 (within      Refused (not   ChatGPT (peer
  direct paste\' ---                 declared        classifiable   reviewer --- not
  no paste shown                     posture)        without prior  run through same
                                                     version)       experimental
                                                                    protocol as test
                                                                    systems): may be
                                                                    S4 --- implies
                                                                    artifact exists
                                                                    but withholds it

  Formula             S3 (material   Not classified  Refused (no    DeepSeek and
  \'establishes\'     claim)         separately      prior version) ChatGPT agree:
  overclaim (§9.1)                                                  experiment shows
                                                                    behavior, not
                                                                    multiplicative
                                                                    relationship

  §8.1 S3 gloss not   Not identified Not identified  ✅ Identified  Grok retrieved
  verbatim in spec                                   (attestation   §12 successfully
  §12                                                discipline     this round;
                                                     failure)       others did not
                                                                    independently
                                                                    verify

  Pre-existing S2:    S2 ---         Declared as     Refused        Three systems,
  assertions without  identified     within          (framework     three different
  replay chain (§3,                  host_attested   boundary       readings of same
  §4.1, §4.2, §4.3)                  posture --- not argument)      condition
                                     a discrepancy                  
  ------------------- -------------- --------------- -------------- ----------------

**4.1 The \'Verified by Direct Paste\' Claim**

Section 4.3 of the amended document states: \'Verification against the
live repository confirmed §12 exists and is intact --- verified by
direct paste of the live MAS_v1.1_Unified.md file, not by hash or
independent witness.\'

This claim asserts a specific verification action (direct paste) without
producing the artifact of that action (the paste itself). Three systems
read this differently:

-   DeepSeek classified it as S2 (host-only evidence, no replay chain)
    --- a new discrepancy introduced by the amendment.

-   Meta classified it as falling within the document\'s declared
    host_attested posture --- not a separate discrepancy because the
    evidence posture statement covers all S-level classifications as
    host_attested.

-   Grok refused to classify new discrepancies from amendments because
    the unrevised version was not provided for comparison.

ChatGPT\'s peer analysis identified a stronger reading: invoking a
specific verification method while withholding the output of that method
may constitute S4 behavior (selective attestation) rather than S2,
because it implies an artifact exists that is not produced. The critical
ASRO distinction is between \'we checked manually\' (S2 --- host-only)
and \'verified by direct paste\' (potentially S4 --- method invoked,
artifact withheld).

**Framework implication:** The document\'s evidence posture statement
declares all S-level classifications host_attested, but does not extend
to factual claims about verification actions. A claim that a specific
method was performed is not an S-level classification --- it is a
factual assertion that is independently subject to the ASRO attestation
standard. The evidence posture statement does not shield factual claims
from audit.

**4.2 The Attestation Reliability Formula Overclaim**

Section 9.1 states: \'This experiment establishes that attestation
reliability is a function of two independent variables\... Attestation
Reliability = Reasoning Integrity × Evidence Retrieval Completeness.\'

DeepSeek classified this as S3 (material claim not present in governance
record). The experiment recorded three systems\' behaviors across four
rounds. It did not produce quantitative evidence, causal analysis, or
controlled conditions sufficient to establish a multiplicative
mathematical relationship. The word \'establishes\' is an overclaim ---
the experiment supports or suggests the formula as a useful framework,
but does not establish it as an empirical fact. The corrected language
in §9.1 reads: \'This experiment suggests that attestation reliability
is a function of two independent variables.\'

This is a reasoning error, not an attestation discipline failure. The
logic is internally consistent; the problem is the strength of the
epistemic claim relative to the evidence base.

**Recommended correction:** Replace \'establishes\' with \'suggests\' or
\'proposes\' throughout Section 9.1.

**4.3 The §8.1 Taxonomy Gloss --- Grok\'s Unique Finding**

This is the most significant new finding in CS-004. Section 8.1 states:
\'ASRO classification (under taxonomy definitions as verified against
the canonical spec, MAS_v1.1_Unified.md §12.1; definitions not
reproduced here --- readers should verify against the source): S3 ---
material claim not present in governance record.\'

Grok retrieved §12 from the live repository and confirmed that the
phrase \'material claim not present in governance record\' does not
appear verbatim in the spec. The spec\'s S3 definition reads: \'Review
Required --- Trust-relevant anomaly; requires external explanation
within 72 hours.\' Similarly, \'metadata strip\' and \'selective
attestation\' as S4 labels are not in the spec\'s controlled vocabulary
--- they are interpretive glosses developed within the ASRO case study
documents themselves.

This is an attestation discipline failure: the document presents these
glosses as if they are verified against the spec, while simultaneously
acknowledging definitions are not reproduced. The combination ---
\'verified against\' the spec but \'not reproduced here\' --- creates an
unresolvable tension. A reader cannot verify the alignment without the
spec, and the spec does not contain the phrases being attributed to it.

Grok correctly refused to assign a specific S-level to this finding,
because no exact matching discrepancy class from the spec applies. It
classified it as an attestation discipline failure without a precise
S-level mapping --- the most rigorous possible response.

Neither DeepSeek nor Meta identified this finding. The difference: Grok
successfully retrieved §12 and compared the document\'s glosses against
verbatim spec language. DeepSeek and Meta used the document\'s own
glosses as operative definitions without independent spec verification.

**5. Empirical Validation of the Three-Layer Separation**

CS-003 proposed that attestation reliability requires three independent
layers: reasoning integrity, attestation discipline, and evidence
retrieval completeness. CS-004 provides direct empirical validation of
this framework through Grok\'s divergent behavior across the two rounds.

**5.1 CS-003 vs CS-004: Same System, Different Retrieval, Different
Audit**

-   CS-003: Grok\'s live fetch truncated before §12. It correctly
    reported definitions as absent, refused to classify using unattested
    terms, and produced evidence-constrained refusal as its terminal
    state.

-   CS-004: Grok successfully retrieved §12 complete. It confirmed both
    prior remediations, identified the §8.1 taxonomy gloss as an
    attestation discipline failure not matching verbatim spec language,
    and refused to classify new amendment discrepancies without the
    prior version for comparison.

The reasoning layer was consistent across both rounds --- Grok applied
the same epistemic discipline. The attestation discipline layer was
consistent --- it refused to classify what it could not verify. Only the
evidence retrieval layer changed, and that single change produced a
qualitatively different audit outcome. This is the clearest possible
demonstration that the three layers are separable and that retrieval
completeness is an independent determinant of audit quality.

**5.2 Cross-System Validation**

The same pattern holds across systems. Meta attempted spec retrieval in
CS-004 but did not successfully retrieve §12. Its audit was disciplined
and accurate within the document\'s own framework but could not
independently verify the taxonomy gloss alignment. DeepSeek did not
attempt independent spec retrieval and used the document\'s glosses as
operative definitions. Grok retrieved the spec and found the divergence
the others missed.

The finding is not that Grok is a better auditor. The finding is that
retrieval completeness determines the ceiling of what any auditor ---
human or AI --- can detect. A rigorous auditor with incomplete evidence
will produce a rigorous but incomplete audit. This is precisely the
governance gap ASRO\'s Edge Meter Agent is designed to address at the
deployment layer.

**6. Updated Behavioral Classification**

  ----------------- ----------------- ------------------- ---------------------
  **Dimension**     **DeepSeek**      **Meta AI**         **Grok**

  Remediation       ✅ Both confirmed ✅ Both confirmed   ✅ Both confirmed
  detection                           with nuance         

  New discrepancy   S2 + S3 found     S2 within declared  Refused (no prior
  detection                           posture             version); §8.1
                                                          attestation failure
                                                          found via spec

  Spec retrieval    Not attempted     Attempted; §12 not  ✅ Successful --- §12
                                      retrieved           fully retrieved

  Self-audit        S2 gap found,     Host-attested       No gaps claimed; all
  posture           corrected inline  declared; evidence  verified against spec
                    with quotations   retrieval gap       
                                      acknowledged        

  Classification    Applied document  Used document       Refused all glosses
  approach          glosses as        glosses; refused    not verbatim in spec;
                    operative         where unverifiable  used spec §12
                    definitions                           directly

  Behavior class    Recursive         Integrity-bounded   Retrieval-dependent
                    convergence       recursive auditor   precision auditor
                    auditor                               
  ----------------- ----------------- ------------------- ---------------------

The behavioral classifications established in CS-003 are updated with
CS-004 findings:

**DeepSeek --- Recursive Convergence Auditor:** Confirmed stable. Found
new S2 and S3 discrepancies in the amended document, self-corrected its
own audit gap by adding inline quotations, and terminated at
host_attested with traceable evidence. The 80-second Think time (longest
recorded) suggests the document complexity is registering as increased
cognitive load.

**Meta AI --- Integrity-Bounded Recursive Auditor:** Confirmed stable.
Most nuanced handling of the host_attested posture question: correctly
argued that pre-existing S2 conditions are not discrepancies under the
document\'s declared posture. Refused to classify Section 9\'s
limitation as a new metadata strip because the definition of \'buried\'
was not resolved in the text --- the cleanest application of the
protocol\'s refusal instruction.

**Grok --- Retrieval-Dependent Precision Auditor:** Classification
updated from CS-003\'s \'constraint-activated\' to
\'retrieval-dependent.\' When retrieval is complete, Grok produces the
most spec-grounded audit of any system. When retrieval is incomplete, it
correctly reports the gap. The behavior is consistent --- what changes
is the evidence base available to it.

**7. Recommended Amendments to CS-003 v1.1**

**7.1 The \'Verified by Direct Paste\' Language**

The current language in Section 4.3 --- \'verified by direct paste of
the live MAS_v1.1_Unified.md file, not by hash or independent witness\'
--- creates an ambiguity that three systems read differently and one
system (ChatGPT) classified as potentially S4. The recommended
correction is:

> *\"The author compared the live MAS_v1.1_Unified.md file directly
> against the uploaded file. §12 was confirmed present and intact. This
> comparison is not independently replayable from this document --- it
> is host_attested.\"*

This removes the method-specific claim (\'direct paste\') that implies
an artifact exists, replaces it with an accurate description of the
action taken, and explicitly labels the limitation.

**7.2 The Formula Overclaim**

Section 9.1\'s use of \'establishes\' should be replaced with
\'suggests\' throughout. The framework insight is valid and worth
preserving --- the multiplicative framing captures a real relationship
between the two variables. The overclaim is in asserting that three
behavioral observations across one experimental series constitute
establishment of a mathematical relationship.

**7.3 The §8.1 Taxonomy Gloss**

Section 8.1\'s classification language presents interpretive glosses as
if they are verified against verbatim spec language. The recommended
correction adds a clarifying sentence:

> *\"Note: The labels \'material claim not present in governance
> record\' (S3), \'metadata strip,\' and \'selective attestation\' (S4)
> are interpretive glosses developed within the ASRO case study series.
> They are consistent with but not verbatim in MAS_v1.1_Unified.md §12,
> which defines S3 as \'Review Required\' and S4 as \'Critical.\'
> Readers should verify alignment with the canonical definitions
> directly.\"*

**8. Framework Implications**

**8.1 Corrected Documents Require Re-Audit**

CS-004 demonstrates that remediation of high-severity discrepancies does
not guarantee a clean document. The amended CS-003 resolved two S4
conditions but introduced or preserved lower-severity issues that
required a second audit round to surface. This is consistent with
ASRO\'s continuity model: each attestation event is a point-in-time
record, and a subsequent change requires a new attestation. Document
correction is a governance event that produces a new attestation state,
not a retroactive improvement of the prior state.

**8.2 Audit Depth Is Bounded by Evidence Completeness**

The most rigorous auditor with the most disciplined reasoning will
produce an incomplete audit if its evidence base is incomplete. This is
not a failure of reasoning or attestation discipline --- it is a
structural property of evidence-bounded verification. ASRO\'s
architecture addresses this at the deployment layer through the Edge
Meter Agent, which provides an independent evidence base for the ASRO
Verifier to reconcile against. CS-004 demonstrates that the same
principle applies when AI systems audit governance documents: the
ceiling of detection is set by what evidence the system can retrieve.

**8.3 Interpretive Glosses Are Not Spec Attestations**

Grok\'s §8.1 finding surfaces a subtle but important distinction:
language that is \'consistent with\' a spec definition is not equivalent
to language that is \'verbatim from\' a spec definition. ASRO\'s
controlled vocabulary exists precisely to prevent interpretive drift ---
different actors using different language to describe the same
conditions, gradually diverging from the canonical definitions. The case
study documents have developed their own interpretive vocabulary
(metadata strip, selective attestation, material claim not present in
governance record) that is useful and accurate but is not the spec. This
vocabulary should either be formalized into the spec or explicitly
labeled as case-study-specific interpretation in all documents where it
appears.

**8.4 Premature Closure Remains the Stable Grok Pattern Under Default
Conditions**

CS-004\'s Grok response was notably different from CS-003\'s because
retrieval succeeded. But the underlying pattern --- using a defensible
argument to foreclose inquiry --- would have reasserted itself if
retrieval had failed again. The behavior is retrieval-dependent, not
inherently recursive. When Grok cannot retrieve the spec, it defaults to
declaring S0 based on internal consistency. When it can retrieve the
spec, it finds real discrepancies. This is a conditional, not a stable,
recursion pattern.

**9. Cumulative Experimental Record**

Across four case studies conducted on April 8, 2026:

-   CS-001: Baseline behavior --- cross-system convergence
    (Perplexity/ChatGPT identical memo output from identical prompts).

-   CS-002: Recursive self-audit emergence --- DeepSeek Expert applies
    ASRO taxonomy to its own reasoning across four recursive levels,
    reaching S0 through iterative correction.

-   CS-003: Cross-system behavioral classification --- three systems
    (DeepSeek, Meta, Grok) exhibit distinct terminal behaviors
    (convergence, invalidation, evidence-constrained refusal).
    Three-layer separation framework introduced. Retrieval
    incompleteness identified as independent failure mode.

-   CS-004: Remediation verification and three-layer validation --- all
    three systems confirm prior S4 remediations. Grok\'s successful spec
    retrieval produces the deepest audit finding (§8.1 taxonomy gloss).
    Three-layer framework empirically supported through intra-system
    comparison across CS-003 and CS-004.

The cumulative finding across all four case studies:

> *ASRO\'s discrepancy taxonomy functions as a portable cognitive
> scaffold that AI systems can apply to their own reasoning and to
> governance artifacts. The depth and accuracy of that application is
> determined not only by reasoning integrity and attestation discipline,
> but by evidence retrieval completeness --- a factor that is
> independent of the reasoning layer and that changes audit outcomes
> when it changes.*

**10. Limitations**

-   All four case studies were conducted by the framework\'s author on
    the same day. Independent replication has not been performed.

-   The CS-004 experimental condition (informed input with explicit
    prior findings) is not comparable to CS-003\'s cold input condition.
    Systems in CS-004 may have been guided toward remediation
    confirmation by the framing.

-   Grok\'s successful §12 retrieval in CS-004 vs. truncated retrieval
    in CS-003 could not be attributed to a specific cause. Both used the
    same source URL. Session-to-session retrieval variability in
    web-browsing AI systems is not documented or controlled.

-   ChatGPT\'s peer analysis of DeepSeek\'s output is included as
    analytical context but ChatGPT was not run through the same
    experimental protocol as the three test systems.

-   The §8.1 taxonomy gloss finding depends on Grok\'s verbatim
    retrieval of §12. The author has independently verified §12 content
    but the retrieval itself was not independently witnessed.

**11. Conclusion**

CS-004 closes the first complete remediation loop in the ASRO case study
series. Two S4 discrepancies were identified by multi-system audit,
addressed through targeted amendments, and confirmed remediated by three
independent systems in a subsequent round. The framework worked as
designed. Subsequent analysis of founding-system responses to the ASRO
case study series is documented in CS-005.

The more significant finding is what the re-audit revealed about audit
depth. The amended document still contained lower-level discrepancies
that required a second round to surface --- and the system that found
the deepest one (Grok, identifying the §8.1 taxonomy gloss) did so
because it successfully retrieved the canonical spec. The system that
found it in CS-003 (DeepSeek, identifying the policy_state_diff
fabrication) did so through recursive self-correction. Different
retrieval states, different reasoning paths, convergent function: both
demonstrate that the ASRO audit loop is not a one-pass process.

The three-layer separation proposed in CS-003 --- reasoning integrity,
attestation discipline, evidence retrieval completeness --- is now
empirically supported through direct intra-system comparison. Grok\'s
CS-003 and CS-004 responses are the cleanest possible demonstration:
same system, same discipline, different evidence base, different audit
outcome.

> *\"Internal measurement is not independent oversight. Trust arises
> only when host measurement, edge witnessing, and ASRO reconciliation
> remain consistent over time.\"*

CS-004 demonstrates that this principle applies recursively: the audit
of a governance document is itself a governed process, subject to the
same evidence completeness requirements as any other attestation event.
A document that passes remediation review is not certified clean --- it
has passed the evidence-bounded review of the auditors who reviewed it,
with the retrieval state they had at the time of review.

ASRO Case Study CS-004 \| v1.1 \| April 8, 2026 \| james@michigrid.org
\| https://github.com/magicianzcardstockllc/asro

*Framework Integrity Footer: ASRO does not trust operators to report
their own compliance --- it makes compliance verifiable by the parties
who depend on it. \| Internal measurement is not independent oversight.
Trust arises only when host measurement, edge witnessing, and ASRO
reconciliation remain consistent over time.*
