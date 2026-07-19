# M02 Migration and Hammer-Execution Plan

## Status

Planning-only M02 artifact. No SQL, migration file, SQLite database, ORM model, CRUD code, application persistence, or dashboard implementation is authorized.

## Purpose

Translate the owner-approved persistence contract, lifecycle model, UD-01 through UD-12 decisions, and 20-invariant adversarial matrix into the bounded evidence and execution requirements that a later M03 implementation package must satisfy.

This document defines sequence, gates, fixtures, proof, and recovery expectations. It does not define physical tables, columns, data types, indexes, triggers, or migration code.

## Governing Inputs

- `m02-operational-persistence-contract.md`
- `m02-lifecycle-state-model.md`
- `m02-owner-approved-persistence-decisions.md`
- `m02-adversarial-test-matrix.md`
- `hammer-test-strategy.md`
- M01 exact-byte evidence and payload analysis
- D016, D019, and D020

## M03 Physical-Design Boundary

The first M03 package may design and implement only the record classes required for the smallest complete loop:

```text
owned platform account
capture/work item
source reference and bounded source note
canonical evidence reference
immutable content revision
human review decision
exact-revision approval binding
manual-ready designation
manual publication record
metric observation snapshot
audit event
migration and recovery state
idempotent operation identity
```

The following remain excluded unless separately approved:

- claim or entity graph;
- generic model-output warehouse;
- reusable media library beyond approval-bound identity;
- mention operational workflow;
- repost and cross-platform publication expansion;
- Truth Social implementation;
- Anomaly Vault;
- No Coincidences;
- Qdrant;
- autonomous platform actions.

## Required Implementation Sequence

### Phase 1 — Physical design packet

Before code:

1. map every proposed physical record to one accepted conceptual record class;
2. identify the workflow operation that requires it;
3. identify every required uniqueness, foreign-key, state, evidence, and audit invariant;
4. identify the HT invariant IDs that attack it;
5. reject fields or records with no demonstrated M03 workflow need;
6. document path, hash, timestamp, observation-state, and exact-revision semantics;
7. obtain owner approval for the bounded physical design.

### Phase 2 — Fixture and evidence harness

Before the first migration is accepted, create versioned synthetic fixture classes for:

- known-valid complete workflow;
- invalid relationships and fabricated identifiers;
- illegal lifecycle transitions;
- exact-text and Unicode mutations;
- stale, reused, missing, and conflicting approval bindings;
- raw-evidence missing, moved, altered, truncated, re-encoded, and orphaned cases;
- duplicate and replay operations;
- concurrent writers and `SQLITE_BUSY`;
- partial transaction failure;
- audit mutation and broken-chain cases;
- explicit missing/empty/withheld/unavailable/malformed/error observations;
- dirty and interrupted migration cases;
- corrupted backup and three-way restore disagreement;
- detector failure and deliberate non-detection.

Every fixture must have an immutable fixture ID, version, purpose, input summary, expected result, and mapped HT invariant.

### Phase 3 — Empty-database baseline migration

The initial migration must be executed only after physical-design approval.

Acceptance requires:

- known migration-manifest hash;
- expected predecessor state;
- durable dirty marker before work;
- required SQLite connection settings;
- successful completion on a new disposable database;
- version, constraint, index, trigger, and object inventory verification;
- dirty marker cleared only after all verification succeeds;
- exact command and evidence preserved.

### Phase 4 — Real-engine invariant implementation

Implement and prove each HT-01 through HT-20 invariant against real SQLite wherever engine behavior matters.

A passing application-level test is insufficient when SQLite constraints, transaction behavior, locking, rollback, migration, or restore behavior is part of the invariant.

### Phase 5 — Dirty and interrupted migration proof

Before accepting migration infrastructure, deliberately interrupt or corrupt representative migration states, including:

- wrong or stale version marker;
- modified migration file;
- dirty marker present;
- leftover temporary table;
- interrupted table-copy step;
- missing predecessor;
- invalid legacy row;
- partial index or trigger recreation;
- rerun after uncertain interruption.

Each case must refuse normal operation and route only to a documented recovery action.

### Phase 6 — Backup and disposable restore proof

Create a complete generation containing the safe SQLite backup, canonical evidence files, manifests, non-secret configuration, migration state, and audit verification material.

Acceptance requires:

1. archive and encrypt the generation;
2. restore it to a clean disposable location;
3. verify archive and per-file hashes;
4. verify SQLite integrity and migration state;
5. verify audit chain;
6. reconcile database hashes, evidence manifests, and actual bytes;
7. verify exact approved text and identifier relationships;
8. run the accepted read-only validation suite;
9. preserve exact commands and results.

A copied archive without restore proof is not an accepted backup.

## SQLite Operating Contract for M03

Every operational connection must verify:

