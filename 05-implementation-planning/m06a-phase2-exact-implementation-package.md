# M06-A Phase 2 Exact Implementation Package — Observation, Acquisition, Artifacts, and Foundational Recovery

## Status

```text
Package status: owner-accepted and implementation-authorized through D034
Phase 1: independently verified and owner-accepted for closure
Phase 2 implementation authority: exact package and 33-invariant profile authorized
Parser admission: none
M06-B authority: none
```

This package extracts the exact Phase 2 boundary from the owner-accepted M06-A canonical plan and adversarial matrix. It does not redesign the architecture and does not authorize implementation merely by existing.

## Governing Baseline

Application repository:

```text
C:\dev\x\discrepancy-desk
HEAD/origin: 5f0f9ae139a6c2dece123b503a90a656c13671fe
Phase 1 implementation: 8fe3be4cc9da3183da88cee4bf4b19e2979b901d
```

Documentation repository baseline before this package:

```text
C:\dev\x\discrepancy-desk-docs
eb405c483db1ee15814d84f6288920dbe498de68
```

Required decisions and reviews:

- D027 through D031;
- D032 deferred-audit timing decision;
- independent Phase 1 verdict `M06-A PHASE 1 INDEPENDENTLY VERIFIED`;
- Phase 1 review disposition at `08-audits/m06a-phase1-independent-review-disposition.md`;
- the controlling canonical plan and adversarial matrix.

## Objective

Add one fully manual, selected-Vault intake and immutable-artifact foundation that truthfully records observations and acquisition encounters, rejects retention-ineligible material before canonical byte admission, stores admitted bytes under content-addressed no-overwrite paths, and proves a foundational per-Vault backup and disposable restore path.

Phase 2 must establish useful canonical storage without admitting any parser or derived interpretation layer.

## Fixed Scope Boundary

### Included

- Vault migration `V0002_ingestion_artifacts_policy_and_backup.py`;
- sources, source items, occurrences, observations, acquisitions;
- immutable content-addressed artifact objects and encounter-specific links;
- rights/retention versions and immutable artifact-policy bindings;
- content-free intake-rejection receipts;
- operation-scoped temporary intake and reconciliation;
- manual locator-only observations with truthful `no_artifact` handling;
- manual local-file byte admission through Tauri and the token-gated desktop API;
- foundational per-Vault backup generation, verification, and disposable restore proof;
- Tauri intake/backup operator surface only;
- exact Phase 2 hammer suite and immutable commit-named evidence preservation;
- bounded hardening of Phase 1 findings L-01 and L-02 within the Phase 2 review surface.

### Excluded

- parser definitions, parser workers, parser execution, normalized packages, document versions, elements, or regions;
- Markdown, SRT, VTT, JSON, RSS/Atom, HTML, or PDF parsing;
- OCR, media download, browser rendering, URL fetch, crawling, monitoring, feeds, or WebSub;
- search, FTS, chunks, projections, assertions, entities, questions, dossiers, or context runs;
- provider calls, live LLM integration, Qdrant, embeddings, graph work, or cross-Vault import execution;
- automatic deletion, deletion scheduling, purge, purge tombstones, or a promise to delete later;
- autonomous publishing or any change to central exact-content approval/publication authority;
- new Jinja/browser Vault product routes or browser/Tauri parity work;
- M06-B planning or implementation.

## Initial Intake Limits

Phase 2 manual byte intake uses a server-owned maximum of:

```text
64 MiB per admitted file
```

The backend must enforce the actual streamed byte count. Tauri-provided size and media type are advisory metadata only. Increasing this ceiling requires an owner-reviewed package amendment.

Phase 2 does not execute or render admitted files. Unknown media types remain `application/octet-stream` and are stored only as inert immutable bytes.

## Exact Migration

Create:

```text
vault_migrations/versions/V0002_ingestion_artifacts_policy_and_backup.py
```

Update:

```text
vault_migrations/manifest.sha256
```

Central migration state remains exactly `0005`. No central schema change is admitted.

