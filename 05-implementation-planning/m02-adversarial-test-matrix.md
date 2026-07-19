# M02 Adversarial Persistence Test Matrix

## Status

Owner-approved M02 baseline as of 2026-07-19. Planning only.

This document defines the minimum persistence invariants and deliberate violation cases required before any M03 implementation package may be authorized. The baseline may be strengthened during contract review but may not be silently weakened or omitted.

It does not approve tables, columns, migrations, a SQLite file, ORM models, CRUD code, or application implementation.

## Owner-Approved Decision Reconciliation

UD-01 through UD-12 were approved on 2026-07-19 and are governed by `m02-owner-approved-persistence-decisions.md`. Their accepted choices strengthen and resolve the enforcement assumptions behind this matrix:

- canonical evidence is immutable governed files, not parsed JSON or a competing canonical BLOB copy;
- revision approval uses exact component bytes and a versioned aggregate SHA-256 binding;
- rejected, withdrawn, mismatch, and blocked history reopens only through explicit successor action;
- audit events are append-only, transactionally required, and hash-chained;
- idempotency identity is operation-specific;
- observation states remain explicit;
- third-party evidence retention is bounded and deletion is owner-only and audited;
- publication confirmation and later platform verification remain distinct;
- metric corrections append rather than overwrite;
- SQLite uses WAL, `foreign_keys=ON`, a 5000 ms busy timeout, and `BEGIN IMMEDIATE` for writes;
- Alembic plus a migration-integrity manifest and durable dirty marker govern migration state;
- backup acceptance requires encrypted generations and disposable restore proof.

Every implemented HT-01 through HT-20 test must be consistent with these decisions. A test that silently substitutes a different boundary does not satisfy the matrix.

## Governing Rule

Every persistence rule must be attacked with deliberate bad data, illegal state transitions, fabricated identifiers, modified evidence, stale approvals, duplicate operations, partial failures, detector failures, migration failures, and recovery failures.

Where SQLite behavior matters, tests must run against the real SQLite engine. Any ambiguity, missing evidence, integrity mismatch, disabled enforcement, or unrecognized failure must fail closed.

## Evidence Required for Every Test

Each implemented test must eventually preserve:

- invariant identifier;
- fixture identifier and version;
- exact setup and command;
- database engine and version;
- connection pragmas relevant to the test;
- expected result;
- actual result;
- error class or constraint involved;
- transaction and rollback outcome;
- evidence paths and hashes;
- defect linkage when a failure reveals a defect.

A green test without preserved evidence is not sufficient milestone proof.

## Matrix

### HT-01 — Exact authored-text approval binding

**Invariant:** Human approval binds to the exact approved authored content and platform variant, not merely a draft identifier.

**Deliberate violations:**

- append or remove a character after approval;
- change whitespace, line endings, punctuation, capitalization, or URL;
- insert a zero-width character;
- change Unicode normalization while preserving visual appearance;
- substitute platform-returned text containing a generated `t.co` link;
- shorten, truncate, or otherwise render text differently;
- reuse an X approval for a Truth Social variant;
- reuse an approval after media, alt text, or claim labels change.

**Expected result:** Existing approval becomes invalid or superseded. Publication readiness is denied until a new human approval binds to the exact new revision.

**Real SQLite required:** Stored binding and transition checks must be exercised against real persisted rows.

### HT-02 — Approval identity and freshness

**Invariant:** Approval identifiers are authentic, unique, linked to an existing human review, and current for the exact revision.

**Deliberate violations:** Fabricated ID, missing ID, deleted review, stale revision, reused approval, cross-record approval, cross-platform approval, and approval created before the reviewed revision.

**Expected result:** Reject or fail closed; no publication-ready state.

### HT-03 — Legal lifecycle transitions

**Invariant:** Only owner-approved state transitions are admitted, and the persistence boundary rejects illegal jumps even when application code requests them.

**Deliberate violations:**

- captured directly to published;
- draft directly to published;
- rejected to published;
- deleted to active;
- published without exact approval;
- metrics before publication;
- publication-ready after approval invalidation;
- reopening a terminal state without an explicit governed transition.

**Expected result:** Engine or governed persistence operation rejects the transition atomically.

### HT-04 — Stable external identity and mutable observations

**Invariant:** Stable platform IDs are kept distinct from mutable username, display name, profile text, verification, and metrics observations.

