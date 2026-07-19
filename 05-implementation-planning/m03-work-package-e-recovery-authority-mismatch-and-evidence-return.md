# M03 Work Package E — Recovery, Authority, Mismatch, and Evidence Return

## Status

Implemented, validated, committed, and pushed in the application repository. M03 remains active.

## Application Commit

```text
f22ca583c35c5fa11880e10a3d069d37442e5602
Add governed recovery authority and mismatch handling
```

Repository:

```text
https://github.com/St3nch/discrepancy-desk
```

## Validation

```text
uv run ruff check .
All checks passed

uv run pytest
37 passed
```

The post-commit pytest session automatically emitted ignored runtime evidence at:

```text
runtime/test-evidence/latest-pytest-session.json
```

The record binds the run to commit `f22ca583c35c5fa11880e10a3d069d37442e5602`, Python `3.11.15`, SQLite `3.50.4`, 37 passed tests, and zero failures, errors, skips, expected failures, or unexpected passes.

## Implemented Boundary

### Governed migration recovery

- Dirty markers are structured JSON rather than substring-inspected text.
- Completed-migration recovery requires the exact operation identity.
- Recovery verifies the migration manifest, database presence, SQLite integrity, Alembic version table, and non-empty version marker before clearing a dirty marker.
- Failed empty migrations may be discarded only when no persistent user objects exist.
- Databases containing user tables, indexes, triggers, or views cannot be discarded through the empty-failure path.
- Wrong operation identity, malformed marker, missing version state, or persistent objects fail closed and preserve the dirty marker.

### Human-authority enforcement

- The generic lifecycle transition operation may no longer transition directly to `approved`, `manual_ready`, `published`, or `publication_mismatch`.
- Those states require their dedicated governed operations.
- Detector results are explicitly advisory and cannot mutate persistence state.
- Deliberate fixtures cover detector `flagged`, `not_detected`, and `errored` outcomes.

### Publication mismatch

- A mismatched real publication requires a non-empty human mismatch reason.
- It preserves the intended revision and approval relationship.
- It records a publication with `verified_mismatch`.
- It consumes the approval because an external publication occurred.
- It moves the work item to `publication_mismatch` without rewriting history.
- The operation is idempotent and conflicting operation-key reuse fails closed.

### Explicit unknown-state queries

- Metric queries require at least one explicit observation state.
- Unsupported invented states are rejected.
- `observed_value`, `unavailable`, and the other accepted states remain query-distinct.
- Empty results cannot silently stand in for failed, unavailable, or unsupported observations.

### Automatic test-session evidence

- Every pytest session writes a deterministic structured result into ignored `runtime/test-evidence/`.
- It records the exact command, current Git commit, Python version, SQLite version, exit status, and outcome counts.
- Runtime evidence remains outside Git under approved repository governance.

## HT Ledger Effect

The application HT ledger was strengthened:

- HT-03 lifecycle legality and invalidation: substantial;
- HT-13 explicit unknown states: substantial;
- HT-17 interrupted migration recovery: substantial;
- HT-20 detector/classifier non-authority: substantial.

No HT invariant was falsely marked complete.

## Defect/Compatibility Finding

The new dedicated-governed-operation rejection initially changed the established error classifier from `illegal transition` to a new phrase. An existing hammer test caught the incompatibility. The final message preserves the stable `illegal transition` class while identifying the required dedicated operation.

## Remaining M03 Work

- minimal complete capture-to-publication operator service layer;
- exact owned-account and work-item creation operations rather than fixture-only inserts;
- read/query service operations for the local control room;
- broader approval invalidation on successor revision and evidence failure;
- correction/replacement publication successor workflow;
- complete named evidence emission per HT fixture rather than session summary only;
- final HT-01 through HT-20 closure review;
- local FastAPI/Jinja/HTMX interface after the service loop is proven.

## Next Gate

Proceed with the minimal operator service loop, keeping the UI behind the service and persistence boundary.
