# M06-A `m06a.text.v1` Owner Admission and Canonical Execution — Exact Implementation Package

## Status

```text
Package status: owner-accepted for exact implementation through D039
Implementation authority: exact package and exact 28-invariant profile only
Parser admission authority: exact per-Vault human ceremony for the recorded tuple only
Canonical parsing authority: human-triggered plain-text execution only
Application baseline: 1337b5ac450ae82664aa1ad9667a85af41c4351e
Docs authorization baseline: 728d7d9
Phase 3A closure: D038
Authorization: D039
Phase 3B or later authority: none
M06-B authority: none
```

This package is the separate parser-admission gate required after the owner-closed Phase 3A framework. It does not reopen Phase 3A and does not inherit implementation authority from D036, D037, or D038.

Owner acceptance of this exact package would authorize only:

1. correction of the stale V0003 desktop Vault gate to the live V0004 head;
2. an explicit human-only, per-Vault `m06a.text.v1` admission ceremony for the exact proven tuple below;
3. human-triggered canonical plain-text parsing of an already admitted same-Vault artifact;
4. immutable execution, package, document, element, region, audit, idempotency, backup, restore, and evidence proof for that one parser tuple.

It would not authorize any other parser or later M06-A capability.

---

# 1. Why This Is the Next Gate

The owner-accepted parser admission plan states that:

- only `owner_admitted` may execute on canonical intake;
- every parser is admitted individually;
- changed implementation, configuration, dependency lock, package schema, or worker security profile returns the parser to `under_test`;
- plain text is the recommended first admission.

Phase 3A proved the common worker and installed `m06a.text.v1` as `under_test`. D038 closed that implementation without granting admission or canonical parsing. Therefore the next bounded step is the separate plain-text admission gate, not Phase 3B and not SRT/VTT/JSON implementation.

Live inspection also identified one narrow integration defect:

```text
desktop/src/App.tsx still checks and reports V0003
live Vault migration head is V0004
result: healthy V0004 Vaults suppress records and parser-status panels
```

The exact correction is included because the owner cannot perform or observe the admission ceremony through the supported Tauri interface while that gate remains stale.

---

# 2. Exact Parser Tuple Eligible for Review

```text
parser_id: m06a.text.v1
implementation_version: 1.1.0
implementation_sha256: d0d5a2973d4cf2f4327708255e7403659e51baa255658223c0f3d84f62ec3f67
resource_manifest_sha256: e01a6232960c94cf972ff6000ff9dfade5744487d3606c30dd3c437c263545c4
dependency_lock_sha256: feb1aea2f45166a25c6b1618798790f65656db9490dc63d77481c519c8765351
config_sha256: 79df7fe8740844674c12e4ab4de6d801acdef7889354b426843c4ea6431e254f
package_schema_version: m06a.normalized-package.v1
deterministic_contract_version: m06a.parser-determinism.v2
security_profile_id: m06a.parser-worker.windows.v2
```

Exact immutable IDs:

```text
parser_definition_id:
parser-definition-m06a-text-v1-3994417cea6c1a93

parser_configuration_version_id:
parser-config-m06a-text-v1-3994417cea6c-79df7fe874084467

current under_test admission ID:
parser-admission-m06a-text-v1-under-test-56adc0c43e09ae76
```

No implementation may accept a different tuple under this package. A tuple mismatch stops the package and returns to owner review.

---

# 3. Admission Evidence Baseline

The admission successor must bind the exact proven evidence:

```text
fixture_manifest_sha256:
5d9b3776becf33ca19464790b0b9136aa8744954e870d21b14893586c0a8d0c7

clean Phase 3A aggregate evidence SHA-256:
dab23159b696574ee972aa5ecec3037fcef546a72f6f6a7ca8fedf81df54185e

no-egress HT-035 evidence SHA-256:
28a462caf68f4bd98703f225b51188e69a2055dff0bb1143558f67a48fd48889

packaged-sidecar HT-044 evidence SHA-256:
b98df13a904efecb2740061628fba13716e7effba88a104aceb5c0b89e12f31d

dependency_lock_sha256:
feb1aea2f45166a25c6b1618798790f65656db9490dc63d77481c519c8765351
```

