# M06-A Phase 3A-C1 Independent Review Correction Package

## Status

```text
Package status: owner-authorized for preparation and implementation through D037
Base application commit: 251b3ca841af46e63485b9ab5bf292cbae55a418
Parser admission authority: none
Canonical parsing authority: none
Phase 3B or later authority: none
```

## Purpose

Correct the independently reviewed and Project-Steward-reproduced Phase 3A assurance, worker-boundary, coverage, packaged-parity, and immutable-lineage defects without widening M06-A scope.

## Governing Findings

The package dispositions are:

```text
C1-F01  Current-head inherited regression proof gap                     blocking
C1-F02  os.open/filesystem mutation/process denial gaps                 blocking
C1-F03  emitted-locator coverage not independently reconciled           blocking
C1-F04  parser tuple identity and execution/package/document lineage    blocking
C1-F05  packaged implementation/dependency verification conditional     correction
C1-F06  worker read-boundary and deterministic-environment wording      correction
```

## Authorized Application Changes

### Existing files

```text
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
src/discrepancy_desk/migration_spec.py
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parser_service.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parsers/plain_text_v1.py
src/discrepancy_desk/vault_backup.py
src/discrepancy_desk/vault_filesystem.py
src/discrepancy_desk/vault_router.py
src/discrepancy_desk/vault_service.py
tests/conftest.py
tests/test_m06a_actor_authority.py
tests/test_m06a_backup_restore.py
tests/test_m06a_ingestion_artifacts.py
tests/test_m06a_migrations.py
tests/test_m06a_parser_framework.py
tests/test_m06a_parser_packaging.py
tests/test_m06a_parser_stdlib.py
tests/test_m06a_policy_context.py
parser_resources/manifest.sha256
parser_resources/configs/m06a.text.v1.json
parser_resources/schemas/m06a.normalized-package.v1.json
tests/fixtures/m06a/parsers/manifest.sha256
vault_migrations/manifest.sha256
```

Only files actually required by the correction may change.

### New files

```text
vault_migrations/versions/V0004_phase3a_c1_lineage_and_identity.py
```

Additional synthetic fixture files are allowed only under:

```text
tests/fixtures/m06a/parsers/
```

## Required Corrections

### Current-head regression proof

- restore the default M06-A Vault fixture to the live Vault migration head;
- retain explicitly named historical V0002 fixtures only for historical Phase 2 compatibility tests;
- remove hard-coded V0002 migration-head values from generic backup helpers;
- execute every schema-dependent inherited Phase 3A regression against V0004;
- retain independent Phase 2, Phase 1, and legacy suites.

### Worker security profile

- correctly classify low-level `os.open` flags;
- deny writes and mutations outside the operation directory, including remove, rename, replace, truncate, directory, link, permission, and timestamp mutation events;
- deny `os.exec*`, `os.startfile`, spawn, shell, and subprocess paths;
- preserve required immutable packaged-resource and interpreter reads;
- move enforceable deterministic environment settings to child launch;
- add source and packaged adversarial proof for each corrected path.

### Coverage reconciliation

- validate all element and region locators structurally;
- independently reconcile locator byte spans against the complete input;
- reject gaps, overlaps, out-of-range spans, omitted terminal bytes, inconsistent counts, and raw-text/locator disagreement;
- represent BOM bytes explicitly in coverage without fabricating document text.

### Immutable identity and lineage

- add V0004 rather than editing V0003;
- version parser-definition identity from the immutable parser tuple so changed implementation/security tuples install as new `under_test` records;
- require exact same-Vault execution/package/document chain;
- scope document `version_ordinal` to acquisition-artifact lineage;
- define deterministic package-byte reuse coherently without weakening append-only execution history;
- refuse destructive downgrade while any V0004 authority exists.

### Packaged parity

- make implementation-source and dependency-lock byte verification mandatory in source and packaged modes;
- fail closed when either exact byte resource is absent;
- bind the updated security profile and resource tuple into evidence.

## Validation and Evidence

Required before implementation commit:

```text
uv sync --frozen
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
uv run pytest -o addopts= --disable-warnings -q tests/test_m06a_parser_framework.py tests/test_m06a_parser_stdlib.py tests/test_m06a_parser_packaging.py tests/test_m06a_backup_restore.py tests/test_m06a_migrations.py
uv run python scripts/run_ht_evidence.py --suite m06a-phase3a
uv run python scripts/run_ht_evidence.py --suite m06a-phase2
uv run python scripts/run_ht_evidence.py --suite m06a-phase1
uv run python scripts/run_ht_evidence.py --suite legacy
uv run alembic -c alembic.ini heads
uv run alembic -c vault_migrations/alembic.ini heads
npm --prefix desktop test
npm --prefix desktop run build
cargo test --manifest-path desktop/src-tauri/Cargo.toml
uv run python scripts/build_desktop_sidecar.py
git diff --check
```

After commit, rerun the complete stack against the clean commit SHA and create immutable commit-named evidence.

## Explicit Exclusions

This package does not authorize:

```text
owner_admitted parser state
canonical parsing
parser execution or admission UI/API
Phase 3B
Phases 4 through 6
M06-B
SRT/VTT/JSON/Markdown/XML/HTML/PDF/OCR
network retrieval or providers
live LLM integration
FTS/chunks/projections/assertions/entities/dossiers
Qdrant or graph work
purge
publication automation
pyproject.toml or uv.lock changes
central migrations
editing V0001, V0002, or V0003
```

## Closure Gate

Phase 3A-C1 requires clean commit-bound evidence, independent finding disposition, documentation synchronization, and explicit owner closure. Successful correction does not admit `m06a.text.v1`.
