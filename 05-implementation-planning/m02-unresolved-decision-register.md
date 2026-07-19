# M02 Unresolved Persistence Decision Register

## Status

UD-01 through UD-12 were resolved by the owner on 2026-07-19. Their accepted choices are governed by `05-implementation-planning/m02-owner-approved-persistence-decisions.md`.

UD-13 through UD-18 remain deferred and unresolved within their stated later-milestone boundaries. The original questions below are retained as decision history.

This register prevents implementation choices from being smuggled into conceptual planning as defaults.

## Decision Rule

For each item:

1. identify the real workflow and evidence need;
2. compare the smallest viable options;
3. identify hammer-test consequences;
4. record the owner decision and rationale;
5. update the persistence contract, lifecycle model, adversarial matrix, milestone, status, and decision log when the choice materially changes them;
6. do not begin physical design until all decisions required by the M02 exit gate are closed.

## Resolved Decisions for M02 Exit

### UD-01 — Canonical raw-evidence storage boundary

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** Should canonical raw API evidence remain immutable files referenced by governed path plus SHA-256, be stored as exact-byte database BLOBs plus SHA-256, or use a deliberately bounded combination?

**Known facts:**

- M01 integrity depends on exact response bytes.
- Parsed JSON cannot replace the original bytes.
- The current M01 evidence is already preserved as files and must not be relocated merely for symmetry.
- The main assets/evidence directory is intentionally non-Git.

**Options to evaluate:**

- immutable external file plus stored relative locator, SHA-256, provenance, and verification state;
- exact-byte BLOB plus SHA-256;
- file as canonical with optional exact-byte database copy clearly marked as duplicate;
- BLOB as canonical with governed export/archive proof.

**Must resolve:** canonical authority, path portability, file-root boundary, maximum evidence size, orphan detection, backup scope, restore reconciliation, and behavior when database and file disagree.

**Hammer impact:** HT-05, HT-06, HT-18, HT-19.

### UD-02 — Exact revision identity and approval-binding method

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What exact deterministic representation binds human approval to text, links, media, alt text, platform, account, and review-bound labels?

**Known facts:**

- Visual equality is insufficient.
- Whitespace, newline, Unicode normalization, zero-width, URL, media, alt-text, and platform mutations must invalidate approval.
- Platform-returned text must remain separate from authored text.

**Options to evaluate:**

- canonical component bundle serialized under a fixed versioned procedure and hashed;
- independently hashed components plus a versioned aggregate binding;
- immutable revision identity backed by exact stored component bytes and a calculated integrity value.

**Must resolve:** serialization/versioning rules, Unicode handling, newline preservation, media identity, link ordering, label ordering, and what happens when the binding procedure changes.

**Hammer impact:** HT-01, HT-02, HT-03, HT-19.

### UD-03 — Lifecycle state vocabulary and reopening rules

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** Does the owner approve the companion state model exactly, including `withdrawn`, `publication_mismatch`, and `evidence_blocked`, and which non-published terminal states may reopen?

**Known facts:**

- Illegal transitions must be rejected at the persistence boundary.
- Published historical revisions must not move backward.
- Rejection must preserve history.

**Must resolve:** whether rejected items may return to drafting, whether withdrawn items may reopen, how a corrected publication is represented, and whether blocked states restore the prior valid state or create an explicit reconciled successor state.

**Hammer impact:** HT-03, HT-09, HT-20.

### UD-04 — Audit immutability or tamper-detection mechanism

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What mechanism will make accepted audit history immutable through normal operations and detect out-of-band mutation?

**Options to evaluate later:**

- application-denied updates/deletes plus database triggers;
- append-only event records plus hash chaining;
- signed or separately manifested audit batches;
- a combination with external verification evidence.

**Must resolve:** trusted verification boundary, key/secret implications if signatures are considered, chain-break behavior, audit-write atomicity, and recovery verification.

**Hammer impact:** HT-09, HT-11, HT-18, HT-19.

### UD-05 — Named idempotency keys and uniqueness boundaries

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What exact operation identity governs capture, evidence registration, approval, manual-ready designation, publication recording, and metric snapshots?

**Known facts:**

- Duplicate behavior must be deliberate.
- Blind retry after timeout or `SQLITE_BUSY` is unsafe.
- Stable platform post IDs should prevent duplicate publication identity.

**Must resolve for each operation:** key source, scope, retention, collision handling, response to replay, concurrent duplicate behavior, and whether a duplicate is a no-op or explicit rejection.

**Hammer impact:** HT-04, HT-08, HT-09, HT-10, HT-12.

### UD-06 — Representation of missing, empty, withheld, unavailable, malformed, and errored values

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What conceptual and later physical representation preserves these states without inventing defaults?

**Known facts:**

- Empty text is not the same as missing data.
- API failure is not an empty successful result.
- Withheld and unavailable values may carry different meaning.

**Must resolve:** which record classes require explicit observation status, whether error detail is retained separately, how queries distinguish not-observed from observed-empty, and how unsupported future values fail closed.

**Hammer impact:** HT-12, HT-13, HT-20.

### UD-07 — Evidence retention, deletion, and third-party-data boundary

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What raw evidence and derived observations may be retained, for how long, and under what deletion or withdrawal rules?

**Known facts:**

- M01 evidence contains public third-party identifiers, text, links, and metadata.
- The main directory remains non-Git pending approved data and publication policy.
- Referenced evidence cannot silently disappear.

