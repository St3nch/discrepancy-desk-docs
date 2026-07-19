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