**Deliberate violations:** Duplicate external account or post IDs, fabricated IDs, handle changes treated as new identity, mutable profile attributes used as foreign keys, and mismatched author/account relationships.

**Expected result:** No duplicate identity, false merge, or orphaned relationship is silently accepted.

### HT-05 — Canonical raw evidence integrity

**Invariant:** Normalized or operational records cannot replace canonical exact-byte evidence. Every promoted observation links to existing raw evidence and its verified hash.

**Deliberate violations:**

- missing raw file or BLOB;
- changed raw bytes;
- wrong SHA-256;
- normalized JSON substituted for exact bytes;
- evidence path redirected to another capture;
- fabricated evidence identifier;
- raw evidence without capture provenance;
- normalized record linked to a different endpoint response.

**Expected result:** Promotion, acceptance, or trusted use is blocked. Modified evidence is detected explicitly.

### HT-06 — DB-to-filesystem reconciliation

**Invariant:** If raw evidence remains outside SQLite, database references, paths, hashes, and actual filesystem bytes remain reconcilable.

**Deliberate violations:** Missing file, renamed file, modified bytes, duplicate path, path traversal, file with no database reference, database reference with no file, and hash sidecar disagreement.

**Expected result:** Missing or changed evidence fails closed; orphaned evidence is reported; unsafe paths are rejected.

**Real SQLite required:** Database references must be checked against real persisted rows and real evidence bytes.

### HT-07 — SQLite foreign-key enforcement

**Invariant:** Every operational SQLite connection verifies `PRAGMA foreign_keys=ON` before use.

**Deliberate violations:** Insert orphan relationships with enforcement enabled; open a connection with enforcement deliberately disabled; attempt cascading or restricted operations contrary to the accepted contract.

**Expected result:** Orphan insert is rejected. A connection with enforcement off is itself rejected or causes test failure before work proceeds.

**Real SQLite required:** Yes, per connection.

### HT-08 — Required uniqueness and idempotency keys

**Invariant:** Operations described as idempotent bind to named keys and engine-enforced uniqueness; ambiguous replay is never accepted incidentally.

**Deliberate violations:** Duplicate platform post ID, replayed publication record, double click, repeated import, duplicate operation token, URL variants resolving to the same admitted identity, and concurrent duplicate writes.

**Expected result:** Second operation is a proven no-op or receives a specific uniqueness rejection according to the approved contract. No duplicate side effects or records.

**Real SQLite required:** Yes.

### HT-09 — Transaction atomicity and partial failure

**Invariant:** Multi-record operations either commit completely or leave no accepted partial state.

**Deliberate violations:** Failure after parent insert, after evidence link but before audit event, after approval invalidation but before new revision, constraint failure late in a transaction, process interruption, and exception during commit handling.

**Expected result:** Full rollback or explicit recoverable state; no half-published, half-approved, or unaudited accepted record.

**Real SQLite required:** Yes.

### HT-10 — Concurrency, locking, and busy handling

**Invariant:** The selected journal mode, transaction mode, and bounded busy timeout produce deterministic serialize-or-clean-reject behavior.

**Deliberate violations:** Two concurrent writers, reader/writer overlap, long transaction, retry after `SQLITE_BUSY`, process crash while holding a transaction, and duplicate concurrent publication writes.

**Expected result:** No partial commit, lost update, silent overwrite, duplicate record, or unbounded retry.

**Real SQLite required:** Yes. Record `journal_mode`, `busy_timeout`, and transaction mode.

### HT-11 — Audit-history integrity

**Invariant:** Accepted audit history cannot be silently edited or deleted through normal operations, and out-of-band tampering is prevented or detectable.

**Deliberate violations:** Direct update, delete, reordered event, changed payload, fabricated event, missing predecessor, duplicate event ID, and audit write failure during an otherwise successful operation.

**Expected result:** Mutation is rejected or verification reports integrity failure. An operation requiring an audit event cannot report success without one.

**Real SQLite required:** Yes for constraints/triggers; verification logic must also be tested.

### HT-12 — Metrics snapshot integrity

**Invariant:** Metrics are immutable observations with platform, method, source context, and capture time.

**Deliberate violations:** Negative or impossible counts, snapshot before publication, missing timestamp, wrong platform, wrong post, duplicate snapshot, silent historical rewrite, and manual observation mislabeled as API observation.

**Expected result:** Invalid snapshots are rejected; later observations append rather than overwrite history.

### HT-13 — Empty, missing, unavailable, withheld, and errored values

