# M03 Work Package D — Guarded Migrations, Encrypted Archives, and Evidence Return

## Status

Implemented, validated, committed, and pushed in the application repository. M03 remains active.

## Application Commit

```text
de1b487 — Add guarded migrations encrypted archives and evidence ledger
```

## Implemented

- guarded Alembic upgrade runner;
- migration-manifest verification before execution;
- durable dirty-state marker created before migration;
- successful migration clears the marker only after SQLite integrity and Alembic version verification;
- failed migration deliberately retains the marker and blocks retry;
- deterministic ZIP generation with stable file order and metadata;
- real `age` encryption using an explicit recipient;
- encrypted archive hash and size manifest;
- fail-closed removal of partial encrypted output when `age` fails;
- deterministic per-fixture test-evidence JSON writer;
- attachment SHA-256 and byte-size recording;
- active HT-01 through HT-20 coverage ledger.

## Validation

```text
uv run ruff check .
All checks passed

uv run pytest
28 passed
```

The encryption test used the installed `age` and `age-keygen` executables with a throwaway identity created only under the pytest temporary directory. The encrypted archive was decrypted and compared byte-for-byte with the source ZIP.

## Defect Found and Corrected

The first guarded migration attempt created schema objects but did not leave an accepted Alembic version marker when invoked programmatically. The runner correctly refused to clear the dirty marker.

Root cause: the Alembic online environment did not explicitly commit the connection transaction around pragma initialization and migration completion under SQLAlchemy 2 behavior.

Correction:

- commit after connection pragma initialization;
- run Alembic migration transaction;
- commit after migration completion;
- retain the independent post-migration version-marker and integrity checks.

The corrected suite proves successful migration, retained dirty state on failure, and blocked retry while dirty.

## Current Boundary

The code can now create and verify an encrypted archive, but no production recipient identity has been selected or stored. Credentials and private age identities remain outside Git.

The HT coverage ledger intentionally marks partial, substantial, and deferred coverage. It does not falsely declare all HT-01 through HT-20 gates complete.

## Remaining Bounded Work

- migration-specific governed recovery operation after a retained dirty marker;
- exact execution-evidence collection integrated with pytest runs;
- complete named-fixture mapping for every HT invariant;
- detector/classifier deliberate error and non-detection fixtures;
- broader explicit unknown-state query semantics;
- publication mismatch reconciliation;
- minimal capture-to-publication operator service loop;
- local dashboard only after persistence exit evidence is accepted.
