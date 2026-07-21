# Independent Technical Review — M06-A Local Manual Vault Planning Package

Reviewed external planning input:

```text
08-audits/m06a-local-manual-vault-planning-package.md
```

Review mode:

```text
read-only
```

---

# A. Live Repository Truth

## Tooling

```text
Server:   B1-L7 Control Room
Version:  0.1.0
Roots:    healthy and accessible
```

## Documentation repository

```text
Path:    C:\dev\x\discrepancy-desk-docs
Branch:  main
HEAD:    531377cb1db50409851251e749cec9ba666efc79
Commit:  Reconcile M06 milestone planning gate
Status:  synchronized with origin/main
```

Working tree:

```text
?? 08-audits/m06a-local-manual-vault-planning-package.md
```

The Claude package is untracked external planning input.

Its Git object hash was measured before and after this review:

```text
5a5f260449ba3a19c53652f1f602c6014631ed9f
```

The hash did not change.

## Application repository

```text
Path:    C:\dev\x\discrepancy-desk
Branch:  main
HEAD:    6cbd0366e55bfba0c9687201615b72e70bf485d5
Commit:  Bind AC-01 correction evidence
Status:  clean and synchronized with origin/main
```

## Actions not performed

No file was:

- edited;
- created;
- moved;
- renamed;
- staged;
- committed;
- deleted.

No migration, database, Vault, dependency installation, parser execution, build, or implementation test was performed.

---

# B. Final Verdict

```text
M06-A PLAN READY WITH CORRECTIONS
```

This verdict does **not** mean the plan is ready for implementation authorization.

The Claude package is architecturally serious and broadly faithful to M06-D01 through M06-D16. Its scope boundaries, local-only direction, hybrid SQLite/filesystem model, immutable evidence intent, parser caution, account isolation intent, static context-run boundary, and recovery emphasis are sound.

However, it is not yet an exact implementable planning package. Several load-bearing parts are internally inconsistent, unenforceable in SQLite, incompatible with the live application, or incomplete:

- acquisitions cannot correctly preserve repeated encounters with identical bytes;
- the acquisition lifecycle records a new attempt as failed before it fails;
- observations and unresolved questions are missing from the physical model;
- actor identity and human-only authority are not enforceable;
- multiple authority-bearing references are polymorphic strings without referential integrity;
- the package’s deterministic-hash claim is false as written;
- physical Vault identity cannot be enforced by the proposed filesystem-dependent `CHECK`;
- the proposed second Alembic environment is not executable through the live migration runner;
- desktop routing still assumes one database;
- full SQLite backup cannot physically exclude the proposed derived tables;
- rights, retention, internal use, export, and public eligibility are collapsed into one field;
- parser admissions remain alternatives rather than exact admissions;
- parser network denial is asserted but not technically enforced;
- the hammer matrix is a threat inventory, not an executable closure matrix;
- cross-database account and approval references are not authority-safe;
- the proposed file-change surface materially understates the required changes.

These defects can be corrected without reopening M06-D01 through M06-D16. That is why the verdict is **ready with corrections**, rather than a rejection of the M06-A direction.

---

# C. Executive Assessment

## What Claude got right

The package correctly recognizes that M06-A introduces four difficult boundaries simultaneously:

1. SQLite plus filesystem canonical authority;
2. physically isolated account Vaults;
3. hostile local-file parsing;
4. rebuildable derivatives that must never flow backward into authority.

It correctly preserves these accepted boundaries:

- M06-A remains local and human-triggered.
- M06-B remains blocked.
- No provider or network retrieval is admitted.
- No OCR, browser rendering, monitoring, Qdrant, graph, or live LLM integration is admitted.
- Original artifacts and normalized packages are immutable.
- Parser reruns do not overwrite prior packages.
- Projections are generated and read-only.
- Entity merge/split acceptance remains human-only.
- Editorial use remains distinct from exact publication approval.
- Backups are per-account and require disposable restore proof.

## Where the package fails as an exact plan

The central problem is not the overall architecture. It is the gap between architectural prose and enforceable physical design.

The report frequently states that a rule is “enforced” when the proposed schema or live application does not enforce it. Examples include:

```text
human-only actors
polymorphic foreign references
same-account provenance
Vault identity checks
cross-database account references
cross-database approval references
derived-data backup exclusion
parser network denial
deterministic package hashing
```

The correction package must replace those assertions with exact mechanisms.

---

# D. Accepted-Architecture Compliance

| Ruling | Review result | Assessment |
|---|---|---|
| M06-D01 | Partial | Hybrid SQLite/filesystem authority is preserved, but backup semantics, optional file-only context manifests, and polymorphic references weaken the exact authority boundary. |
| M06-D02 | Partial | The universal envelope is recognized, but the physical schema omits `observations`, and acquisition/artifact cardinality is incorrect. |
| M06-D03 | Partial | Separate Vault roots are proposed, but the SQLite physicality, Vault identity, routing, and cross-database contracts remain unresolved. |
| M06-D04 | Compliant | Markdown/HTML projections remain generated and read-only. |
| M06-D05 | Partial | Research, support, editorial use, and publication approval are conceptually distinct, but cross-database approval references are unsafe and the proposed gating question is largely already settled. |
| M06-D06 | Partial | Immutable correction and supersession intent is correct, but generic target references and mutable rights fields undermine enforceability. |
| M06-D07 | Partial | The package field set is strong, but the claimed deterministic serialization is false because run-specific data is inside the hash. |
| M06-D08 | Mostly compliant | Static context-run structure and no-provider authority are preserved. Persistence and manifest authority need exact resolution. |
| M06-D09 | Partial | Human-only merge/split intent is correct, but no authoritative actor model enforces it. |
| M06-D10 | Partial conflict | Events and chronologies are correctly omitted, but `observations`—explicitly included in the accepted foundation—are also omitted. |
| M06-D11 | Partial | The correct parser classes are listed, but several admission records still contain unresolved implementation alternatives. |
| M06-D12 | Partial | Search and chunk concepts are correct; the exact FTS5 schema, packaged availability gate, current-version selection, and fallback behavior are incomplete. |
| M06-D13 | Partial | Backup contents and restore tests are strong, but full SQLite backup contradicts the claimed exclusion of derived tables. |
| M06-D14 | Compliant | No scheduled, provider, monitoring, or autonomous ingestion is admitted. |
| M06-D15 | Compliant | Qdrant and graph work remain deferred. |
| M06-D16 | Compliant | No live LLM or provider integration is admitted. |

The package does not reopen an accepted owner ruling, but it does not yet implement several rulings faithfully enough on paper.

---

# E. Existing-Application Compatibility

## Correctly identified live behavior

The package accurately identifies that the application uses:

```text
raw sqlite3
STRICT tables
WAL
foreign_keys=ON
busy_timeout=5000
BEGIN IMMEDIATE
Alembic
migration manifests
dirty migration markers
operation_keys
append-only audit events
FastAPI
Tauri loopback backend
desktop launch token
PyInstaller packaging
pytest evidence
hammer-runner evidence
```

## Important compatibility limits

### Audit

The live `append_audit()` function writes specifically to:

```text
audit_events
```

