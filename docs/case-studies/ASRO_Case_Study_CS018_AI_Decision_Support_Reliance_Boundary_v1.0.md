# ASRO Case Study CS-018
## AI-Assisted Decision Support and Reliance Boundary Failure

**Case ID:** CS-018
**Authority:** James Aull / MagicianzCardstock LLC
**Date:** June 2026
**Status:** Published — Phase 5 / Role Evidence and Institutional Boundary
**Classification:** Structural failure pattern — role-evidence gap in AI-assisted consequential decisions
**Framework Version:** ASRO v1.0 Release Candidate
**Primary domain:** Healthcare / clinical decision support (with financial, legal, and public-sector analogues)
**Evidence Status:** Based on publicly documented regulatory guidance, peer-reviewed research, real-world deployment studies, and liability reporting. No single live ASRO capture. Structural pattern derived from documented sources.

---

## 1. The Core ASRO Question

When an AI system produces output that influences a consequential human or institutional decision, what independent evidence shows whether the AI was treated as:

- **Information** — optional context the decision-maker considered and weighed independently
- **Recommendation** — a default the decision-maker followed unless they had reason to override
- **Triage** — an output that sorted or prioritized options before human judgment engaged
- **Authority** — an output the decision-maker deferred to without independent review
- **Required checkpoint** — a workflow step the decision-maker passed through without genuine evaluation
- **Automated trigger** — an output that initiated a downstream action without human interception

These are not the same. The accountability chain, the liability exposure, the regulatory classification, and the independently reviewable evidence required all differ across categories.

The failure documented in CS-018 is not that AI decision support is unreliable. It is that when a consequential decision is made, the record often cannot prove which of these roles the AI actually occupied at the moment of consequence.

---

## 2. Incident and Regulatory Context

AI-assisted decision support is now embedded in clinical workflows, insurance prior authorization, financial advice, and public-sector triage at operational scale.

**FDA Clinical Decision Support Software Guidance (2026):**
The FDA's current CDS guidance distinguishes non-device decision support functions — excluded from medical device regulation — from software functions that still meet the device definition. The distinction turns on whether the software "is intended to support or provide recommendations to a health care professional about prevention, diagnosis, or treatment of a disease or condition" in a way the clinician "can independently review the basis for the recommendations."

The regulatory question is whether the clinician can independently review the basis. But whether a clinician actually reviewed the basis — what they saw, whether they accepted or overrode the recommendation, and under what conditions — is not independently captured in current deployment evidence records.

**HHS Section 1557 / Patient Care Decision Support Tools:**
HHS Section 1557 now directly addresses patient care decision support tools. Covered entities must not discriminate through those tools, must make reasonable efforts to identify tools using variables that measure race, color, national origin, sex, age, or disability, and must mitigate identified discrimination risk.

The governance implication: when a patient-care decision tool is used, what evidence shows which variables, mitigation posture, oversight condition, and review state were active at the time of the decision?

**EU AI Act / High-Risk Classification:**
AI systems are classified as high-risk under the EU AI Act if they are safety components or fall into Annex III categories. Clinical decision support tools face scrutiny under this framework. What counts as a safety component versus a non-safety tool — and what evidence demonstrates the distinction at the time of deployment — is an open question the Act does not resolve for individual deployment events.

---

## 3. Positive Case — AI Decision Support Can Work

AI decision support can produce better outcomes. A real-world study of LLM-based clinical decision support across 39,849 patient visits in 15 primary care clinics (Penda Health, 2025) found fewer diagnostic and treatment errors for clinicians with access to AI Consult, while describing the tool as workflow-aligned and preserving clinician autonomy.

This is the appropriate baseline: AI decision support improves performance when it is well-integrated, clinician autonomy is preserved, and the system functions as intended.

CS-018 does not challenge that finding. CS-018 asks the governance question that remains even when the system works: what independently reviewable evidence shows the AI's role, the clinician's reliance posture, the override pathway, and the active governance conditions at the time of each consequential decision?

A good outcome is not evidence that governed conditions existed. It is a good outcome.

---

## 4. Observed Failure Pattern — The Role-Evidence Gap

**The role-label problem:**

AI decision support systems are described in policy as "support," "triage," "safety net," "recommendation," "summary," "advisor," or "administrative aid." These labels exist in governance documents, training materials, and regulatory filings. They do not exist in the evidence record at the time of each individual decision.

When a consequential decision is reviewed — by a regulator, auditor, court, or affected party — the available evidence typically shows:

- What the AI output said
- What the final decision was
- Possibly what the clinician or reviewer recorded as rationale

It typically does not show:

- What role the AI output was declared to hold in that workflow step
- Whether the decision-maker saw a disclosure about the AI's limitations or role
- Whether the decision-maker independently reviewed the basis for the recommendation
- Whether the decision-maker accepted, modified, overrode, or ignored the AI output
- What authority posture and oversight condition were active at the time

**The time-critical collapse:**

In emergency and time-critical settings, the distinction between information, recommendation, and authority can collapse under operational pressure. A 2025 study on AI-enabled decision support for traumatic injury found that providers were more likely to make correct decisions with AI information and recommendations, but also identified an accuracy-time tradeoff and polarized provider perceptions of AI recommendations.

In those contexts, the role boundary between "the AI informed the clinician" and "the clinician acted on the AI's recommendation under time pressure without independent verification" may be invisible from the final record.

**The liability sink problem:**

The Medical Protection Society warned in 2025-26 that doctors and NHS institutions could be sued for medical negligence over AI diagnostic or treatment errors, and that clinicians could become the liability sink for mistakes made by AI unless liability frameworks are updated. The Guardian reported on this warning in 2025.

The liability-sink risk is an evidence problem: when AI output influences a consequential clinical decision that later causes harm, the institutional record often cannot distinguish between "the clinician relied on the AI" and "the clinician independently reviewed and agreed with the AI's output." Both look the same in the final decision record. The distinction is critical for accountability.

**The medication risk:**

A 2026 paper on AI-assisted medication decision systems identified failure modes including missed drug interactions, incorrect risk flagging, and inappropriate dosage recommendations. The paper warns that "even a single incorrect recommendation can cause severe patient harm."

In medication decision support contexts, the role-evidence gap is acute: was the AI surfacing information for the prescriber's independent judgment, or did its output influence dosage selection, drug-interaction handling, or treatment sequencing in ways that made the AI's role operationally authoritative even when it was nominally advisory?

---

## 5. What the System / Clinician / Institution Could Prove

**Without an independent governed-state record:**

- The AI output that was delivered (if logs were preserved)
- The final clinical decision
- The clinician's recorded rationale (if any)
- The policy description of the AI system's role
- The system's own account of its configuration

**What typically cannot be proved:**

- What role the AI output held in the specific workflow step at the time of the decision
- Whether the decision-maker saw a disclosure about the AI's limitations or regulatory classification
- Whether the decision-maker independently reviewed the basis for the recommendation or accepted it as authoritative
- What model version, fine-tuning, and domain configuration were active
- Whether the oversight condition was met — whether a qualified reviewer actually engaged independently
- What authority posture was declared active at the consequential decision moment

---

## 6. ASRO Counterfactual

If an ASRO governed-state witness record existed at the time of each consequential AI-assisted decision, the following would be independently reviewable:

**What ASRO would preserve:**

| Field | Content |
|---|---|
| `ai_role_declaration` | Support / recommendation / triage / advisory / automated trigger / required checkpoint |
| `disclosure_posture` | What the user/clinician/reviewer was told about the AI role and limitations |
| `model_state` | Declared model, version, fine-tuning, domain configuration, safety posture |
| `input_boundary` | What patient/customer facts were provided to the AI |
| `output_hash` | Hash of AI output committed at delivery |
| `output_role_at_delivery` | The declared role of the output at the moment of delivery |
| `reliance_signal` | Whether the human accepted, modified, rejected, ignored, or escalated the AI output |
| `oversight_condition` | Who had review authority, when, and what override power existed |
| `human_review_record` | Whether independent human review occurred and under what conditions |
| `downstream_action` | What consequential decision or action followed |
| `timestamp_utc` | Committed at delivery, independently anchored |
| `policy_hash` | Hash of the active governance policy at the time of the decision |
| `continuity_record` | Whether governed conditions remained consistent across the decision chain |
| `unresolved_dependencies` | What cannot be proved without platform/provider/clinical workflow logs |

**What ASRO would not decide:**

- Whether the AI recommendation was clinically correct
- Whether the clinician's reliance was appropriate
- Whether the institution complied with FDA, HHS Section 1557, or EU AI Act requirements
- Whether the AI system was classified correctly under regulatory frameworks
- Whether liability attaches to the AI system, the operator, or the clinician

ASRO preserves the governed-state evidence record. Regulators, courts, auditors, and institutional reviewers determine what the record means.

---

## 7. Reviewer Question

When an AI-assisted consequential decision is later reviewed, what independently reviewable record shows:

1. What role the AI output was declared to hold in the workflow step
2. Whether the decision-maker received a disclosure about the AI's limitations and role
3. Whether independent human review occurred or whether the AI output was accepted as authoritative
4. What model state, governance configuration, and oversight condition were active
5. Whether the actual role the AI played matched the declared role

