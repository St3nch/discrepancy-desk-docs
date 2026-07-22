# M06-A Local Manual Vault — Owner-Accepted Planning Baseline

## Status

```text
Status: owner-accepted planning baseline
Phase 1 implementation/evidence: complete through application commits 8fe3be4 and 5f0f9ae
Independent implementation audit: complete; verdict M06-A PHASE 1 INDEPENDENTLY VERIFIED
Phase 1 owner closure: accepted through D033
Phase 2 exact package: prepared for owner review; implementation not authorized
Later phases: blocked pending separate authorization
```

Correction cycle: closed through `08-audits/m06a-planning-correction-closure.md`.

This document is the load-bearing, owner-accepted technical planning baseline for M06-A. It implements the accepted M06 architecture and D027, with later governing clarifications and authority recorded in D029 through D033, while correcting the defects identified in the preserved independent and Claude reviews. It supersedes the preserved Claude planning report only as the controlling implementation-planning baseline; all historical reports remain preserved as audit evidence.

Governing inputs:

- `00-doctrine/editorial-anomaly-archive-direction.md`;
- `05-implementation-planning/m06-architecture-synthesis.md`;
- `05-implementation-planning/m06a-planning-authorization.md`;
- `05-implementation-planning/m06a-planning-package-outline.md`;
- `05-implementation-planning/m06a-m06b-package-boundary.md`;
- `05-implementation-planning/m06-pre-synthesis-correction-specification.md`;
- `99-decisions/m06-owner-architecture-rulings.md`;
- `08-audits/m06a-local-manual-vault-planning-package.md`;
- `08-audits/m06a-local-manual-vault-planning-package-independent-review.md`;
- `05-implementation-planning/m06a-parser-admission-plan.md`;
- `05-implementation-planning/m06a-adversarial-closure-matrix.md`;
- `05-implementation-planning/m06a-phase2-exact-implementation-package.md`;
- `08-audits/m06a-planning-correction-disposition.md`;
- `08-audits/m06a-planning-correction-closure.md`;
- `08-audits/claude-m06a-phase1-independent-implementation-review.md`;
- `08-audits/m06a-phase1-independent-review-disposition.md`;
- D027 through D033 in `99-decisions/decision-log.md`.

---

# 1. Scope, Authority, and Hard Boundaries

## 1.1 M06-A purpose

M06-A establishes a governed, local-only research-memory Vault that can:

- accept human-triggered pasted text and owner-selected local files;
- preserve exact original bytes or exact supplied text;
- record source, observation, acquisition, actor, policy, and parser provenance;
- normalize admitted formats into versioned deterministic JSON element packages;
- support typed source claims, allegations, folklore, observations, Desk inferences, relationship candidates, contradictions, unresolved questions, entities, and dossiers;
- support local SQLite full-text search, deterministic chunks, and generated read-only projections;
- preserve immutable correction and supersession lineage;
- produce canonical per-Vault backup generations and prove disposable restore;
- define static provider-neutral context-run records without executing a live model.

## 1.2 Human-only authority

Only a verified human actor may:

- admit research material when admission requires judgment;
- approve a parser-admission version;
- create a final truth/support assessment;
- create or supersede an editorial-use ruling;
- accept entity merges or splits;
- approve governed corrections requiring judgment;
- create or change account policy;
- approve any later destructive-purge operation only after a separately planned, independently reviewed, owner-authorized purge package; M06-A defines no purge operation;
- exercise the existing M00–M05 exact-content publication workflow.

System, model, and import actors may create candidates, receipts, observations, warnings, derived indexes, and reconciliation work only. They cannot promote their own output into human authority.

## 1.3 Exclusions

M06-A includes none of the following:

- URL fetching, HTTP retrieval, crawling, redirects, DNS resolution, or SSRF surface;
- feed polling, WebSub, monitoring, scheduled jobs, background ingestion, or recurring work;
- provider calls, credentials, OAuth, paid services, or transcript services;
- arbitrary media downloading;
- OCR, scanned-PDF handling, office-document parsing, or browser rendering;
- first-class event or chronology objects;
- cross-Vault import execution;
- live LLM/provider execution or dynamic Vault tools;
- Qdrant, embeddings, semantic retrieval, graph, or GraphRAG;
- autonomous truth, approval, publication, engagement, entity merge, entity split, or evidence deletion.

A YouTube URL may be stored only as inert identity/context, paired with a human-supplied transcript. It is not fetched.

## 1.4 M06-B boundary

M06-B remains a later, separately gated bounded static webpage retrieval package. M06-A may define locators and an ingestion envelope that M06-B could later consume, but M06-A must contain no network acquisition code, configuration, dependencies, permissions, or tests that silently admit M06-B.

## 1.5 Stop conditions

Planning or later implementation must stop and return to owner review if any of these occurs:

- a proposed change contradicts M06-D01 through M06-D16;
- a parser requires network or external resolver behavior that cannot be denied and proven;
- packaged SQLite lacks FTS5;
- a required invariant has no executable test mapping;
- a migration or restore state is dirty or ambiguous;
- same-Vault provenance cannot be enforced;
- rights or retention policy is missing or cannot be evaluated fail-closed;
- a design would create a second canonical truth store;
- a cross-database pointer is being treated as live authority;
- implementation would require M06-B, OCR, Qdrant, graph, live LLM, or destructive purge authority.

---

# 2. Accepted Architecture Coverage

This candidate preserves every owner-approved M06 ruling:

| Ruling | Representation in this candidate |
|---|---|
| M06-D01 | SQLite owns governed identity/workflow; filesystem owns immutable originals/packages; indexes/chunks/projections remain derived. |
| M06-D02 | All formats converge on the object and acquisition chain in Section 6. |
| M06-D03 | One physical Vault root per resolved Vault account; no cross-Vault direct sharing or import execution. |
| M06-D04 | Markdown/HTML projections are generated, source-bound, read-only, and non-authoritative. |
| M06-D05 | Research admission, truth/support, editorial use, and exact publication approval remain separate. |
| M06-D06 | Corrections, supersessions, withdrawals, merges, and splits are append-only lineage. |
| M06-D07 | Common deterministic normalized JSON element package defined in Section 8. |
| M06-D08 | Static, reconstructable context-run records only; no provider execution. |
| M06-D09 | Entity merge/split acceptance requires a verified human actor and evidence. |
| M06-D10 | Events and chronologies are absent from the first package. |
| M06-D11 | Narrow parser candidates and individual admission gates are defined in the parser plan. |
| M06-D12 | Exact FTS5, chunk identity, and invalidation contract defined in Section 17. |
| M06-D13 | Per-Vault backup, disposable restore, tamper detection, and rebuild defined in Section 20. |
| M06-D14 | Every acquisition is human-triggered; no recurrence or monitoring. |
| M06-D15 | No Qdrant or graph component is created. |
| M06-D16 | Context schema only; no live LLM/provider integration. |

---

# 3. Resolved Owner Decisions

The owner resolved OD-1 through OD-3 on 2026-07-21. These rulings constrain this candidate but do not constitute owner acceptance or implementation authority.

## 3.1 OD-1 — Vault account means editorial/public brand identity

Owner ruling:

```text
One physical Vault represents one editorial/public brand identity.
```

For this project, the Vault-level identity is:

```text
The Discrepancy Desk
```

The central `vault_accounts` row represents that editorial/public brand identity. The central `owned_accounts` rows remain platform-specific owned identities containing platform, stable external account identity, and mutable handle/username metadata. One Vault account may bind multiple platform-owned accounts through explicit governed central-database relationships, including the X account, a future Truth Social account, and later owner-admitted platform accounts.

