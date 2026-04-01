# ASRO v1 Attestation Bundle Specification
## AI Systems Reliability Operator
*Version 1.0 — March 2026 | Status: Final Draft for Phase 3.5 Submission*

---

## 0. Purpose and Scope

ASRO v1 defines a minimal attestation protocol for recording material AI system changes in a way that is externally verifiable, cryptographically anchored, privacy-preserving, and resistant to post-hoc manipulation.

Its purpose is not to expose prompts, user data, model weights, or internal policy text. Its purpose is to prove that a qualifying change occurred, that it was classified and recorded at the time it occurred, and that the resulting record has not been altered.

**ASRO v1 defines:**
- the attestation bundle
- the event taxonomy
- operator obligations
- verification workflow
- public verification states

**ASRO v1 does not define:**
- content-level moderation
- per-user personalization logs
- model-weight disclosure
- full regulatory enforcement
- complete governance structure

---

## 1. Design Constraints

ASRO v1 is built under four non-negotiable constraints:

### 1.1 Independence
The verifier MUST not be controlled by any entity whose systems it verifies.

### 1.2 Non-Determinism
AI systems cannot be reliably audited through replay. Verification MUST occur at the time of change.

### 1.3 Minimal Disclosure
No ingestion of: user data, prompts, model weights, or proprietary policy text.

### 1.4 Operational Realism
The protocol MUST be implementable using existing logging systems, standard cryptographic signing, and minimal integration overhead.

---

## 2. Attestation Bundle

Each ASRO bundle is a single signed record representing one qualifying change event.

```
ASRO-BUNDLE-v1
  bundle_id
  timestamp_utc
  system_id
  operator_id
  event_class
  state_fingerprint
  change_summary
  impact_metrics
  policy_version
  previous_bundle_hash
  bundle_hash
  signature
  integrity_assertion
```

### 2.1 Field Definitions

| Field | Description |
|-------|-------------|
| bundle_id | Globally unique identifier |
| timestamp_utc | Time event was logged (not export time) |
| system_id | Model/service identifier |
| operator_id | Responsible signing entity |
| event_class | Classification from ASRO taxonomy |
| state_fingerprint | One-way cryptographic representation of system state |
| change_summary | Non-sensitive description of change (≤500 chars) |
| impact_metrics | Aggregated behavioral deltas (optional) |
| policy_version | Version reference |
| previous_bundle_hash | Hash of prior bundle |
| bundle_hash | Hash of current bundle |
| signature | Cryptographic signature |
| integrity_assertion | Structured operator attestation |

### 2.2 Field Constraints

- No field may contain user data, prompts, model weights, or proprietary policy text
- `state_fingerprint` MUST be one-way (SHA3-256 or BLAKE3) and MUST NOT include PII-derived inputs
- `change_summary` MUST NOT include user-identifying or reconstructive detail
- `impact_metrics` MUST be aggregated over ≥10,000 requests and non-identifiable
- `signature` MUST follow a documented key-management policy

### 2.3 integrity_assertion (Required Structure)

The `integrity_assertion` MUST be a structured, independently signable object:

```
integrity_assertion:
  operator_id
  attestation_timestamp_utc
  obligations_version
  completeness_asserted        (boolean)
  classification_asserted      (boolean)
  timeliness_asserted          (boolean)
  authorized_signer_asserted   (boolean)
  assertion_signature
```

**Requirements:**
- MUST bind operator to Section 4 obligations
- MUST be independently signed (not only covered by bundle signature)
- MUST be retained as part of canonical record
- `obligations_version` MUST reference the published operator-obligations version in force at signing

---

## 3. Event Class Taxonomy

Each bundle MUST include exactly one event class. The v1 taxonomy is intentionally compact — its purpose is reliable first-pass classification, not exhaustive semantic precision. Taxonomy disputes are resolved through public versioning, audit review, and formal amendment processes.

| Class | Scope |
|-------|-------|
| MODEL_UPDATE | Changes to weights, architecture, or inference logic |
| POLICY_UPDATE | Safety systems, moderation rules, system prompts |
| SYSTEM_CONFIG_CHANGE | Infrastructure, runtime parameters, thresholds |
| DEPLOYMENT_EVENT | Deployments, rollbacks, version transitions |
| ROUTING_CHANGE | Prompt routing, tool usage, orchestration |
| DATA_REFRESH | RAG corpora, fine-tuning datasets |
| BEHAVIORAL_TUNING | Fine-tuning, RLHF adjustments, output shaping |
| EMERGENCY_OVERRIDE | Manual overrides, incident response actions |
| EVALUATION_RUN | Behavioral benchmarks updating impact metrics |

**Taxonomy is:** publicly defined, versioned, and amendable only through formal process.

---

## 4. Operator Obligations

Participating operators do not merely submit bundles; they attest to the validity of the logging process itself. Failure to meet obligations is a protocol failure even if the submitted bundle is cryptographically valid.

### 4.1 Logging Windows

- `EMERGENCY_OVERRIDE` → MUST be logged within **1 hour**
- All other events → MUST be logged within **24 hours**

*Rationale: emergency overrides occur during high-risk operating windows and require accelerated attestation to preserve trust in the record.*