The exact admission-material hash currently resolves to:

```text
82de317451a7bbe3ac5f303bddec5459001193155b196d7796d162abb34b5dba
```

The implementation may derive an owner-admitted successor ID from that complete material, for example:

```text
parser-admission-m06a-text-v1-owner-admitted-82de317451a7bbe3
```

The full canonical material, not the human-readable suffix, is authoritative.

---

# 4. Owner Admission Ceremony

## 4.1 Per-Vault and human-only

Admission is never seeded by migration, startup, package installation, or application upgrade.

For each selected physical Vault, the supported Tauri interface may submit one explicit admission request. The trusted backend must prove:

- the central registry, Vault marker, SQLite singleton identity, migration head, integrity, audit chain, and reconciliation state are healthy;
- the actor is the active human Vault owner with `vault_admin` authority;
- the current definition/configuration/admission tuple exactly matches Section 2;
- the current admission is exactly the named `under_test` row and has no successor;
- every Section 3 evidence hash matches the owner-accepted manifest;
- no current `owner_admitted`, suspended, revoked, retired, prohibited, or ambiguous successor exists;
- the request includes the exact expected IDs/hashes and explicit confirmation text;
- the operation key is bounded and idempotent.

Required confirmation text:

```text
ADMIT m06a.text.v1 FOR THIS VAULT
```

## 4.2 Immutable successor

Admission inserts a new append-only `parser_admission_versions` row:

```text
state: owner_admitted
supersedes_admission_id: parser-admission-m06a-text-v1-under-test-56adc0c43e09ae76
admitted_by_actor_id: active human owner
admitted_at: trusted UTC timestamp
reason: exact D039 owner-approved plain-text admission package
```

The `under_test` row is never updated or deleted.

The service writes exact operation-key and audit records in the selected Vault transaction. Replay of the same request returns the same result; reuse of the operation key with different material fails.

## 4.3 Admission does not parse

Successful admission creates no parser execution, package, document, element, or region. Parsing remains a separate human-triggered operation.

No other Vault inherits admission. No other parser ID is admitted.

---

# 5. Canonical Plain-Text Execution

## 5.1 Eligible input

A canonical parse request names one existing `acquisition_artifact_link_id` in the selected Vault and one bounded operation key.

Before worker launch, the service proves:

- the selected Vault is healthy at V0004;
- the link, acquisition, rights/retention version, and artifact belong to that Vault;
- acquisition outcome is successful;
- retention eligibility is `allow` and remains preservation-compatible;
- the artifact path is the exact content-addressed path for the recorded SHA-256;
- the file is regular, non-reparse, exact-size, and exact-hash;
- exactly one current `owner_admitted` admission matches the installed tuple;
- no reconciliation work blocks the Vault.

Any failure occurs before worker launch and creates no package or document.

## 5.2 Human-triggered operation

Canonical parsing is available only through the supported Tauri/loopback path. There is no background parsing, bulk parsing, automatic post-intake parsing, watcher, schedule, provider, agent, or direct database path.

The operator selects an admitted artifact and explicitly chooses:

```text
Parse as plain text
```

The UI must display the parser ID, state, package schema, and security profile before execution.

## 5.3 Execution lifecycle

The trusted service:

1. creates an idempotency record and `parser_executions` row in `started` state;
2. copies or reads only the verified admitted artifact bytes into the bounded worker operation;
3. launches the fixed worker with the exact owner-admitted tuple;
4. validates the receipt, complete coverage, warnings, determinism contract, and candidate bytes;
5. assembles canonical package bytes;
6. stores or exactly reuses the content-addressed package;
7. finalizes the execution;
8. links execution to package through `parser_execution_package_links`;
9. creates or exactly reuses the initial document version;
10. persists deterministic elements and regions;
11. appends audit evidence and completes the idempotency record.

A failed worker finalizes the execution as failed with a safe receipt and creates no package/document authority.

## 5.4 Initial-version boundary