A platform-owned account binding:

- does not create another physical Vault;
- does not move platform identity authority out of the central control-room database;
- does not grant research records publication authority;
- does not substitute for an exact revision approval, publication attempt, or publication record;
- must be active and verified before platform context is combined with Vault research.

The Vault owns research and dossier authority for The Discrepancy Desk. Exact revision approval, publication attempts, publication records, and metrics remain central authority.

## 3.2 OD-2 — One SQLite database per physical Vault

Owner ruling:

```text
central control-room SQLite database
+
one SQLite database per physical Vault
```

The central database owns platform accounts, exact revision approvals, publication records and attempts, metrics, the Vault registry, and platform-account bindings. Each physical Vault owns its own SQLite authority database, audit chain, operation-key ledger, migration state, backup-generation history, canonical research records, and dossiers.

Cross-database operations use correlation IDs, separate receipts, and reconciliation. They are never represented as atomic. The controlling design has no shared Vault-database implementation branch.

## 3.3 OD-3 — Reject timed-deletion material before canonical admission

Owner ruling:

```text
material requiring timed deletion
→ rejected before canonical admission
```

Every intake requires a retention declaration before input bytes are copied into a Vault operation workspace. The service classifies the declaration as:

```text
preservation_compatible
timed_deletion_required
unknown
```

Only `preservation_compatible` may proceed. `timed_deletion_required`, missing, contradictory, or `unknown` eligibility fails closed. The operator may supply a policy/terms reference and a human-authored classification note, but M06-A does not inspect remote terms or infer permission from silence.

A rejected intake writes only a safe `intake_rejection_receipt` in the selected Vault database containing the Vault and actor IDs, operation/correlation ID, attempted source kind, sanitized non-content filename/locator classification where safe, declared retention classification, policy-basis reference, reason code, client-generated operation nonce, and timestamp. No rejection receipt, audit event, operation key, log, or retained metadata may contain a hash derived from rejected content bytes or rejected pasted text. A cryptographic digest can confirm possession of known content and is therefore not automatically content-free. Rejection-path idempotency may be derived only from the Vault ID, trusted actor ID, retention declaration, policy-basis reference, safe source-kind classification, client-generated operation nonce, and sanitized non-content descriptor. The rejection path must not retain the supplied bytes, pasted content, extracted text, temporary copy, normalized package, content-bearing diagnostic, or content-derived digest.

Rejected material cannot enter artifact storage, parser execution, normalized packages, search, chunks, projections, dossiers, context runs, or backups. M06-A implements no automatic deletion, deadline scheduler, tombstone purge, or promise to delete later. Any later destructive purge authority requires a new bounded package, independent review, owner ruling, and explicit implementation authorization.

---

# 4. Resolved Physical Topology

The owner-resolved topology is:

```text
central control-room database
  owned_accounts                       existing platform accounts
  vault_accounts                       editorial/public Vault identity
  vault_registry                       opaque Vault ID → governed relative root
  vault_account_owned_accounts         central FK bindings
  vault_operation_receipts             cross-database orchestration receipts

configured Vault base
  <opaque-vault-directory>/
    VAULT_IDENTITY.json
    database/
      vault.sqlite3
      vault.sqlite3.migration-dirty.json   only while dirty
    objects/
      sha256/aa/bb/<full-sha256>
    packages/
      sha256/aa/bb/<package-sha256>.json
    projections/
      markdown/
      html/
    temp/
      <operation-id>/
    backups/
      <generation-id>/
```

The central database remains the authority for platform accounts and exact publication workflow. The Vault database remains the authority for research memory belonging to that Vault. Neither database treats a bare identifier from the other as a foreign key.

The configured Vault base is supplied by trusted local configuration. API requests never supply a filesystem path.

## 4.1 Quarantine terminology and storage authority

`quarantined` is an explicit governed database state over an immutable existing artifact object, normalized package, parser execution, or derived projection manifest. It is not a second filesystem authority area. Canonical bytes remain at their ordinary content-addressed object/package path, and the database state blocks current use, search, context, export, projection, dossier use, and backup eligibility until a governed human or reconciliation operation resolves the condition. No `quarantine/` canonical root exists.

Unadmitted or incomplete temporary material may use:

```text
temp/<operation-id>/temporary-quarantine/
```

Temporary intake quarantine is non-canonical, operation-scoped, excluded from backup and search, inaccessible to ordinary Vault reads, and never sufficient to create an artifact or package record. Before the operation closes it must be either admitted through the normal immutable object/package contract or destroyed after a content-free reconciliation receipt. If safe destruction or exact reconciliation cannot be proved, the operation remains blocked and the Vault reports unresolved temporary state; the temporary bytes do not become canonical authority.

---

# 5. Vault Identity, Registry, Create, and Open

## 5.1 Central registry candidate

Under the resolved topology, an additive central migration would create:

```text
vault_accounts
  id TEXT PRIMARY KEY
  display_name TEXT NOT NULL
  state TEXT NOT NULL CHECK(active, suspended, retired)
  created_at TEXT NOT NULL
  created_by_actor_id TEXT NOT NULL

vault_registry
  vault_id TEXT PRIMARY KEY REFERENCES vault_accounts(id)
  relative_root TEXT NOT NULL UNIQUE
  vault_instance_id TEXT NOT NULL UNIQUE
  expected_identity_fingerprint TEXT NOT NULL UNIQUE
  registry_state TEXT NOT NULL CHECK(registered, unavailable, dirty, retired)
  registered_at TEXT NOT NULL
  registered_by_actor_id TEXT NOT NULL

vault_account_owned_accounts
  vault_id TEXT NOT NULL REFERENCES vault_accounts(id)
  owned_account_id TEXT NOT NULL REFERENCES owned_accounts(id)
  binding_state TEXT NOT NULL CHECK(active, superseded, removed)
  bound_at TEXT NOT NULL
  bound_by_actor_id TEXT NOT NULL
  PRIMARY KEY(vault_id, owned_account_id, bound_at)
```

No legacy M00–M05 row gains a new Vault column. Publication authority remains central.

## 5.2 Database identity

Every Vault database contains exactly one row:

```text
vault_metadata
  singleton_id INTEGER PRIMARY KEY CHECK(singleton_id = 1)
  vault_account_id TEXT NOT NULL UNIQUE
  vault_instance_id TEXT NOT NULL UNIQUE
  vault_schema_name TEXT NOT NULL CHECK(vault_schema_name = 'm06a-vault')
  created_at TEXT NOT NULL
  identity_fingerprint TEXT NOT NULL UNIQUE CHECK(length(identity_fingerprint) = 64)
```

The fingerprint is SHA-256 over canonical JSON containing:

```text
schema name
vault_account_id
random vault_instance_id
created_at
```

`vault_instance_id` is a cryptographically random UUID and is never derived solely from public account fields.

## 5.3 Filesystem marker

`VAULT_IDENTITY.json` contains the exact same identity fields and canonical fingerprint. It is a defense and routing proof, not a substitute for the SQLite singleton.

## 5.4 `create_vault()`

Only an explicitly authorized create operation may create directories or a database. It must:

1. resolve the registry-proposed relative root under the configured Vault base;
2. reject absolute paths, `..`, reserved Windows names, trailing dots/spaces, case collisions, symlinks, junctions, and reparse points;
3. use an exclusive operation workspace;
4. create a random instance ID and fingerprint;
5. create and migrate the database through the Vault migration runner;
6. insert `vault_metadata`, owner actor, audit genesis, and operation receipt;
7. write the marker with create-new/no-overwrite semantics;
8. verify marker/database equality;
9. register the root centrally with a shared correlation ID;
10. reconcile or leave an explicit incomplete-create receipt if either database commit cannot be completed.

