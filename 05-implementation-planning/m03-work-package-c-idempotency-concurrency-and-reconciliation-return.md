# M03 Work Package C — Idempotency, Concurrency, and Reconciliation Return

## Status

Implemented, validated, committed, and pushed in the application repository. M03 remains active.

## Application Commit

```text
0a25ed5 — Strengthen idempotency concurrency and restore reconciliation
```

Repository:

```text
https://github.com/St3nch/discrepancy-desk
```

Branch:

```text
main
```

## Implemented Boundary

- operation-specific idempotency lookup and conflict rejection;
- idempotent manual-ready operation;
- atomic manual publication recording;
- exact platform, owned-account, revision, and approval validation;
- approval consumption after accepted publication;
- immutable metric snapshots;
- metric correction links without historical overwrite;
- publication and metric replay returning the original accepted result;
- conflicting reuse of an idempotency key failing closed;
- real SQLite overlapping-writer rejection;
- bounded `SQLITE_BUSY` behavior with no partial write;
- durable migration dirty-marker guard;
- migration guard operation-identity verification;
- out-of-band audit mutation detection through chain verification;
- three-way restored evidence reconciliation among database-held hash/size, backup manifest, and actual bytes;
- orphan restored evidence rejection.

## Validation

Commands:

```text
uv run ruff check .
uv run pytest
```

Results:

```text
ruff: All checks passed
pytest: 22 passed
```

Secret-pattern review returned no findings.

## Deliberate Failure Cases Proven

- manual-ready replay does not create another audit event or side effect;
- conflicting content under the same idempotency key is rejected;
- publication replay returns the original publication identity;
- X approval cannot satisfy a Truth Social publication request;
- publication cannot proceed without `manual_ready` state;
- consumed approval and published state are updated atomically;
- duplicate metric capture session with identical content is a no-op;
- duplicate metric capture session with changed content is rejected;
- metric correction must reference the same publication;
- a second writer encountering a held `BEGIN IMMEDIATE` transaction fails cleanly with no partial update;
- a dirty migration marker blocks operation;
- a mismatched operator cannot clear another migration operation's marker;
- dropped audit trigger plus modified payload is detected by hash-chain verification;
- restored orphan evidence is rejected;
- database-held evidence hash disagreement is detected even when the backup manifest is modified to match the tampered database file.

## Current Hammer Coverage

The current suite materially covers HT-01 through HT-13 and portions of HT-15 through HT-19. Coverage is not yet claimed complete merely because the test count is 22.

Still incomplete or requiring stronger evidence:

- explicit publication-mismatch reconciliation operation and successor flow;
- deeper interrupted-Alembic/table-copy recovery fixtures;
- migration preflight integration so the dirty guard wraps actual migration execution;
- encrypted archive creation and verification using `age` or another separately approved available mechanism;
- deterministic test-evidence result package with command, environment, commit, fixture, and hash records;
- explicit HT-01 through HT-20 coverage ledger showing positive and deliberate violation cases;
- detector/classifier non-authority implementation fixtures beyond persistence-level rejection rules;
- minimal local operator interface and full manual walkthrough.

## Gate Result

This batch is accepted as implementation progress, not M03 completion. M03 may continue inside the approved persistence, hammer, recovery, and local-operator boundary.