The live `verify_audit_chain()` function reads specifically from:

```text
audit_events
```

The proposed separate Vault table is named:

```text
vault_audit_events
```

Those are not interchangeable.

### Idempotency

The live idempotency helpers write specifically to:

```text
operation_keys
```

The proposed `V0001` phase does not include an `operation_keys` table.

### Migration runner

The live runner:

- derives the Alembic configuration path from `migrations_root.parent / "alembic.ini"`;
- sets `script_location` dynamically;
- verifies only a table named `alembic_version`.

The proposed Vault migration tree and `vault_alembic_version` do not work through that runner as written.

### Desktop backend

The desktop backend currently receives exactly:

```text
one database path
one evidence root
one migration root
```

FastAPI stores those as single application-state values, and `_connection()` always opens the one central database.

### Backup

The live backup code copies:

```text
one complete SQLite snapshot
one complete evidence tree
```

Its verification code is hard-coded to:

```text
database/discrepancy-desk.sqlite3
evidence_refs
audit_events
```

### Database opening

The live `connect(path)` function creates parent directories and allows SQLite to create a database file.

That behavior is acceptable for an explicitly governed create operation, but dangerous for opening an existing Vault: a typo or wrong path could silently create a new empty database.

---

# F. Detailed Findings

## F-01 — Live-tree status statement is no longer current

**Classification:** documentation inconsistency
**Severity:** Low

**Affected Claude section**

```text
A. Live Repository Truth
X. Planning-Package Completeness Matrix
```

**Live evidence**

The report describes its ephemeral docs clone as clean. The current live docs working tree contains the untracked Claude report.

**Why it matters**

The report’s historical statement is not dishonest, but it must not later be promoted as a statement about the current governed tree.

**Bounded correction**

In the future disposition record, distinguish:

```text
Claude review clone state: clean at review checkpoint
current live docs state: dirty only by preserved untracked external report
```

Do not edit the Claude report itself.

**Blocking effect**

- Planning acceptance: No
- Implementation authorization: No
- Phase-specific: None

---

## F-02 — The physical model omits accepted core records

**Classification:** accepted-architecture conflict
**Severity:** High

**Affected Claude section**

```text
E. Physical SQLite Schema Proposal
X. Planning-Package Completeness Matrix
```

**Live governing evidence**

The accepted architecture explicitly includes:

```text
observation
```

in the universal ingestion envelope.

M06-D10 lists:

```text
sources
source items
occurrences
observations
acquisitions
original artifacts
document versions
elements
assertions
dossiers
```

The milestone scope also includes unresolved questions.

Claude defines no `observations` table.

Claude allows:

```text
dossier_memberships.member_ref_type = 'question'
```

but defines no question table.

**Why it matters**

Occurrence, observation, and acquisition are intentionally different:

- occurrence: where an item was encountered;
- observation: what was seen at a time through an instrument/method;
- acquisition: an attempt to obtain a representation.

Collapsing observation timestamps into occurrences or acquisitions loses the accepted vocabulary and prevents repeated observations of the same occurrence.

A dossier membership pointing to a nonexistent question object cannot be referentially valid.

**Bounded correction**

Add exact models for:

```text
observations
unresolved_questions
```

At minimum, an observation must bind:

- Vault account;
- occurrence;
- observation time;
- observation method/instrument;
- actor;
- observed state;
- receipt;
- optional resulting acquisition.

Questions must have:

- stable identity;
- account scope;
- question text;
- lifecycle;
- actor;
- evidence or dossier relationships;
- correction/supersession behavior.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phases 2 and 5

---

## F-03 — Original-artifact and acquisition cardinality is incorrect

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
E.3 Acquisition and artifacts
F. Transaction and Atomicity Model
I. Original Artifact Contract
R. VT-10 and VT-11
```

**Live/report evidence**

Claude proposes:

```text
original_artifacts.acquisition_id
UNIQUE(vault_account_id, content_sha256)
```

It also requires repeated identical intake to produce one artifact object.

**Why it matters**

A second acquisition of identical bytes is a separate historical event with its own:

- occurrence;
- actor;
- timestamp;
- source;
- supplied filename;
- receipt;
- rights/retention context.

With the proposed uniqueness rule, only the first acquisition can own the one artifact row. Later acquisition receipts cannot be linked correctly without either:

- violating the unique constraint;
- overwriting the first acquisition relationship;
- losing the later acquisition history.

The duplicate relationship table does not fix this because it does not model acquisition-to-object cardinality.

**Bounded correction**

Separate storage identity from acquisition linkage.

Recommended structure:

```text
artifact_objects
  one row per Vault + exact content hash
  owns stored_relpath, byte_size, exact hash

original_artifacts or acquisition_artifacts
  one immutable supplied-artifact record per acquisition
  owns supplied filename, declared type, rights binding, receipt context

acquisition_artifact_links
  acquisition_id
  artifact_object_id
  role
  created_at
```

A repeated identical acquisition creates:

- a new acquisition record;
- a new immutable acquisition-artifact receipt or link;
- no second stored object.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 2

---

## F-04 — Acquisition lifecycle is untruthful and locator recording is conflated with acquisition

**Classification:** accepted-architecture conflict
**Severity:** High

**Affected Claude section**

```text
E.3 Acquisition and artifacts
F. Transaction and Atomicity Model
K.9 YouTube identity/context
```

**Live/report evidence**

Claude’s transaction table says:

```text
start acquisition
→ insert acquisitions(outcome='failed'→updated)
```

Its terminal vocabulary contains only:

```text
succeeded
failed
rejected
quarantined
```

It also treats:

```text
manual_url_locator
```

as an acquisition method.

**Why it matters**

A newly started acquisition has not failed.

Pre-recording it as failed corrupts historical meaning and creates ambiguity between:

- currently running;
- interrupted;
- actually failed;
- never started.

Also, recording an inert YouTube URL or source locator is normally an occurrence operation. No representation has been obtained merely because a URL was typed.

**Bounded correction**

Separate operation lifecycle from terminal outcome:

```text
state:
  started
  completed
  interrupted

outcome:
  succeeded
  failed
  rejected
  quarantined
  null while non-terminal
