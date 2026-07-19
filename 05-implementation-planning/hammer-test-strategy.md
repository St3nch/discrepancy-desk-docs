# Hammer-Test Strategy — The Discrepancy Desk

## Purpose

This document preserves the current testing doctrine before implementation details are fully known.

Hammer tests are deliberate adversarial tests that try to make the system accept invalid data, lose provenance, cross a policy boundary, return misleading results, or claim success without sufficient evidence.

They supplement normal unit, integration, migration, and end-to-end tests. Happy-path tests alone are not sufficient.

## Living-Contract Rule

This strategy is provisional and must evolve from real schemas, payloads, workflows, defects, and retrieval results.

For every subsystem:

- define invariants before implementation;
- include both a valid case and a deliberate violation case;
- use the real storage or retrieval engine where engine behavior matters;
- preserve exact test commands and evidence;
- convert every escaped defect into a regression or hammer test;
- update the active milestone when new failure modes are discovered;
- fail closed when validity, approval, provenance, or evidence cannot be established.

Mock and fixture data are required where repeatable known outcomes are needed. Prefer fictional, synthetic, owned, or public-domain material.

## Owner-Approved M02 Doctrine

Accepted on 2026-07-19:

> Every persistence rule must be attacked with deliberate bad data, invalid state transitions, fabricated identifiers, modified evidence, stale approvals, duplicate operations, partial failures, detector failures, migration failures, and recovery failures. Tests must run against the real SQLite engine where database behavior matters. Any ambiguity, missing evidence, integrity mismatch, or unrecognized failure must fail closed. Happy-path tests alone prove nothing.

The M02 hammer-test contract must explicitly require:

- exact approved text cannot be replaced by X-returned text containing generated links or other platform-added material;
- any edit after approval invalidates the prior approval;
- fabricated account, post, media, approval, publication, metric, and evidence identifiers are rejected;
- modified raw evidence is detected through hash mismatch;
- missing raw evidence blocks normalized promotion or acceptance;
- invalid lifecycle jumps are rejected at the persistence boundary;
- duplicate publication records, double submits, and replayed operations are either safely idempotent or explicitly rejected;
- metric snapshots require source, observation method, and capture time;
- empty, unavailable, withheld, missing, and errored values remain distinct;
- spam or relevance classification never deletes or rewrites original mention evidence;
- classifier, detector, or review-tool failure never auto-promotes content, mentions, claims, or pattern candidates;
- migration interruption, rollback, and recovery behavior are tested against the real engine;
- backup and restore preserve identifiers, relationships, exact approved text, provenance, and evidence hashes;
- tests deliberately prove non-detection, malformed evidence, partial writes, modified evidence, fabricated IDs, empty result sets, and recovery failures are caught.

This doctrine is the minimum. M02 may add stricter invariants when the persistence contract is defined.

## Operational Database Categories

Before schema or migration work, M02 must refine these categories into an owner-approved test matrix.

### Approval integrity

Prove that approval binds to the exact reviewed revision, text, links, media, claim labels, and platform variant. Any change after approval must invalidate or supersede approval. Fabricated, missing, reused, stale, or cross-platform approval identifiers must fail.

### Workflow transitions

Attack illegal transitions such as capture-to-published, rejected-to-published, deleted-to-active, or draft-to-published without review. The database must reject invalid transitions even if the UI or application code attempts them.

### Provenance and evidence

Prove sources cannot silently disappear or be replaced while referenced; manual notes, quotations, source text, and AI interpretation remain distinguishable; modified raw evidence is detected or versioned; and withdrawn sources remain visibly represented.

### Platform boundaries

Prove X and Truth Social records cannot be misclassified or satisfy each other's requirements. Truth Social API, automated capture, parser, and automated-metrics classifications must fail under the current manual-only admission.

### Identity, duplicates, and retries

Test duplicate external IDs, URL variants, repeated imports, duplicate publication records, double clicks, retries after timeout, handle changes, and concurrent writes. Operations must be idempotent where intended and reject ambiguity elsewhere.

### Claim-label safety

Prove uncertainty labels cannot silently disappear; unsupported claims cannot become sourced or verified; dependent claims react visibly to source deletion or withdrawal; and conflicting claims coexist without overwriting one another.

### Metrics integrity

Reject negative or impossible counts, snapshots before publication, wrong-platform attachment, duplicate snapshots, missing observation methods, false equivalence between manual and API observations, and silent rewriting of historical measurements.

### Migration and transaction safety

Test empty and populated databases, invalid legacy rows, reruns, interruption, rollback, version mismatch, partial writes, constraint failures, and recovery. Approval, provenance, evidence, and audit history must survive migrations without silent truncation or invented defaults.

### Deletion, retention, audit, backup, and restore

Test soft/hard deletion boundaries, dependent references, audit emission, audit immutability through normal paths, backup integrity, disposable restore, record counts/hashes, corrupted or partial backups, and recovery on a clean environment.

## Anomaly Vault Categories

M05 must build a mock corpus with known-good, conflicting, malformed, and adversarial notes.

Hammer tests must cover:

- source and claim provenance;
- distinction between quotation, summary, human note, and AI interpretation;
- fabricated and duplicate IDs;
- broken links, orphan notes, circular links, alias collisions, and entity merges;
- malformed or missing frontmatter, invalid dates/types/statuses, and empty required fields;
- source, quote, URL, attachment, or note mutation outside the application;
- deleted or withdrawn sources and their dependent claims;
- sync-conflict files, interrupted writes, duplicate files, and partial restore;
- note, attachment, link, and hash verification after restore.

Malformed or ambiguous records must be rejected, quarantined, or flagged for human review rather than silently normalized.

