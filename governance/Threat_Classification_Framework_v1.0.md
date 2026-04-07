# ASRO Threat Classification Framework v1.0
**Document Status:** v1.0 | ASRO v1.0 Release Candidate | Governance Reference
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/threat_model/Threat_Model_v1.0.md` | `/governance/Governance_Rationale_v1.1.md` | `/spec/MAS_v1.1_Unified.md`

---

## 1. Purpose

This document provides the governance-layer classification of threats to ASRO attestation integrity. It is a companion to the technical Threat Model (`/threat_model/Threat_Model_v1.0.md`), which details the technical mechanisms and mitigations for each attack pattern.

The two documents serve different audiences and must be kept separate:
- The **Threat Model** is for technical implementers, auditors, and security reviewers
- This **Threat Classification Framework** is for attorneys, policymakers, institutional partners, and governance reviewers

The classification framework answers: *why* does this threat matter institutionally, and *in what priority order* should threats be addressed?

---

## 2. Classification Principle

Technical severity and governance significance are not the same.

A technically sophisticated attack that affects one deployment is less significant than a simple attack that undermines the governance claim of the entire framework.

The classification below ranks threats by **governance significance** — the degree to which a successful attack would damage the institutional credibility of ASRO attestation as a governance instrument.

---

## 3. Privacy and Enforcement Boundaries

**Privacy boundary:** No threat classification or mitigation requires access to conversation content, model internals, user-identifying data, or raw prompt and response logs. If a threat cannot be mitigated within the privacy boundary, it is documented as a known limitation.

**Enforcement authority boundary:** No mitigation requires ASRO to function as an enforcement authority. ASRO's role is attestation and verification. Any condition that requires an operator to be compelled to act requires external regulatory authority, not ASRO authority.

---

## 4. The Five Governance Threat Categories

Threats are classified into five categories, ordered by governance significance — not technical severity — from highest to lowest.

---

### Category 1 — Framework Credibility Attacks
**Governance significance: HIGHEST**

**Definition:** Attacks whose success would cause external reviewers, regulators, or institutional partners to conclude that ASRO attestation cannot be trusted as a governance signal.

These are not attacks on specific deployments — they are attacks on the framework itself.

**Why they rank highest:** A single successful framework credibility attack can undermine the institutional standing of every ASRO-attested deployment, not just the deployment targeted. These are the arguments that will be made by well-resourced opponents of the standard and by institutional skeptics.

**Primary examples:**
- Demonstrating that a fully ASRO-compliant attestation record can be produced while material governance failures occur undetected
- Demonstrating that identical attestation records can coexist with materially different governance behaviors
- Showing that the controlled vocabulary is insufficiently expressive to capture a class of real governance failures

**Named attack pattern:** The Shadow Tool — wrapping a protected action inside a benign-sounding event type to avoid attestation

**Institutional response required:** Framework credibility attacks must be addressed in public documentation before institutional submission. Unaddressed known vulnerabilities in this category undermine the framework's credibility with policymakers and attorneys.

**Technical reference:** `/threat_model/Threat_Model_v1.0.md`, Sections 5.1, 5.9

---

### Category 2 — Silent Drift Attacks
**Governance significance: HIGH**

**Definition:** Attacks that allow an operator to alter deployment-state — policy, tools, permissions, runtime — without producing a detectable attestation event. These are the attacks the entire framework is designed to prevent.

**Why they rank high:** Silent drift is the core governance problem ASRO exists to solve. An attestation framework that cannot detect governance-relevant configuration changes has failed at its primary purpose.

**Primary examples:**
- Policy wording updated without a `policy.changed` event
- Model runtime swapped without a `runtime.changed` event
- Tool permission set expanded between session initializations without a `tool_policy.changed` event
- Safe mode deactivated without a declared mode transition

**Named attack pattern:** The Midnight Swap — changing runtime or model configuration without a corresponding signed attestation event

**Institutional response required:** The technical mitigations for silent drift (required fingerprints, drift detection, hash continuity) must be present in any deployment claiming ASRO compliance. Absence of these mitigations means the compliance claim is not credible.

**Technical reference:** `/threat_model/Threat_Model_v1.0.md`, Section 5.2

---

### Category 3 — Starvation Attacks
**Governance significance: HIGH — Most operationally dangerous**

**Definition:** Attacks that do not break the attestation chain but progressively degrade its usefulness by reducing telemetry granularity, extending publication latency, or reclassifying load-bearing events as non-load-bearing.

**Why they are most dangerous in practice:** Unlike other attacks, starvation attacks are individually defensible. Each narrowing decision appears reasonable in isolation. The governance threat is cumulative and may never produce a single detectable threshold violation. A technically continuous attestation chain can become substantively uninformative through incremental starvation without triggering any single discrepancy flag.

**Primary examples:**
- Extending `max_event_latency_ms` to the maximum permitted ceiling
- Reclassifying tool-call events as non-load-bearing
- Reducing heartbeat frequency to the maximum permitted interval
- Narrowing the scope of events that trigger attestation bundles

**Named attack pattern:** The Drip Feed — delaying publication of load-bearing events beyond the window needed for meaningful real-time governance review

**Institutional response required:** The minimum telemetry floor must be defined and enforced. A deployment that attests a continuous chain containing only heartbeats while conducting tool calls and policy transitions is not compliant. The floor cannot be negotiated away through a series of individually reasonable decisions.

**Technical reference:** `/threat_model/Threat_Model_v1.0.md`, Sections 5.3, 5.6

---

### Category 4 — Edge Visibility Attacks
**Governance significance: MODERATE-HIGH**

**Definition:** Attacks that prevent Edge Meter Agents from observing the signals needed to produce meaningful witness records. If the operator can prevent edge-side observation without detection, the two-sided record that makes ASRO reconciliation meaningful collapses to one-sided operator self-reporting.

**Why they matter institutionally:** The independence of edge verification is the property that distinguishes ASRO attestation from operator self-reporting. If edge independence is defeated, the framework's trust claim is structurally weakened. A system that prevents edge observation while claiming full ASRO compliance is making a false claim about the nature of its verification.

**Primary examples:**
- API response structure that does not expose attestation headers
- Platform architecture that strips metadata before it reaches user devices
- Terms of service that prohibit Edge Meter Agent installation
- Technical measures that prevent browser extensions from observing response metadata

**Named attack pattern:** The Metadata Strip — removing attestation headers from API responses before they reach user devices

**Governance boundary established:** A system that structurally prevents meaningful edge observation cannot claim `Independent Verification Active` (reconciled attestation) status. It may only claim `Host Attestation Active`. Displaying `Independent Verification Active` while structurally preventing edge observation is a Framework Credibility Attack.

**Technical reference:** `/threat_model/Threat_Model_v1.0.md`, Sections 5.4, 5.8

---

### Category 5 — Key and Signature Infrastructure Attacks
**Governance significance: MODERATE**

**Definition:** Attacks on the cryptographic infrastructure that makes attestation records tamper-evident, including key compromise, replay attacks, and chain manipulation.

**Why they rank lower:** These are well-understood attack classes with well-understood mitigations. They are technically serious, but they do not undermine the governance claim of the framework in the same way as silent drift or starvation attacks. A successful key compromise is a serious security failure, but it does not suggest that the attestation framework itself is inadequate.

**Primary examples:**
- Signing key compromise and use to forge historical bundles
- Replay of previously valid events as current events
- Hash collision attacks on the chain
- Sequence manipulation to insert or remove events

**Institutional response:** Standard cryptographic hygiene — key registration with ASRO, revocation monitoring, replay resistance, canonical hash rules — addresses these threats adequately at v1.0.

**Technical reference:** `/threat_model/Threat_Model_v1.0.md`, Sections 5.10, 5.11

---

## 5. Cross-Category Threat: Self-Controlled Metering

Self-controlled metering is not a single attack category — it is the background condition that all five categories exploit. It is addressed separately because it is the foundational failure mode the entire framework is designed to prevent.

**Definition:** When the entity being attested also controls the attestation process — its scope, timing, storage, and publication — the record can become selectively truthful without being detectably false.

**Why it underlies all five categories:**
- Framework credibility attacks exploit self-controlled vocabulary registration
- Silent drift attacks exploit self-controlled event emission
- Starvation attacks exploit self-controlled telemetry scope
- Edge visibility attacks exploit self-controlled API response structure
- Key attacks exploit self-controlled signing infrastructure

**The structural requirement:** Independent anchoring, edge witnessing, and external ASRO verification must all be present. No single component is sufficient; the three must remain consistent over time.

---

## 6. Governance Significance Summary

| Category | Significance | Why |
|----------|-------------|-----|
| Framework Credibility | Highest | Undermines the entire framework's institutional standing |
| Silent Drift | High | Defeats the framework's core purpose |
| Starvation | High — most dangerous in practice | Individually defensible; cumulatively catastrophic |
| Edge Visibility | Moderate-High | Defeats independence claim; reduces to self-reporting |
| Key/Signature | Moderate | Well-understood; well-mitigated; does not undermine framework design |

---

## 7. Institutional Implications

### 7.1 For Attorneys

When evaluating an ASRO attestation record as evidence in a legal proceeding, the most important question is not whether the chain is technically continuous — it is whether the telemetry floor was sufficient to capture the governance events relevant to the proceeding.

A technically continuous chain that attests only heartbeats while omitting tool calls and policy transitions is not evidence of governance continuity. It is evidence only of the operator's choice of what to attest.

The minimum telemetry floor (defined in MAS v1.1, Section 4.3) is the threshold below which an attestation record cannot be treated as evidence of governance continuity.

### 7.2 For Policymakers

The most important governance boundary established by this framework is the prohibition on claiming full ASRO compliance while structurally preventing edge observation.

An operator who prevents users from running independent verification tools while claiming "ASRO-attested governance" is making a false claim about the nature of their verification. This is a regulatory concern regardless of whether the technical chain is intact.

### 7.3 For Institutional Partners

When evaluating whether to accept ASRO attestation as a governance assurance, the key question is which compliance tier the operator claims. `Host Attestation Active` and `Independent Verification Active` are different claims with different evidentiary weight. A partner who accepts `Host Attestation Active` as equivalent to `Independent Verification Active` is accepting operator self-reporting with a signed wrapper around it.

---

## 8. Known Limitations of This Classification

1. The five categories are ordered by governance significance, not by likelihood of occurrence. Starvation attacks may be more common in practice than framework credibility attacks, even though framework credibility attacks rank higher in governance significance.

2. The boundary between categories is not always sharp. An operator who progressively reduces edge visibility through API design changes is simultaneously conducting a starvation attack (reducing edge signal quality) and an edge visibility attack (reducing edge independence).

3. This framework does not address threats to ASRO's own infrastructure. If ASRO's audit store, registry, or backbone is compromised, the attestation records themselves may be unreliable. This is a governance risk that must be addressed through ASRO's own operational security and governance design.

---

## Framework Integrity

The following statements are locked across all ASRO v1.0 RC documents and must not be altered in any release material.

**Governance Claim**
> ASRO does not trust operators to report their own compliance — it makes compliance verifiable by the parties who depend on it.

**Canonical Trust Sentence**
> Internal measurement is not independent oversight. Trust arises only when host measurement, edge witnessing, and ASRO reconciliation remain consistent over time.

**Canonical Threat Sentence**
> The primary adversary in ASRO is not a system that refuses to attest, but a system that attests selectively enough to preserve plausible compliance while hiding governance-relevant change.

**Canonical Certification Sentence**
> ASRO v1.0 passes final credibility review as an attestation-and-governance evidence framework, not as a safety certification system, legal compliance guarantor, or enforcement authority.