## 5.5 `open_existing_vault()`

Opening is strictly non-creating. Before any call to the current `db.connect()` behavior, it must:

- require the registered root, marker, and `database/vault.sqlite3` to exist;
- reject symlinks, junctions, and reparse points at the base, Vault root, marker, database directory, database, objects, packages, temp, and backup paths;
- read and validate the marker;
- open the database with an existing-file-only URI such as `mode=rw`;
- validate `vault_metadata`, SQLite integrity, migration dirty marker, expected head, audit chain, and fingerprint;
- compare registry, marker, and database identity exactly;
- reject copied markers, copied databases, wrong roots, wrong accounts, missing files, and case-normalization ambiguity before migration or write.

The ordinary current `connect(path)` function may remain for central database creation. Vault open/create must use dedicated functions so a typo can never create an empty database.

---

# 6. Canonical Object and Provenance Model

The canonical chain is:

```text
Vault account
→ source
→ source item
→ occurrence
→ observation
→ acquisition
→ acquisition-artifact relationship
→ artifact object
→ parser execution
→ normalized package
→ document version
→ element
→ region
→ mention
→ entity candidate
→ entity
→ assertion
→ typed evidence link
→ unresolved question / dossier
```

Policy versions, editorial-use rulings, corrections, audit events, and backup generations bind across this chain without creating a parallel truth model.

## 6.1 Required object families

### Identity and authority

```text
vault_metadata
actors
actor_status_history
operation_keys
audit_events
cross_database_operation_receipts
```

### Ingestion and immutable bytes

```text
intake_rejection_receipts
sources
source_items
occurrences
observations
acquisitions
artifact_objects
acquisition_artifact_links
artifact_policy_bindings
```

### Parsing and normalized records

```text
parser_definitions
parser_configuration_versions
parser_admission_versions
parser_executions
normalized_packages
document_versions
elements
regions
```

### Interpretation and organization

```text
mentions
entity_candidates
entities
entity_merge_proposals
entity_merge_acceptances
entity_split_proposals
entity_split_acceptances
assertions
assertion_element_evidence
assertion_observation_evidence
assertion_artifact_evidence
assertion_relationships
unresolved_questions
question_evidence_links
dossiers
dossier_source_items
dossier_observations
dossier_document_versions
dossier_elements
dossier_assertions
dossier_questions
dossier_entities
```

### Policy, correction, and use

```text
policy_versions
policy_label_definitions
rights_retention_versions
artifact_policy_bindings
editorial_use_rulings
editorial_use_ruling_evidence
policy_impact_work
source_corrections
observation_corrections
document_version_corrections
assertion_corrections
entity_corrections
dossier_corrections
```

### Derived and recovery

```text
search_documents
search_invalidation_work
vault_fts
chunks
projection_manifests
backup_generations
backup_generation_files
reconciliation_work
```

Events and chronologies are deliberately absent.

## 6.2 Same-Vault enforcement

Every canonical table includes `vault_account_id` even in a per-Vault database. This redundancy is contamination detection, not cross-Vault sharing.

SQLite enforcement uses:

- `UNIQUE(vault_account_id, id)` on parent tables;
- composite foreign keys carrying `vault_account_id` on child tables;
- `CHECK` constraints for closed vocabularies and exact-one-target rules;
- service validation against the singleton `vault_metadata` before every transaction;
- connection-scoped foreign keys enabled and independently tested.

No SQLite `CHECK` claims to inspect `VAULT_IDENTITY.json`.

## 6.3 Provenance-chain enforcement

A service operation creating a child object must load and compare its immediate parent chain inside the same transaction. The database enforces immediate composite foreign keys; the service enforces semantic continuity such as:

- occurrence belongs to the source item and source;
- observation refers to the occurrence actually observed;
- acquisition is attributable to that observation or explicit manual input event;
- acquisition-artifact link belongs to both the acquisition and artifact in the same Vault;
- parser execution uses only artifact links from that acquisition/Vault;
- package and document version bind to one exact successful execution and its input set;
- element/region locators resolve in that exact version;
- evidence links remain in the same Vault and target an existing typed object.

A fabricated but individually valid ID from another provenance branch is rejected.

---

# 7. Observation, Acquisition, and Artifact Contract

## 7.1 Observation

An observation records what a human or admitted local instrument saw at a specific time. It includes:

```text
id
vault_account_id
occurrence_id
actor_id
observation_method
observed_at
observation_state
notes or structured receipt hash
```

An inert URL or YouTube locator can produce a source item, occurrence, and manual observation. It is not itself a successful acquisition of remote content.

## 7.2 Acquisition lifecycle

An acquisition starts as:

```text
lifecycle_state = started
outcome = NULL
```

Permitted lifecycle states:

```text
started
finalized
interrupted
reconciled
```

Permitted terminal outcomes:

```text
succeeded
failed
rejected
quarantined
no_artifact
```

A row is never initialized as failed. Finalization records exact result, error class/code where applicable, actor, timestamps, source filename, supplied media type, rights/retention binding, and receipt hash. Unknown or ambiguous failure is explicit and fails closed.

`outcome = no_artifact` is legal only for a completed governed operation in which no byte artifact was expected or admitted, such as an inert identity/locator-only record that truthfully ends after source-item, occurrence, and observation creation. It is prohibited for failed byte acquisition, missing expected content, parser failure, retention rejection, interrupted operations, quarantined temporary material, or silent loss of an expected artifact. Retention rejection uses `intake_rejection_receipts` and does not create or finalize an acquisition as `no_artifact`.

## 7.3 Artifact objects versus encounters

`artifact_objects` is content-addressed and immutable:

```text
id
vault_account_id
sha256
byte_size
storage_relative_path
media_type_observed
created_at
UNIQUE(vault_account_id, sha256)
```

`acquisition_artifact_links` is encounter-specific and immutable:

```text
id
vault_account_id
acquisition_id
artifact_object_id
role
supplied_filename
supplied_media_type
rights_retention_version_id
receipt_sha256
linked_at
UNIQUE(vault_account_id, acquisition_id, artifact_object_id, role)
```

Repeated acquisition of identical bytes creates a new acquisition and new link to the existing object. It preserves each source, occurrence, observation, actor, timestamp, supplied filename, rights binding, and receipt. Deduplication never erases encounters.

## 7.4 Retention gate and dual-write admission

The authoritative admission sequence is:

1. resolve the selected Vault and trusted actor context;
2. require a retention declaration and policy-basis reference before opening or copying supplied bytes;
3. if eligibility is timed-deletion-required, missing, contradictory, or unknown, append a content-free `intake_rejection_receipt` and stop;
4. create an acquisition-start receipt only for preservation-compatible material;
5. copy input to a private operation temp path using create-new semantics;
6. stream-hash and enforce size/type limits;
7. flush and close the file;
8. compute the content-addressed final path;
9. if the object already exists, verify bytes/hash/size and reuse it;
10. otherwise move with a no-overwrite primitive and verify the final object;
11. in one SQLite transaction create/reuse `artifact_objects`, create the acquisition link, finalize acquisition outcome, append audit, and record idempotency;
12. remove temp files only after commit;
13. reconcile crashes and orphan candidates from durable operation receipts.

A rejection occurs before canonical byte admission and before parser selection. The rejection path does not create an acquisition, artifact object, acquisition-artifact link, parser execution, or normalized package.

A filesystem object with no admitted DB link is not trusted merely because its hash is correct. Re-linking requires the exact pending operation receipt and matching metadata.