```

Enforce:

- `finished_at` required for terminal states;
- `outcome` null while started;
- no terminal-to-terminal rewrite;
- interrupted attempts remain preserved.

Treat identity-only URL recording as:

```text
source item + occurrence
```

not a successful artifact acquisition.

A later supplied transcript creates its own acquisition.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 2

---

## F-05 — Deterministic package hashing is impossible under the proposed package fields

**Classification:** factual error
**Severity:** High

**Affected Claude section**

```text
E.4 Document versions
J. Normalized JSON Element-Package Contract
K. Parser Admission Records
```

**Report evidence**

Claude states that identical input, parser version, and configuration produce a byte-identical package.

The proposed hashed package includes:

```text
package_id
document_version.id
element_id values
execution.started_at
execution.finished_at
execution.host_note
```

These values naturally vary between executions.

**Why it matters**

The report simultaneously requires:

```text
every parser execution gets a receipt
identical executions produce identical package hashes
reruns remain preserved
```

Those requirements cannot be satisfied by hashing run-specific IDs and timestamps into the normalized content package.

The uniqueness rule:

```text
UNIQUE(original_artifact_id, parser_id, parser_version, parser_config_sha256)
```

also prevents preserving a second execution using the same inputs and configuration.

**Bounded correction**

Separate:

```text
parser_executions
normalized_document_packages
document_versions
```

`parser_executions` should preserve:

- attempt ID;
- start/finish time;
- host/runtime data;
- warnings;
- failure;
- actor;
- operation key.

The deterministic normalized package should include only content-derived and versioned inputs:

- artifact hash;
- parser identity/version;
- parser configuration hash;
- normalized elements;
- deterministic locators;
- deterministic element identifiers;
- warning/partial-result content;
- package schema version.

Run timestamps and random UUIDs must not participate in the deterministic content hash.

A successful rerun may:

- link to an already-existing identical package;
- preserve a separate execution receipt.

A differing package from the same pinned inputs must be treated as a determinism failure, not blocked silently by a uniqueness constraint.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 3

---

## F-06 — Human-only authority is not enforceable

**Classification:** authority or safety blocker
**Severity:** Critical

**Affected Claude section**

```text
E.5 Entity merge/split
E.6 Assertions
E.9 Policy
E.10 Editorial use
M. Assertions, Evidence, and Review Workflows
N. Identity, Duplicate, Merge, and Split Plan
```

**Live repository evidence**

The live audit table stores:

```text
actor_type TEXT NOT NULL
actor_id TEXT NOT NULL
```

There is no `CHECK` on actor type and no actor table.

The live service enforces human/system attribution by calling `append_audit()` with hard-coded values such as:

```text
human
system
owner-local
owner-desktop
revision-service
evidence-verifier
```

Claude adds many `actor_id TEXT` columns but no authoritative actor model.

The proposed statement:

```text
actor_id must be human actor_type in audit
```

is not enforceable from the proposed schema.

**Why it matters**

M06-D09 requires that only a human may accept merge/split operations.

The same human-only boundary applies to:

- research admission;
- truth/support rulings;
- editorial-use rulings;
- corrections;
- accepted conclusions;
- policy changes.

An arbitrary string in `actor_id` does not prove human authority.

**Bounded correction**

Define an exact actor principal contract before Phase 1.

At minimum:

```text
actors
  id
  actor_type CHECK ('human','system','model','import')
  display_name
  state
  created_at

trusted ActorContext
  supplied by the service boundary
  not accepted from arbitrary request payloads
```

Human-only operations must:

1. require an authenticated/trusted `ActorContext`;
2. require `actor_type='human'`;
3. reject disabled or unknown actors;
4. write the same actor identity into the governed record and audit event;
5. prove system/model actors cannot invoke the operation.

The desktop launch token authenticates the desktop process, not the identity of every actor. The plan must state the admitted single-owner mapping explicitly.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phases 1 and 5

---

## F-07 — Authority-bearing polymorphic references lack referential integrity

**Classification:** authority or safety blocker
**Severity:** High

**Affected Claude section**

```text
E.5 duplicate_relationships
E.7 corrections
E.8 dossier_memberships
E.9 policy_impact_records
E.11 projection_manifests
E.11 search_invalidation_work
```

**Report evidence**

The design repeatedly uses:

```text
ref_type
ref_id
```

without real foreign keys.

Examples:

```text
left_ref_type + left_ref_id
target_ref_type + target_ref_id
member_ref_type + member_ref_id
impacted_ref_type + impacted_ref_id
subject_ref_type + subject_ref_id
```

The report later claims:

```text
all cross-record authority is relational with FKs and CHECKs
```

That claim is false.

**Why it matters**

SQLite cannot enforce that:

- the referenced record exists;
- the ID belongs to the declared type;
- the record belongs to the same Vault;
- the target has not been fabricated;
- a replacement record is the correct type.

This directly conflicts with the hammer doctrine requiring fabricated IDs to fail at the persistence boundary.

**Bounded correction**

Define an exact integrity strategy for every polymorphic relationship.

Authority-bearing relationships should use one of:

1. typed junction tables;
2. nullable typed FK columns with an exact-one-target `CHECK`;
3. a rigorously governed object registry plus database triggers proving the underlying typed row exists.

Typed tables are the safer default for:

- corrections;
- dossier memberships;
- assertion evidence;
- policy impact;
- merge/split lineage.

Derived-only work queues may use a generic reference only if:

- service validation is explicit;
- fabricated IDs are rejected;
- the queue cannot create authority;
- hammer tests prove failure.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phases 4 and 5

---

## F-08 — Same-account and provenance-chain consistency is not enforced

**Classification:** authority or safety blocker
**Severity:** High

**Affected Claude section**

```text
E.2 through E.10
M. Assertion binding
```

**Report evidence**

Most tables independently carry:

```text
vault_account_id
```

and independently reference parent IDs.

For example, `assertion_evidence` may simultaneously reference:

```text
source_item_id
original_artifact_id
document_version_id
element_id
region_id
```

Nothing in the proposed schema proves those objects belong to:

- the same Vault;
- the same source chain;
- the same document version;
- the same artifact.

**Why it matters**

A row could technically bind:

- an element from document A;
- a document-version ID from document B;
- an artifact from account C;
- a source item unrelated to any of them.

Individual foreign keys would all pass.

That would create fabricated provenance while appearing relationally valid.

**Bounded correction**

Prefer one authoritative evidence anchor rather than duplicating ancestor IDs.

For example:

```text
assertion_element_evidence
  assertion_id
  element_id
  optional region_id
  evidence_role
```

The application derives:

```text
element
→ document_version
→ original artifact
→ acquisition
→ occurrence
→ source item
→ source
```

Where multiple evidence target types are needed, use typed tables.

For shared-DB designs, use composite keys and composite foreign keys including `vault_account_id`.

For separate per-Vault databases, still preserve the account ID redundantly and test contamination, but do not claim a filesystem `CHECK` can enforce it.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phases 2 through 5

---

## F-09 — Physical Vault identity enforcement is incomplete and partly impossible

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
E. Account scoping rule
H. Per-Account Vault Filesystem Layout
Q. Wrong-account restore
```

**Live/report evidence**

Claude says every table’s account ID will be enforced by:

```text
a CHECK against the Vault identity marker
```

SQLite cannot evaluate `VAULT_IDENTITY.json` from a table `CHECK`.

The live `connect(path)` function:

- creates parent directories;
- permits SQLite to create a missing file.

**Why it matters**

The identity marker is useful, but it cannot be the only authority guard.

A misplaced database, copied marker, typo, or wrong path must fail before the application creates or writes anything.

**Bounded correction**

Use a two-part identity contract.

### Inside SQLite

Create a singleton table such as:

```text
vault_metadata
  singleton_id CHECK(singleton_id = 1)
  vault_account_id UNIQUE
  vault_instance_id UNIQUE
  vault_schema
  created_at
  identity_fingerprint
```

### On filesystem

`VAULT_IDENTITY.json` contains the same:

```text
vault_account_id
vault_instance_id
vault_schema
created_at
identity_fingerprint
```