### 4.2 Core Obligations

Operators MUST:
1. Log all qualifying events (completeness)
2. Classify events correctly using the published taxonomy
3. Maintain hash-chain continuity
4. Use authorized signing keys under documented rotation policy
5. Maintain internal audit backbone cryptographically linked to `state_fingerprint`
6. Publish daily Merkle root anchors to independent timestamping service
7. Acknowledge non-determinism limits
8. Accept independent ASRO governance

---

## 5. Cryptographic Structure

### 5.1 Hash Chain

Each bundle MUST include `previous_bundle_hash` and `bundle_hash`. ASRO MUST detect: missing entries, tampering, reordering, and chain breaks.

### 5.2 External Anchoring

Operators MUST publish Merkle-root anchors to at least one independent public timestamping service.
- v1 minimum: **daily**
- Recommended: **hourly**

---

## 6. Minimum Viable Implementation (MVI)

ASRO v1 MVI proves that attestation-at-change-time is operationally viable without requiring full governance maturity or full regulatory integration.

### 6.1 Minimum Deployment

A valid v1 pilot can consist of:
- one participating operator
- one event class subset
- one authorized signing workflow
- one hash-chain continuity rule
- one external timestamp anchoring routine
- one public verification surface

### 6.2 Success Condition

A deployment is valid if a participating operator can:
1. Classify a change into an approved event class
2. Generate a compliant bundle
3. Sign the bundle with an authorized key
4. Append it to a valid hash chain
5. Anchor continuity to an external timestamp mechanism
6. Expose a public verification status for that record

---

## 7. Verification Workflow

1. Event occurs
2. Classification assigned
3. Bundle generated
4. Bundle signed
5. Bundle chained
6. Bundle submitted
7. ASRO verifies
8. ASRO publishes verification status and continuity state

---

## 8. Public Verification States

| State | Meaning |
|-------|---------|
| VERIFIED | Bundle valid, chain intact, signature confirmed |
| VERIFIED_WITH_WARNING | Valid but anomaly flagged |
| INCOMPLETE | Required fields missing |
| INVALID_SIGNATURE | Signature verification failed |
| CHAIN_BREAK | Hash-chain continuity broken |
| CLASSIFICATION_ANOMALY | Behavioral delta inconsistent with stated event class |
| PENDING_REVIEW | Under manual review |

*Public output reflects validity, continuity, and classification consistency — not internal system state, user data, or model behavior.*

---

## 9. Interoperability

ASRO is designed to sit alongside existing systems, not replace them. It integrates with internal logs, CI/CD pipelines, and audit systems.

ASRO is to AI system changes what software-signing transparency systems are to software artifacts: a verifiable integrity layer over independently produced operational events. This positions ASRO as the AI-domain counterpart to Sigstore for software artifact signing.

---

## 10. Privacy Constraints

ASRO is designed to receive no user data. Required safeguards:
- `change_summary` content controls and PII scanning at ingestion (500-char cap)
- Aggregation thresholds for `impact_metrics` (≥10,000 requests)
- SHA-3 or BLAKE3 required for all fingerprints
- `state_fingerprint` MUST be derived from model or configuration state only — not from logs, prompts, or user-derived artifacts. Fingerprint inputs MUST exclude any user-derived data sources by construction.

---

## 11. Probabilistic Replay Constraint

Modern AI systems are not reliably auditable through replay. The same prompt at a different time or system state may not produce the same output. ASRO therefore does not attempt retrospective behavioral reconstruction.

Instead, ASRO provides the strongest historical verification available in a non-deterministic environment: proof that a meaningful change was logged, classified, signed, and chained at the time it occurred.

**This is the maximum verifiability achievable in non-deterministic systems.**

---

## 12. Threat Model Alignment

ASRO v1 is explicitly designed against: event omission, misclassification, retroactive log construction, signature tampering, chain manipulation, and operator collusion.

**Known limitations (explicit):**
- Omission detection is external (audits required)
- Classification accuracy not fully enforceable without behavioral metrics
- Behavioral metrics not guaranteed truthful without independent evaluation

If `impact_metrics` are omitted, ASRO's ability to detect classification anomalies through behavioral-delta analysis is reduced. This limitation MUST be disclosed wherever anomaly detection is claimed.

---

## 13. Independence Constraint

ASRO MUST be independently governed, prevent majority control by any participant, and enforce structural separation between operator and verifier. If `operator == verifier`, system integrity collapses.

If incubated under a host organization, the transition to independent governance MUST be governed by a separately published trigger rule in the companion governance document. That trigger rule MUST define participation thresholds using objective criteria such as formal spec adoption, covered-system scale, number of participating operators, or elapsed operating period.

---

## 14. Scope Boundary

**ASRO v1 defines:** attestation structure, taxonomy, obligations, verification workflow, public signal format.

**ASRO v1 does not define:** enforcement mechanisms, regulatory authority, governance completion, content moderation, per-user personalization logging.

---

## 15. Companion Artifacts

This specification is paired with:
- Threat Model
- Privacy & Legal Stress Memo
- Public provenance record
- Outreach documentation

---

*ASRO v1 Attestation Bundle Specification | james@michigrid.org | March 2026*