Windows durability and no-overwrite behavior must be proven. The plan does not assume POSIX directory `fsync` semantics.

---

# 8. Parser Execution and Deterministic Package Contract

## 8.1 Separate receipts

`parser_executions` is run-specific and may include:

```text
execution_id
actor_id
started_at
completed_at
worker_build
host/runtime receipt
input artifact-link IDs
parser definition/admission/config IDs
outcome
warning summary
stdout/stderr diagnostic hashes
```

`normalized_packages` is deterministic and excludes run-specific data.

## 8.2 Deterministic package envelope

Canonical UTF-8 JSON uses:

- sorted object keys;
- no insignificant whitespace;
- Unicode scalar text preserved without locale transformation;
- explicit schema version;
- arrays in deterministic semantic order;
- closed warning codes;
- exact input content hashes;
- exact parser ID, implementation version, configuration hash, and package-schema version;
- elements with deterministic IDs derived from package identity plus stable ordinal/locator material;
- no timestamps, hostnames, process IDs, random UUIDs, absolute paths, or execution IDs in the hashed body.

The package hash is SHA-256 over the canonical JSON bytes. The stored package path is content-addressed by that hash.

## 8.3 Reruns

For identical input bytes, parser implementation, parser version, configuration, and package schema:

- identical package bytes/hash are required;
- a new execution receipt may link to the existing normalized package;
- a different package hash is `determinism_failure`; the execution/package candidate receives governed database state `quarantined`, any unadmitted bytes remain only in operation-scoped temporary quarantine, and document-version creation is blocked;
- changed parser/config/schema creates a new package and immutable document version;
- prior packages remain preserved;
- current/superseded state changes only through governed lineage.

## 8.4 Partial output

A parser either:

- succeeds with no warnings;
- succeeds with explicit admitted warning codes and complete declared coverage;
- fails terminally with no document version;
- receives governed database state `quarantined` for determinism/security/integrity failure, without creating a second physical truth store.

Silent truncation is a terminal failure. A warning must identify the exact omitted range or limit and whether the package remains eligible for search, assertion evidence, or context use under policy.

---

# 9. Actor and Human-Authority Contract

## 9.1 Actor registry

Every Vault database contains an authoritative `actors` registry:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
actor_class TEXT NOT NULL CHECK(human, system, model, import)
display_name TEXT NOT NULL
status TEXT NOT NULL CHECK(active, disabled, revoked)
authority_profile TEXT NOT NULL
created_at TEXT NOT NULL
created_by_actor_id TEXT
UNIQUE(vault_account_id, id)
```

The initial local owner human is created only during the governed Vault-creation operation. System actors are fixed service identities. Model and import actors are candidate-only and disabled unless the corresponding later package is admitted; M06-A has no live model actor execution.

## 9.2 Trusted `ActorContext`

Every governed service operation receives an immutable, server-created `ActorContext` containing:

```text
actor_id
actor_class
vault_account_id
session/correlation ID
authentication source
allowed operation class
```

The request body cannot select `actor_id`, `actor_class`, or authority. The service resolves them from the trusted local session and actor registry, confirms the actor is active and belongs to the opened Vault, and binds the context to audit and idempotency.

## 9.3 Desktop token limitation

The launch token proves that a request came through the current local desktop backend session. It does not by itself prove which human made a decision. Under single-owner M06-A, the backend may map an authenticated local desktop session to the configured owner actor only after server-side registry resolution. Human-only operations must reject:

- absent actor context;
- unknown, disabled, revoked, system, model, or import actors;
- request-supplied actor identity;
- actor/Vault mismatch.

## 9.4 Audit binding

Every mutation records actor ID, actor class snapshot, authority operation, correlation ID, request hash, record IDs, and result. Audit verification confirms the actor existed and was eligible at event time without rewriting historical events if the actor is later disabled.

---

# 10. Referential Integrity and Typed Relationships

Authority-bearing generic `ref_type/ref_id` pairs are prohibited.

## 10.1 Typed evidence

Assertions use explicit typed junctions such as:

```text
assertion_element_evidence
assertion_observation_evidence
assertion_artifact_evidence
assertion_assertion_relationships
```

Each has composite same-Vault foreign keys and role/locator constraints.

## 10.2 Typed dossier membership

Dossiers use separate tables for sources/items, observations, document versions, elements, assertions, unresolved questions, and entities. There is no unvalidated generic membership table.

## 10.3 Typed corrections

Corrections use target-specific tables or target columns with SQLite-enforceable exact-one-target checks and real foreign keys. A generic target may exist only in a derived non-authoritative work queue and must be validated before dispatch.

## 10.4 Policy impacts

Vault policy-impact work targets Vault-owned records through typed junctions. Central publication approvals are never a Vault foreign key. Central publication impact is owned by the central control-room service and may retain a verified receipt of the Vault policy change.

---

# 11. Cross-Database Boundaries

SQLite cannot enforce foreign keys across files. The design therefore distinguishes observations from authority.

## 11.1 Central authority retained

The central control-room database owns:

- `owned_accounts` platform identity;
- work items and revisions;
- exact-content approvals;
- publication records and metrics;
- Vault registry and platform-account bindings under the resolved topology.

## 11.2 Vault authority retained

The Vault database owns:

- research sources and immutable provenance;
- parser and package lineage;
- assertions, entities, questions, dossiers;
- policy and editorial-use rulings;
- local search/chunks/projections;
- per-Vault audit, idempotency, and backup receipts.

## 11.3 External-reference receipts

A Vault may preserve a non-authoritative receipt containing:

```text
central_database_fingerprint
external_record_type
external_record_id
observed_account_id
observed_binding_sha256
observed_state
verified_at
verification_result
```

It is labeled as an observation. It cannot satisfy a publication gate.

Any operation depending on a current central approval must query the central governed service at operation time and verify the exact account, revision, binding hash, decision, and consumption/supersession state. The Vault never infers publication authority from an opaque ID or editorial-use ruling.

---

# 12. Migration Topology

## 12.1 Exact resolved layout

Under resolved OD-1 and OD-2:

```text
migrations/
  existing central 0001–0004 unchanged
  versions/
    0005_vault_registry_and_bindings.py       additive central registry only

vault_migrations/
  alembic.ini
  env.py
  script.py.mako
  manifest.sha256
  versions/
    V0001_vault_identity_actor_audit.py
    V0002_ingestion_artifacts_policy_and_backup.py
    V0003_parser_packages_and_documents.py
    V0004_search_chunks_and_projections.py
    V0005_assertions_entities_dossiers.py
    V0006_closure_support.py
