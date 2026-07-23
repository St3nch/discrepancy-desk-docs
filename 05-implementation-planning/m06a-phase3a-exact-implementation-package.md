# M06-A Phase 3A Exact Implementation Package — Parser Framework and Plain-Text Under-Test Candidate

## Status

```text
Package status: independently reviewed, corrected through D037, clean-evidence-bound, and owner-closed through D038
Implementation authority: exact Phase 3A package and 35-invariant profile only
Original implementation commit: 251b3ca841af46e63485b9ab5bf292cbae55a418
Corrected implementation commit: 1337b5ac450ae82664aa1ad9667a85af41c4351e
Correction package: 05-implementation-planning/m06a-phase3a-c1-independent-review-correction-package.md
Parser admission authority: none
Current admitted parsers: none
Phase 1: owner-closed through D033
Phase 2: owner-closed through D035
Application baseline: 1e8cba8f0ef88c2e05b9617956872be26753993e
Implementation return: 08-audits/m06a-phase3a-implementation-return.md
M06-B authority: none
```

This document extracts one exact, bounded Phase 3A candidate from the owner-accepted M06-A canonical plan and parser-admission plan.

D036 records owner acceptance of this exact package and authorizes only the common parser framework and the plain-text candidate in `under_test` state. It does not admit a parser for canonical Vault use.

---

# 1. Governing Baseline

## 1.1 Repositories

Application repository:

```text
C:\dev\x\discrepancy-desk
required implementation base: 1e8cba8f0ef88c2e05b9617956872be26753993e
branch: main
working tree at package preparation: clean
```

Documentation repository before this candidate:

```text
C:\dev\x\discrepancy-desk-docs
base: 32f7baff38e07cf2a5e46dcc1812893467996bbb
branch: main
working tree at package preparation: clean
```

## 1.2 Governing documents

- `00-doctrine/operating-doctrine.md`;
- `00-doctrine/editorial-anomaly-archive-direction.md`;
- `00-doctrine/human-approval-policy.md`;
- `05-implementation-planning/implementation-roadmap.md`;
- `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`;
- `05-implementation-planning/m06-architecture-synthesis.md`;
- `05-implementation-planning/m06a-m06b-package-boundary.md`;
- `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`;
- `05-implementation-planning/m06a-parser-admission-plan.md`;
- `05-implementation-planning/m06a-adversarial-closure-matrix.md`;
- `05-implementation-planning/m06a-phase2-exact-implementation-package.md`;
- `08-audits/claude-m06a-phase2-independent-implementation-review.md`;
- `08-audits/m06a-phase2-correction-and-closure.md`;
- M06-D01 through M06-D16;
- D027 through D035.

## 1.3 Controlling authority distinction

This package separates three different decisions:

```text
framework implementation
!=
parser candidate implementation
!=
parser owner admission
```

Acceptance of this package may authorize the first two only.

No implementation result, passing test, installed module, packaged resource, migration row, or successful synthetic fixture run automatically creates an `owner_admitted` parser admission.

---

# 2. Objective

Implement the smallest useful Phase 3A slice that proves the common parser security, identity, determinism, package, migration, packaging, and recovery contracts end to end without granting canonical parser authority.

The package contains:

```text
common parser framework
+
plain-text parser candidate m06a.text.v1
+
under_test-only candidate evidence
```

The package must prove that the product can safely represent and enforce parser admission before any parser is admitted.

---

# 3. Fixed Scope Boundary

## 3.1 Included

- Vault migration `V0003_parser_packages_and_documents.py`;
- parser definitions;
- immutable parser configuration versions;
- immutable parser admission versions;
- parser execution receipts;
- normalized package records;
- document versions, elements, and regions;
- exact normalized-package schema `m06a.normalized-package.v1`;
- canonical JSON serialization and package hashing;
- short-lived fixed-entrypoint worker process;
- no-network, no-DNS, no-subprocess, and bounded-filesystem controls;
- operation-scoped temporary quarantine;
- parent validation before canonical package linkage;
- plain-text implementation candidate `m06a.text.v1`;
- synthetic fixture corpus and malicious worker test doubles;
- source and packaged-sidecar parser resource manifests;
- deterministic output proof;
- migration, backup, restore, tamper, and packaged-resource regression proof;
- read-only parser status through the Tauri-supported loopback path;
- exact Phase 3A hammer profile and commit-bound evidence.