## No Coincidences Categories

M09 must use labeled known-positive, known-negative, ambiguous, and malformed fixtures.

Hammer tests must cover:

- false positives and false negatives;
- same-name and alias collisions;
- conflicting dates, organizations, and source identities;
- fabricated, modified, deleted, or missing evidence;
- detector exceptions and partial execution;
- empty candidate sets and deliberate non-detection;
- explanation/provenance mismatch;
- repeated or duplicated sources posing as independent corroboration;
- deterministic reproduction of candidate results;
- human rejection, suppression, and later re-evaluation.

No detector result may promote a pattern to truth. Failure or uncertainty must yield no candidate or explicit review, never a confident invented connection.

## Qdrant Categories

Qdrant is a rebuildable retrieval index, never canonical truth.

M10 must use a labeled mock corpus with expected relevant, irrelevant, ambiguous, and no-answer results.

Hammer tests must cover:

- exact, paraphrased, ambiguous, alias, date, misspelled, misleading, unrelated, and no-answer queries;
- precision, recall, top-k hit rate, irrelevant-result rate, and no-answer correctness;
- duplicate names, conflicting claims, satire mixed with research, keyword spam, low-quality repetition, multi-topic chunks, and prompt-injection text;
- stale vectors after note changes;
- orphan vectors after note deletion;
- duplicate embeddings and interrupted ingestion;
- changed chunking rules, metadata, embedding model, model version, or vector dimensions;
- invalid, missing, mutually exclusive, or corrupted metadata filters;
- collection mismatch, Qdrant outage, partial collection loss, rebuild, and fallback retrieval;
- traceability from every result to canonical source, revision, chunk, model, and embedding version.

The application must remain usable without Qdrant. A full collection must be rebuildable from canonical records, and stale or orphan vectors must be detectable.

## Mock-Data Contract

Each subsystem's fixture corpus should include:

1. Clean fixtures with known-valid outcomes.
2. Conflict fixtures with contradictory evidence or labels.
3. Malformed fixtures with invalid structure or relationships.
4. Adversarial fixtures designed to confuse validation, detection, or retrieval.

Each fixture must record:

- input;
- expected validation result;
- expected relationships;
- expected retrieval or detector result, when applicable;
- expected rejection, quarantine, or review behavior;
- why the fixture exists.

## Milestone Evidence Rule

A milestone using this strategy is not complete until its completion record identifies:

- invariants tested;
- positive and deliberate violation cases;
- fixture corpus/version;
- exact commands and results;
- real-engine tests performed;
- defects found and resolved;
- remaining unknowns;
- owner-approved deviations.

## Governing Principle

SQLite and the Anomaly Vault hold canonical records according to their defined boundaries. No Coincidences proposes reviewable candidates. Qdrant retrieves candidates. None of them may silently change project truth.

Hammer-test plans must become more specific as the system becomes real. The point is not to freeze today's guesses. The point is to prevent tomorrow's implementation from forgetting what must be attacked.

## M02 Required SQLite-Specific Categories

M02 must refine each category below into a named invariant, valid fixture, deliberate violation fixture, expected fail-closed result, real-engine requirement, and preserved evidence requirement.

### Connection-level foreign-key enforcement

Every operational SQLite connection must prove `PRAGMA foreign_keys=ON`. Tests must deliberately use a connection with enforcement disabled and fail the test rather than silently accepting orphaned relationships. Engine rejection must be demonstrated for missing raw evidence, source, post, approval, publication, and metric references.

### Locking, journal mode, and busy handling

Choose and record journal mode and bounded busy-timeout behavior. Real-engine tests must use overlapping writers and prove the result is deterministic serialization or clean rejection with no partial commit, lost audit event, or ambiguous retry state. `SQLITE_BUSY` and interrupted transactions must be explicit outcomes, not unclassified exceptions.

### Named idempotency and uniqueness keys

Every operation described as idempotent must identify the exact operation key or uniqueness constraint that makes it so. Replayed captures, approvals, manual-publication records, metric snapshots, and external platform identifiers must be deliberately duplicated against real constraints. The second operation must be a proven no-op or an explicit rejection, never accidental duplication.

### Audit-event immutability and tamper detection

Normal application paths must not update or delete accepted audit events. Direct mutation fixtures must be prevented by engine controls or detected by an independently verifiable integrity mechanism. A detector error, skipped verification, or broken integrity chain must fail closed rather than report a clean audit history.

### Database-to-raw-evidence reconciliation

The selected raw-evidence boundary must detect a referenced file or object that is missing, moved, altered, truncated, re-encoded, or hash-mismatched. It must also report orphan raw evidence with no operational reference. Parsed JSON or normalized rows cannot satisfy the canonical evidence requirement when exact bytes are unavailable.

### Dirty and interrupted SQLite migrations

Real SQLite migration tests must cover interruption during table-copy or batch operations, invalid legacy rows, stale or incorrect version markers, leftover temporary tables, partial indexes or triggers, reruns, and rollback. A dirty or ambiguous migration state must refuse further implementation activity until reconciled with preserved evidence.

### Disposable restore and cross-store reconciliation

Restore tests must run in a clean disposable location. They must reconcile identifiers, relationships, record counts, exact approved text, canonical raw bytes, database-held hashes, sidecar manifests, and audit history. Missing files, corrupted archives, stale database copies, or three-way hash disagreement must fail explicitly.

### Exact-text mutation and Unicode adversaries

Approval tests must cover appended platform links, shortened text, newline and whitespace changes, Unicode normalization variants, zero-width characters, punctuation substitutions, link changes, media changes, and platform-specific variants. Any non-identical approved revision must invalidate or supersede approval even when it appears visually similar.