Use a random `vault_instance_id`; do not derive the entire identity only from public fields.

### Open behavior

Separate:

```text
create_vault()
open_existing_vault()
```

`open_existing_vault()` must:

- require the root and database to exist;
- read the identity marker before calling a connection function that can create a DB;
- open and validate `vault_metadata`;
- compare all identity fields;
- reject symlinks/reparse points;
- refuse a mismatch before migration or write.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 1

---

## F-10 — The separate Alembic environment is not executable as described

**Classification:** implementation-design defect
**Severity:** Critical

**Affected Claude section**

```text
G. Alembic Migration and Rollback Plan
S. Exact Proposed Application-Repository Change Set
U. Validation and Evidence Plan
```

**Live repository evidence**

`run_guarded_upgrade()`:

```text
Config(migrations_root.parent / "alembic.ini")
set script_location = migrations_root
SELECT version_num FROM alembic_version
```

`recover_completed_migration()` also requires:

```text
alembic_version
```

Claude proposes:

```text
migrations/versions/V0001…V0006
migrations/vault/
vault_alembic_version
target_revision='vault@head'
modify migrations/env.py
leave migration_runner.py untouched
leave migration_integrity.py untouched
```

Those elements do not form one valid migration layout.

If `migrations_root` is:

```text
migrations/vault
```

the live runner looks for:

```text
migrations/alembic.ini
```

which does not currently exist.

A distinct migration tree normally uses:

```text
head
```

not necessarily `vault@head`, unless branches are deliberately combined.

**Additional incompatibilities**

The separate Vault DB needs:

```text
operation_keys
audit_events
```

if existing helpers are reused.

Claude’s `V0001` sequence does not list `operation_keys` and names the audit table `vault_audit_events`.

**Bounded correction**

Choose one exact migration topology.

A coherent example:

```text
vault_migrations/
  env.py
  script.py.mako
  manifest.sha256
  versions/
    V0001...
vault_alembic.ini
```

Then parameterize or add a Vault-specific migration runner that explicitly receives:

- config path;
- migrations root;
- version table name;
- expected head;
- manifest root.

Alternatively, retain the table name `alembic_version` inside each separate Vault DB. Separate databases do not require distinct version-table names.

The corrected plan must identify exact changes to:

```text
migration_runner.py
migration_integrity.py
build_desktop_sidecar.py
packaging tests
migration tests
```

`migrations/env.py` does not need modification merely to change `script_location`; the runner already changes that setting.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 1

---

## F-11 — Per-Vault audit and idempotency are internally contradictory

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
A. Compatibility constraints 3 and 4
D. Existing Application Compatibility Map
E.11 Audit
S. vault_persistence.py
```

**Live/report evidence**

Claude first states:

```text
Any new governed mutation must extend the existing audit_events chain,
not a parallel audit table.
```

It later recommends one physically separate database per Vault and a separate:

```text
vault_audit_events
```

chain.

It also says existing `append_audit`, `verify_audit_chain`, and `operation_keys` are reused unchanged.

They cannot be reused unchanged with different table names or missing tables.

**Why it matters**

With separate databases, there cannot be one transactionally global audit chain across the control-room DB and every Vault DB.

Cross-store operations also cannot append to both chains atomically.

**Bounded correction**

For a separate-DB topology:

- use the same local table names `audit_events` and `operation_keys` inside every Vault DB, allowing reuse of current helpers; or
- parameterize the helpers explicitly.

Define:

```text
one audit chain per physical authority database
```

Cross-database orchestration must use:

```text
shared operation/correlation ID
central audit event
Vault audit event
reconciliation proof if one side fails
```

Do not claim atomicity across databases.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 1

---

## F-12 — Desktop/backend routing for multiple physical Vaults is missing

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
D. Web/Tauri boundary
S. API / web / Tauri
T. Phases 4 and 5
```

**Live repository evidence**

The Tauri backend supervisor passes:

```text
DISCREPANCY_DESK_DESKTOP_DATABASE
DISCREPANCY_DESK_DESKTOP_EVIDENCE_ROOT
DISCREPANCY_DESK_DESKTOP_MIGRATIONS_ROOT
```

FastAPI stores one database path.

`_connection()` always opens that one database.

Claude claims:

```text
all desktop/src-tauri/* untouched
```

and lists no desktop TypeScript pages, API types, or account/Vault-selection changes.

**Why it matters**

A separate per-Vault database design requires an exact routing authority:

- how a Vault is discovered;
- where the Vault registry lives;
- how a requested Vault ID maps to a root;
- how path traversal is prevented;
- how a Vault is opened and migrated;
- how the active Vault is selected;
- how health reports central and Vault state;
- how the operator uses the feature through Tauri.

Adding `/vault/*` routes to `web.py` does not answer those questions.

**Bounded correction**

Define an explicit backend routing layer:

```text
central control-room DB
→ Vault registry
→ verified Vault root
→ verified Vault metadata
→ bounded Vault connection
```

A request must provide an opaque Vault ID, never a filesystem path.

The service resolves that ID through a trusted registry and verifies the marker/database identity before opening.

The corrected change surface must determine whether Vault base can safely be derived from the existing app-data root. If not, the Rust launcher must pass a Vault-root configuration value.

At minimum, expect changes to:

```text
web.py
desktop/src/api/client.ts
desktop/src/api/types.ts
desktop pages/navigation/state
desktop contract tests
health/system reporting
```

Rust may remain unchanged only if the corrected path-derivation contract proves it.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phases 1, 4, and 5

---

## F-13 — Backup treatment of derived SQLite data is contradictory

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
E.11 Search/projection/chunk tables
F. Backup transaction
Q. Backup, Restore, and Rebuild Plan
```

**Live repository evidence**

The accepted backup mechanism uses SQLite’s complete database backup API.

A complete SQLite backup copies every table in the file, including:

```text
FTS tables
search_index_state
search_invalidation_work
chunks
projection_manifests
```

Claude states these tables are excluded from canonical backup.

It also says the filesystem `COMPLETE` marker and `backup_generations.completed=1` are set in “the same governed step.” They cannot be committed atomically across filesystem and SQLite.

**Why it matters**

A restored database may contain stale derived rows even though those rows are declared rebuildable.

An incomplete backup could have disagreement between:

- source DB receipt;
- copied SQLite file;
- manifest;
- completion marker.

**Bounded correction**

Choose and document one honest model.

### Recommended compatibility model

Use a full SQLite snapshot, but declare:

```text
derived rows may be physically present in the snapshot
derived rows are non-authoritative
restore clears or invalidates them before use
restore rebuilds them from canonical records
```

The manifest records which table classes are canonical and derived.

Define an exact completion sequence with reconciliation, for example:

1. create generation workspace;
2. record generation-start receipt;
3. take SQLite snapshot;
4. copy canonical filesystem objects/packages;
5. write manifest;
6. hash manifest;
7. write `COMPLETE`;
8. append final generation-complete audit in the live Vault DB;
9. reconcile any crash between steps 7 and 8.

Do not claim filesystem and DB completion are one transaction.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Backup foundation and final closure

---

## F-14 — Rights, retention, retrieval, export, and public eligibility are conflated

**Classification:** authority or safety blocker
**Severity:** High

**Affected Claude section**

```text
E.3 original_artifacts
I. Original Artifact Contract
L. Search
O. Projections
Q. Backup
```

**Report evidence**

Claude proposes:

```text
rights_class:
  unspecified
  owner_supplied
  third_party
  restricted