## 3.2 Explicitly excluded

- owner admission of `m06a.text.v1`;
- any production Vault row with state `owner_admitted`;
- any operator action that creates an `owner_admitted` row;
- canonical parsing of the owner's real Vault material;
- SRT, VTT, or JSON parser implementation;
- Markdown, RSS/Atom, HTML, or PDF work;
- parser plugins, dynamic imports, extension discovery, or alternate entrypoints;
- new third-party Python dependencies;
- network retrieval, URL fetching, monitoring, feeds, WebSub, or M06-B work;
- OCR, browser rendering, media download, or office-document parsing;
- FTS, search, chunks, or projections;
- assertions, entities, questions, dossiers, policy-impact work, or context runs;
- live LLM/provider integration;
- Qdrant, embeddings, graph, or GraphRAG;
- cross-Vault import execution;
- purge or timed-deletion authority;
- publication authority or automation;
- new Jinja/browser Vault product features.

## 3.3 No dependency change

This package must not modify:

```text
pyproject.toml
uv.lock
```

The common framework and plain-text candidate use the Python standard library and project code only.

A discovered need for a new dependency is a stop condition requiring a package amendment and renewed owner review.

---

# 4. Runtime Authority Model

## 4.1 Parser lifecycle

The accepted lifecycle remains:

```text
candidate
under_test
owner_admitted
suspended
revoked
retired
prohibited
```

Only `owner_admitted` may process canonical intake in a real Vault.

## 4.2 Product state after this package

If implementation succeeds, the installed product state must still be:

```text
m06a.text.v1: under_test
canonical parser availability: none
```

The package must not seed, migrate, or automatically generate an `owner_admitted` row.

## 4.3 Synthetic admission fixture

Tests may create an `owner_admitted` row only inside a disposable synthetic Vault to prove the runtime gate's positive and negative behavior.

That fixture:

- uses synthetic actor and Vault identities;
- is created only by test code;
- cannot target the configured operator Vault base;
- is destroyed after the test;
- is never copied into packaged runtime defaults;
- is not evidence of owner admission;
- cannot be invoked through the desktop API.

## 4.4 No admission mutation surface

This package creates no production service, API route, Tauri command, CLI, migration seed, or startup behavior that changes a parser admission state to `owner_admitted`.

A later exact admission package must define the human operation, evidence hashes, owner decision, and resulting admission-version row.

---

# 5. Exact Migration Boundary

## 5.1 Migration files

Create:

```text
vault_migrations/versions/V0003_parser_packages_and_documents.py
```

Update:

```text
vault_migrations/manifest.sha256
src/discrepancy_desk/migration_spec.py
```

Required heads after implementation:

```text
central: 0005
Vault:   V0003
```

No central migration is admitted.

## 5.2 Required tables

### `parser_definitions`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
format_id TEXT NOT NULL
implementation_kind TEXT NOT NULL
implementation_entrypoint TEXT NOT NULL
implementation_version TEXT NOT NULL
implementation_sha256 TEXT NOT NULL
resource_manifest_sha256 TEXT NOT NULL
dependency_lock_sha256 TEXT NOT NULL
license_id TEXT NOT NULL
package_schema_version TEXT NOT NULL
deterministic_contract_version TEXT NOT NULL
security_profile_id TEXT NOT NULL
created_at TEXT NOT NULL
created_by_actor_id TEXT NOT NULL
```

Constraints:

- `implementation_kind` is limited to `stdlib`, `internal`, `external_python`, or `native`;
- hashes are lowercase 64-character SHA-256 values;
- `UNIQUE(vault_account_id, id)`;
- no update or delete through ordinary service authority.

### `parser_configuration_versions`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
parser_definition_id TEXT NOT NULL
canonical_config_json BLOB NOT NULL
config_sha256 TEXT NOT NULL
size_limit_bytes INTEGER NOT NULL
depth_limit INTEGER
element_limit INTEGER NOT NULL
line_limit INTEGER
maximum_line_bytes INTEGER
warning_policy_version TEXT NOT NULL
created_at TEXT NOT NULL
created_by_actor_id TEXT NOT NULL
```