**Must resolve:** retention periods, owner-only deletion authority, legal/policy recheck triggers, withdrawn-source handling, minimum metadata retained after deletion, backup-generation deletion, and whether any raw evidence may ever enter a remote repository.

**Hammer impact:** HT-05, HT-06, HT-11, HT-18, HT-19.

### UD-08 — Manual publication confirmation and mismatch evidence

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What minimum evidence is required to accept that the human published the exact approved revision?

**Options to evaluate:**

- owner-entered external post ID and URL plus explicit confirmation;
- later read-only API observation when separately admitted and available;
- manual screenshot or browser observation, subject to evidence policy;
- combination of owner confirmation and platform identifier.

**Must resolve:** observed time versus claimed publication time, handling of copy mistakes, platform-generated link differences, deleted or inaccessible posts, and mismatch reconciliation authority.

**Hammer impact:** HT-01, HT-02, HT-03, HT-08, HT-09.

### UD-09 — Metric snapshot identity and duplicate semantics

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What makes two metric observations distinct or duplicate?

**Must resolve:** timestamp precision, observation method, capture-session identity, exact metric-set identity, whether same-time manual and API observations coexist, correction behavior for a human entry error, and treatment of unavailable or partial metrics.

**Known boundary:** snapshots append and historical values are not silently overwritten.

**Hammer impact:** HT-08, HT-12, HT-13.

### UD-10 — SQLite journal mode, transaction mode, and bounded busy timeout

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** Which SQLite operational settings are required for the expected local workflow?

**Known facts:**

- Every operational connection must verify `PRAGMA foreign_keys=ON`.
- Overlapping writers must serialize or reject cleanly.
- Retry behavior must be bounded and state-aware.

**Must resolve before M03 implementation:** journal mode, transaction-start behavior, busy timeout, connection initialization checks, treatment of `SQLITE_BUSY`, and recovery after uncertain interruption.

**Hammer impact:** HT-07, HT-09, HT-10.

### UD-11 — Migration authority, version state, and dirty-state marker

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What evidence and state prove that the database is at a known migration version and safe to operate?

**Known facts:**

- Alembic is the planned migration tool, but no migration exists or is authorized yet.
- Unknown, partial, modified, or interrupted states must fail before destructive action.

**Must resolve:** migration-file integrity verification, version-marker authority, dirty-state representation, preflight requirements, temporary-object detection, rollback evidence, and operator recovery procedure.

**Hammer impact:** HT-16, HT-17.

### UD-12 — Backup unit, archive format, and disposable restore acceptance

**Resolution:** Closed by owner approval on 2026-07-19. See `m02-owner-approved-persistence-decisions.md`.

**Question:** What exact material constitutes one recoverable generation?

**Must resolve:** database file and sidecars, raw evidence roots, manifests, configuration excluding secrets, archive format, encryption boundary, generation identity, retention count, restore location, and acceptance evidence.

**Known boundary:** a backup is not accepted until restored into a clean disposable location and reconciled.

**Hammer impact:** HT-18, HT-19.

## Decisions That May Be Deferred Beyond M02 if Explicitly Excluded from M03

### UD-13 — Standalone source, claim, and entity records

The smallest contract admits source references and notes where needed but does not require a full claim or entity graph.

**Decision needed later:** when workflow volume and research needs justify standalone claims, entities, quotations, and dependency behavior.

### UD-14 — Standalone model-output records

The smallest contract requires producer provenance for AI-created revisions or notes but does not require a generic model-output warehouse.

**Decision needed later:** whether reproducibility, cost, safety review, or model comparison justifies preserving prompts, responses, model version, parameters, and context bundles as dedicated records.

### UD-15 — Media asset lifecycle beyond approval binding

The contract requires exact media identity and alt text when content approval depends on them.

**Decision needed later:** whether a reusable media library, derivative tracking, licensing metadata, or transformation history is required.

### UD-16 — Mention and external-interaction operational workflow

M01 observed a mention and established that evidence must survive human classification.

**Decision needed before implementation of mention handling:** admitted human states, spam/relevance labels, relationship to source evidence, retention, and whether mentions belong in M03 or a later X-operations batch.

No automated reply, link-following, or engagement action is admitted.

### UD-17 — Repost, cross-post, and corrected-publication semantics

**Decision needed later:** whether one approval can ever authorize more than one manual publication, how cross-platform variants require separate approvals, and how deleted/corrected/replacement posts relate without rewriting history.

Default until decided:

```text
one exact approval supports at most one accepted publication record on one platform and owned account
```

### UD-18 — Truth Social manual evidence details

Truth Social remains secondary and manual-only.

**Decision needed before M07:** minimum owned-post recording evidence, manual metric observation rules, and platform-specific lifecycle differences.

No Truth Social API, DOM parser, content script, automated screenshot, recurring collection, or automated metric polling is admitted.

## Decision Closure Template

When the owner resolves an item, record:

```text
Decision ID:
Date:
Owner-approved choice:
Rationale:
Rejected alternatives:
Affected contract sections:
Affected lifecycle rules:
Affected hammer invariants:
Implementation boundary opened or still blocked:
Required evidence:
```

## Current Gate Result

UD-01 through UD-12 are closed. M02 may proceed through final contract reconciliation, migration/test planning, and exit-gate review.

UD-13 through UD-18 remain deferred and do not authorize expansion of M03 beyond the accepted smallest operational contract.