retention_class:
  arbitrary text
```

It then treats:

```text
restricted
```

as grounds to exclude material from local search and projections.

It also describes original artifacts as immutable while suggesting rights fields may later restrict them.

**Why it matters**

These are different questions:

- May the bytes be retained?
- For how long?
- May they be searched internally?
- May they enter an LLM context packet?
- May they be exported?
- May they appear in a generated internal projection?
- May they be publicly published?
- May excerpts be quoted or redistributed?
- Must they be purged after a deadline?

A document may be:

```text
permitted for private search
prohibited from public projection
permitted for temporary retention
prohibited from export
```

One `restricted` value cannot express that.

Updating rights on an “immutable” artifact row also contradicts the immutability claim.

**Bounded correction**

Create immutable, policy-versioned rights and retention bindings separate from artifact bytes.

The contract should distinguish at least:

```text
retention disposition
retention deadline
internal retrieval eligibility
context-run eligibility
export eligibility
internal projection eligibility
public projection eligibility
quotation/redistribution eligibility
```

Changes create new bindings and policy-impact work; they do not rewrite the artifact record.

Default behavior must fail closed.

The plan must also address retention expiry and governed purge/tombstone behavior.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phases 2, 4, 5, and backup/restore

---

## F-15 — Parser admission records are not exact or machine-enforceable

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
K. Parser Admission Records
S. Parsers
T. Phase 3
```

**Report evidence**

Several parser records still contain unresolved alternatives:

```text
markdown-it-py OR in-house block splitter
lxml OR html5lib OR defusedxml-guarded HTML
license: verify
version: pin later
```

The proposed machine artifact is primarily:

```text
parsers/ADMISSIONS.md
```

No parser-definition or parser-admission authority is present in the proposed schema.

**Why it matters**

M06-D11 requires each parser to have:

- an admission record;
- fixture corpus;
- failure behavior;
- provenance contract;
- owner authorization before use.

A Markdown file alone cannot prevent an unadmitted parser from being invoked.

The HTML proposal is especially unresolved. `defusedxml` is not a general HTML parser, while `lxml` introduces native-library and resolver behavior that affects packaging and egress proof.

**Bounded correction**

Define:

```text
parser_definitions
parser_admission_versions
parser_configuration_versions
```

or an equivalently hash-bound, machine-validated admission manifest.

Each admitted parser record must fix:

- parser ID;
- exact implementation/library;
- exact version;
- license;
- supported subset;
- media types;
- size/page/depth/count limits;
- warning codes;
- terminal failure rules;
- network behavior;
- deterministic-output contract;
- fixture corpus version;
- admission state;
- owner approval receipt.

Sequence parser implementation:

1. parser execution framework;
2. stdlib-only parsers;
3. one external dependency at a time;
4. packaging and egress proof for each;
5. admission only after its tests pass.

Any dependency change must include:

```text
pyproject.toml
uv.lock
PyInstaller/package tests
license record
```

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 3

---

## F-16 — Parser network-egress denial is not technically enforceable as written

**Classification:** authority or safety blocker
**Severity:** Critical

**Affected Claude section**

```text
K. Global parser rules
K.6 RSS/Atom
K.7 HTML
K.8 PDF
R. VT-23, VT-25, VT-26, VT-30
```

**Report evidence**

The package relies on statements such as:

```text
no parser may import a networking client
URLs are inert
test proves no socket
```

No runtime denial mechanism is defined.

**Why it matters**

A parser or dependency can reach the network through:

```text
socket
urllib
http clients
subprocess
native XML/HTML resolvers
external commands
```

A hostile file does not need to contain Python code to trigger unsafe resolver behavior in a parser library.

“No networking client imported” is not a security boundary.

**Bounded correction**

Define an exact parser execution boundary.

A defensible local Python design should include:

- parser worker separated from normal service logic;
- allowlisted parser modules;
- Python audit hook or equivalent denial for socket connect/listen, DNS, and subprocess execution;
- explicit monkeypatch/interception in tests;
- custom XML/HTML resolver that rejects all external resolution;
- no native parser admitted until its resolver behavior is proven;
- local loopback canary server proving no connection occurs;
- malicious fixture attempting socket, DNS, URL resolver, and subprocess paths;
- fail-closed worker termination on an egress attempt.

For native dependencies, Python-level interception alone may be insufficient. Their admission records must state how native network/resolver paths are denied.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 3

---

## F-17 — Search, chunk, and projection design needs executable detail

**Classification:** implementation-design defect
**Severity:** Medium

**Affected Claude section**

```text
E.11
L. Search and Deterministic Chunk Plan
O. Read-Only Projection Plan
W-3
```

**Report evidence**

Claude calls `vault_fts` an external-content FTS table but does not define:

- the content table;
- integer rowid mapping;
- synchronization strategy;
- trigger/rebuild behavior.

Canonical IDs are TEXT UUIDs, while FTS external-content designs normally depend on integer rowids.

The proposed fallback is:

```text
LIKE/instr
```

which is not equivalent to the accepted SQLite full-text search requirement.

`document_versions` also has no exact current/superseded state even though search requires current versions only.

**Why it matters**

Search must not return:

- superseded versions;
- withdrawn records;
- policy-restricted material;
- cross-account content.

A vague FTS schema cannot support exact migration or hammer tests.

**Bounded correction**

Treat packaged FTS5 availability as an admission test, not an owner preference.

The corrected plan must define:

- exact FTS table mode;
- rowid mapping;
- tokenizer;
- rebuild transaction;
- current-version selection;
- policy/retention filtering;
- invalidation state machine;
- malformed query handling;
- packaged sidecar feature probe.

If packaged FTS5 is unavailable, Phase 4 returns to planning. Do not silently treat `LIKE` as equivalent full-text search.

Projection manifests must bind to an exact canonical source digest and must be regenerated after rights, correction, policy, or supersession changes.

**Blocking effect**

- Planning acceptance: No, once recorded as a Phase 4 correction
- Implementation authorization: Phase 4 only
- Phase-specific: Phase 4

---

