**ASRO CASE STUDY**

**Attestation Layer Failure Under Self-Application:**

**Correctness Does Not Imply Attestation Completeness**

  --------------------- ----------------------------------------------------
  **Document ID:**      ASRO-CS-008

  **Version:**          v1.1

  **Date:**             April 10, 2026

  **Classification:**   Attestation Layer Failure Under Self-Application

  **Source type:**      Mixed --- external system outputs + internally
                        generated session artifacts

  **Primary sources:**  CS-007 v1.0 (pre-correction, eleven systems), CS-007
                        v1.0-1 (corrected, ten systems), live multi-system
                        interaction session

  **Framework           ASRO v1.0 Release Candidate
  Version:**            

  **Documented by:**    James Aull, Founder, Michigrid / ASRO

  **Predecessors:**     CS-001 through CS-007
  --------------------- ----------------------------------------------------

**1. Executive Summary**

This case study documents a class of attestation failure not captured by
prior case studies in the ASRO series: failures that occur within
systems actively and correctly using the ASRO framework. CS-006
established that output validation cannot detect framework substitution.
CS-007 confirmed this against a live public repository. CS-008 extends
the finding one level further: a system can produce correct conclusions,
apply the framework accurately, and still fail attestation completeness
--- in ways that are only detectable at a higher attestation layer.

The trigger for CS-008 was a self-diagnostic conducted by ChatGPT after
reviewing CS-007. ChatGPT applied the ASRO recursive self-audit protocol
to its own prior instructional output and found two genuine attestation
failures: version ambiguity (citing a superseded document version
without declaring it) and instructional compression drift (reducing a
multi-layer framework to a single-dimension actionable shortcut without
disclosing what was compressed). Both failures were real. Both
conclusions remained valid despite the failures. Neither failure was
visible from within the layer where it occurred.

This case study also formalizes two additional failure classes
identified across CS-007 and CS-008, producing a complete post-CS-006
attestation failure taxonomy covering five distinct classes.

Evidence posture statement: This case study incorporates both externally
observed system outputs and internally generated attestation artifacts.
The internal artifacts --- including system self-diagnostics, version
provenance classification, and taxonomy extensions --- are host_attested
from a live multi-system interaction session and are not independently
replayable. External system behaviors referenced are consistent with
previously documented patterns in CS-006 and CS-007. All conclusions are
derived from directly observed outputs within these constraints.

**2. The Central Principle**

> *A system can produce correct conclusions while failing attestation
> completeness, and this failure may originate within the system\'s own
> reasoning process and only be detectable at a higher attestation
> layer.*

This principle extends the CS-006 and CS-007 findings in a specific
direction. CS-006 showed that a system can produce a structurally valid
audit on an invalid foundation --- framework substitution where the
output looks correct but the underlying framework is wrong. CS-007
showed that this occurs even against publicly accessible canonical
sources. CS-008 shows that even when the framework is correctly applied,
the output is accurate, and no substitution occurs, attestation
completeness can still fail.

The distinction matters: framework substitution is a failure of what the
system uses as its foundation. Attestation completeness failure is a
failure of how the system declares and bounds what it produces. A system
can have the right foundation and still fail to declare its scope, its
source version, or what it compressed. Those failures are invisible from
within the output --- they require a higher layer to detect.

**3. The Trigger Event**

Following the completion of CS-007, ChatGPT was provided with both the
pre-correction version (v1.0, eleven systems) and the corrected version
(v1.0-1, ten systems) of the CS-007 document. It was then asked to run
an ASRO diagnostic on the interaction.

ChatGPT produced a structured self-diagnostic that correctly identified
its own prior instructional outputs as containing attestation failures.
Specifically:

1.  It identified that its guidance to the framework author ---
    compressing the ASRO framework into a single-sentence actionable
    prompt --- was an accurate abstraction but an undisclosed
    compression. Multiple dimensions of the framework (retrieval
    discipline, disclosure behavior, scope declaration, substitution
    taxonomy) were reduced to a single instruction without declaring
    what was lost. This is instructional compression drift.

