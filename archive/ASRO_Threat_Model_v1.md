# ASRO Threat Model & Adversarial Scenarios
*Version 1.0 | March 2026 | Prepared for Phase 3.5 Spec Development*

---

## Purpose and Scope

This document serves two functions. First, it enumerates the attack surface of the ASRO attestation system — the ways a bad actor, a non-compliant participant, or a structurally flawed design could undermine the integrity claims ASRO makes. Second, and more importantly, it establishes that those claims survive adversarial pressure.

The ASRO framework's legal stress test argument depends on this document. The claim that ASRO provides a defensible posture in litigation is only credible if the adversarial model has been explicitly worked through. This is not a list of theoretical vulnerabilities compiled for academic completeness. It is the document an opposing counsel would use in deposition — and the document ASRO must be able to answer.

---

## Foundational Constraint: What ASRO Can and Cannot Prove

**ASRO proves:**
- That a change event was cryptographically signed at a specific time
- That the signed record has not been altered since submission
- That the chain of records has no detectable gaps
- That aggregate behavioral metrics changed or did not change in association with a logged event

**ASRO does not prove:**
- That all change events were submitted (omission is undetectable)
- That reported behavioral metrics accurately reflect actual behavior
- That event type classifications are accurate
- That the internal audit backbone is complete or accurate

ASRO proves the integrity of what was submitted. Independent auditors must periodically verify that what was submitted faithfully represents what occurred.

---

## Why Traditional Auditing Fails: The Probabilistic Replay Limitation

Modern AI systems are non-deterministic. Given identical input at two different points in time, the same system will not reliably produce identical output. Retroactive auditing through replay is therefore not viable. You cannot verify what a system was doing on a specific date by running the same prompts again.

ASRO's attestation-at-change-time model is the only verification approach that works within this constraint. Signed proof at the moment of change provides the maximum verifiability available — not a compromise on full transparency, but the ceiling of what is technically achievable. This is ASRO's affirmative case, not a caveat.

---

## Attack Vector Taxonomy

### Category 1: Participating System (Lab) Attacks

**1. Event omission**
- *Mechanism:* Lab fails to log a significant change event.
- *Detection:* None. Hash chain continuity only verifies submitted records. ASRO cannot detect what was never submitted.
- *Mitigation:* Spot audits. Auditors with internal access verify submitted records against internal change logs.
- *Residual risk:* Collusion between lab and auditor.

**2. Event mislabeling**
- *Mechanism:* Change logged with wrong event type (e.g., safety policy change labeled as data refresh).
- *Detection:* Partial. Behavioral delta anomalies may flag mismatches where metrics diverge from stated event type.
- *Mitigation:* Cross-system consistency checks. Auditors verify classification accuracy in spot audits.

**3. State fingerprint manipulation**
- *Mechanism:* Hash submitted does not correspond to actual deployed system state.
- *Detection:* None at submission time. ASRO verifies chain, not content hashed.
- *Mitigation:* Quarterly backbone attestation. Independent auditors verify fingerprints match the internal audit backbone.
- *Note:* This is the most technically serious attack vector within the ASRO layer itself.

**4. Behavioral metric falsification**
- *Mechanism:* Reported aggregate metrics do not reflect actual system behavior.
- *Detection:* Partial. Cross-system consistency checks compare behavioral patterns across participating systems.
- *Mitigation:* Independent behavioral evaluation by third parties.

**5. Retroactive log construction**
- *Mechanism:* Clean-looking change log built after the fact rather than logged in real time.
- *Detection:* Partial. Daily Merkle root anchoring limits retroactive construction to the most recent 24-hour window.
- *Mitigation:* Public timestamp anchoring to external services.
- *Residual risk:* Within-day window remains.

---

### Category 2: ASRO Operator Attacks

These attacks become structurally possible if ASRO is captured. The independence requirement exists specifically to prevent this category.

**1. Selective publication**
- *Mechanism:* ASRO operator suppresses integrity signals for a specific participating system.
- *Prevention:* Independent governance. No single company controls publication decisions.
- *Residual risk:* Governance capture if board is dominated by one company's interests.