### Required canonical tables

#### `sources`

- `id`;
- `vault_account_id`;
- source kind constrained to the Phase 2 manual vocabulary;
- optional platform label that cannot select another Vault;
- human-readable non-authority label;
- created actor/time;
- `UNIQUE(vault_account_id, id)`.

#### `source_items`

- `id`;
- `vault_account_id`;
- `source_id` composite-scoped to the same Vault;
- inert locator or manual-item identity;
- item state;
- created actor/time;
- no remote-fetch success field.

#### `occurrences`

- `id`;
- `vault_account_id`;
- `source_item_id`;
- occurrence kind and observed locator/version descriptor;
- occurrence time when known;
- created actor/time.

#### `observations`

- `id`;
- `vault_account_id`;
- `occurrence_id`;
- actor ID;
- observation method;
- observed time;
- observation state;
- optional bounded human note or structured receipt hash;
- immutable creation fields.

#### `rights_retention_versions`

Required dimensions:

```text
retention_eligible
retention_deadline
internal_retrieval_eligible
context_run_eligible
export_eligible
internal_projection_eligible
public_projection_eligible
quotation_redistribution_eligible
policy_basis
human_classification_note
reviewed_by_actor_id
reviewed_at
```

Only preservation-compatible Phase 2 material may have `retention_eligible=allow`, and its `retention_deadline` must be null. Unknown defaults to deny for every other capability.

#### `intake_rejection_receipts`

Append-only and content-free:

- Vault and actor IDs;
- operation/correlation ID;
- attempted source-kind classification;
- safe non-content descriptor classification only;
- declared retention classification;
- policy-basis reference;
- reason code;
- client-generated nonce;
- occurred time.

It must prohibit content bytes, pasted text, content-bearing filename/locator text, content-derived hashes, and raw diagnostics.

#### `acquisitions`

- `id`;
- `vault_account_id`;
- observation ID or explicit manual-intake event reference;
- actor ID;
- lifecycle state constrained to `started`, `finalized`, `interrupted`, or `reconciled`;
- outcome nullable while started and constrained terminally to `succeeded`, `failed`, `rejected`, `quarantined`, or `no_artifact`;
- operation/correlation ID;
- start/final timestamps;
- error class/code when applicable;
- supplied filename/media type metadata;
- rights/retention version ID;
- receipt hash.

A row starts only as `started` with `outcome=NULL`. `no_artifact` is legal only for an operation that never expected bytes, such as locator-only manual intake.

#### `artifact_objects`

- `id`;
- `vault_account_id`;
- SHA-256;
- byte size;
- canonical storage-relative path;
- observed media type;
- creation time;
- `UNIQUE(vault_account_id, sha256)`;
- no update/delete path through normal service authority.

#### `acquisition_artifact_links`

- `id`;
- `vault_account_id`;
- acquisition ID;
- artifact object ID;
- role;
- supplied filename/media type;
- rights/retention version ID;
- receipt SHA-256;
- linked time;
- `UNIQUE(vault_account_id, acquisition_id, artifact_object_id, role)`.

Repeated identical bytes create one artifact object but preserve separate acquisitions and links.

#### `artifact_policy_bindings`

- immutable binding identity;
- Vault-scoped artifact ID;
- rights/retention version ID;
- binding state;
- supersession lineage;
- actor/time;
- only one current binding per artifact through an engine-enforced rule.

A new policy binding supersedes; it never mutates the artifact or prior binding.

#### `backup_generations`

- generation ID;
- Vault identity;
- correlation ID;
- lifecycle state;
- start/completion timestamps;
- migration head;
- application commit;
- manifest SHA-256;
- completion-marker SHA-256;
- actor ID;
- failure/reconciliation code where applicable.

#### `backup_generation_files`

- generation ID;
- Vault account ID;
- relative path;
- file family/classification;
- SHA-256;
- byte size;
- canonical/derived classification;
- required flag;
- unique path per generation.

#### Foundational backup receipts/reconciliation