Constraints:

- same-Vault composite reference to `parser_definitions`;
- canonical JSON bytes hash exactly to `config_sha256`;
- positive server-owned limits;
- immutable after creation;
- `UNIQUE(vault_account_id, config_sha256)`.

### `parser_admission_versions`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
parser_definition_id TEXT NOT NULL
parser_configuration_version_id TEXT NOT NULL
state TEXT NOT NULL
fixture_manifest_sha256 TEXT NOT NULL
focused_test_evidence_sha256 TEXT NOT NULL
no_egress_evidence_sha256 TEXT NOT NULL
packaged_sidecar_evidence_sha256 TEXT NOT NULL
dependency_lock_sha256 TEXT NOT NULL
admitted_by_actor_id TEXT
admitted_at TEXT
supersedes_admission_id TEXT
reason TEXT NOT NULL
created_at TEXT NOT NULL
created_by_actor_id TEXT NOT NULL
```

Constraints:

- state is limited to the accepted lifecycle;
- same-Vault composite references throughout;
- `owner_admitted` requires all evidence hashes, an active verified human actor, and `admitted_at`;
- non-admitted states must not claim an admitting actor or admission timestamp;
- a changed implementation, config, package schema, dependency lock, security profile, or resource manifest requires a new admission version;
- historical admissions remain immutable;
- ambiguous multiple-current admissions fail runtime selection.

The migration creates no `owner_admitted` row.

### `parser_executions`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
vault_instance_id TEXT NOT NULL
acquisition_artifact_link_id TEXT NOT NULL
parser_definition_id TEXT NOT NULL
parser_configuration_version_id TEXT NOT NULL
parser_admission_version_id TEXT NOT NULL
security_profile_id TEXT NOT NULL
input_sha256 TEXT NOT NULL
input_size_bytes INTEGER NOT NULL
state TEXT NOT NULL
terminal_outcome TEXT
warning_codes_json BLOB NOT NULL
started_at TEXT NOT NULL
finished_at TEXT
worker_receipt_sha256 TEXT
package_sha256 TEXT
error_code TEXT
operation_id TEXT NOT NULL
actor_id TEXT NOT NULL
```

Lifecycle:

```text
started
→ succeeded
→ succeeded_with_warnings
→ failed
```

Terminal outcomes use the accepted closed vocabulary. Only `succeeded` and `succeeded_with_warnings` may reference a normalized package.

### `normalized_packages`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
parser_execution_id TEXT NOT NULL
package_schema_version TEXT NOT NULL
package_sha256 TEXT NOT NULL
byte_size INTEGER NOT NULL
storage_relative_path TEXT NOT NULL
coverage_sha256 TEXT NOT NULL
warning_codes_json BLOB NOT NULL
state TEXT NOT NULL
created_at TEXT NOT NULL
```

Constraints:

- one successful execution produces at most one package;
- package path derives only from the package SHA-256;
- state is limited to `current`, `superseded`, or `quarantined`;
- no ordinary update/delete path;
- package bytes and database metadata must reconcile before document-version creation.

### `document_versions`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
normalized_package_id TEXT NOT NULL
source_artifact_sha256 TEXT NOT NULL
parser_execution_id TEXT NOT NULL
version_ordinal INTEGER NOT NULL
state TEXT NOT NULL
created_at TEXT NOT NULL
```

Constraints:

- exact same-Vault execution/package/artifact chain;
- `version_ordinal` is positive and unique within the relevant acquisition-artifact lineage;
- state is limited to `current`, `superseded`, `withdrawn`, or `quarantined`;
- prior versions remain immutable.