This package supports the initial canonical document version for one artifact under the one exact admitted tuple.

- First successful parse creates `version_ordinal=1` and `state=current`.
- Exact replay of the same operation returns the same execution/result.
- A different operation producing the identical package for the identical artifact and exact tuple may link the new successful execution to the existing package and existing document version; it must not create version noise.
- A request that would require a changed parser tuple, changed configuration, a new document ordinal, correction, withdrawal, or supersession stops and returns to a later exact package.

This limitation avoids inventing document-version transition authority before its dedicated phase.

## 5.5 Partial-write and reconciliation behavior

If package bytes are stored but database finalization fails:

- the package remains non-authoritative until linked;
- content-free reconciliation work is required;
- Vault health and backup fail closed;
- retry may reconcile only when exact package hash, tuple, artifact, and operation evidence match;
- no untracked package is silently trusted or deleted.

No worker temporary bytes survive successful or failed operation closure.

---

# 6. Read and UI Surface

Authorized endpoints:

```text
GET  /desktop-api/v1/vaults/{vault_id}/parsers
POST /desktop-api/v1/vaults/{vault_id}/parsers/m06a.text.v1/admit
POST /desktop-api/v1/vaults/{vault_id}/artifacts/{acquisition_artifact_link_id}/parse-text
GET  /desktop-api/v1/vaults/{vault_id}/documents
```

The existing parser status response may add only safe immutable IDs/hashes, admission readiness, and canonical availability. It must not expose absolute paths, fixture locations, evidence locations, secrets, tokens, or raw artifact content.

The Tauri Vault screen may add:

- corrected V0004 health gating and migration text;
- plain-text admission status;
- an explicit owner confirmation control;
- a parse action for eligible artifact rows only;
- read-only execution/document result summaries.

It may not add parser configuration editing, state suspension/revocation controls, bulk admission, bulk parsing, background work, browser/Jinja parity, or raw SQL tools.

---

# 7. Exact Application Change Surface

## Existing files allowed

```text
desktop/src/App.tsx
desktop/src/api/client.ts
desktop/src/api/client.test.ts
desktop/src/api/types.ts
desktop/src/state/vault.ts
desktop/src/state/vault.test.ts
scripts/run_ht_evidence.py
src/discrepancy_desk/parser_service.py
src/discrepancy_desk/vault_filesystem.py
src/discrepancy_desk/web.py
tests/conftest.py
tests/test_m06a_backup_restore.py
tests/test_m06a_desktop_workflow.py
tests/test_m06a_parser_framework.py
tests/test_m06a_parser_packaging.py
```

## New files allowed

```text
tests/test_m06a_text_admission.py
tests/test_m06a_text_canonical_execution.py
```

An implementation may omit an allowed path. It may not add or edit any other application path without returning to owner review.

## Explicitly forbidden

```text
pyproject.toml
uv.lock
migrations/versions/*
vault_migrations/versions/*
vault_migrations/manifest.sha256
parser_resources/*
src/discrepancy_desk/parsers/plain_text_v1.py
src/discrepancy_desk/parser_contract.py
Jinja templates
additional parser modules
provider/network/LLM/Qdrant/graph/publication files
```

No migration or dependency change is authorized. The existing V0004 schema must be sufficient; otherwise the package stops.

---

# 8. Exact Adversarial Profile

The implementation must register and execute an exact admission/canonical profile covering at least these 28 invariants:

```text
M06A-TEXT-ADMIT-001 exact tuple and evidence manifest
M06A-TEXT-ADMIT-002 active human Vault-owner guard
M06A-TEXT-ADMIT-003 explicit confirmation text required
M06A-TEXT-ADMIT-004 immutable successor; under_test row preserved
M06A-TEXT-ADMIT-005 stale or mismatched tuple/evidence refusal
M06A-TEXT-ADMIT-006 ambiguous/current-successor refusal
M06A-TEXT-ADMIT-007 per-Vault admission isolation
M06A-TEXT-ADMIT-008 admission idempotent replay and conflict
M06A-TEXT-ADMIT-009 admission creates no parser output
M06A-TEXT-ADMIT-010 no automatic/seeded owner admission
M06A-TEXT-CANON-011 V0004 desktop records/parser visibility
M06A-TEXT-CANON-012 same-Vault artifact and retention gate
M06A-TEXT-CANON-013 artifact path/size/hash/reparse verification
M06A-TEXT-CANON-014 worker launches only after exact admission
M06A-TEXT-CANON-015 source-worker canonical execution
M06A-TEXT-CANON-016 real packaged-sidecar canonical execution
M06A-TEXT-CANON-017 deterministic package and exact coverage
M06A-TEXT-CANON-018 failure creates no package/document authority
M06A-TEXT-CANON-019 execution operation-key replay/conflict
M06A-TEXT-CANON-020 exact package reuse and execution link
M06A-TEXT-CANON-021 initial document version and element/region fidelity
M06A-TEXT-CANON-022 identical rerun creates no version noise
M06A-TEXT-CANON-023 package-before-database failure requires reconciliation
M06A-TEXT-CANON-024 backup/restore includes canonical parser output
M06A-TEXT-CANON-025 missing/extra/tampered/cross-Vault output refusal
M06A-TEXT-CANON-026 API/UI mutation surface remains exact
M06A-TEXT-CANON-027 no path, secret, evidence-location, or content leakage
M06A-TEXT-CANON-028 no later-parser, Phase 3B, provider, agent, or publication leakage
```

Every node must have a unique executable test mapping, immutable receipt, zero skipped/xfail/xpass/error, and commit-bound aggregate evidence.

Inherited Phase 3A, Phase 2, Phase 1, and legacy profiles must remain green.

---

# 9. Validation and Closure

Required validation includes:

```text
uv sync --frozen
uv sync --frozen --extra dev
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
focused admission/canonical test package
exact 28-invariant admission/canonical profile
M06-A Phase 3A 35-invariant regression
M06-A Phase 2 33-invariant regression
M06-A Phase 1 31-invariant regression
legacy regression
central Alembic head 0005
Vault Alembic head V0004
Tauri Vitest
Tauri production build
Rust tests
real PyInstaller sidecar build and packaged canonical execution
exact changed-path and forbidden-file audit
git diff --check
clean commit-bound evidence
independent implementation review
finding disposition
explicit owner closure
```

Successful implementation does not automatically admit the parser in every Vault. It creates the governed ceremony; each physical Vault still requires an explicit human admission action.

---

# 10. Stop Conditions

Stop and return to owner review if:

- the parser tuple or any Section 3 evidence hash changes;
- a migration or dependency change is required;
- canonical execution requires modifying the parser implementation, schema, package schema, or worker security profile;
- safe initial-version behavior cannot be implemented without document supersession authority;
- an agent, startup, migration, or package install would create `owner_admitted` state;
- parsing cannot remain human-triggered and per-artifact;
- any input can reach the worker before retention and exact admission checks;
- any exact package, document, element, region, audit, or reconciliation invariant cannot be made executable;
- application changes escape the exact path surface;
- any SRT, VTT, JSON, Markdown, feed, HTML, PDF, FTS, chunk, projection, assertion, dossier, LLM, network, Qdrant, graph, purge, or publication capability becomes necessary.

---

# 11. Owner Authority Block

This document is a candidate only. No application work begins until the owner accepts this exact package.

Exact authorization language:

```text
I approve the exact M06-A m06a.text.v1 Owner Admission and Canonical Execution package and its exact 28-invariant profile. Proceed with implementation.

This authorizes only the V0004 Tauri gate correction, the explicit per-Vault human owner-admission ceremony for the exact m06a.text.v1 tuple and evidence manifest, and human-triggered canonical plain-text parsing of already admitted same-Vault artifacts.

This does not authorize automatic or bulk parser admission, admission in every Vault, parser configuration editing, parser lifecycle controls beyond the exact admission successor, SRT, VTT, JSON, Phase 3B, Phases 4 through 6, M06-B, providers, network retrieval, agents, live LLM integration, Qdrant, graph work, purge, or publication automation.
```