## F-18 — The hammer matrix is not yet executable

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
R. Full Adversarial and Hammer-Test Matrix
U. Validation and Evidence Plan
X. Completeness Matrix
```

**Live repository evidence**

The live hammer runner maps each invariant to exact pytest node IDs.

Claude says its matrix contains columns for:

```text
Evidence
Closure
```

but the displayed matrix does not contain those columns.

It also lacks:

- exact pytest nodes;
- fixture IDs and versions;
- positive controls;
- real-engine requirement;
- exact evidence files;
- phase-closure requirement;
- executable detector/non-detection implementation.

The live hammer runner currently returns success when no executed invariant fails. It does not independently fail merely because the execution list is empty.

**Why it matters**

The 75 rows are useful threats, but they are not yet proof that:

- tests exist;
- tests execute;
- the real engine is used;
- closure cannot be claimed from an empty run;
- fabricated evidence cannot be accepted.

Several rows claim that “no code path exists,” which requires an exact structural test—not an assertion in prose.

**Bounded correction**

Produce an executable matrix containing, for every invariant:

```text
VT ID
governing requirement
valid fixture
deliberate violation fixture
exact pytest node IDs
real-engine requirement
expected result
evidence destination
phase gate
closure requirement
```

Modify the runner contract so it fails when:

- zero required invariants execute;
- a required invariant has no test mapping;
- an evidence file is absent;
- the evidence commit SHA differs;
- any required invariant is deferred without an approved disposition;
- a detector errors;
- a detector is skipped;
- the matrix and runner IDs diverge.

The matrix should group related VT cases into executable HT invariants where useful, rather than mechanically creating 75 independent process launches.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Every phase

---

## F-19 — Cross-database account and publication references are not authority-safe

**Classification:** authority or safety blocker
**Severity:** High

**Affected Claude section**

```text
E.1 vault_account_owned_accounts
E.9 policy_impact_records
E.10 publication_approval_refs
G. Cross-database dependencies
W-1 and W-4
```

**Report evidence**

Under the recommended separate-DB topology:

```text
owned_accounts
approvals
```

remain in the central control-room database.

Claude nevertheless describes FK-like Vault tables referencing them.

It later concedes that approval IDs would be:

```text
opaque cross-DB references validated only advisorily
```

**Why it matters**

SQLite cannot enforce a foreign key into another database file.

An opaque approval ID can be:

- fabricated;
- stale;
- from another account;
- bound to a different revision;
- consumed;
- superseded;
- deleted from current authority.

A Vault must never infer publication authority from such a pointer.

**Bounded correction**

Separate evidence references from authority checks.

A Vault may retain a non-authoritative external-reference receipt containing:

```text
central database identity/fingerprint
external record type
external ID
observed binding hash
observed account ID
observed state
verified_at
verification result
```

It must be labeled as an observation, not a foreign key or authority.

Any operation that depends on a live central approval must query and validate the central control-room service at the time of the operation.

Preferably:

- the central control-room DB owns links affecting publication workflow;
- the Vault owns research and editorial-use records;
- no `publication_approval_refs` table in the Vault is treated as canonical publication authority.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Phase 5

---

## F-20 — Implementation phases and file-change surface are not yet safely bounded

**Classification:** implementation-design defect
**Severity:** High

**Affected Claude section**

```text
S. Exact Proposed Application-Repository Change Set
T. Recommended Implementation Phases
V. Rollback
```

**Live/report evidence**

The report claims only two existing Python files need changes:

```text
web.py
migrations/env.py
```

It leaves these untouched:

```text
migration_runner.py
migration_integrity.py
build_desktop_sidecar.py
desktop TypeScript
```

Live behavior proves those files or equivalent new wrappers are involved.

The report also delays all backup/restore work until Phase 6, after canonical artifacts and assertions have already been introduced.

It cites non-destructive downgrade as general precedent, but:

```text
0001 drops tables
0002 drops tables
0004 drops tables
```

Only `0003` deliberately avoids destructive column downgrade.

**Why it matters**

Canonical dual-store data should not exist across several phases without at least a foundational backup and restore path.

The exact change surface is a required planning output. Understating it defeats owner review and audit.

**Bounded correction**

Revise the phase sequence so recovery appears when canonical filesystem data first appears.

A safer sequence is:

```text
Phase 1
  Vault identity, actor contract, audit, idempotency,
  migration environment, routing skeleton

Phase 2
  observations, acquisitions, artifact-object storage,
  reconciliation, foundational per-Vault backup/restore

Phase 3A
  parser execution/package framework
  plain text, SRT, VTT, JSON

Phase 3B
  Markdown, RSS/Atom, HTML, PDF admitted one at a time

Phase 4
  lexical search, chunking, projections

Phase 5
  assertions, questions, entities, dossiers,
  policy bindings, editorial-use workflows,
  static context-run validation

Phase 6
  complete backup/rebuild proof,
  complete hammer closure,
  packaging and desktop parity