### `elements`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
document_version_id TEXT NOT NULL
ordinal INTEGER NOT NULL
element_kind TEXT NOT NULL
source_locator_json BLOB NOT NULL
raw_text TEXT NOT NULL
normalized_text TEXT NOT NULL
content_sha256 TEXT NOT NULL
warning_codes_json BLOB NOT NULL
```

Constraints:

- ordinals start at zero and are contiguous;
- locator JSON is canonical and validates against the package schema;
- raw and normalized text remain distinct;
- `UNIQUE(vault_account_id, document_version_id, ordinal)`.

### `regions`

Required columns:

```text
id TEXT PRIMARY KEY
vault_account_id TEXT NOT NULL
document_version_id TEXT NOT NULL
element_id TEXT
ordinal INTEGER NOT NULL
region_kind TEXT NOT NULL
source_locator_json BLOB NOT NULL
content_sha256 TEXT NOT NULL
```

Regions support exact blank-separator and source-range fidelity. They do not create editable projection authority.

## 5.3 Append-only and downgrade behavior

V0003 must:

- block ordinary mutation/deletion of definitions, configs, admissions, completed executions, packages, versions, elements, and regions;
- permit execution finalization only through the exact lifecycle transition;
- refuse destructive downgrade when any V0003 governed row exists;
- preserve exact V0002 schema parity after empty V0003→V0002 downgrade;
- retain the corrected V0002 migration behavior and manifest integrity.

---

# 6. Normalized Package Contract

## 6.1 Schema identity

```text
package_schema_version: m06a.normalized-package.v1
canonical_json_version: m06a.canonical-json.v1
deterministic_contract_version: m06a.parser-determinism.v1
```

## 6.2 Canonical serialization

Canonical package bytes use:

```text
UTF-8
no BOM
JSON object keys sorted lexicographically
compact separators
ensure_ascii=false
no NaN or Infinity
no trailing newline
integers only for integral fields
no timestamps, host paths, process IDs, random IDs, or machine-specific metadata
```

The implementation must expose one shared canonical serializer. Parser modules may not serialize packages independently.

## 6.3 Required package fields

```text
schema_version
vault_account_id
source_artifact_sha256
parser_id
parser_implementation_version
parser_implementation_sha256
parser_config_sha256
parser_admission_id
security_profile_id
encoding
line_ending_profile
coverage
elements
regions
warnings
```

The package excludes:

- execution timestamps;
- actor display names;
- absolute paths;
- operation IDs;
- process IDs;
- hostnames;
- test-run identifiers;
- mutable admission labels.

Those belong only in `parser_executions` or evidence.

## 6.4 Coverage

Coverage must declare:

```text
input_byte_count
consumed_byte_ranges
decoded_character_count
source_line_count
emitted_element_count
emitted_region_count
complete
```

`complete` must be true for a successful package. Gaps, overlaps that violate the parser contract, silently omitted terminal content, or truncated output produce terminal failure.

## 6.5 Package storage

Canonical package path:

```text
packages/sha256/<first-2>/<next-2>/<full-sha256>.json
```

Rules:

- path is derived only from validated lowercase SHA-256;
- create-new/no-overwrite semantics;
- an existing file is reusable only after exact size and full-hash verification;
- mismatch is an integrity incident;
- package bytes are canonical authority alongside the Vault database manifest record;
- no `quarantine/` authority directory exists.

Under-test output remains in operation-scoped temporary quarantine and is not moved to the real operator Vault's canonical package tree.

---

# 7. Worker Boundary

## 7.1 Entrypoint

The trusted parent launches one fixed module:

```text
python -I -m discrepancy_desk.parser_worker
```

No request may supply:

- module name;
- callable name;
- command;
- shell text;
- URL;
- arbitrary input path;
- arbitrary output path;
- environment override.

The parent resolves the exact parser definition and packaged resource entrypoint from trusted code and admission metadata.

## 7.2 Worker input

The worker receives one length-prefixed canonical JSON request over stdin containing only:

```text
protocol_version
parser_id
implementation_sha256
config_sha256
security_profile_id
verified_input_relative_name
verified_input_sha256
verified_input_size
output_filename
```

The parent supplies already-opened or operation-scoped bounded files. The protocol contains no operator filesystem path.

## 7.3 Worker output

The worker writes only:

```text
candidate-package.json
worker-receipt.json
```

inside the operation output directory.

The parent validates both files, hashes, schema, coverage, warnings, and security result before any database finalization or canonical move.

## 7.4 No-egress controls

Before loading parser code, the worker must:

- clear proxy variables;
- clear cloud, package-manager, and common credential environment variables;
- deny `socket.socket`;
- deny `socket.create_connection`;
- deny `socket.getaddrinfo`;
- deny `urllib` and common HTTP-client imports/use;
- deny subprocess creation, `os.exec*`, `os.spawn*`, Windows `os.startfile*`, `os.system`, and shell launch;
- deny arbitrary dynamic-library loading;
- permit reads only from the exact operation input plus immutable interpreter, standard-library, application-package, frozen-bundle, and parser-resource roots required to import and execute the fixed parser;
- deny low-level and ordinary writes outside the exact operation output directory;
- deny remove, rename, replace, truncate, directory, link, permission, ownership, and timestamp mutations outside the exact operation output directory;
- install an audit hook before parser import;
- self-test the low-level write, mutation, exec, and Windows startfile denials before parsing;
- set deterministic timezone, locale, and hash-seed environment values at child launch where enforceable;
- enforce wall-clock, input, output, line, element, and memory limits through the parent plus worker checks.

Monkeypatching alone is not represented as an OS sandbox. The package claims only the exact defense-in-depth controls proved by source and packaged tests.

## 7.5 Terminal security behavior

Any denied operation yields:

```text
terminal_outcome: security_boundary_violation
canonical package: none
document version: none
partial canonical write: none
worker receipt: preserved
```

---

# 8. Plain-Text Candidate `m06a.text.v1`

## 8.1 Implementation

```text
parser_id: m06a.text.v1
implementation_kind: internal
license: project code
candidate state after implementation: under_test
```

## 8.2 Accepted synthetic input subset

- UTF-8;
- UTF-8 with BOM;
- UTF-16 LE with explicit BOM;
- UTF-16 BE with explicit BOM;
- LF, CRLF, and CR line endings;
- empty files;
- Unicode scalar text including combining marks and zero-width characters.

## 8.3 Rejected subset

- locale-dependent encodings;
- undecodable input;
- replacement-character recovery;
- NUL-heavy or binary-like input;
- compressed/archive content;
- input over the exact size limit;
- lines or element counts over configured limits;
- partial output.

## 8.4 Exact initial configuration

```text
input_size_limit_bytes: 10485760
character_limit: 1000000
line_limit: 100000
maximum_line_bytes: 1048576
element_limit: 50000
paragraph_separator: one-or-more blank logical lines
preserve_blank_regions: true
normalize_line_endings_in_derivative: LF
unicode_normalization: none
partial_output: prohibited
```

## 8.5 Element model

Each emitted paragraph element contains:

```text
ordinal
kind = paragraph
source byte start/end
source character start/end
source line start/end
raw text
normalized text
content SHA-256
warning codes
```

Blank separators are represented as regions when needed to preserve exact source coverage.

## 8.6 Warning vocabulary

The candidate may emit only:

```text
encoding_bom_removed
line_ending_normalized
```

Warnings must be stable across identical runs.

## 8.7 Terminal outcomes

- `encoding_failure`;
- `limit_exceeded`;
- `malformed_input` for binary/NUL heuristics;
- `partial_output_failure`;
- `security_boundary_violation`;
- `determinism_failure`;
- `packaging_mismatch`;
- `internal_error`.

No failed run creates a document version.

## 8.8 Fixture corpus

Create immutable fixtures and a hashed fixture manifest covering:

- ASCII UTF-8;
- multilingual UTF-8;
- UTF-8 BOM;
- UTF-16 LE BOM;
- UTF-16 BE BOM;
- LF;
- CRLF;
- CR;
- empty file;
- leading/trailing blank lines;
- repeated blank separators;
- one maximum-sized admitted line;
- line one byte over the limit;
- input exactly at the size limit;
- input one byte over the limit;
- invalid UTF-8;
- missing BOM for UTF-16-like bytes;
- embedded NUL;
- combining marks;
- zero-width characters;
- deterministic repeated parse;
- malicious worker egress attempts;
- malicious subprocess attempt;
- malicious filesystem escape attempt;
- incomplete-coverage test double;
- nondeterministic-output test double.

Fixture bytes are synthetic and contain no secrets or real user content.

---

# 9. Service and API Boundary

## 9.1 Parser service

The parser service owns:

- selected-Vault identity verification;
- retention eligibility verification;
- acquisition-artifact provenance verification;
- exact admission tuple selection;
- worker launch;
- execution lifecycle;
- package validation and storage;
- document-version creation;
- audit/idempotency;
- cleanup and reconciliation.

Parser modules receive no database connection and no actor authority.

## 9.2 Canonical invocation rule

Canonical invocation must fail before worker launch unless:

```text
selected Vault identity is valid
artifact link belongs to that Vault
retention is preservation-compatible
parser admission state is owner_admitted
all implementation/config/resource/evidence hashes match
no ambiguous current admission exists
actor context is active and properly scoped
```

Since this package creates no real `owner_admitted` row, canonical invocation in the operator's Vault remains unavailable.

## 9.3 Read-only parser status endpoint

Add:

```text
GET /desktop-api/v1/vaults/{vault_id}/parsers
```

The endpoint may return:

- installed definitions;
- current configuration versions;
- current admission states;
- canonical availability;
- mismatch or suspension reason codes;
- package schema version;
- worker security profile ID.

It must not return local paths, evidence paths, actor secrets, or raw fixture content.

No parser execution, admission, suspension, revocation, or configuration mutation endpoint is admitted.

## 9.4 Tauri surface

The selected-Vault workspace may display a read-only parser status card showing:

```text
Plain Text — Under Test
Canonical use — Not Admitted
```

There is no parse button and no admission button in this package.

No Jinja/browser parser interface is admitted.

---

# 10. Backup and Restore Changes

Phase 2 backup code already reserves package authority. Phase 3A must prove the implemented behavior.

Backup generation must:

- include canonical normalized packages referenced by the selected Vault database;
- exclude temporary candidate output;
- exclude unlinked or quarantined temporary output;
- classify packages as canonical, not derived;
- verify package hash and size;
- reject missing, extra, cross-Vault, or tampered package files.

Disposable restore must:

- restore package bytes only from the selected generation;
- validate every restored package against database metadata;
- reject a package from another Vault even when parser and artifact hashes match;
- preserve document versions and elements exactly;
- reject incomplete package trees;
- leave no live registration or operator-Vault mutation.

Because no real parser is admitted, production backup may still contain zero packages. Synthetic restore tests must create a disposable admitted fixture to prove the non-empty path.

---

# 11. Exact Application Change Surface

This list is the complete allowed change surface if the package is owner-accepted. Escaping it requires an owner-reviewed amendment.

## 11.1 Existing application files allowed to change

```text
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
src/discrepancy_desk/migration_spec.py
src/discrepancy_desk/vault_backup.py
src/discrepancy_desk/vault_filesystem.py
src/discrepancy_desk/vault_persistence.py
src/discrepancy_desk/vault_router.py
src/discrepancy_desk/vault_service.py
src/discrepancy_desk/web.py
tests/conftest.py
desktop/src/App.tsx
desktop/src/api/client.ts
desktop/src/api/types.ts
desktop/src/state/vault.ts
vault_migrations/manifest.sha256
```

Existing files may change only for the exact V0003, parser framework, package backup/restore, read-only status, evidence, and packaging responsibilities in this package.

## 11.2 New application files allowed

```text
vault_migrations/versions/V0003_parser_packages_and_documents.py
parser_resources/manifest.sha256
parser_resources/configs/m06a.text.v1.json
parser_resources/schemas/m06a.normalized-package.v1.json
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parser_service.py
src/discrepancy_desk/parsers/__init__.py
src/discrepancy_desk/parsers/plain_text_v1.py
tests/fixtures/m06a/parsers/manifest.sha256
tests/fixtures/m06a/parsers/<synthetic fixture files>
tests/test_m06a_parser_framework.py
tests/test_m06a_parser_stdlib.py
tests/test_m06a_parser_packaging.py
```

A fixture directory may contain the exact synthetic files named by its manifest without requiring a separate path amendment.

## 11.3 Explicitly forbidden path changes

```text
pyproject.toml
uv.lock
migrations/versions/*
legacy Jinja templates
M06-B modules
search/chunk/projection modules
assertion/entity/dossier modules
provider or LLM modules
```

No existing migration file may be edited.

---

# 12. Exact Phase 3A Hammer Profile

## 12.1 Phase-specific invariants

```text
M06A-HT-032
M06A-HT-033
M06A-HT-034
M06A-HT-035
M06A-HT-039
M06A-HT-040
M06A-HT-041
M06A-HT-042
M06A-HT-043
M06A-HT-044
M06A-HT-099
M06A-HT-100
M06A-HT-103
M06A-HT-105
M06A-HT-106
```

## 12.2 Required inherited regressions

```text
M06A-HT-007
M06A-HT-012
M06A-HT-014
M06A-HT-015
M06A-HT-025
M06A-HT-027
M06A-HT-030
M06A-HT-062
M06A-HT-063
M06A-HT-064
M06A-HT-065
M06A-HT-066
M06A-HT-067
M06A-HT-068
M06A-HT-069
M06A-HT-070
M06A-HT-075
M06A-HT-076
M06A-HT-095
M06A-HT-096
```

The exact Phase 3A profile therefore contains:

```text
15 Phase 3A closure invariants
20 inherited authority/migration/recovery regressions
35 total invariants
0 admitted deferrals
```

The Phase 1, Phase 2, and legacy suites remain independently required and green.

## 12.3 Required exact test mappings

The package must preserve the matrix's named test nodes. Where the accepted matrix uses `tests/test_m06a_packaging.py` but this package creates `tests/test_m06a_parser_packaging.py`, the runner registry must map the invariant to the exact collected node without editing or weakening the accepted matrix row. The implementation return must record this bounded path realization explicitly.

No invariant may be marked passed through source inspection alone when its matrix row requires real SQLite, filesystem, worker, or packaged execution.

---

# 13. Evidence Contract

The Phase 3A runner must write:

```text
runtime/ht-evidence/m06a-phase3a/latest-ht-evidence.json
runtime/test-evidence/m06a-phase3a/by-commit/<full-commit-sha>.json
runtime/test-evidence/hammer/m06a-phase3a/<invariant-id>.json
```

Commit-named aggregate evidence uses create-new/no-overwrite semantics.

Evidence must record:

- tested commit SHA;
- clean/dirty working-tree state;
- exact 35-invariant order;
- matrix hash;
- runner registry hash;
- fixture manifest hash;
- parser resource manifest hash;
- parser implementation SHA-256;
- parser config SHA-256;
- normalized-package schema SHA-256;
- security profile ID;
- central and Vault migration heads;
- Python and SQLite versions;
- source or packaged execution mode;
- collected, executed, passed, failed, skipped, xfailed, xpassed, and errored counts;
- exact worker denial results;
- deterministic package hashes;
- backup/restore package proof;
- no admitted production parser record.

The runner fails on zero execution, missing mapping, missing evidence, detector error, unexpected skip, fabricated result, wrong commit, dirty closure evidence, or resource-manifest mismatch.

---

# 14. Required Validation

Before implementation commit:

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

Additional required proofs:

- fresh V0003 migration in two synthetic Vaults;
- populated V0002→V0003 upgrade;
- empty V0003→V0002 schema parity;
- populated destructive downgrade refusal;
- dirty migration interruption and exact recovery;
- candidate remains `under_test` in product defaults;
- canonical invocation fails with no admitted parser;
- synthetic exact admission succeeds only in disposable tests;
- candidate, suspended, revoked, retired, prohibited, mismatched-hash, ambiguous-admission, wrong-Vault, and retention-ineligible invocation all fail before worker launch;
- socket, DNS, HTTP, subprocess, shell, and filesystem escape attempts fail;
- identical runs in separate workers produce byte-identical packages;
- timestamps, operation IDs, paths, and host details do not enter package bytes;
- invalid encoding and every exact limit fail without partial package;
- temporary candidate output cannot enter backup or ordinary Vault reads;
- canonical synthetic package backup/restore succeeds;
- missing, altered, extra, or cross-Vault package backup fails;
- source and packaged sidecar contain matching parser/config/schema/resource hashes;
- packaged worker denial controls execute rather than merely existing in source;
- read-only Tauri status shows `under_test` and canonical unavailable;
- no parser mutation or browser route exists;
- exact changed paths match this package;
- no secret or real user content appears in fixtures, logs, manifests, or evidence.

After implementation commit, rerun the complete stack against the clean SHA and create immutable commit-named evidence.

---

# 15. Independent Review Gate

Before this implementation phase may close, an independent implementation review must examine:

- exact package compliance;
- migration and downgrade behavior;
- admission-state enforcement;
- absence of a production `owner_admitted` row or mutation surface;
- worker protocol and denial controls;
- no-egress proof in source and packaged modes;
- deterministic package bytes and coverage;
- package/object/provenance binding;
- backup/restore package inclusion and isolation;
- Tauri-only supported operator surface;
- exact 35-invariant evidence;
- secret and path-leakage review;
- absence of Phase 3B, Phase 4, Phase 5, M06-B, provider, LLM, Qdrant, graph, purge, and publication leakage.

Findings must be reproduced, dispositioned, corrected as required, and bound to clean evidence before owner closure.

A successful framework review does not admit the parser.

---

# 16. Later Plain-Text Admission Gate

A separate owner-reviewed admission package is required after framework implementation and review.

That later package must bind the exact:

```text
parser definition ID
implementation version
implementation SHA-256
resource manifest SHA-256
configuration version and SHA-256
fixture manifest SHA-256
focused evidence SHA-256
no-egress evidence SHA-256
packaged sidecar evidence SHA-256
dependency lock SHA-256
package schema version
security profile ID
owner decision
human actor
admission reason
```

Only then may a new immutable admission version enter `owner_admitted` and canonical plain-text parsing become available.

Any code, config, resource, fixture, package schema, dependency lock, or security-profile change after admission returns the new tuple to `under_test`.

---

# 17. Stop Conditions

Stop and return to owner review if:

- a new dependency appears necessary;
- the worker requires arbitrary paths, commands, modules, or environment input;
- no-egress cannot be proven in the packaged Windows sidecar;
- canonical package bytes contain run-specific or machine-specific data;
- exact byte/character/line coverage cannot be represented deterministically;
- V0003 requires a central migration;
- a real operator Vault would need an `owner_admitted` seed to test the framework;
- implementation creates an admission mutation endpoint or UI;
- temporary candidate bytes cannot be safely destroyed or reconciled;
- package backup/restore cannot preserve Vault isolation;
- any existing Phase 1 or Phase 2 invariant must be weakened;
- application changes escape the exact path surface;
- the work requires SRT, VTT, JSON, Markdown, XML, HTML, PDF, FTS, dossiers, LLM, network, Qdrant, graph, purge, or publication authority;
- a concrete finding reopens accepted architecture.

---

# 18. Owner Review and Authority Block

```text
Current package state: independently reviewed, corrected at 1337b5ac450ae82664aa1ad9667a85af41c4351e, clean-evidence-bound, and owner-closed through D038
Implementation authority: exact Phase 3A package and 35-invariant profile only
V0003 creation authority: exercised within D036 boundary
Plain-text candidate implementation authority: exercised in under_test state only
Plain-text owner admission authority: none
Canonical parsing authority: none
```

D036 accepts this exact package. The resulting authority is limited to:

```text
implement common parser framework
create V0003
implement m06a.text.v1 as under_test
create synthetic fixtures and exact evidence
add read-only parser status through Tauri/loopback API
prove package backup/restore and packaged no-egress behavior
```

Even after package acceptance:

```text
owner_admitted parser: blocked
canonical parsing of real Vault content: blocked
SRT/VTT/JSON: blocked
Phase 3B: blocked
Phase 4 through Phase 6: blocked
M06-B: blocked
providers/network/monitoring/live LLM/Qdrant/graph/purge/publication automation: blocked
```

D036 records the original implementation authority. Independent review required correction, and D037 authorized the exact correction package. Application implementation is corrected and clean-evidence-bound at `1337b5ac450ae82664aa1ad9667a85af41c4351e`; D038 accepts the disposition and closes Phase 3A. No review or closure result may be interpreted as parser admission.
