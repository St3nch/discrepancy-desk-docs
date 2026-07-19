# M03 Work Package B — Initial Persistence Implementation Return

## Status

Implemented locally and committed in the application repository. M03 remains active; the full HT-01 through HT-20 package is not yet complete.

## Application Repository

```text
C:\dev\x\discrepancy-desk
```

Branch:

```text
main
```

Commits:

```text
8590399 — Initialize governed M03 persistence baseline
6281f44 — Add migration integrity and restore proof
```

The application repository currently has no configured remote. No push was attempted.

## Governance Applied

- Git initialized only after owner approval of the D017 repository-governance package.
- `evidence/`, runtime databases, backups, restores, virtual environments, credentials, and secret-bearing files are ignored.
- Only approved production brand assets were admitted to Git.
- Secret-pattern review returned no findings.
- No tracked file exceeded the approved 10 MiB ordinary-file ceiling.
- The live X credential file was not accessed.

## Implemented Boundary

- Python project and locked dependency environment.
- Alembic migration environment.
- Initial SQLite persistence migration.
- WAL journal mode.
- `PRAGMA foreign_keys=ON` verification.
- 5000 ms busy timeout.
- `BEGIN IMMEDIATE` write transactions.
- Stable owned-account identity.
- Work-item lifecycle states.
- Canonical evidence references using governed relative paths and SHA-256.
- Immutable exact-component revision binding.
- Human approval records bound to exact revision hashes.
- Manual publication identity and uniqueness constraints.
- Immutable metric snapshot structure with explicit observation states.
- Operation-key persistence boundary.
- Append-only hash-chained audit events.
- Migration-file SHA-256 manifest verification.
- SQLite backup generation and clean restore verification.

## Validation

Commands:

```text
uv sync --extra dev
uv run ruff check .
uv run pytest
```

Results:

```text
ruff: All checks passed
pytest: 14 passed
```

## Hammer Coverage Present

Current tests deliberately cover:

- connection-level foreign-key enforcement;
- fabricated relationship rejection;
- exact text, line-ending, whitespace, zero-width, Unicode-sensitive, and platform binding changes;
- illegal lifecycle transition rollback;
- legal transition plus audit emission;
- missing, escaping, and hash-mismatched evidence rejection;
- stale approval binding rejection;
- approval from the wrong lifecycle state;
- audit update and delete rejection;
- duplicate external publication identity rejection;
- migration-manifest tamper rejection;
- backup generation and clean restore verification;
- modified backup evidence rejection.

## Defect Found and Corrected

The first approval implementation used connection-wide `total_changes` to determine whether the lifecycle update succeeded. That value includes earlier writes and could falsely accept an invalid approval-state transition. It was replaced with the exact update cursor's `rowcount`, and a regression test now proves approval from the wrong state rolls back.

## Remaining Work in the Authorized M03 Package

The implementation is not yet a complete HT-01 through HT-20 proof. Remaining bounded work includes:

- explicit idempotency replay operations and conflicting-key tests;
- publication mismatch reconciliation operations;
- metric correction and duplicate-session operations;
- real overlapping-writer and `SQLITE_BUSY` tests;
- deliberate audit-chain out-of-band tamper detection beyond normal-path trigger rejection;
- dirty-state migration marker and interrupted-migration fixtures;
- full three-way database, manifest, and raw-file restore reconciliation;
- encrypted archive handling with `age` or an explicitly approved available equivalent;
- exact test-evidence result packaging;
- minimal complete capture-to-publication persistence service operations;
- later local dashboard work after persistence gates pass.

## Current Gate

M03 may continue within the already approved persistence and hammer-test boundary. A remote repository URL and visibility decision are required before the application repository can be pushed.