V0002 must include the start/final receipt structures required before backup execution, either through typed columns/tables above or an exact extension of the existing per-Vault receipt and reconciliation tables. Phase 6 may add closure accounting but may not introduce foundational Phase 2 backup authority.

### Constraints and downgrade

V0002 must enforce:

- same-Vault composite references;
- exact acquisition state/outcome combinations;
- no successful byte acquisition without an artifact link;
- locator-only `no_artifact` cannot mask failure, rejection, interruption, missing expected bytes, or parser failure;
- preservation-compatible policy before artifact linkage;
- immutable policy-binding lineage;
- one current policy binding;
- append-only rejection, acquisition finalization history, artifacts, links, backup generations, and generation-file records through triggers or service-plus-tamper verification where appropriate;
- no destructive downgrade when any V0002 governed row exists.

## Governed Intake Sequence

### Step 1 — Retention decision before bytes

Tauri submits metadata only to:

```text
POST /desktop-api/v1/vaults/{vault_id}/intake
```

The request contains:

- client operation key and nonce;
- manual source kind;
- optional inert locator;
- sanitized display descriptor;
- declared retention classification;
- policy-basis reference;
- human classification note;
- advisory filename, media type, and byte size;
- whether bytes are expected.

The request never supplies actor ID, Vault filesystem path, or canonical destination.

If retention is missing, contradictory, `unknown`, or `timed_deletion_required`, the backend writes only a content-free rejection receipt and returns a stable refusal. It must not accept a multipart body, open a supplied file, retain a content hash, create an acquisition, or create a temp directory containing user bytes.

If preservation-compatible and bytes are expected, the backend creates the source/item/occurrence/observation chain, a rights/retention version, a started acquisition, and a one-time server-owned upload authorization bound to that acquisition.

If no bytes are expected, the backend may finalize the governed locator-only acquisition as `no_artifact` after proving the operation is truthfully locator-only.

### Step 2 — Byte upload after authorization

Tauri sends the user-selected file as a bounded multipart stream to:

```text
POST /desktop-api/v1/vaults/{vault_id}/acquisitions/{acquisition_id}/artifact
```

The backend must:

1. resolve the selected Vault by opaque ID;
2. resolve the trusted actor server-side;
3. verify the started acquisition, one-time authorization, retention version, operation key, and Vault identity;
4. stream into `temp/<operation-id>/` using create-new semantics;
5. reject over 64 MiB without canonical admission;
6. hash while streaming;
7. close and flush the temp file;
8. derive `objects/sha256/aa/bb/<full-sha256>`;
9. verify an existing object by byte size and full hash before reuse;
10. otherwise move with no-overwrite semantics and verify the final object;
11. in one Vault transaction create/reuse the artifact row, create the encounter link and current policy binding, finalize acquisition, append audit, and bind idempotency;
12. destroy temp bytes only after commit;
13. leave typed reconciliation state after any interruption.

No caller supplies a canonical relative path.

## Filesystem and Reconciliation Contract

Create a dedicated module for content-addressed operations rather than extending central evidence-file helpers ambiguously.

Required behavior:

- object paths derive only from validated lowercase SHA-256;
- no path traversal, absolute path, alternate stream, reserved name, symlink, junction, or reparse traversal;
- no canonical overwrite;
- existing object mismatch creates an integrity incident and blocks reuse;
- temp directories are operation-scoped and excluded from ordinary reads and backup;
- crash before object move leaves no canonical object/link;
- crash after object move but before DB commit leaves a non-authoritative orphan candidate;
- orphan relinking requires the exact pending receipt and matching acquisition/size/hash/policy/Vault metadata;
- unresolved temporary or orphan state blocks clean health and backup readiness.

Phase 2 should apply the audit L-01 hardening by rechecking the created Vault/object path reparse chain immediately before migration or canonical byte movement.

## Foundational Backup and Disposable Restore

Create a Vault-specific backup module. Existing central `backup.py` behavior remains unchanged.

### Generation

For one selected Vault only:

1. resolve and verify the Vault;
2. refuse unresolved temporary/orphan/reconciliation work;
3. create a unique generation workspace under that Vault's `backups/` root with create-new semantics;
4. append a generation-start record;
5. use the SQLite backup API for a consistent Vault database snapshot;
6. copy admitted immutable objects using no-overwrite semantics;
7. classify every included file as canonical or derived;
8. write a deterministic manifest containing Vault identity, migration head, application commit, policy state, relative paths, sizes, and hashes;
9. verify every required file and the manifest;
10. create `COMPLETE` with create-new semantics;
11. append the generation-complete record;
12. reconcile a crash between filesystem completion and the final database receipt.

Phase 2 has no normalized packages yet, so package inclusion is empty and represented honestly.

A generation must never include another Vault's database, objects, packages, temp data, prior backup history, or rejected material.

### Disposable restore proof

Restore operates only into an empty caller-independent proof workspace generated by the backend. It does not register or replace a live Vault.

It must verify:

- empty target;
- manifest and `COMPLETE` marker;
- selected Vault account/instance identity;
- every required file/hash/size;
- SQLite integrity;
- migration head `V0002`;
- singleton identity;
- audit chain;
- acquisition/artifact/link/policy consistency;
- no missing or orphan canonical object;
- no rejected operation content;
- no other Vault content.

Derived tables/files, when later present, are non-authoritative and must be cleared before use. Phase 2 records this classification even though Phase 4 derivatives do not yet exist.

## Tauri-Only Operator Surface

Add one substantial selected-Vault workspace in Tauri containing:

- manual locator observation;
- manual local-file selection;
- retention classification and policy-basis fields;
- visible refusal reason without raw path/content leakage;
- upload progress and final acquisition result;
- recent observations/acquisitions/artifacts;
- current policy-binding status;
- backup generation, verification, and disposable restore-proof actions;
- blocked-health/reconciliation state.

Use a Tauri/webview file input to obtain a user-selected `File` and send bytes through the token-gated loopback API. Do not send arbitrary source filesystem paths to the backend. Tauri does not decide retention eligibility, acquisition state, canonical path, deduplication, policy binding, backup completeness, or restore validity.

No `/vaults` Jinja product route or new M06 template is admitted.

## Exact Application Change Surface

### Existing files likely modified

```text
vault_migrations/manifest.sha256
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
src/discrepancy_desk/vault_identity.py
src/discrepancy_desk/vault_persistence.py
src/discrepancy_desk/vault_router.py
src/discrepancy_desk/vault_service.py
src/discrepancy_desk/web.py
src/discrepancy_desk/test_evidence.py
tests/conftest.py
desktop/src/App.tsx
desktop/src/api/client.ts
desktop/src/api/client.test.ts
desktop/src/api/types.ts
desktop/src/state/vault.ts
desktop/src/state/vault.test.ts
```

Inherited M00–M05 and Phase 1 tests may be updated only for the expected Vault head change from `V0001` to `V0002` or strengthened contracts. Assertions may not be weakened.

### New files admitted

```text
vault_migrations/versions/V0002_ingestion_artifacts_policy_and_backup.py
src/discrepancy_desk/vault_filesystem.py
src/discrepancy_desk/vault_ingestion.py
src/discrepancy_desk/vault_backup.py
src/discrepancy_desk/vault_reconciliation.py
tests/test_m06a_ingestion_artifacts.py
tests/test_m06a_policy_context.py
tests/test_m06a_backup_restore.py
tests/test_m06a_phase2_desktop_workflow.py
```

A separate module may be consolidated only when the responsibilities and tests remain explicit. No parser module or dependency file change is admitted.

## Phase 2 Hammer Suite

The dedicated Phase 2 suite must map exactly these Phase 2 closure invariants:

```text
M06A-HT-018 through M06A-HT-025
M06A-HT-027
M06A-HT-029
M06A-HT-030
M06A-HT-066 through M06A-HT-070
M06A-HT-073
M06A-HT-092
M06A-HT-096
M06A-HT-098 through M06A-HT-105
M06A-HT-108
```