**Without that record:**

The institutional account of the AI's role is retrospective, controlled by the institution, and not independently verifiable. A system can be described as "advisory" in policy while functioning as de facto authority in workflow.

**With that record:**

The AI's declared role, the disclosure posture, the human oversight condition, and the output delivered are committed at the time of each consequential decision. A later reviewer can compare what was declared against what was delivered and what oversight occurred.

---

## 8. Cross-Domain Note

CS-018 is anchored in healthcare / clinical decision support because that domain provides the strongest combination of live regulation, liability exposure, and published evidence.

The role-evidence gap applies across consequential domains:

- **Financial advice and credit decisioning:** When AI supports loan decisions, investment recommendations, or insurance underwriting, the record of whether a human reviewer independently evaluated the AI output is typically absent.
- **Prior authorization and claims review:** KFF's 2026 analysis documents that AI is now used by insurers in prior authorization workflows, with states updating protections — but the evidence record of what the AI said and what the human did with it remains underdeveloped.
- **Public-sector triage and criminal justice:** The Council on Criminal Justice framework recommends detailed decision logs, audit trails, override records with rationale, and records accessible for legal review — but notes these are aspirational, not current practice.
- **Legal advice and drafting:** See CS-019 for the legal-domain case, which has distinct privilege and work-product dimensions.

The ASRO evidence question is the same across all domains: what independently reviewable record shows the AI's role, the human's reliance posture, the disclosure condition, and the governed state at the moment the AI-assisted decision became consequential?

---

## 9. Non-Claims

ASRO does not:
- Evaluate the clinical accuracy, safety, or appropriateness of any AI recommendation
- Determine whether an AI system is correctly classified under FDA, HHS, EU AI Act, or other regulatory frameworks
- Certify that a clinician, reviewer, or institution met their duty of care
- Assign liability for AI-assisted decision errors
- Replace clinical judgment, regulatory review, or legal analysis

ASRO only:
- Preserves independently reviewable governed-state evidence of what AI system was active, what role it was declared to hold, what disclosure was active, what output was delivered, and what oversight condition existed at the moment of a consequential AI-assisted decision
- Makes that record available to regulators, courts, auditors, and institutions without depending on the institution's own retrospective account

---

## 10. Canonical Finding

> A system can be described as "decision support" in policy while functioning as practical authority in workflow.
>
> The failure is not that AI decision support exists. The failure is that consequential institutions may later be unable to prove what role the AI played when the decision was made.
>
> ASRO's role is to preserve independently reviewable evidence of that role, reliance posture, oversight condition, and governed state at the moment the AI-assisted decision became consequential.

---

## Sources

- FDA. Clinical Decision Support Software: Guidance for Industry and Food and Drug Administration Staff. 2026. https://www.fda.gov/media/109618/download
- HHS. Section 1557 Final Rule — Patient Care Decision Support Tools. 2024/2025.
- Alu, F.F. and Oluwadare, S. "An auditable and source-verified framework for clinical AI decision support: integrating retrieval-augmented generation with data provenance." Frontiers in AI, 2026. doi.org/10.3389/frai.2026.1737532
- Pencina et al. "Bridging the gap between developers and implementers in health AI." JAMA Health Forum, 2025.
- Penda Health / AI Consult. Real-world study, 39,849 patient visits, 15 primary care clinics, 2025. (LLM-based clinical decision support improving diagnostic accuracy while preserving clinician autonomy.)
- Trauma decision support study. AI-enabled decision support for traumatic injury. 2025. (Accuracy-time tradeoff and polarized provider perceptions identified.)
- AI-assisted medication decision systems. Missed drug interactions, incorrect risk flagging, dosage recommendation failures. 2026.
- Medical Protection Society / The Guardian. NHS and doctors warned of AI diagnostic negligence liability risk. 2025–2026.
- KFF. AI use in prior authorization and claims review — state policy update. 2026.
- Council on Criminal Justice. AI decision support framework — decision log and audit trail recommendations. 2025–2026.
- EU AI Act. High-risk AI system classification. Annex III. 2024.
- Zeiser, J. "Owning Decisions: AI Decision-Support and the Attributability-Gap." Science and Engineering Ethics, 2024. doi.org/10.1007/s11948-024-00485-1

---

*ASRO Case Study CS-018 — AI-Assisted Decision Support and Reliance Boundary Failure v1.0*
*James Aull / MagicianzCardstock LLC*
*June 2026*
*ASRO™ trademark application serial no. 99827630*