```text
PRAGMA foreign_keys = ON
journal_mode = WAL
busy_timeout = 5000 ms
write transaction start = BEGIN IMMEDIATE
```

The implementation must also verify supported SQLite and application/migration version state before writes.

Continued `SQLITE_BUSY` after the bounded timeout is a classified clean rejection. Blind retries are prohibited. A governed retry must reuse the original idempotency identity and first determine whether the previous attempt committed.

## Migration Authority and Integrity

Alembic is the sole migration authority.

A governed migration manifest must record the admitted migration files and SHA-256 values. Historical migration modification is a failure, not an update strategy.

Preflight must verify:

- database identity;
- current revision and expected predecessor;
- migration manifest integrity;
- no dirty marker;
- no unexpected temporary migration objects;
- SQLite integrity;
- required connection settings;
- accepted recovery point for destructive or table-copy operations.

Unknown or dirty state blocks normal application operation and further migration except explicit recovery.

## Audit Proof

Accepted state-changing operations must emit append-only hash-chained audit events in the same transaction.

Required tests include:

- update and delete rejection through normal paths;
- direct mutation detection;
- changed payload;
- missing, duplicated, or reordered event;
- broken predecessor hash;
- audit-write failure causing full operation rollback;
- full chain verification after backup and restore.

Digital signatures remain deferred.

## Evidence Proof Contract

Canonical raw evidence remains immutable governed files.

Operational references must use a configured evidence-root-relative locator and preserve SHA-256, byte size, provenance, content type when known, and verification state.

Required reconciliation detects:

- missing referenced file;
- orphan file;
- unsafe or escaping path;
- changed bytes;
- wrong size or hash;
- manifest disagreement;
- evidence linked to the wrong observation;
- stale database or evidence generation after restore.

No parser output or normalized record may satisfy exact-byte evidence requirements.

## Approval-Binding Proof

The first implementation must define a versioned deterministic aggregate binding over the exact owner-approved component bundle without Unicode, whitespace, or newline normalization.

Tests must mutate every approval-bound component independently and in combinations. Any changed component creates a successor revision and invalidates prior publication readiness.

Old bindings must remain reproducible under their original procedure version.

## Idempotency Proof

Each admitted operation must document:

- exact key composition;
- scope and retention;
- identical replay result;
- conflicting replay result;
- concurrent duplicate result;
- timeout and retry procedure.

The test suite must prove one accepted side effect at most.

## Required Test Evidence

Every hammer-test result must preserve:

- HT invariant ID;
- fixture ID and version;
- exact command;
- code and migration commit identity;
- SQLite and Python versions;
- relevant connection pragmas;
- starting migration state;
- expected and actual result;
- error class or constraint;
- transaction and rollback outcome;
- database, log, manifest, and evidence paths;
- SHA-256 values where applicable;
- defect linkage and corrective commit when a defect is found.

A green summary without inspectable evidence does not satisfy the gate.

## M03 First Package Recommendation

The first implementation package should be limited to:

1. repository/application skeleton only as needed for persistence tests;
2. governed configuration for database and evidence roots, excluding secrets;
3. SQLite connection factory enforcing required pragmas and version checks;
4. migration-manifest and dirty-state preflight infrastructure;
5. the smallest approved physical design for the complete manual workflow;
6. initial Alembic migration;
7. immutable evidence-reference and exact-revision binding utilities;
8. append-only audit-chain mechanism;
9. persistence operations required to exercise the complete loop;
10. HT-01 through HT-20 fixture and real-engine test harness;
11. backup-generation and disposable-restore proof tooling;
12. documentation and implementation-return evidence.

It must not include public UI polish, platform writes, recurring API reads, Truth Social integration, extension work, Anomaly Vault, Qdrant, No Coincidences, or autonomous behavior.

## M02 Exit-Gate Verification

M02 is ready for owner closure review when all of the following are true:

- owner-approved hammer doctrine exists;
- the 20-invariant adversarial matrix is accepted;
- the smallest operational persistence contract is accepted;
- lifecycle and approval invalidation rules are accepted;
- UD-01 through UD-12 are closed;
- deferred UD-13 through UD-18 do not leak into M03 scope;
- canonical evidence, migration, audit, locking, idempotency, retention, backup, and restore decisions are explicit;
- every required conceptual record maps to a demonstrated MVP workflow need;
- M01 payload assumptions remain bounded to what was observed;
- the migration and hammer-execution plan is explicit;
- the first M03 implementation package is bounded;
- no physical design or implementation has begun without approval.

## Next Gate

The next genuine owner gate is:

```text
Approve M02 closure and authorize preparation/execution of the bounded first M03 physical-design and persistence implementation package.
```

Until that approval, schema, migrations, SQLite creation, ORM models, CRUD code, application persistence, and dashboard implementation remain blocked.