Required inherited regression invariants must also execute in the Phase 2 closure profile:

```text
M06A-HT-007
M06A-HT-014
M06A-HT-065
M06A-HT-095
M06A-HT-097
```

The phase profile therefore executes 33 exact invariants: 28 Phase 2 closure invariants plus 5 inherited authority/recovery regressions. The legacy and Phase 1 suites must remain green independently.

## Evidence Hardening

The mutable ignored `latest` evidence behavior must be strengthened before Phase 2 closure.

For full-suite and hammer aggregates, write both:

```text
runtime/.../by-commit/<full-commit-sha>.json
runtime/.../latest-....json
```

Commit-named evidence uses create-new/no-overwrite semantics. An existing same-commit file must compare byte-for-byte; mismatch fails closed. Closure records bind the immutable commit-named file, not only the mutable alias.

Evidence must record:

- commit SHA and clean/dirty state;
- suite/matrix identity and exact invariant order;
- migration heads `0005` and `V0002`;
- Python/SQLite/platform versions;
- exact node results and counts;
- sidecar/package evidence where applicable.

## Required Validation

Before implementation commit:

```text
uv sync --frozen
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
uv run pytest -o addopts= --disable-warnings -q tests/test_m06a_ingestion_artifacts.py tests/test_m06a_policy_context.py tests/test_m06a_backup_restore.py tests/test_m06a_phase2_desktop_workflow.py
uv run python scripts/run_ht_evidence.py --suite m06a-phase2
uv run python scripts/run_ht_evidence.py --suite m06a-phase1
uv run python scripts/run_ht_evidence.py --suite legacy
uv run alembic -c alembic.ini heads
uv run alembic -c vault_migrations/alembic.ini heads
pnpm --dir desktop test
pnpm --dir desktop build
pnpm --dir desktop exec cargo test --manifest-path src-tauri/Cargo.toml
uv run python scripts/build_desktop_sidecar.py
git diff --check
```

Additional proofs:

- fresh and populated V0001→V0002 migration in two synthetic Vaults;
- interruption and dirty-state recovery at V0002;
- central remains at `0005`;
- actual 64 MiB boundary behavior without reading an ineligible upload;
- duplicate bytes preserve encounters;
- mismatch at an existing object path fails;
- temp and orphan reconciliation;
- timed-deletion and unknown retention reject before bytes;
- rejection outputs contain no content or content-derived hash;
- per-Vault backup excludes another Vault and prior backup history;
- disposable restore rejects dirty target, wrong Vault, missing original, tamper, and incomplete generation;
- Tauri-only route and no browser Vault surface;
- packaged sidecar contains and executes V0002;
- staged-scope, secret, generated-output, and migration-manifest review.

After implementation commit, rerun the full stack against the clean implementation SHA, create immutable commit-named evidence, obtain independent Claude implementation review, correct findings, and bind closure evidence in a follow-up commit.

## Stop Conditions

Stop before implementation or commit when:

- any schema or service choice would require parser authority;
- retention policy cannot be classified before bytes reach the backend;
- the Tauri upload path requires arbitrary backend filesystem paths;
- canonical object overwrite cannot be proven impossible;
- backup would mix Vaults or include rejected/temp/backup-history content;
- restore would alter or register a live Vault;
- a migration or evidence detector is ambiguous;
- a new dependency appears necessary;
- a finding changes the accepted architecture;
- application changes escape the exact admitted surface without owner amendment.

## Owner Authorization

The owner explicitly accepted this package and its exact 33-invariant profile through D034 on 2026-07-22.

```text
Phase 2 application edits: authorized within this exact package
V0002 creation: authorized
manual artifact intake: authorized
foundational backup/restore execution: authorized
parser work: blocked
Phase 3 through Phase 6: blocked
M06-B: blocked
```

Implementation authority ends at this package boundary. Phase 2 closure still requires clean commit-bound evidence, independent review, finding disposition, and explicit owner acceptance.