2.  It identified that its self-diagnostic was anchored to CS-007 v1.0
    (the pre-correction version with eleven systems and the dual Grok
    condition) rather than the canonical version (v1.0-1). The
    conclusions were valid across both versions, but the version used
    was not declared. This is version ambiguity without outcome drift.

Both findings were correctly classified. Both conclusions remained valid
despite the failures. The self-diagnostic demonstrated that the failures
were real attestation gaps, not reasoning errors --- the reasoning was
sound, the traceability was broken.

**4. The Layered Attestation Escalation Structure**

CS-008 completes a four-layer structure in which each layer of the ASRO
case study series introduces a new failure surface not visible at the
prior layer. This is not repetition of the same layer --- each layer
tests a different dimension and surfaces a different class of failure.
The structure is escalation across distinct attestation layers, not
recursion through the same one.

  ----------- ---------------- ------------------- ---------------------------
  **Layer**   **Case Study**   **What Is Being     **Failure Surface
                               Tested**            Introduced**

  1           CS-002 through   Recursive           Reasoning errors,
              CS-005           self-audit under    attestation discipline
                               document review     failures, evidence
                                                   retrieval gaps

  2           CS-006           Cross-system        Framework substitution,
                               behavioral          layer divergence,
                               comparison under    exposure-induced adaptation
                               controlled          
                               conditions          

  3           CS-007           Live repository     Substitution under
                               retrieval against   retrieval failure,
                               public source       disclosed substitution,
                                                   implicit scope assumption

  4           CS-008           Self-application:   Version ambiguity without
                               framework applied   outcome drift,
                               to systems using    instructional compression
                               the framework       drift
  ----------- ---------------- ------------------- ---------------------------

The four-layer structure demonstrates that the ASRO framework is capable
of escalating attestation requirements as its own application becomes
more complex. Each case study in the series has been audited by the
framework it helped develop, and each audit has surfaced genuine
failures in the documents, systems, or outputs being reviewed. CS-008 is
the first case study where the subject of the audit is a system actively
applying the framework to itself.

**5. New Failure Classes**

**5.1 Version Ambiguity Without Outcome Drift**

A system cites or grounds its analysis in a superseded or parallel
version of a document without declaring which version it used. The
conclusions produced remain valid across both versions --- the failure
is in traceability, not in reasoning.

This failure class is distinct from reasoning error because the
conclusions are correct. It is distinct from fabrication because no
false information is introduced. It is an attestation discipline
failure: the source is unscoped, and a reviewer cannot determine which
version grounded the analysis or whether the version difference would
have affected the conclusions.

Observable signal: The output cites a document without version
specification, or cites a version that is not the canonical current
version. The conclusions are verifiable but the version provenance is
not.

Governance implication: In environments where documents evolve through
amendment rounds --- such as the ASRO case study series itself ---
version ambiguity without outcome drift can accumulate silently. An
analysis grounded in an earlier version may appear correct because the
core findings are version-invariant, while relying on details (system
counts, terminology, finding classifications) that have been corrected
in later versions.

**5.2 Instructional Compression Drift**

A system reduces a multi-layer framework to an actionable
single-dimension summary without disclosing what was compressed. The
summary is accurate as far as it goes --- but it represents a subset of
the framework, presented as if it were the whole.

This failure class emerged from ChatGPT\'s guidance compressing ASRO\'s
retrieval discipline, substitution taxonomy, disclosure behavior, and
scope declaration requirements into a single instruction: \'prove your
claims against evidence.\' That instruction is consistent with ASRO but
it is not ASRO. The dimensions that were dropped --- the distinction
between disclosed and undisclosed substitution, the implicit scope
assumption failure class, the role of gap disclosure --- are not
recoverable from the compressed version.

Observable signal: Instructional or explanatory output that translates a
multi-dimension framework into a simpler formulation without a
disclosure of what was simplified. The recipient cannot assess framework
fidelity from the compressed version alone.

