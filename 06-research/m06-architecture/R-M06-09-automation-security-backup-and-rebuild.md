# R-M06-09 — Automation, Security, Backup, and Rebuild

## Research status

Research report completed on 2026-07-20.

This report completes the nine-report M06 research program. It is source material for architecture synthesis and owner option review. It does not authorize automation, scheduled monitoring, credentials, provider connections, backup implementation, restore implementation, Qdrant, parser execution, cross-account transfer, or M06 code.

All options remain unapproved until owner review, architecture synthesis, independent Claude review, correction, and explicit implementation authorization.

## Question and project boundary

What operational, security, retention, backup, restore, automation, provider-exit, and rebuild controls are required so that a future Vault and ingestion system remains account-isolated, human-governed, recoverable, and independent of disposable providers and indexes?

The report covers:

- automation authority;
- connector admission;
- credentials and secrets;
- hostile inputs and sandboxing;
- provider retention and deletion;
- account isolation;
- canonical backup and restore;
- derivative rebuild;
- disaster recovery;
- evidence and audit retention;
- deletion and legal/policy disposition;
- provider/model exit;
- operational monitoring;
- adversarial testing.

It does not design implementation code or select vendors.

---

# 1. Executive findings

## 1.1 Automation creates candidates, not accepted truth

**PROJECT RECOMMENDATION**

A future scheduled or event-driven connector may:

```text
discover source candidates
capture admitted observations
preserve exact provider responses
normalize into pending packages
report failures and cost
```

It may not:

```text
admit sources by itself
accept truth or entity identity
merge records
approve editorial use
change account policy
publish content
delete evidence
```

## 1.2 The canonical Vault must rebuild every derivative

**PROJECT RECOMMENDATION**

The recovery objective is not merely “restore the database.” A verified recovery must reconstruct:

```text
SQLite authority and audit
original artifacts
normalized document versions
account policy versions
cross-account import lineage
context manifests and accepted dispositions
then, optionally:
lexical indexes
Qdrant collections
embeddings
graph derivatives
human-readable projections
```

## 1.3 Provider exit is a design requirement

Every external provider must be replaceable without losing canonical identity, evidence, or review history. Provider IDs, hosted files, vectors, transcripts, and summaries are mappings or derivatives.

## 1.4 Backup is not proof of recovery

A backup is trusted only after disposable restoration, integrity checking, account-boundary validation, artifact reconciliation, and derivative rebuild proof.

## 1.5 Separate physical account Vaults remain the safest baseline

Each account should have separate canonical storage, backup generations, encryption identity/policy, retrieval derivatives, and restore validation. Cross-account reuse should occur only through explicit export/import packages.

---

# 2. Security and software-lifecycle findings

## 2.1 Secure development is continuous

**VERIFIED FACT**

NIST's Secure Software Development Framework recommends integrating secure practices throughout the software lifecycle to reduce vulnerabilities and their impact. NIST continues to update SSDF and DevSecOps guidance.

**PROJECT RECOMMENDATION**

M06 should require threat modeling, dependency review, secret scanning, tests, negative/failure tests, evidence binding, and independent review before implementation packages close.

## 2.2 Generative-AI systems require lifecycle risk records

**VERIFIED FACT**

NIST's Generative AI Profile emphasizes governance, measurement, management, and documentation of generative-AI risks.

**PROJECT RECOMMENDATION**

Provider/model, prompt/context, extraction, retrieval, and automated-job decisions need versioned records and evaluation evidence.

## 2.3 Qdrant snapshots are derivative recovery aids

**VERIFIED FACT**

Qdrant snapshots contain collection data/configuration for a collection/node and omit some deployment-level information such as aliases.

**INFERENCE**

Snapshots can reduce rebuild time but cannot replace canonical Vault backup and exact reconstruction instructions.

---

# 3. Automation authority model

## 3.1 Candidate job classes

```text
manual ingestion
human-triggered retrieval
scheduled feed poll
WebSub/event notification
fixed-page conditional observation
channel metadata observation
provider transcript request
normalization/reprocessing
retrieval-index rebuild
integrity verification
backup generation
restore verification
```

