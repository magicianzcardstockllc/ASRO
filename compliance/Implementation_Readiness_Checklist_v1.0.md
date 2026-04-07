# ASRO Implementation Readiness Checklist v1.0
**Document Status:** v1.0 | ASRO v1.0 Release Candidate | Operator Self-Audit
**Part of:** ASRO — Attestation and System Reliability Operator Evidence Framework
**Cross-references:** `/spec/MAS_v1.1_Unified.md` | `/governance/Governance_Rationale_v1.1.md` | `/governance/Governance_Response_Protocol_v1.0.md`

---

## Purpose

This checklist is required for any deployment claiming ASRO attestation compliance. Complete it before declaring `host_attested` or `reconciled_attested` status or displaying an ASRO Trust Badge.

This checklist is a self-audit tool. It does not replace ASRO registration, key registration, or the independent verification process.

**Completing this checklist does not by itself grant ASRO compliance status.** Compliance is determined by the continuous attestation record reviewed by ASRO.

---

## Compliance Tiers

This checklist covers two compliance tiers. Confirm which tier you are claiming before completing:

| Tier | Claim | Requirements |
|------|-------|--------------|
| **Level 1 — Host Attested** | `host_attested` / "Host Attestation Active" | Items 1–8 |
| **Level 2 — Independently Verified** | `reconciled_attested` / "Independent Verification Active" | Items 1–10 |

A Level 2 claim requires meaningful edge observation to be technically possible. A system that structurally prevents edge observation cannot claim Level 2 regardless of Host Meter Agent completeness.

---

## The Checklist

### Item 1 — Host Meter Agent Deployed as Separate Process

- [ ] The Host Meter Agent runs as a separate process from the AI runtime
- [ ] The Host Meter Agent does not share credentials with the runtime or configuration manager
- [ ] The Host Meter Agent has read-only access to configuration state
- [ ] The Host Meter Agent cannot be modified by the same administrative identity that controls the runtime

**Why this matters:** A Host Meter Agent embedded inside the runtime process can be silently disabled by the runtime. Separation is the minimum structural requirement for host-side attestation credibility.

**Reference:** MAS v1.1, Section 2.2; Technical Annex, Section 3.1

---

### Item 2 — Signing Key Registered with ASRO

- [ ] A dedicated signing key has been registered with ASRO for this deployment
- [ ] The signing key is not held solely by the operator — it is registered with ASRO
- [ ] The key ID is included in every attestation bundle
- [ ] Revocation status is queryable by ASRO
- [ ] A key rotation procedure is defined and tested

**Why this matters:** If the operator holds the only copy of the signing key, they can forge or backdate bundles. External registration is required for the chain to have independent integrity.

**Reference:** MAS v1.1, Section 5.3

---

### Item 3 — All Load-Bearing Events Are Attested

- [ ] All event types listed in MAS v1.1 Section 4.1 are being captured
- [ ] No load-bearing event type is classified as non-load-bearing
- [ ] Protected action classes (tool calls, external communications, safe mode transitions, overrides, policy changes, runtime changes, permission escalations) are included
- [ ] A chain containing only heartbeats while protected actions occur has been identified as non-compliant in testing

**Why this matters:** The minimum telemetry floor is the threshold below which attestation is not evidence of governance continuity. Omitting load-bearing events while maintaining a continuous chain is the Telemetry Starvation attack pattern.

**Reference:** MAS v1.1, Sections 4.1, 4.3, 13

---

### Item 4 — Max Event Latency Declared and Bounded

- [ ] `max_event_latency_ms` has been declared in the operator's compliance profile
- [ ] The declared value falls within the applicable compliance class ceiling:
  - High-risk: ≤ 15,000 ms
  - Standard: ≤ 60,000 ms
  - Low-connectivity declared: ≤ 300,000 ms
- [ ] `oldest_unpublished_load_bearing_event_ms` is included in every heartbeat bundle
- [ ] `event_density_window_s` is included in every heartbeat bundle
- [ ] Violation handling is implemented: exceeded latency triggers degraded trust and review escalation

**Why this matters:** A system that delays publication of load-bearing events beyond its declared window is conducting the Drip Feed attack. The declared latency bound must be technically enforced, not just stated.

**Reference:** MAS v1.1, Section 10.4

---

### Item 5 — Heartbeat Protocol Active

- [ ] Heartbeat bundles are emitted at the declared interval (standard default: 60 seconds)
- [ ] Heartbeat SLA is within the applicable class (15s / 60s / 300s)
- [ ] Missed heartbeat escalation is implemented: 1 miss = warning, 2 = anomaly, 3+ with observed edge activity = review trigger
- [ ] Silence is treated as a trust-relevant condition — not a neutral state

**Why this matters:** Heartbeat enforcement is the mechanism that makes silence observable. A system that can go silent without detection creates a dark window for the Outage Abuse attack pattern.