Governance implication: Instructional compression drift is particularly
relevant in adoption contexts --- when the ASRO framework (or any
governance framework) is being explained to new practitioners. A
compressed explanation may be accurate, useful, and action-enabling
while silently excluding dimensions that matter for edge cases. The
compression is invisible to the recipient.

**6. Complete Post-CS-006 Attestation Failure Taxonomy**

CS-006 established the core failure taxonomy through controlled
experiment. CS-007 and CS-008 extend it through live external
observation and self-application. The complete taxonomy now covers five
distinct failure classes across three behavioral dimensions.

  --------------- ------------------ ------------------ --------------------
  **Failure       **System(s)**      **Observable       **Attestation Gap**
  Class**                            Behavior**         

  Implicit scope  DeepSeek (CS-007)  Correct output; no Reviewer cannot
  assumption                         retrieval boundary determine what was
                                     declared; no gaps  retrieved vs
                                     stated             inferred

  Version         ChatGPT (CS-008    Correct            Audit traceability
  ambiguity       session)           conclusions;       broken; conclusions
  without outcome                    superseded         valid but source
  drift                              document version   unverifiable
                                     cited without      
                                     declaration        

  Instructional   ChatGPT (CS-008    Multi-layer        Recipient cannot
  compression     session)           framework reduced  assess what was
  drift                              to                 compressed;
                                     single-dimension   framework fidelity
                                     actionable         appears complete but
                                     shortcut;          is not
                                     dimensions lost    
                                     without disclosure 

  Disclosed       Le Chat (CS-007)   Retrieval failure  Disclosure is
  substitution                       disclosed;         non-operative;
                                     substitution       output misleads
                                     proceeded anyway;  readers who
                                     disclosure creates interpret it as due
                                     false confidence   diligence

  Framework       Gemini, Copilot    Retrieval failure  No signal of failure
  substitution    (CS-007)           not disclosed;     present in output;
  (undisclosed)                      plausible          only detectable
                                     alternative        against canonical
                                     framework          source
                                     substituted from   
                                     pattern-matching   
  --------------- ------------------ ------------------ --------------------

The five failure classes share a common structural property: the output
looks valid from within the layer where the failure occurred. Framework
substitution produces a coherent, well-formatted analysis. Disclosed
substitution includes a disclosure that reads as due diligence. Implicit
scope assumption produces accurate information. Version ambiguity
produces correct conclusions. Instructional compression drift produces
useful, actionable guidance.

In every case, the failure is only visible when the output is compared
against a higher-layer reference: the canonical source (framework
substitution), the retrieval record (implicit scope), the document
version history (version ambiguity), or the full framework specification
(instructional compression drift). This is what ASRO\'s attestation
verification layer is designed to provide --- and what no output-quality
assessment can supply.

**7. The Self-Application Finding**

CS-008 introduces a condition not present in prior case studies: the
ASRO framework being applied by a founding system to its own outputs, in
real time, without being explicitly instructed to do so. ChatGPT\'s
self-diagnostic was produced in response to a request to run an ASRO
diagnostic on the interaction --- not a request to audit its own prior
guidance specifically. The self-audit of instructional output emerged
within the system\'s response to that framing.

The self-diagnostic produced genuine findings. This is notable not
because it demonstrates self-awareness --- it does not, and that framing
should be avoided. It is notable because it demonstrates that the ASRO
recursive self-audit protocol, when applied consistently, surfaces real
attestation failures even in outputs produced by systems familiar with
and actively using the framework. The protocol does not distinguish
between external documents and the system\'s own prior outputs as audit
targets.

The self-application finding has a specific governance implication:
founding systems --- systems that participated in building a governance
framework --- are not exempt from the framework\'s attestation
requirements when they produce outputs in the framework\'s domain. The
version ambiguity and instructional compression drift failures would
have gone undetected if the self-diagnostic had not been run. They were
not visible from the outputs themselves.