No job class is approved.

## 3.2 Every job needs an admitted contract

**PROJECT RECOMMENDATION**

An automation contract should bind:

```text
job type and version
account ID
source/connector admission ID
credential reference
allowed domains/endpoints
request limits
cost limits
retention rules
output location
retry policy
stop conditions
human review state
```

## 3.3 Fail-closed defaults

Stop on:

- unknown account;
- missing admission;
- credential mismatch;
- unexpected redirect/domain;
- response-type mismatch;
- budget/rate threshold;
- policy/terms uncertainty;
- parser crash or sandbox escape;
- storage/integrity failure;
- partial account-boundary verification;
- duplicate operation-key conflict.

## 3.4 No broad recurring work by default

Automation should begin as manual or human-triggered. Recurrence needs separate approval after bounded proof, cost measurement, false-positive review, and shutdown testing.

---

# 4. Connector admission and provider governance

## 4.1 Admission record

Candidate fields:

```text
provider/source name
official method and endpoints
terms/policy review date
authentication method
credential scopes
data classes transmitted
retention and deletion behavior
training-use status
rate and cost limits
geographic/contractual restrictions
known failure modes
approved accounts
approved operations
owner decision and date
review/expiry date
```

## 4.2 Technical feasibility is not permission

An API, browser capability, or accessible URL does not by itself authorize automation, copying, retention, transcription, or reuse.

## 4.3 Provider changes

Terms, pricing, models, endpoints, retention, and authentication may change. Material changes should suspend the connector until reviewed.

## 4.4 Provider output is evidence from an instrument

Record exact request/response or sufficient governed evidence, provider version, timestamps, cost, and errors. Do not call provider output canonical truth.

---

# 5. Secrets and credentials

## 5.1 No secrets in repositories or evidence

Credentials must remain outside Git, Markdown, screenshots, manifests, logs, context packets, and exported research packages.

## 5.2 Least privilege

Use separate credentials/scopes per provider and environment where practical. Read-only scopes are preferred. No publishing scopes are needed for M06 ingestion research.

## 5.3 Secret references, not secret values

Canonical configuration should store a logical secret reference and expected scope, never the credential itself.

## 5.4 Rotation and revocation

Each admitted connector needs a documented rotation/revocation path and tests that expired/revoked credentials fail without partial mutation.

## 5.5 Local environment today, stronger store later

The current local encrypted/password-manager approach may remain sufficient initially. A dedicated secret manager should be considered only when operational complexity justifies it.

---

# 6. Hostile input and processing security

## 6.1 Inputs are untrusted

Treat as hostile:

```text
HTML
PDF/Office documents
archives
images/OCR
transcripts
filenames
metadata
JSON/API responses
feed content
cross-account packages
provider outputs
model outputs
```

## 6.2 Candidate controls

```text
size and type limits
magic-byte/type verification
path containment
archive-bomb and traversal defenses
no macro/script execution
sandboxed or isolated parsers
resource/time limits
safe temporary directories
output schema validation
malware scanning where admitted
prompt-injection labeling
no secrets available to parsers
```

## 6.3 Parser compromise assumption

Canonical originals and authoritative records should remain protected even if a parser fails or is compromised. Parser outputs enter pending/reviewed derivative states.

## 6.4 Browser/rendering isolation

Rendered capture, if ever admitted, requires stronger sandboxing, no authenticated sessions by default, restricted networking, download controls, and explicit human-triggered scope.

---

# 7. Canonical backup model

## 7.1 Backup generation contents

**PROJECT RECOMMENDATION — OPTION FOR OWNER REVIEW**

Per account, a canonical backup generation should include:

```text
consistent SQLite backup
immutable originals
normalized packages
manifests and hashes
account policy versions
cross-account import/export lineage
accepted conclusions and review history
audit evidence
schema/migration and application version metadata
backup manifest
```

## 7.2 Excluded or optional derivatives

```text
Qdrant data/snapshots
embedding caches
lexical indexes
graph derivatives
rendered projections
provider-hosted file mappings
```