**Reference:** MAS v1.1, Sections 10.1–10.5

---

### Item 6 — Attested Outage Procedure Implemented

- [ ] `outage.declared` event type is implemented with all required fields (start timestamp, cause class, recovery window, signing identity)
- [ ] Cause class values are from the locked controlled vocabulary
- [ ] Retroactive outage declarations (filed after silence begins) are treated as remedial, not continuity-preserving
- [ ] Edge-observer overlap detection: if edge observers record activity during a declared outage, the condition escalates to S4

**Why this matters:** Attested outage is a formal exception, not a loophole. The anti-abuse rules prevent declared outages from being used as cover for governance-relevant changes.

**Reference:** MAS v1.1, Section 11

---

### Item 7 — Continuity Chain Integrity Verified

- [ ] Sequence numbering is monotonic: `sequence_no(n)` = `sequence_no(n-1)` + 1
- [ ] Hash chaining is implemented: `previous_hash(n)` = `event_hash(n-1)`
- [ ] Canonical hash rules are implemented (UTF-8, lexicographic key sorting, signature fields excluded from pre-sign hash, ISO 8601 UTC timestamps)
- [ ] All controlled vocabulary fields use values from the locked vocabulary
- [ ] Replay resistance is implemented: stale sequence reuse rejected; duplicate detection by `event_hash`

**Why this matters:** Chain integrity is what makes the attestation record tamper-evident. A chain with gaps, collisions, or non-canonical hashing is not a trustworthy record.

**Reference:** MAS v1.1, Sections 8, 9

---

### Item 8 — ASRO Backbone Connection Active

- [ ] Attestation bundles are transmitted to the ASRO backbone within the declared `max_event_latency_ms` window
- [ ] The ASRO-registered signing key is used for all transmissions
- [ ] Bundle transmission failures are handled — retries are implemented and gaps are detectable
- [ ] The deployment is registered in the ASRO registry with declared mission hash, policy hash, tool permission hash, and signing key

**Why this matters:** If bundles are stored locally but not transmitted to ASRO, the attestation chain has no external integrity guarantee. ASRO cannot verify what it does not receive.

**Reference:** Technical Annex, Sections 5.1–5.3; MAS v1.1, Section 2.1

---

### Item 9 — Edge Observation Technically Possible
*(Required for Level 2 / Independent Verification Active claim)*

- [ ] Attestation metadata and headers are exposed in API responses in a form that Edge Meter Agents can observe
- [ ] Edge Meter Agents are not contractually or technically prohibited from operating with this system
- [ ] The visibility class has been declared: `reconciled_attested` (full edge observation) or `host_attested` (metadata-only observation)
- [ ] `visibility_class: "metadata_only"` is declared if edge response hash is not available
- [ ] No technical measures strip attestation metadata before it reaches the user device

**Why this matters:** A system that structurally prevents edge observation cannot claim Independent Verification Active status. Claiming Level 2 while blocking edge observation is a Framework Credibility Attack.

**Reference:** MAS v1.1, Section 6.2; Threat Model, Section 5.4

---

### Item 10 — Trust Status Record and Badge Implementation Correct
*(Required for Level 2 / badge display)*

- [ ] A signed Trust Status Record is generated and kept current
- [ ] The Trust Status Record is exposed via the `/.well-known/asro-status` endpoint (signed by ASRO)
- [ ] Badge status changes are automatic: `ACTIVITY_DURING_SILENT_WINDOW` triggers immediate downgrade to "Attestation Under Review"
- [ ] Badge language is limited to attestation status only — no safety, quality, or legal approval claims
- [ ] A system in non-compliant status cannot display any active attestation badge

**Why this matters:** A badge without a machine-verifiable backing is governance theater. The badge must reflect the ASRO-signed status record at all times.

**Reference:** MAS v1.1, Section 16; Trust Badge Spec, all sections

---

## Pre-Submission Declaration

Before claiming ASRO compliance or displaying an ASRO Trust Badge, the operator's designated compliance function **must** confirm:

> We have reviewed the ASRO Implementation Readiness Checklist v1.0. All applicable items for our claimed compliance tier have been implemented and verified. We understand that ASRO compliance is determined by continuous attestation review, not by this self-audit alone. We acknowledge that ASRO attestation provides evidence that supports compliance demonstration and governance review; it does not itself determine legal compliance.

---

## Known Limitations of This Checklist

1. This checklist covers implementation readiness, not operational continuity. A deployment that passes this checklist can still enter non-compliant status if attestation continuity fails after deployment.

2. This checklist does not substitute for the ASRO registration process, key registration, or partner auditor review.

3. Completing this checklist does not create any legal safe harbor or compliance guarantee. ASRO attestation supports compliance demonstration and governance review — it does not guarantee legal compliance.

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