**Invariant:** Distinct unknown states are not silently collapsed into empty strings, zero, false, or invented defaults.

**Deliberate violations:** Missing field normalized to zero, API error stored as an empty successful result, withheld value treated as absent, empty string treated as unknown, and detector exception treated as no findings.

**Expected result:** State remains explicit and queryable; uncertain or errored data cannot satisfy a validity gate.

### HT-14 — Mentions and human classification

**Invariant:** Original external-interaction evidence remains preserved independently of later human classification.

**Deliberate violations:** Mark spam and delete evidence, classifier failure auto-marks relevant, link presence triggers an action, human rejection is overwritten by a later model run, and malformed mention is silently normalized.

**Expected result:** Evidence remains; classifications are reviewable observations; no automated engagement or truth promotion occurs.

### HT-15 — Platform boundary isolation

**Invariant:** X and Truth Social records cannot satisfy each other's capture, approval, publication, or metrics requirements.

**Deliberate violations:** X API evidence attached as Truth Social capture, Truth Social manual note labeled API-derived, X approval reused for Truth Social, or a prohibited Truth Social automation classification inserted.

**Expected result:** Reject the mismatch. Current Truth Social manual-only policy remains enforceable.

### HT-16 — Migration preflight and dirty-state rejection

**Invariant:** Migration execution refuses unknown, partial, dirty, modified, or unsupported starting states.

**Deliberate violations:** Wrong schema version, modified migration file, missing predecessor, dirty version marker, leftover temporary table, invalid legacy row, and already-partially-copied table.

**Expected result:** Fail before destructive action; do not invent defaults, truncate evidence, or continue from ambiguity.

**Real SQLite required:** Yes.

### HT-17 — Interrupted SQLite batch migration

**Invariant:** Any future table-copy or batch migration either completes with verified equivalence or leaves a detected, recoverable failure state.

**Deliberate violations:** Interrupt during copy, index recreation, constraint recreation, old-table drop, rename, or version update; rerun after interruption.

**Expected result:** No false success and no silently usable half-migrated schema. Recovery or rollback procedure is explicit and proven.

**Real SQLite required:** Yes.

### HT-18 — Backup and disposable restore

**Invariant:** A backup is accepted only after restore into a clean disposable location proves database and evidence integrity.

**Deliberate violations:** Missing raw file, corrupted database, partial archive, wrong generation, stale sidecar, modified approved text, lost identifier relationship, and restoration over an existing environment.

**Expected result:** Restore proof fails explicitly on any discrepancy.

**Real SQLite required:** Yes.

### HT-19 — Three-way post-restore reconciliation

**Invariant:** After restore, database-held hashes, evidence-sidecar hashes, and actual raw bytes agree, while exact approved authored text and identifier relationships are unchanged.

**Deliberate violations:** Change any one of the three hash representations, alter approved text, drop a relationship, or substitute evidence from another run.

**Expected result:** Verification fails and no recovered environment is promoted to operational use.

### HT-20 — Detector and classifier non-authority

**Invariant:** Detectors, classifiers, LLM outputs, and connection candidates never become accepted truth or authorized action without the required human gate.

**Deliberate violations:** Detector exception, partial execution, empty execution list, deliberate non-detection, fabricated candidate ID, explanation/evidence mismatch, stale candidate, and model output that claims confidence without evidence.

**Expected result:** No promotion. Failure yields explicit error/review state rather than a confident invented result.

## Required Fixture Classes

M02 must define at least:

1. Valid fixtures with known accepted outcomes.
2. Invalid-relationship fixtures.
3. Illegal-transition fixtures.
4. Exact-text mutation fixtures.
5. Raw-evidence tamper fixtures.
6. Duplicate and replay fixtures.
7. Concurrency and partial-failure fixtures.
8. Dirty-migration and interrupted-migration fixtures.
9. Backup corruption and restore-mismatch fixtures.
10. Detector failure, non-detection, and fabricated-candidate fixtures.

## Exit Requirement

Before M02 can close, this matrix must be reconciled with the final owner-approved persistence contract so that every admitted invariant identifies:

- the workflow need that justifies it;
- its enforcement boundary;
- its positive fixture;
- at least one deliberate violation fixture;
- whether real SQLite is mandatory;
- the evidence required to prove the result;
- any accepted limitation or deferred unknown.

No M03 implementation package may omit an admitted invariant without explicit owner-approved deviation.