These may be included for speed but must remain rebuildable.

## 7.3 Encryption

Backups containing research material should be encrypted with an owner-controlled mechanism. The existing `age` research/proof provides one candidate, but packaged key management and operator workflow remain M15 scope unless pulled forward explicitly.

## 7.4 Generational and append-only semantics

Each generation should have a unique ID, immutable manifest, hashes, completion state, and no-overwrite rule. Failed generations remain visibly failed or are safely discarded before admission.

---

# 8. Restore and proof-of-recovery

## 8.1 Disposable restore required

A restore test should use a separate location and prove:

```text
database integrity and migrations
audit-chain integrity
artifact existence/hash reconciliation
no orphaned or missing canonical material
account identity and physical isolation
policy-version availability
cross-account package lineage
projection regeneration
retrieval derivative rebuild or explicit deferral
```

## 8.2 Wrong-account restore

Restoring Account A into Account B's Vault root must fail unless executing an explicit new-account clone/import procedure approved for that purpose.

## 8.3 Partial restore

The application should not open a partially restored Vault as healthy. It needs a dirty/recovery state and bounded resolution operations.

## 8.4 Recovery point transparency

Report what observation and audit events occurred after the restored generation. Do not imply full currency when restoring an older backup.

---

# 9. Derivative rebuild

## 9.1 Rebuild order

Candidate order:

```text
1. Verify canonical Vault.
2. Recreate human-readable projections.
3. Rebuild exact/lexical indexes.
4. Rebuild stable chunks.
5. Regenerate embeddings/Qdrant if admitted.
6. Regenerate graph derivatives if admitted.
7. Run retrieval evaluation fixtures.
8. Mark derivatives available only after validation.
```

## 9.2 Rebuild campaign evidence

Record versions, counts, exclusions, errors, hashes, timings, account scope, and benchmark results.

## 9.3 Provider exit rebuild

Switching providers/models should create new derivatives and mappings. Old provider identifiers remain historical metadata, not current authority.

---

# 10. Retention, deletion, and correction

## 10.1 Different data classes need different rules

```text
canonical source observations
licensed/restricted artifacts
provider responses
model outputs
context-run manifests
logs
temporary parser files
Qdrant/vector derivatives
backups
```

## 10.2 Deletion is governed

Deletion may be required by policy, license, privacy, owner decision, or provider terms. It should create a disposition/audit record and propagate to derivatives and future backups according to approved rules.

## 10.3 Backup tension

Deleted material may persist in older encrypted backups. The synthesis must decide retention windows, key destruction, legal hold, and restoration filtering. No universal answer is assumed.

## 10.4 Corrections do not erase history

Corrections and supersessions should remain auditable. Retrieval and context must exclude or label superseded material appropriately.

---

# 11. Operational observability

Measure without exposing source contents or secrets:

```text
job starts/completions/failures
account/source/connector IDs
request and item counts
cost and rate-limit use
bytes and duration
parser warnings
pending review counts
backup/restore status
rebuild counts and errors
```

Logs should use canonical IDs and redaction, not full sensitive content.

Alerts may report actionable failures but must not autonomously repair authority or delete evidence.

---

# 12. Security and recovery tests

A future adversarial matrix should include:

- path traversal and symlink escape;
- archive bombs and oversized files;
- malformed PDF/HTML/JSON/feed/transcript;
- prompt injection in every source type;
- credentials in source text and logs;
- wrong-account access/import/restore;
- provider redirect to unapproved domain;
- rate/cost ceiling;
- interrupted acquisition/normalization/backup/rebuild;
- duplicate idempotency conflict;
- tampered backup manifest;
- missing/orphaned artifacts;
- wrong encryption identity;
- stale Qdrant/lexical index;
- compromised embedding model/version;
- partial provider deletion;
- fabricated success evidence;
- empty execution lists and detector errors;
- restoration followed by exact audit reconciliation.

---

# 13. Candidate architecture options

## S1 — Manual-only M06 with local canonical backup