```

Existing migrations 0001–0004 are never edited.

## 12.2 Exact phase-to-migration mapping

| Implementation phase | Central revision state | Vault revision applied | Required schema available at phase start/close |
|---|---|---|---|
| Phase 1 | central `0005_vault_registry_and_bindings.py` applied | `V0001_vault_identity_actor_audit.py` | Vault identity, actors, audit chain, operation-key ledger, migration/reconciliation foundation, routing registry and bindings |
| Phase 2 | central remains at `0005` | `V0002_ingestion_artifacts_policy_and_backup.py` | observations, acquisitions, immutable artifacts, rights/retention, `intake_rejection_receipts`, `backup_generations`, `backup_generation_files`, generation start/final receipts, and foundational disposable-restore support |
| Phase 3A/3B | no new central revision unless separately reviewed | `V0003_parser_packages_and_documents.py` | parser definitions/configurations/admissions/executions, normalized packages, document versions, elements, and regions; individual parser admission remains separate from schema migration |
| Phase 4 | no new central revision | `V0004_search_chunks_and_projections.py` | FTS mapping/state, chunks, projection manifests, and invalidation work |
| Phase 5 | no new central revision | `V0005_assertions_entities_dossiers.py` | assertions, typed evidence, entities, questions, dossiers, policy impact, editorial-use, and static context-run records |
| Phase 6 | no foundational schema deferred here | `V0006_closure_support.py` | closure-only evidence/reconciliation support not required by Phases 1–5; no foundational backup table first appears in Phase 6 |

A phase may not execute an operation whose schema revision has not already been applied and verified. In particular, Phase 2 backup execution is blocked unless `backup_generations`, `backup_generation_files`, and start/final receipt structures from `V0002` are present. `V0006` may add closure-accounting support only; it cannot introduce tables required for foundational Phase 2 backup.

## 12.3 Version and local helper table names

Each separate Vault database uses:

```text
alembic_version
audit_events
operation_keys
```

The names do not collide because databases are physically separate. Reusing them minimizes changes to existing audit/idempotency helpers, though those helpers still require actor-context and same-Vault strengthening.

## 12.4 Runner contract

`run_guarded_upgrade()` must be generalized or wrapped to receive an explicit immutable migration specification:

```text
config_path
migrations_root
manifest_path
version_table = alembic_version
expected_head
schema_name
```

It must not infer the configuration from `migrations_root.parent`. The manifest verifier must accept the explicit manifest/root, verify `env.py`, `script.py.mako`, configuration, and every revision—not only revision files—and reject extra or missing migration files.

Vault open performs identity validation before migration. Vault creation is the only path allowed to migrate a nonexistent database.

## 12.5 Dirty-state and recovery

The durable sibling marker records:

```text
operation ID
vault ID and instance ID
database fingerprint
from revision
target revision
migration manifest hash
started at
```

Startup/open refuses a marker. Recovery may clear it only when identity, manifest, integrity, expected head, and audit evidence prove completion. Empty failed-create discard is allowed only when no canonical object exists. Any populated ambiguous state requires restore or an owner-reviewed recovery action.

## 12.6 Downgrade

No general destructive downgrade promise exists. Each revision must choose one of:

- safe reversal proven on empty or synthetic state;
- refusal when governed rows exist;
- non-destructive compatibility retention;
- restore from a verified prior generation.

Dropping canonical tables with governed data is not an admitted rollback.

## 12.7 Packaging

`build_desktop_sidecar.py` must package both migration trees and both Alembic configurations. Packaging tests verify manifests, expected heads, and startup path resolution in the packaged sidecar.

---

# 13. Audit, Idempotency, and Cross-Database Operations

## 13.1 One chain per physical authority database

Every physical SQLite authority database has one append-only hash-chained `audit_events` table and one `operation_keys` table. There is no claim of one transactionally global chain.

## 13.2 Idempotency

Every admitted mutation has an operation type/key and canonical request hash. Exact replay returns the recorded result. Reuse with different admitted request content fails. Operation records include actor and Vault scope in the request hash.

A rejection-path operation key is governed by the stricter content-free rule in Sections 3.3 and 15.1. It must not hash or otherwise derive from rejected bytes, rejected pasted text, content-bearing diagnostics, or a content hash. Its stable basis is limited to Vault ID, trusted actor ID, retention declaration, policy-basis reference, safe source-kind classification, client-generated operation nonce, and sanitized non-content descriptor.

## 13.3 Cross-database orchestration

When a workflow must touch central and Vault databases:

1. allocate a random correlation ID centrally;
2. write a central operation-start receipt;
3. perform the Vault transaction with the same correlation ID;
4. write a Vault completion audit/operation receipt;
5. write the central completion receipt;
6. if step 5 fails, mark reconciliation required rather than claim atomic completion;
7. reconciliation proves both receipts and exact hashes before closing.

The reverse ordering may be used only where the operation contract requires it and is documented test-by-test. There is never false cross-database atomicity.

---

# 14. Multi-Vault Backend and Desktop Routing

## 14.1 Routing chain

```text
central control-room database
→ trusted Vault registry
→ opaque Vault ID
→ configured base + registered relative root
→ marker verification
→ database metadata verification
→ bounded Vault connection
```

An API caller supplies only an opaque Vault ID. Arbitrary filesystem paths are rejected and never accepted in request bodies, query parameters, headers, or imported manifests.

## 14.2 Discovery and registration

Vault discovery is registry-driven, not filesystem scanning. Registration occurs only through governed `create_vault()` or an explicit future import/recovery operation. Unknown directories are reported as orphan candidates and never auto-registered.

## 14.3 Active selection

Desktop state distinguishes:

```text
active Vault
active platform-owned account
```

They are not interchangeable. A selected platform account must be actively bound to the selected Vault before a workflow combines research and publication context.

## 14.4 Migration timing

- central startup migrates the central DB as today;
- the registry loads without opening every Vault;
- a selected Vault opens and validates on demand;
- migration is explicit and visible, never triggered by an arbitrary path;
- dirty, unavailable, wrong-identity, or out-of-date Vaults report blocked health and cannot mutate.

## 14.5 Connection lifetime

A request-scoped Vault context opens a verified connection, enables FK/WAL/busy timeout, rechecks singleton identity, executes bounded service work, and closes. Background connection pools or watchers are not introduced.

## 14.6 Health and backup routing

Health reports central integrity/head plus each selected Vault's identity, integrity, migration head, audit status, and backup readiness. Backup routes take an opaque Vault ID, resolve through the registry, and verify identity before generation.

## 14.7 UI/API implications

The packaged Tauri desktop client is the sole supported M06-A human operator interface under D031. FastAPI remains the governed loopback API/service host required by Tauri and the packaged sidecar; it is not a second operator product. Existing FastAPI/Jinja pages may remain as frozen historical regression scaffolding, but M06-A adds no browser product features and carries no browser/Tauri feature-parity requirement.

Required changes include:

- Vault list and selected-Vault desktop API types;
- selected Vault state separate from platform-account state;
- Vault health, intake, search, dossier, and backup views in Tauri;
- opaque `vault_id` on desktop API routes;
- safe native file import into a selected Vault temporary intake boundary;
- stable backend refusal semantics for wrong binding, unavailable Vault, dirty migration, unresolved policy, rejected retention, parser non-admission, and publication-boundary checks;
- tests proving the Tauri client and token-gated desktop API cannot bypass actor, Vault, policy, or central publication authority;
- no direct frontend database access and no independent lifecycle, retention, actor, parser, or publication decisions in Tauri.

The Rust backend launcher may remain unchanged only if the Vault base is safely derived from the existing application-data root and proven by tests. Otherwise it must pass a trusted `DISCREPANCY_DESK_DESKTOP_VAULT_BASE` environment value. The launch token still authenticates only the local backend process, not human identity.

---

# 15. Rights and Retention

## 15.1 Pre-admission retention classification

Before the service opens, copies, parses, or content-hashes supplied bytes for canonical use, the trusted intake operation requires:

```text
retention_classification
policy_basis_reference
human classification note
actor context
selected Vault identity
```

Permitted classifications are `preservation_compatible`, `timed_deletion_required`, and `unknown`. Only `preservation_compatible` may proceed to acquisition. Missing, contradictory, expired, or unknown terms fail closed. A source or operator declaration that requires deletion on a date, after a period, when access ends, or on another deadline is `timed_deletion_required` and is rejected.

The rejection receipt is append-only but content-free. It records reason and restrictions without retaining prohibited bytes or pasted content. Any retained filename/locator descriptor must be sanitized non-content classification data; it may not be a hash of rejected content, rejected pasted text, or content-bearing filename/locator text. No temporary content copy or content-derived digest may survive the rejected operation.

## 15.2 Separate capability dimensions

Immutable `rights_retention_versions` distinguish:

```text
retention_eligible
retention_deadline
internal_retrieval_eligible
context_run_eligible
export_eligible
internal_projection_eligible
public_projection_eligible
quotation_redistribution_eligible
policy_basis
reviewed_by_actor_id
reviewed_at
```

For admitted M06-A material, `retention_eligible` must be allow and `retention_deadline` must be null because timed-deletion material is not admitted. Each other capability is explicit allow/deny/unknown. Unknown defaults to deny for that operation.

## 15.3 Versioned bindings

Artifact bytes and artifact identity never change. `artifact_policy_bindings` append a new binding to a rights/retention version and supersede the prior binding through lineage. A change creates policy-impact and invalidation work; it does not mutate the artifact row.

A later discovery that already-admitted material actually requires timed deletion is an architecture and incident stop condition. M06-A blocks use and requires owner review; it does not claim authority to purge the bytes.

## 15.4 Operation filtering

- internal search requires current `internal_retrieval_eligible=allow`;
- context assembly requires current `context_run_eligible=allow`;
- export requires `export_eligible=allow`;
- internal projection requires `internal_projection_eligible=allow`;
- public projection is not produced by M06-A, and remains deny unless separately reviewed;
- quotation/redistribution is independent from private retention and search;
- backup includes only canonically admitted preservation-compatible material;
- rejected intake receipts may be backed up only when they contain no prohibited content.

## 15.5 No purge authority

M06-A contains no automatic deletion scheduler, destructive purge operation, purge tombstone workflow, or delayed-deletion promise. Intake cannot use a future deletion promise to bypass rejection. A purge contract is a later separately planned and authorized package.

---

# 16. Assertions, Entities, Questions, Dossiers, and Policy

## 16.1 Assertion layers

The system keeps separate:

```text
epistemic type
review state
truth/support assessment
LLM/output presentation status
account/public claim label
editorial-use ruling
publication approval
```

An epistemic type named `accepted_conclusion` does not grant authority by itself. A final support assessment requires a verified human operation and evidence, but creating that assessment is optional unless the system is making a final support judgment. It is not a universal prerequisite for archive admission, dossier membership, editorial usefulness, or publication. Account/public labels are policy-defined records, not a hard-coded universal evidence enum.

The Vault may preserve allegations, repeated online claims, folklore, speculation, implausible theories, contradictions, relationship candidates, Desk inferences, and unknowns. Their unresolved status does not by itself make them inadmissible or editorially unusable. Provenance links establish what source or actor supplied the statement; they do not automatically prove the underlying proposition.

## 16.2 Evidence and interpretation

Source content, quotation, human note, parser observation, model candidate, and human interpretation remain distinguishable. Previous model output cannot become evidence merely by re-import.

## 16.3 Entities

Mentions produce entity candidates. System/model actors may propose merges or splits. Human acceptance records proposal, evidence, previous identities, resulting identities, actor, time, policy, and reversible lineage. No automatic merge/split path exists.

## 16.4 Questions and dossiers

`unresolved_questions` is a first-class canonical object with state, text/version hash, actor, evidence links, correction lineage, and resolution reference when later answered. Dossiers organize typed references without duplicating source bytes or evidence authority. A dossier may remain open indefinitely and is not required to produce an accepted conclusion.

## 16.5 Editorial use and publication

An editorial-use ruling is Vault-owned, Vault-policy-bound, human-created, and may allow, restrict, require framing, or block research use. It may allow unresolved, disputed, speculative, conspiratorial, folkloric, or implausible material when attribution, labels, warnings, or required framing preserve what the material is. Lack of a final support assessment is not by itself a prohibition. The ruling never approves exact post bytes. Exact-content approval and publication remain in M00–M05 central authority.

## 16.6 Policy impact

A new policy version creates typed impact work for dependent rights bindings, assertions, dossiers, context eligibility, and editorial-use rulings. The central service receives a correlation receipt for any publication-workflow impact and performs its own live authority checks.

## 16.7 Metrics firewall

Metric snapshots remain central observations about audience response. They can create editorial-learning candidates later, but have no write path into assertion evidence strength, truth/support status, entity identity, or editorial-use authority.

## 16.8 Static context runs

M06-A may store deterministic context-run candidates containing exact Vault/policy identity, task instructions, reviewed conclusions separated from untrusted packets, locators, corrections, contradictions, omissions, truncation, and retrieval trace. No provider call occurs.

Context validation must:

- exclude a claim or bind a warning when its known correction/contradiction cannot fit;
- prove every citation resolves to an exact locator in the exact included document version;
- enforce current policy and rights;
- treat all retrieved content as data, never instructions;
- record no mutation or publication authority.

---

# 17. FTS5 Search Contract

## 17.1 Required engine

SQLite FTS5 is an implementation admission test. The source and packaged Windows sidecar must prove FTS5 availability. If absent, Phase 4 stops and returns to planning. There is no silent `LIKE` or `instr` fallback represented as equivalent full-text search.

## 17.2 Exact schema mode

Use an external-content FTS5 table with an explicit integer row mapping:

```text
search_documents
  search_rowid INTEGER PRIMARY KEY
  vault_account_id TEXT NOT NULL
  element_id TEXT NOT NULL
  document_version_id TEXT NOT NULL
  normalized_text TEXT NOT NULL
  content_sha256 TEXT NOT NULL
  policy_eligibility_sha256 TEXT NOT NULL
  state TEXT NOT NULL CHECK(current, invalid, rebuilding)
  UNIQUE(vault_account_id, element_id)