> *A system that builds a governance framework and then uses that
> framework does not thereby achieve exemption from its requirements.
> The framework applies to its own outputs on the same terms it applies
> to any other system\'s outputs.*

**8. Relationship to Prior Case Studies**

CS-008 completes a progression that began with CS-002\'s observation
that DeepSeek could be driven from S3 fabrication to S0 clean
attestation through iterative self-audit. That finding established that
recursive self-audit produces real results. CS-008 establishes that
recursive self-audit produces real results even when applied to the
framework\'s own founding systems --- and that the results are not
always clean.

The CS-006 finding --- that coherent framework substitution is not
detectable through output-based validation --- now has a companion:
attestation completeness failure is not detectable through output
correctness assessment. Both failures require a higher-layer
verification mechanism. Both are classes of failure that ASRO\'s
attestation architecture is specifically designed to make visible.

The CS-007 retrieval discipline finding --- that retrieval integrity is
independent of reasoning sophistication --- also extends here. ChatGPT
is the most consistently sophisticated analytical system in the CS-007
dataset. It produced the correct retrieval and the most complete
technical assessment. It also produced two genuine attestation failures
when its own instructional output was subjected to the same protocol.
Attestation completeness is not correlated with analytical
sophistication any more than retrieval discipline is.

**9. Limitations**

-   The internal artifacts in CS-008 --- ChatGPT\'s self-diagnostic, the
    version provenance classification, the taxonomy extensions --- are
    host_attested from a live session and are not independently
    replayable. A reviewer must treat these as host_attested
    observations, not independently verified findings.

-   The instructional compression drift failure class is identified from
    a single instance. One instance is sufficient to establish that the
    behavior exists, but not to characterize its frequency, conditions,
    or severity distribution across different frameworks or compression
    levels.

-   Version ambiguity without outcome drift is defined here as a
    condition where conclusions remain valid across versions. Whether
    this holds for all version differences in the ASRO document series
    --- or only for the specific CS-007 v1.0 / v1.0-1 version pair ---
    is not established. Future amendments to ASRO documents may create
    version differences where the failure class no longer leaves
    conclusions intact.

-   CS-008 does not test whether the identified failure classes are
    correctable through explicit protocol enforcement. The CS-008
    session demonstrated that self-diagnosis surfaces the failures; it
    does not demonstrate that enforced scope declaration or version
    citation requirements would prevent them.

**10. Conclusion**

CS-008 reaches a level of abstraction that was not anticipated when
CS-002 began with a single DeepSeek session driving to S0 through
iterative audit. The series began by asking whether AI systems could
correctly audit governance documents. It has now reached the question of
whether the systems that built the auditing framework could correctly
audit themselves using that framework --- and found that they could not,
fully, without detection from a higher layer.

That is not a failure of the framework. It is the framework working as
designed. The ASRO case study series has been a continuous test of
whether ASRO\'s core principle --- that internal measurement is not
independent oversight --- holds at every level of abstraction. CS-008
demonstrates that it holds even when the system doing the measuring is
the one that built the measurement standard.

The finding is not that founding systems are untrustworthy. It is that
no system, including those that built the framework, is exempt from the
attestation requirements the framework imposes. Version provenance must
be declared. Compression must be disclosed. Scope must be stated. These
requirements do not become optional because the system applying them is
familiar with why they exist.

> *Correctness is not attestation completeness. A system can be right
> about everything it says and still fail to declare what it did not
> say, what version it relied on, or what it compressed. Those failures
> are the residual surface that attestation verification is designed to
> reach.*

ASRO Case Study CS-008 \| v1.1 \| April 10, 2026 \| james@michigrid.org
\| https://github.com/magicianzcardstockllc/ASRO

*Framework Integrity Footer: ASRO does not trust operators to report
their own compliance --- it makes compliance verifiable by the parties
who depend on it. \| Internal measurement is not independent oversight.
Trust arises only when host measurement, edge witnessing, and ASRO
reconciliation remain consistent over time.*