**2. Audit timing manipulation**
- *Mechanism:* Spot audits scheduled to systematically avoid catching non-compliance.
- *Prevention:* Randomized spot audit protocol; audits not announced in advance.
- *Residual risk:* Insider knowledge of selection criteria.

**3. Signature verification bypass**
- *Mechanism:* ASRO operator approves records without verifying signatures.
- *Prevention:* Technical enforcement; verification is automated.
- *Residual risk:* Software vulnerability requiring code audit.

---

### Category 3: External / Systemic Attacks

**1. Coordinated omission**
- *Mechanism:* Multiple labs agree to not log a specific category of change across all participating systems.
- *Detection:* None. ASRO has no mechanism to detect coordinated non-submission.
- *Mitigation:* Regulatory mandate. If participation is a regulatory or procurement requirement, omission creates direct legal liability. This is the strongest structural incentive against this attack.

**2. Timestamp anchor compromise**
- *Mechanism:* External timestamp service used for Merkle root anchoring is compromised or coerced.
- *Detection:* None directly.
- *Mitigation:* Use multiple independent timestamp services.

**3. Strategic incompleteness**
- *Mechanism:* Labs log all required events but a category of change (e.g., prompt routing changes) is excluded from the taxonomy.
- *Detection:* None if the category is not in scope.
- *Mitigation:* Event taxonomy must be publicly defined and versioned. Changes require a formal amendment process. This is a spec design requirement.

---

## The Deposition Test

**Q: How do we know the company didn't change the system without logging it?**

The ASRO public record alone cannot prove this. What we can show: (1) submitted records are cryptographically signed and unaltered; (2) the daily Merkle root was anchored to an external timestamp service before this proceeding; (3) spot auditors with internal access have periodically verified submitted records match the internal audit backbone. If a company omitted an event, that omission would need to have gone undetected by independent auditors across multiple quarterly review cycles.

**Q: Couldn't the company label a safety change as a routine data refresh?**

Possibly. Cross-system consistency checks flag behavioral deltas that diverge from stated event types. A significant behavioral change associated with a logged "data refresh" would be flagged as an anomaly requiring explanation. Independent behavioral evaluation by third parties is a structural requirement to address this residual risk.

**Q: What if the hash submitted doesn't correspond to the actual system state?**

Quarterly backbone attestation addresses this. Independent auditors with internal access verify that fingerprints published in the ASRO ledger correspond to what is recorded in the internal audit backbone. If the backbone shows a different system state than what was published, that discrepancy is the finding. ASRO is the external anchor; backbone attestation is the verification layer.

**Q: Couldn't multiple companies just agree to not log certain things?**

This is the hardest attack to detect through ASRO alone. The structural mitigation is legal: if ASRO participation is a condition of operating in certain markets, omission creates direct legal liability independent of ASRO's detection capability. The event taxonomy must be publicly defined and versioned. Any omission of a required category becomes a compliance failure with legal consequences.

---

## Residual Risk Summary

After mitigations, the following risks remain and should be disclosed in any formal ASRO specification or governance document:

- **Within-day retroactive construction:** 24-hour gap between Merkle anchors. Mitigation: more frequent anchoring (hourly is feasible).
- **Auditor capture:** Auditors with lab relationships may not be fully independent. Mitigation: rotation, conflict-of-interest disclosure, ASRO oversight of selection.
- **Event taxonomy gaps:** Changes outside defined taxonomy cannot be logged. Mitigation: public versioning with formal amendment process.
- **Behavioral metric gaming:** Metrics can be optimized to appear stable when behavior has changed in material ways. Mitigation: precise metric definitions, independent evaluation.
- **ASRO governance capture:** If governing body is dominated by participating labs, independence is compromised. Mitigation: hard-coded representation limits and pre-defined independence spin-out trigger.

---

*ASRO Threat Model v1.0 | james@michigrid.org | March 2026*