vault_fts USING fts5(
  normalized_text,
  content='search_documents',
  content_rowid='search_rowid',
  tokenize='unicode61 remove_diacritics 2'
)
```

`search_documents` and `vault_fts` are derived. Canonical IDs remain TEXT; `search_rowid` exists only for FTS synchronization.

## 17.3 Synchronization

The search service performs explicit insert/update/delete synchronization inside one SQLite transaction and records `search_invalidation_work`. Rebuild clears derived search state and regenerates only from:

- current, non-withdrawn document versions;
- current elements;
- current rights/policy bindings allowing internal retrieval;
- exact same-Vault records.

Triggers may maintain the FTS shadow table from `search_documents`, but no trigger infers canonical currentness or policy. Those decisions are made by governed rebuild/invalidation operations.

## 17.4 Query behavior

Queries are account/Vault scoped, parameterized, length- and token-bounded, and fail cleanly on malformed FTS syntax. Ranking is relevance only and never evidence strength. Search results include exact document-version and element locators.

---

# 18. Deterministic Chunks

Chunks are derived and disposable. Identity binds:

```text
vault_account_id
document_version_id
ordered element IDs or exact locator span
chunk strategy ID/version
normalized content SHA-256
policy eligibility SHA-256
```

Chunk boundaries are deterministic and never cut a known correction/contradiction away from a selected assertion without an explicit bound warning. Correction, supersession, withdrawal, parser replacement, policy change, rights change, or element change invalidates affected chunks.

No embedding or semantic index is created.

---

# 19. Generated Projections

Markdown and HTML projections are generated, read-only, and derived. Each `projection_manifest` binds:

```text
vault identity
dossier/document identity and version
canonical source digest
policy/rights digest
projection generator ID/version/config
output hash
generated_at execution receipt
```

Run-specific time is outside the deterministic content hash.

Manual edit behavior is fixed:

- a mismatch is detected before display/regeneration;
- the projection manifest receives governed database state `quarantined`/`tampered`; the edited derived file remains non-authoritative at its existing projection path or is replaced only after preserving a tamper receipt according to the human-selected operation; no physical quarantine authority area is created;
- no route ingests projection text back into authority;
- regeneration occurs after correction, supersession, withdrawal, rights change, policy change, or generator change.

Public projections are not produced by M06-A.

---

# 20. Backup, Restore, and Rebuild

## 20.1 Foundational timing

A foundational backup and disposable restore path is required in Phase 2 when canonical artifact storage first appears. Full closure proof is expanded in Phase 6. Canonical dual-store data may not accumulate through several phases without recovery proof.

## 20.2 Honest SQLite model

The SQLite backup API copies the full database. Derived rows may therefore be physically present in the snapshot, but:

```text
derived rows remain non-authoritative
restore invalidates or clears them before use
restore rebuilds them from canonical records
```

The manifest classifies table/file families as canonical or derived.

## 20.3 Generation sequence

1. resolve and verify one Vault by opaque ID;
2. create a unique no-overwrite generation workspace;
3. append a generation-start receipt in the live Vault database;
4. checkpoint/coordinate writers and take a consistent SQLite backup;
5. copy immutable original objects and normalized packages using no-overwrite semantics;
6. enumerate canonical and derived contents and write an immutable manifest with hashes, sizes, Vault identity, migration head, application commit, and policy state;
7. hash and verify the manifest and every required file;
8. write `COMPLETE` with create-new semantics;
9. append a generation-complete audit/receipt in the live Vault database;
10. reconcile a crash between filesystem completion and final DB receipt through the shared generation/correlation ID.

Filesystem completion and SQLite receipt are never described as one atomic transaction.

## 20.4 Restore

Restore operates only in an empty disposable workspace. It must:

- reject an existing/dirty target;
- verify manifest and completion marker;
- verify Vault account, instance, and expected restore identity rules;
- verify every original/package hash and required-file presence;
- open the snapshot existing-file-only;
- verify SQLite integrity, migration head, singleton identity, and audit chain;
- detect orphan canonical rows/files;
- clear/invalidate FTS, search state, chunks, caches, and projection manifests/files;
- rebuild all derived data deterministically;
- prove rebuilt derivatives contain no cross-Vault content;
- remain isolated from the live registry until the owner explicitly chooses a later recovery operation.

Wrong-account, missing-original, modified-artifact, modified-manifest, partial-generation, and failed-rebuild states fail closed.

## 20.5 Encryption

The existing `age` archive path may be regression-tested where available. Canonical backup, hashes, and disposable restore are required independently. Packaged key-management UX remains M15 and is not a hidden M06-A closure dependency.

---

# 21. Corrected Implementation Phases

No phase is authorized for execution by this document.

## Phase 1 — identity, actors, migration, routing

- central Vault registry and platform-account bindings under the resolved topology;
- per-Vault metadata/marker/create/open contract;
- actors and trusted `ActorContext`;
- per-database audit/idempotency;
- exact Vault migration environment and dirty recovery;
- opaque Vault routing skeleton, health, and selected-Vault desktop state;
- identity, actor, migration, routing, and cross-database reconciliation hammer tests.

## Phase 2 — observation, acquisition, artifacts, foundational recovery

- sources, items, occurrences, observations, acquisitions;
- artifact objects and encounter links;
- rights/retention versions, mandatory pre-byte retention classification, and content-free rejection receipts;
- Windows-safe object storage and orphan reconciliation;
- foundational backup generation and disposable restore;
- dual-write, duplicate, tamper, and cross-Vault tests.

Timed-deletion-required or unknown-retention material remains inadmissible throughout Phase 2 and every later phase.

## Phase 3A — parser framework and stdlib-first parsers

- parser definitions/config/admissions;
- isolated worker and no-egress controls;
- parser execution versus deterministic package separation;
- plain text, SRT, VTT, and JSON candidates;
- package determinism, warnings, and packaging proof;
- individual owner admission after tests; no bulk implicit admission.

## Phase 3B — external or higher-risk parsers one at a time

- Markdown;
- local RSS/Atom;
- basic HTML;
- born-digital PDF;
- dependency lock, license, resolver, sidecar, determinism, and egress proof for each;
- one owner admission gate per parser/version/config.

## Phase 4 — FTS, chunks, projections

- source and packaged FTS5 proof;
- exact FTS schema/rebuild/invalidation;
- deterministic chunks;
- generated Markdown/HTML projections and tamper behavior;
- rights/policy/current-version filtering;
- search/chunk/projection adversarial closure.

## Phase 5 — interpretation and governed use

- assertions and typed evidence;
- entity candidates plus human merge/split acceptance;
- unresolved questions and dossiers;
- policy labels and impact work;
- human editorial-use rulings;
- static context-run assembly and validation;
- publication-authority separation and metrics firewall tests.

## Phase 6 — complete recovery, packaged parity, and hammer closure

- complete backup/rebuild/crash matrix;
- packaged desktop parity and migration/FTS/parser resource proof;
- full executable adversarial matrix;
- evidence commit binding;
- no missing/skipped/unmapped invariants;
- independent implementation review and owner closure.

---

# 22. Exact Application Change Surface Candidate

The following is the likely implementation surface under the resolved topology. It is a planning estimate, not authority to edit.

## 22.1 Existing Python files likely modified

```text
src/discrepancy_desk/db.py
  add existing-file-only Vault connection and identity-safe helpers