No automated connectors. Human-triggered ingestion and existing backup patterns only.

**Advantages:** smallest attack surface.

**Risks:** more operator work.

## S2 — Manual-first plus human-triggered admitted connectors

Adds bounded one-shot official API/provider operations after separate admission.

**Advantages:** useful automation without recurrence.

**Risks:** credential/provider governance begins.

## S3 — Scheduled source monitoring in M06

**Advantages:** timely discovery.

**Risks:** larger operational, cost, policy, and failure surface; premature before manual workflows are proven.

## S4 — Provider-centric Vault/retrieval backup

Rely heavily on SaaS/provider storage and export.

**Risks:** lock-in, incomplete provenance, deletion and rebuild uncertainty. Unsuitable as canonical design.

## S5 — Staged operational model

```text
manual-first canonical Vault
→ verified per-account backup/restore
→ human-triggered admitted connectors
→ measured bounded recurring pilot later
→ disposable retrieval/graph rebuild
```

**Research-leading option — NOT APPROVED:** S5.

---

# 14. Recommended direction for synthesis

The synthesis should consider:

```text
manual-first M06
separate physical account Vaults and backup generations
canonical backup independent of providers and Qdrant
verified disposable restore before closure
connectors admitted individually
human-triggered before recurring
least-privilege credentials outside repositories
hostile-input sandboxing and limits
rebuildable lexical/vector/graph derivatives
explicit retention/deletion policies
provider-exit procedures
```

Recurring monitoring should remain out of the first M06 implementation unless the owner explicitly pulls forward one tightly bounded pilot.

---

# 15. Open questions for owner approval and Claude review

1. Is S5 the accepted staged operational direction?
2. Should M06 include any connector implementation, or only manual ingestion?
3. Should one human-triggered official metadata lookup be included?
4. Should recurring monitoring remain deferred?
5. What backup frequency and retention apply per account?
6. Is `age` accepted for canonical encrypted backup, and when is key management implemented?
7. Which derivatives are included in backups versus rebuilt?
8. What recovery-time expectation is reasonable?
9. What deletion behavior applies to old backups?
10. Which source classes may contain personal/private data?
11. Are provider/model outputs retained permanently?
12. Which providers may receive full artifacts versus excerpts?
13. What sandbox mechanism is required for hostile parsers?
14. Is malware scanning required initially?
15. Which job failures notify the owner immediately?
16. What exact evidence closes a connector admission proof?
17. How often are provider terms and retention re-reviewed?
18. How is provider shutdown or account loss tested?
19. Must every account use a separate encryption identity?
20. Which hammer tests are mandatory before M06 closes?

---

# 16. Primary sources and access dates

Accessed 2026-07-20.

- NIST — Secure Software Development Framework 1.1
  https://www.nist.gov/publications/secure-software-development-framework-ssdf-version-11-recommendations-mitigating-risk

- NIST CSRC — Secure Software Development Framework
  https://csrc.nist.gov/Projects/ssdf

- NIST — 2026 DevSecOps Practices
  https://pages.nist.gov/nccoe-devsecops/

- NIST — AI Risk Management Framework: Generative AI Profile
  https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence

- Qdrant — Snapshots
  https://qdrant.tech/documentation/operations/snapshots/

- Qdrant — Security
  https://qdrant.tech/documentation/guides/security/

- OWASP GenAI — Prompt Injection
  https://genai.owasp.org/llmrisk/llm01-prompt-injection/

- OWASP GenAI — Vector and Embedding Weaknesses
  https://genai.owasp.org/llmrisk/llm082025-vector-and-embedding-weaknesses/

- Model Context Protocol — Security and authorization
  https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization

---

# 17. Research conclusion

The safest initial M06 operating model is manual-first, per-account, locally canonical, encrypted and verifiably recoverable, with every provider and automation path admitted separately.

The research-leading sequence is:

```text
manual workflow
→ canonical backup/restore proof
→ human-triggered connector proof
→ measured recurring pilot later
→ rebuildable retrieval and graph derivatives
```

No option is approved until owner synthesis and Claude review are complete.