```

The corrected file surface must reassess:

```text
migration_runner.py
migration_integrity.py
web.py
desktop TypeScript API/types/pages/state
build_desktop_sidecar.py
pyproject.toml
uv.lock
hammer runner
packaging tests
health/system routes
migration configuration
```

Every downgrade must either:

- prove the affected phase is empty;
- refuse;
- direct the operator to restore.

**Blocking effect**

- Planning acceptance: Yes
- Implementation authorization: Yes
- Phase-specific: Entire package

---

# G. Schema and Relational Integrity Review

## Required additions

The corrected physical model must include or explicitly resolve:

```text
vault_metadata
actors
observations
unresolved_questions
parser_definitions
parser_admission_versions
parser_executions
artifact_objects
acquisition_artifact_links
operation_keys
audit_events
```

## Required relational corrections

### Artifact chain

Recommended:

```text
source
→ source_item
→ occurrence
→ observation
→ acquisition
→ acquisition_artifact_link
→ artifact_object
→ parser_execution
→ document_version
→ element
→ region
```

### Assertion evidence

Do not duplicate every ancestor ID in one row.

Use typed evidence-link tables anchored to the most specific canonical object.

### Account integrity

For shared database designs, composite account-scoped FKs are mandatory.

For separate database designs, retain redundant account IDs for contamination detection, but bind them to singleton Vault metadata.

### Correction lineage

Generic `target_ref_type + target_ref_id` is not sufficient for authority-bearing correction records without an exact typed integrity mechanism.

### Questions

A dossier membership cannot reference a `question` type until a canonical question object exists.

### Parser identity

Parser admission authority must be canonical or hash-bound and machine-checked. A prose Markdown file is not enough.

---

# H. Transaction and Dual-Write Review

## Sound part of Claude’s approach

The proposed ordering is directionally correct:

```text
write to temp
→ hash
→ move into immutable final path
→ commit SQLite reference
→ reconcile orphan states
```

The SQLite commit can serve as the authority point that the filesystem object is admitted.

## Required corrections

### Duplicate objects

The filesystem object may pre-exist while the acquisition is new. That is not an error. The transaction must link the new acquisition to the existing object.

### Acquisition state

Do not initialize an attempt as failed.

### Parser execution

Parser receipts and deterministic packages must be separate.

### Platform durability

The target is Windows. The plan must not assume POSIX directory `fsync` semantics without proving the Windows implementation.

### No-overwrite rename

The exact primitive must be specified. Some rename/replace APIs overwrite existing targets.

The operation must fail or enter duplicate handling when the final content-addressed path already exists.

### Orphan recovery

A filesystem object with no admitted DB link must never be automatically trusted based only on its hash. Re-linking requires a valid pending operation receipt and exact matching metadata.

### Backup completion

Backup DB receipt and filesystem completion marker cannot be one transaction. The crash states must be enumerated and reconciled.

---

# I. Vault Identity and Multi-Database Review

Separate physical Vault roots are accepted.

A separate SQLite file per Vault is a credible and safer implementation, but accepted doctrine does not explicitly say that the DB file itself must be separate.

Before owner decision, the corrected plan must define:

- canonical Vault-account meaning;
- Vault-to-platform-account cardinality;
- Vault registry location;
- Vault instance identity;
- root-to-database binding;
- create versus open behavior;
- wrong-root and wrong-database rejection;
- routing from desktop requests;
- migration timing;
- audit topology;
- backup topology.

The filesystem marker is a defense, not standalone authority.

The DB must contain its own singleton Vault identity.

---

# J. Migration Isolation Review

The current migration system can support another migration root only after exact correction.

The corrected design must settle:

```text
configuration path
migration root
manifest root
version table
expected head
dirty marker
recovery behavior
packaged data path
```

A second physical database may still use:

```text
alembic_version
```

because version-table names need not be globally unique across different files.

Using the same table names:

```text
audit_events
operation_keys
```

inside each separate Vault DB is also compatible and allows more existing helpers to be reused.

The current proposal’s mixed use of:

```text
migrations/versions/V...
migrations/vault/
vault@head
vault_alembic_version
```

must be replaced by one exact layout.

---

# K. Parser Admission and Egress Review

## Recommended sequencing

### First admission group

Prefer parsers with no new dependency and a narrow deterministic contract:

```text
plain text
SRT
VTT
JSON
```

### Second admission group

Admit separately:

```text
Markdown
RSS/Atom
basic HTML
born-digital PDF
```

Each external dependency changes:

- supply chain;
- license;
- packaging;
- resource behavior;
- resolver behavior;
- determinism;
- sidecar size;
- egress risk.

## Dependency direction

The correction package should choose one implementation, not list alternatives.

For HTML, a narrow stdlib or pure-Python parser may be safer than a broad native parser if it satisfies the accepted “basic HTML” scope.

For PDF, the exact pinned `pypdf` version and its malformed-file limits must be tested.

For XML, external entities and DTDs must be rejected before normal parsing.

## Runtime admission

A parser appears in code only after:

```text
definition
fixture corpus
adversarial tests
no-egress proof
package proof
owner-authorized admission
```

---

# L. Search, Chunk, and Projection Review

The accepted direction is sound:

```text
SQLite lexical search
deterministic chunks
generated read-only projections
no Qdrant
```

Required corrections:

- exact FTS5 schema;
- exact packaged-sidecar feature probe;
- no silent `LIKE` substitution;
- exact current-document-version selection;
- rights/policy filtering separate from generic `restricted`;
- deterministic invalidation work;
- projection source-binding hash;
- projection regeneration after correction, policy, rights, or supersession;
- no projection ingestion route;
- tampered projections quarantined or overwritten according to one fixed rule.

FTS rank remains relevance only and must never influence evidence strength.

---

# M. Assertions, Actors, and Authority Review

The four-layer assertion model is conceptually strong.

However:

```text
epistemic_type='accepted_conclusion'
```

is dangerous naming if the same record still requires separate human support/review authority.

The plan correctly says the type alone is not authority, but the physical service must enforce that.

Required controls:

- only a human may create final support assessments;
- only a human may create editorial-use rulings;
- only a human may accept merge/split;
- system/model actors may create candidates only;
- public labels must come from account policy, not a hard-coded global enum;
- policy changes create explicit impact records;
- metrics have no write path into evidence strength;
- previous model outputs cannot be evidence merely because they were re-imported.

The hard-coded public-label set should become policy-defined label identity rather than a fixed SQL enum containing one account’s current vocabulary.

---

# N. Backup, Restore, and Rebuild Review

The proposed restore tests are strong and should be retained:

- modified manifest;
- modified artifact;
- missing original;
- wrong-account restore;
- orphan canonical record;
- audit-chain failure;
- derivative cross-account contamination;
- deterministic chunk and projection rebuild.

Required corrections:

1. admit that full SQLite backup may contain derived rows;
2. invalidate and rebuild derived rows before restored Vault use;
3. define generation-start and generation-complete receipts;
4. define every crash state around manifest and `COMPLETE`;
5. verify the Vault identity before restore;
6. restore only into an empty disposable workspace;
7. prevent restore from silently merging with existing data;
8. bind evidence to implementation commit and exact migration head;
9. define retention/purge effects on historical backups;
10. do not make `age` packaging a hidden closure dependency.

---

# O. Hammer-Matrix Review

The threat coverage is broad and valuable.

It covers nearly all important categories:

- account isolation;
- path traversal;
- symlinks/reparse points;
- duplicate/replay;
- partial writes;
- malformed parsers;
- egress;
- XXE;
- active HTML;
- PDF limits;
- package tamper;
- FTS injection;
- stale search/chunks;
- projection tamper;
- merge authority;
- policy impact;
- context truncation;
- citation validation;
- backup tamper;
- wrong-account restore;
- detector errors;
- empty execution;
- fabricated evidence;
- secret leakage;
- migration failure;
- audit tamper.

What is missing is executable binding.

The corrected matrix must map each threat to:

```text
specific test
specific fixture
specific expected result
specific evidence
specific phase gate
```

The existing hammer runner should remain the execution authority, not a parallel manually curated success list.

---

# P. Application Change-Surface Review

Claude’s proposed change surface is not accurate enough for owner authorization.

## Likely new components

```text
vault identity and registry
vault connection/router
vault persistence
vault filesystem/object storage
vault service
vault queries
parser framework
parser modules
search
chunks
projections
backup/restore
Vault migrations
Vault migration configuration
Vault tests
Vault evidence runners
```

## Existing files likely requiring modification or exact wrapper replacement

```text
src/discrepancy_desk/web.py
src/discrepancy_desk/migration_runner.py
src/discrepancy_desk/migration_integrity.py
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
pyproject.toml
uv.lock
desktop/src/api/client.ts
desktop/src/api/types.ts
desktop navigation/pages/state
desktop packaging tests
desktop API contract tests
```

Potentially:

```text
desktop/src-tauri/src/backend.rs
```

depending on whether the Vault root can be safely derived from the existing application-data paths.

## Files that should remain untouched unless separately justified

```text
existing migrations 0001–0004
existing M00–M05 approval/publication behavior
existing revision binding
existing platform publication authority
existing metric authority
```

---

# Q. W-1 Through W-5 Disposition

## W-1 — Backfill Vault account scope onto legacy M00–M05 tables

```text
Disposition: resolved by live repository constraints
```

Do not backfill or alter the accepted M00–M05 tables in M06-A merely to introduce Vault identity.

The live `owned_accounts` table represents platform-owned accounts. M06-A should introduce an explicit, separately governed Vault identity and binding contract.

Any later central-table alteration requires a separate exact migration and regression package.

**Owner decision required:** No.

---

## W-2 — Separate per-Vault SQLite file or shared DB

```text
Disposition: genuine owner decision
```

Accepted doctrine requires separate physical Vaults and account-bound SQLite authority, but it does not explicitly settle whether the SQLite file is:

```text
one per Vault
```

or:

```text
shared centrally with account scoping
```

This choice affects:

- migration topology;
- audit topology;
- routing;
- backup;
- cross-database references;
- failure blast radius;
- account leakage risk.

The corrected package should recommend separate per-Vault SQLite files, but the owner should make the final ruling after the migration/routing design is corrected.

---

## W-3 — FTS5 packaged availability

```text
Disposition: implementation admission test
```

M06-D12 already requires SQLite lexical/full-text search.

This is not a preference question for the owner.

The packaged Windows sidecar must prove FTS5 availability. If unavailable, Phase 4 returns to planning for an accepted alternative.

No silent downgrade to `LIKE`/`instr`.

**Owner decision required:** No, unless the required engine is unavailable.

---

## W-4 — Editorial-use to publication gating strength

```text
Disposition: already resolved by accepted doctrine
```

M06-D05 already establishes:

- editorial-use ruling is separate;
- it may be a policy-required input;
- it never approves exact text;
- publication approval remains in M00–M05.

The M06-A Vault should not gain publication authority.

The exact account policy may determine whether an editorial-use ruling is required, but that is a policy record—not a new global owner architecture decision.

**Owner decision required:** No.

---

## W-5 — `age` required for M06-A closure

```text
Disposition: already resolved by accepted doctrine
```

M06-D13 says the proven `age` path may protect archives and packaged key management remains M15.

M06-A closure requires:

- canonical backup;
- integrity;
- disposable restore;
- tamper proof.

It does not require packaged key-management UX.

The current `age` path may be regression-tested where available and honestly reported when unavailable.

**Owner decision required:** No.

---

# R. Genuine Owner Decisions Remaining

## OD-1 — Canonical meaning of a Vault account

The accepted documents use “account,” but the live application’s `owned_accounts` rows are platform handles.

The corrected plan must ask whether one physical Vault represents:

### Option A — Editorial/public brand identity

Example:

```text
The Discrepancy Desk
```

One Vault may be associated with multiple platform-owned accounts such as X and later Truth Social.

### Option B — One platform-owned account

Example:

```text
@DiscrepancyDesk on X
Truth Social account separately
```

Each receives a different Vault.

### Recommendation

Use the editorial/public brand identity as the Vault account and bind platform-owned accounts to it through explicit governed relationships.

This matches the research-memory purpose better and avoids duplicating the same research corpus merely because the brand uses two platforms.

This decision affects physical Vault count, routing, backup, policy scope, and cross-platform editorial use.

---

## OD-2 — Physical SQLite topology

After OD-1 is resolved, choose:

```text
separate SQLite file per physical Vault
```

or:

```text
shared SQLite authority with account-scoped rows
```

### Recommendation

Separate SQLite file per physical Vault.

This best matches the accepted physical-isolation direction and minimizes cross-account leakage and M00–M05 migration blast radius.

---

## OD-3 — Retention expiry and governed destruction

Accepted doctrine requires immutable artifacts and preservation, but it does not settle what happens when rights or retention policy requires deletion.

Choose:

### Option A — No purge in M06-A

M06-A admits only material whose retention terms permit preservation until a later governed purge capability exists.

Any material requiring timed deletion is rejected.

### Option B — Governed purge in M06-A

M06-A includes:

- purge authorization;
- tombstone;
- affected-assertion impact;
- derived invalidation;
- backup-generation handling;
- audit proof.

### Recommendation

Choose Option A for the first package.

It keeps M06-A bounded and prevents a rushed destructive-authority system. Material requiring timed deletion should fail admission until a separately reviewed purge contract exists.

---

# S. Required Correction Package

Before owner decisions or implementation authorization, prepare one bounded correction package containing the following.

## 1. Canonical object corrections

- add observations;
- add unresolved questions;
- separate artifact objects from acquisition receipts;
- separate parser executions from deterministic packages;
- define current/superseded document-version behavior.

## 2. Actor and authority contract

- authoritative actor identity;
- human/system/model actor classes;
- human-only service gates;
- audit binding;
- candidate-only system/model behavior.

## 3. Referential integrity contract

- typed corrections;
- typed dossier memberships;
- typed evidence links;
- same-account enforcement;
- provenance-chain enforcement;
- policy-impact targets.

## 4. Vault identity contract

- Vault-account semantics;
- singleton DB identity;
- filesystem identity marker;
- create/open separation;
- wrong-root and wrong-DB refusal.

## 5. Exact migration environment

- exact directory tree;
- exact Alembic configuration;
- exact runner changes;
- exact version-table behavior;
- migration manifest;
- dirty-state recovery;
- packaged sidecar inclusion.

## 6. Desktop routing contract

- Vault registry;
- safe ID-to-path resolution;
- active Vault selection;
- on-demand open/migration;
- health reporting;
- API and UI surface.

## 7. Rights and retention contract

- retention;
- internal search;
- context use;
- export;
- projection;
- public use;
- purge admission boundary.

## 8. Exact parser admissions

- one dependency choice per parser;
- versions;
- licenses;
- limits;
- warning/failure vocabulary;
- fixture versions;
- machine enforcement;
- egress denial.

## 9. Search/chunk/projection contract

- exact FTS schema;
- packaged FTS test;
- invalidation;
- current-version selection;
- rights filtering;
- deterministic projection binding.

## 10. Backup and restore contract

- honest treatment of derived SQLite rows;
- completion sequence;
- crash matrix;
- restore invalidation/rebuild;
- retention effects.

## 11. Executable hammer matrix

- exact pytest nodes;
- fixtures;
- expected outcomes;
- evidence files;
- phase closure;
- empty-run failure;
- matrix/runner reconciliation.

## 12. Corrected implementation phases and file surface

- foundational backup earlier;
- parser subphases;
- exact modified files;
- exact stop conditions;
- exact rollback boundaries.

---

# T. Planning Readiness

## Architecture readiness

```text
Ready
```

The accepted M06 architecture remains coherent.

No M06-D01 through M06-D16 decision needs to be reopened.

## Claude plan readiness

```text
Not ready for owner decision on the current W-1 through W-5 set
Not ready for implementation authorization
Ready to enter a bounded correction/disposition package
```

The package is strong enough to correct rather than replace.

The correction package must be independently reviewed before the owner rules on:

```text
Vault-account meaning
physical SQLite topology
retention/purge boundary
```

## Parser readiness

```text
No parser is admitted
```

## Migration readiness

```text
No migration environment is accepted
```

## Database readiness

```text
No Vault database may be created
```

---

# U. Recommended Next Action

1. Preserve the Claude report unchanged as external planning input.
2. Record this independent review only after owner direction.
3. Prepare the bounded canonical M06-A correction package described in Section S.
4. Independently review the corrected package against live repositories.
5. Present only the three genuine owner decisions in Section R.
6. After owner rulings, reconcile the exact schema, migration topology, routing, phases, tests, and file surface.
7. Require explicit owner implementation authorization before any code or runtime work.

The next action is correction and disposition—not implementation with a clipboard taped to its face.

---

# V. Explicit Authority Block

No M06-A implementation authority exists.

The Claude planning package remains external planning input.
This review does not authorize edits, migrations, databases, Vault
creation, parser work, dependency installation, runtime changes, or
implementation.

Any accepted result must first be dispositioned into a bounded canonical
planning correction package, independently reviewed, owner-accepted, and
explicitly authorized for implementation.