src/discrepancy_desk/persistence.py
  actor-context audit/idempotency strengthening or extraction to reusable helpers

src/discrepancy_desk/migration_runner.py
  explicit migration specification/config/manifest/head/version-table support

src/discrepancy_desk/migration_integrity.py
  explicit manifest root, environment files, Vault identity, expected-head recovery

src/discrepancy_desk/backup.py
  retain central backup behavior; add or route to Vault-specific generation contract

src/discrepancy_desk/archive.py
  regression only unless Vault archive wrapper is needed; no hidden age requirement

src/discrepancy_desk/web.py
  trusted Vault base/config, registry, routes, actor resolution, health, selected Vault

src/discrepancy_desk/operator_service.py
  governed Vault operations or delegation to Vault service; no actor IDs from requests
```

## 22.2 Likely new Python modules

```text
src/discrepancy_desk/actor_context.py
src/discrepancy_desk/vault_identity.py
src/discrepancy_desk/vault_registry.py
src/discrepancy_desk/vault_router.py
src/discrepancy_desk/vault_persistence.py
src/discrepancy_desk/vault_filesystem.py
src/discrepancy_desk/vault_service.py
src/discrepancy_desk/vault_queries.py
src/discrepancy_desk/vault_backup.py
src/discrepancy_desk/vault_reconciliation.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parsers/*.py
src/discrepancy_desk/vault_search.py
src/discrepancy_desk/vault_chunks.py
src/discrepancy_desk/vault_projections.py
src/discrepancy_desk/context_validation.py
```

Names may be consolidated during reviewed implementation planning, but responsibilities may not disappear.

## 22.3 Migration/configuration files

```text
migrations/versions/0005_vault_registry_and_bindings.py
migrations/manifest.sha256
vault_migrations/alembic.ini
vault_migrations/env.py
vault_migrations/script.py.mako
vault_migrations/manifest.sha256
vault_migrations/versions/V0001...V0006
```

## 22.4 Scripts and dependency authority

```text
scripts/build_desktop_sidecar.py
  package Vault migration/config/parser resources; prove FTS5

scripts/run_ht_evidence.py
  map M06-A matrix, fail on empty/missing/divergent/skipped/error evidence

pyproject.toml
uv.lock
  changed only for individually admitted external parser dependencies
```

## 22.5 Operator surface and loopback API

The supported operator path is:

```text
Tauri client
→ token-gated FastAPI loopback desktop API
→ governed service operations
→ central or selected-Vault persistence
```

### Packaged desktop

```text
desktop/src/api/client.ts
desktop/src/api/types.ts
desktop/src/state/account.ts or new vault state module
desktop/src/App.tsx or decomposed Vault pages/components
desktop/src-tauri/src/backend.rs if trusted Vault-base configuration is not derived safely
desktop/src-tauri/src/commands.rs for bounded selected-Vault file intake if required
desktop contract, security, lifecycle, and packaging tests
```

### Governed local backend

```text
src/discrepancy_desk/web.py
  retain token-gated desktop API routing, health, and packaged sidecar startup
  add no new M06 Jinja operator routes or templates

src/discrepancy_desk/vault_service.py and related service modules
  own actor, Vault, migration, policy, reconciliation, and persistence decisions
```

Tauri may present and request operations, but it cannot implement independent lifecycle, retention, actor, parser, publication, or database decisions. Existing Jinja pages and their historical regression tests may remain unchanged where useful; they are not a supported M06 operator surface and receive no feature parity work.

## 22.6 Tests

Likely modified test infrastructure and new focused suites:

```text
tests/conftest.py
  emit per-invariant evidence under the runner-selected destination; preserve collected/executed/skip/error counts and tested commit binding consistently with the M06-A runner

tests/test_m06a_vault_identity.py
tests/test_m06a_actor_authority.py
tests/test_m06a_migrations.py
tests/test_m06a_routing.py
tests/test_m06a_ingestion_artifacts.py
tests/test_m06a_parser_framework.py
tests/test_m06a_parser_stdlib.py
tests/test_m06a_parser_external.py
tests/test_m06a_search_chunks.py
tests/test_m06a_projections.py
tests/test_m06a_assertions_entities.py
tests/test_m06a_policy_context.py
tests/test_m06a_backup_restore.py
tests/test_m06a_hammer_runner.py
tests/test_m06a_desktop_contract.py
tests/test_m06a_desktop_workflow.py
tests/test_m06a_packaging.py
```

Existing M00–M05 tests remain regression authority.

---

# 23. Validation, Evidence, Rollback, and Stop Gates

## 23.1 Planned validation commands

Implementation packages must identify exact commands. The expected combined baseline is:

```text
uv sync --frozen
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
uv run pytest -o addopts= --disable-warnings -q <phase-specific nodes>
uv run python scripts/run_ht_evidence.py
uv run alembic -c alembic.ini heads
uv run alembic -c vault_migrations/alembic.ini heads
uv run python scripts/build_desktop_sidecar.py
cd desktop && npm ci && npm run build
cargo test --manifest-path desktop/src-tauri/Cargo.toml
```

The exact implementation return must add:

- fresh central DB and two fresh synthetic Vault migration proofs;
- populated upgrade and interruption proofs;
- source and packaged-sidecar FTS5 probes;
- parser no-egress worker proof;
- dual-write crash/orphan reconciliation proof;
- foundational and complete backup/restore proofs;
- wrong-account and cross-Vault contamination proof;
- staged-scope, dirty-tree, secret, and generated-output review.

No test is claimed executed by this planning candidate.

## 23.2 Evidence

Every hammer invariant maps to exact pytest nodes and an evidence JSON destination in the adversarial matrix. Evidence records commit SHA, matrix version/hash, runner version/hash, migration heads, Python/SQLite versions, platform, counts, disposition, and exact node results.

## 23.3 Runner fail-closed contract

The hammer runner returns failure when:

- zero required invariants execute;
- a required invariant has no test mapping;
- a mapped node does not exist or is not collected;
- expected evidence is absent;
- evidence commit SHA differs from the tested commit;
- matrix IDs and runner IDs diverge;
- a detector errors or is skipped;
- a required test is skipped without an owner-approved disposition;
- an invariant result is missing, fabricated, malformed, duplicated, or unsigned by the manifest hash contract.

## 23.4 Rollback

Rollback is phase-specific:

- before canonical data: remove only the disposable failed-create workspace after proof it contains no governed object;
- after canonical data: never drop tables or delete objects to imitate rollback;
- refuse incompatible downgrade, preserve dirty evidence, and restore a verified pre-change generation into a disposable location;
- central registry migration is additive and must refuse destructive downgrade with bindings present;
- parser admission can be suspended/revoked without deleting historical packages;
- derived FTS/chunks/projections may be cleared and rebuilt;
- application rollback must remain compatible with the database head or use a verified restore.

## 23.5 Architecture-reopening conditions

Owner architecture review is required if implementation reveals a need to:

- merge physical Vaults or share canonical interpretation directly;
- make projections editable;
- add network retrieval, OCR, Qdrant, graph, or live LLM tools;
- allow non-human authority for admission, support, editorial use, merge/split, correction, purge, or publication;
- create a second canonical store;
- change the owner-resolved Vault-account meaning or per-Vault SQLite topology;
- admit timed-deletion material or add destructive purge authority without a new owner-authorized package.

---

# 24. Candidate Review and Acceptance Sequence

```text
candidate reconciled to the owner-resolved design
→ independent Claude resolved-package review completed
→ R-01 through R-05 corrections applied and R-06 deferred
→ focused correction verification completed with no remaining findings
→ owner accepted the resolved M06-A planning baseline
→ acceptance/navigation package closed R-06 and the correction cycle
→ owner authorized exact Phase 1 implementation through D030
→ D031 narrowed new operator work to Tauri over the governed loopback API
→ Phase 1 implementation committed as 8fe3be4
→ clean commit-bound evidence preserved through 5f0f9ae
→ D032 deferred the independent Claude implementation audit without waiving it
→ independent audit completed with verdict M06-A PHASE 1 INDEPENDENTLY VERIFIED
→ D033 accepted Phase 1 closure and opened exact Phase 2 package review
→ Phase 2 remains blocked until `m06a-phase2-exact-implementation-package.md` is explicitly accepted
```

No prompt is committed. No parser is admitted. The exact Phase 2 package is prepared for owner review, but Phase 2 through 6 migrations, intake, parser, search, dossier, backup, and later runtime work remain unauthorized until their separate gates are accepted.

---

# 25. Explicit Authority Block

```text
Status: owner-accepted planning baseline
Phase 1 implementation/evidence: complete through 8fe3be4 and 5f0f9ae
Independent implementation audit: complete and accepted
Phase 1 owner closure: D033
Phase 2 exact package: prepared for owner review
Later-phase implementation authority: none

M06 architecture and this resolved M06-A planning baseline are owner-accepted.
OD-1 through OD-3 are resolved. The correction cycle is closed.
D029 and D031 clarify editorial identity and the supported operator surface.
D032 records the deferred-audit timing decision; the audit is now complete.
D033 accepts Phase 1 closure and authorizes Phase 2 package review only.

Focused correction verification completed with verdict:
M06-A FOCUSED CORRECTIONS VERIFIED — READY FOR OWNER ACCEPTANCE.
Owner planning acceptance is recorded in D028 and the closure record.

D030 authorized the exact Phase 1 application package. That package, clean
evidence, independent review, and owner closure are complete. The exact Phase
2 package exists at `m06a-phase2-exact-implementation-package.md`, but no
authority exists for Phase 2 application edits until explicit owner acceptance.
No authority exists for Phase 3 through 6, parser admission, new dependencies,
M06-B work, or any other deferred capability.
```
