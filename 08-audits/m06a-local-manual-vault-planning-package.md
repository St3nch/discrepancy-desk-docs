# M06-A Local Manual Vault — Exact Implementation Planning Package

**Prepared:** 2026-07-21 · **Mode:** planning only · **Implementation authority:** none
**Brand:** The Discrepancy Desk · **Persona:** Quinton Clearance
**Doctrine:** AI drafts. Human clears. Database remembers. Metrics judge.

This package is prepared read-only from live repository truth. It defines the exact proposed schema, migrations, filesystem layout, parser admissions, tests, evidence outputs, and phases for M06-A. It does **not** authorize implementation.

---

## A. Live Repository Truth

Both repositories were freshly cloned and inspected read-only in an ephemeral Linux workspace. No file in either repository was modified, staged, committed, or pushed.

### Documentation repository — `discrepancy-desk-docs`

```text
branch:           main
HEAD:             531377cb1db50409851251e749cec9ba666efc79
latest commit:    531377c — Reconcile M06 milestone planning gate (2026-07-21 11:06:05 -0400)
tracking:         main...origin/main (synchronized; no ahead/behind)
working tree:     clean
expected checkpoint: 531377c — Reconcile M06 milestone planning gate   → MATCHES
```

### Application repository — `discrepancy-desk`

```text
branch:           main
HEAD:             6cbd0366e55bfba0c9687201615b72e70bf485d5
latest commit:    6cbd036 — Bind AC-01 correction evidence (2026-07-20 16:43:31 -0400)
tracking:         main...origin/main (synchronized; no ahead/behind)
working tree:     clean
expected checkpoint: 6cbd036 — Bind AC-01 correction evidence          → MATCHES
```

**No deviation from expected checkpoints.** Both repositories are clean and synchronized with `origin/main`.

### Relevant existing application components (verified by reading source)

Stack (`pyproject.toml`): Python ≥3.11; runtime deps `alembic>=1.13`, `fastapi>=0.115`, `jinja2>=3.1`, `python-multipart>=0.0.20`, `sqlalchemy>=2.0`, `uvicorn>=0.34`; dev `pytest>=8`, `ruff>=0.6`, `httpx2>=2.7`; build/packaging `hatchling`, `pyinstaller>=6`. Project scripts expose `discrepancy-desk-desktop-backend`. The toolchain driver is **uv** (`uv run …`, per STATUS).

Persistence (`src/discrepancy_desk/`):

- `db.py` — SQLite connection contract: WAL journal (enforced), `PRAGMA foreign_keys=ON` (enforced, fails closed if disabled), `busy_timeout=5000ms`, `isolation_level=None` (manual transaction control), `row_factory=Row`. `begin_write()` issues `BEGIN IMMEDIATE`.
- `persistence.py` — **raw `sqlite3`** governed operations (the `sqlalchemy` dependency is present but the persistence layer uses hand-written SQL, not the ORM). Contains: hash-chained append-only `append_audit`/`verify_audit_chain`; `transition_work_item` (legal-transition machine; governed states require dedicated functions); `register_evidence` (path-escape guard + hash verify); `create_revision`/`create_successor_revision` (content binding); `approve_revision`; `mark_manual_ready`; `record_publication`/`record_publication_mismatch`/`record_replacement_publication`; idempotency via `existing_operation`/`record_operation`; `detector_advice` (authority = `advisory_only`).
- `binding.py` — deterministic content binding (`RevisionBundle` → canonical JSON → SHA-256; `BINDING_VERSION=1`).
- `migration_integrity.py` — `manifest.sha256` verification; `.migration-dirty.json` guard markers; `recover_completed_migration`; `discard_failed_empty_migration` (refuses to discard if user objects exist).
- `migration_runner.py` — `run_guarded_upgrade` (verify manifest → assert clean → write dirty marker → alembic upgrade → integrity_check + version check → clear marker; marker persists on failure for governed recovery).
- `backup.py` — `create_generation` (SQLite `.backup()` consistent snapshot + evidence tree copy + `manifest.json` of per-file sha256/byte_size); `verify_generation` (manifest hash/size checks, `integrity_check`, audit-chain verify, DB↔evidence reconciliation, orphan-evidence detection).
- `archive.py` — `create_deterministic_zip` (fixed 1980 timestamps, sorted, 0o600) + `encrypt_with_age` (external `age` CLI; fails closed and removes partial output if `age` missing).
- `operator_service.py` — functional service layer (`create_owned_account`, `capture_work_item`, `add_source_record`, `reject_review`, `get_control_room_item`, `run_matched_operator_loop`, `organize_work_item`, `set_work_item_tags`, `schedule_work_item`, `reschedule_work_item`, `unschedule_work_item`, `set_editorial_target`, `record_manual_metric_observation`, `record_manual_publication_result`, `reconcile_publication_result`).
- `editorial_queries.py`, `web.py` (FastAPI `/desktop-api/v1/*`), `test_evidence.py`.

Migrations (`migrations/versions/`): `0001_initial_persistence`, `0002_operator_source_records`, `0003_revision_and_publication_lineage`, `0004_editorial_schedule_contract`. `env.py` uses `render_as_batch=True`, sets WAL/foreign_keys/busy_timeout online. `manifest.sha256` pins all four revision hashes. `alembic.ini` → `sqlite:///runtime/discrepancy-desk.sqlite3`, `prepend_sys_path = src`.

Existing tables (all `STRICT`, TEXT UUID PKs, CHECK-enum columns, `ON DELETE RESTRICT` FKs, SHA-256 `length(...)=64` checks, ISO-8601 TEXT timestamps): `owned_accounts`, `work_items`, `evidence_refs`, `revisions`, `approvals`, `publications`, `metric_snapshots`, `operation_keys`, `audit_events` (+ append-only triggers), `source_records`, `editorial_profiles`, `work_item_tags`, `schedule_slots`, `editorial_targets`.

Desktop (`desktop/src-tauri/`): Tauri shell; `security.rs` generates a 64-char launch token and pins the backend to `http://127.0.0.1:{port}` (loopback only); `web.py` enforces `x-discrepancy-desk-token` on `/desktop-api/v1/*`. Backend ships as a PyInstaller sidecar.

Evidence/tests: `scripts/run_ht_evidence.py` runs numbered hammer invariants HT-01…HT-20 mapped to pytest node IDs and emits per-invariant JSON (commit SHA, sqlite/python versions, expected/actual, pass). `tests/conftest.py` writes a full-suite evidence JSON. `.gitignore` excludes `runtime/`, `backups/`, `restore/`, `evidence/`, `*.sqlite3`, `secrets/`, `*.key`, `*.pem`.

### Compatibility constraints inherited from M00–M05

1. **`account` is overloaded.** In the shipped schema, `owned_accounts` is a **platform handle** (X / Truth Social), not a persona/editorial owner. There is **no persona/Vault-owner table**. The M06 "account = separate physical Vault" concept (D03) does not yet exist in code. This is the single most consequential compatibility fact and drives Owner Decision W-1/W-2.
2. **Raw SQL + STRICT tables.** New tables must follow the established DDL idiom (raw `op.execute`, `STRICT`, CHECK enums, `ON DELETE RESTRICT`, len-64 hash checks), not introduce ORM models.
3. **Append-only audit is global and chained.** Any new governed mutation must extend the existing `audit_events` chain via `append_audit`, not a parallel audit table.
4. **Idempotency is centralized.** New governed write operations reuse `operation_keys`.
5. **Migrations are hash-pinned and guarded.** Every new revision must be appended to `manifest.sha256` and run through `run_guarded_upgrade`.
6. **Downgrade is already non-destructive by precedent** (0003 declines to drop columns).
7. **Backups are currently single-DB / single-evidence-root.** They must be generalized to per-Vault without breaking the M00–M05 control-room backup.

### Drift from STATUS / planning documents

None material. STATUS `Docs Repository Truth` lists "AC-02 closure and M06-A planning authorization: **pending current commit**"; that line is now satisfied by HEAD `531377c` and the committed `ac02-...-closure.md` / `m06a-planning-authorization.md`. F-6 (stale STATUS commit line) from the synthesis review is effectively closed by the current commit. `03-system-design/obsidian-qdrant-sqlite-plan.md` now carries a "Historical Research and Superseded Architecture" header — **F-4 resolved**. No other drift observed.

---

## B. Executive Surgical Assessment

**Why M06-A is high-risk.** It is the first subsystem that (a) writes canonical truth to two stores at once (SQLite + governed filesystem), (b) introduces a persona/Vault account identity above the existing platform-handle model, (c) executes third-party parsers over untrusted local files, and (d) generates derived artifacts (projections, indexes, chunks, static context runs) that must never back-flow into authority. Each is an authority-boundary hazard; together they are a dual-write, multi-tenant, untrusted-input problem layered onto a live, owner-accepted M00–M05 system.

**Authority surfaces it touches.** It *adds to* the append-only audit chain, the migration manifest, the backup/restore contract, and the desktop API boundary. It *must not alter* the M00–M05 editorial/publication authority (work_items lifecycle, revision binding, approval, publication lineage). New Vault authority sits **beside** the editorial pipeline, linked only by explicit, human-governed references.

**What remains untouched.** The X/Truth-Social publication path; approval binding; metric snapshots; schedule/editorial tables; the Tauri security model (loopback + token). No M06-A capability posts, fetches, schedules, or calls a provider.

**Primary failure modes.** (1) SQLite/filesystem dual-write partial states leaving orphan artifacts or orphan rows; (2) cross-account leakage through shared DB rows, search, caches, logs, temp files, or rebuilt derivatives; (3) parser network egress or resource exhaustion on hostile local files; (4) silent parser truncation presented as complete; (5) an edited projection or re-ingested model output silently gaining authority; (6) backup omitting a required original or a wrong-account restore succeeding.

**One operation or phased?** **Phased.** A single migration + full feature drop would be unreviewable and unrollback-able against a live accepted system. The work decomposes cleanly into independently validatable, independently rollback-able phases along the authority-dependency graph (identity → artifacts → normalization/parsers → search/projections → assertions/review → backup/restore/closure).

**Recommended phasing (derived from repository dependency truth).**

```text
Phase 1  Vault account identity + authority primitives (schema only, additive)
Phase 2  Filesystem Vault layout, original-artifact preservation, dual-write reconciliation
Phase 3  Normalized JSON element packages + first narrow parsers (admitted one at a time)
Phase 4  Lexical (FTS5) search, deterministic chunk contract, read-only projections
Phase 5  Assertions/mentions/entities/dossiers + review & policy-impact workflows
Phase 6  Per-account backup/restore/rebuild + full hammer closure
```

Rationale for these six (vs. the outline's candidate six): the outline is close, but repository truth argues for pulling **projections into Phase 4** (they depend only on document versions + search invalidation, not on assertions) and keeping **entities/merge-split with assertions in Phase 5** (both are human-review authority). This yields the smallest independently reviewable slices. See §T.

---

## C. Exact Scope and Exclusions

### Included in the first M06-A implementation

- One **physically separate Vault per persona-account**, with account-bound SQLite authority (Owner Decision W-2 fixes "separate DB file" vs "shared DB + account_id"; this plan recommends **separate per-Vault DB file** and specifies both).
- Governed **local, human-triggered intake only**: pasted text, uploaded text, Markdown, SRT, VTT, JSON, locally-supplied RSS/Atom files, locally-supplied basic HTML files, born-digital PDF files, and YouTube identity/context + human-supplied transcript.
- **Immutable original-artifact preservation** with hashes, media-type evidence, and dual-write reconciliation.
- **Versioned normalized JSON element packages** (never overwritten; superseded).
- **Narrow parsers**, each behind its own admission record and owner gate (§K); none auto-admitted.
- **Local SQLite FTS5 lexical search** + **deterministic chunk contract** with invalidation.
- **Generated read-only Markdown/HTML projections** with tamper detection.
- **Assertions, mentions, entity candidates/entities, dossiers, corrections/supersessions, policy versions + impact records**, all human-governed.
- **Static LLM context-run / evidence-packet schema** (definition + validation only; no provider call).
- **Per-account backup generation + disposable restore proof** with the mandated checks.
- **Account-scoped logs, caches, temp, work dirs**.

### Excluded (must not leak into M06-A)

URL/network fetching; RSS/Atom polling; WebSub; feeds/monitoring; provider calls; media downloading; OCR; scanned PDFs; office documents; rendered-browser capture; recurring/scheduled work; live LLM/provider integration; Qdrant; embeddings; graph/GraphRAG; M06-B static retrieval; cross-account import **execution**; first-class events/chronologies (D10); autonomous truth/approval/publication/merge authority.

### Deferred decisions (carried to §W)

Vault-DB physicality; legacy-table account backfill; FTS5 packaged-build verification; editorial-use → publication gating strength; age-required-for-closure.

### Explicit stop conditions

Any of: a parser performs or attempts network I/O; a dual-write leaves an unreconciled orphan; a projection edit is neither overwritten nor rejected; a wrong-account restore succeeds; cross-account data appears in any derivative; the audit chain fails to verify; a migration leaves a dirty marker that cannot be governed-recovered. On any stop condition, implementation halts and returns to owner (§V).

### Authority that remains human-only

Research admission; truth/support assessment; editorial-use ruling; publication approval; entity merge/split acceptance; correction/supersession acceptance; cross-account import (deferred entirely).

---

## D. Existing Application Compatibility Map

| Concern | Existing component (verified) | M06-A disposition |
|---|---|---|
| SQLite database | `runtime/discrepancy-desk.sqlite3` (single control-room DB) | **new component**: one Vault DB **per account** (recommended), separate from control-room DB; see W-2 |
| Persistence layer | `persistence.py` raw-SQL governed ops | **extends**: add `vault_persistence.py` following the same idioms (begin_write, append_audit, operation_keys) |
| ORM | `sqlalchemy>=2.0` present but **unused** by persistence | **remains documentation-only**: continue raw-SQL DDL; do not introduce ORM models |
| Alembic | 4 revisions, `manifest.sha256`, guarded runner | **extends**: append Vault revisions; if separate Vault DB, a **second migration tree / version table** (W-2) |
| Service/repository pattern | functional modules (`operator_service.py`) | **new component**: `vault_service.py` functional module mirroring the pattern |
| Backup code | `backup.py` single-DB + evidence tree | **extends**: per-Vault generation + missing-original/wrong-account/cross-account checks |
| Archive/encryption | `archive.py` deterministic zip + `age` | **extends**: reuse unchanged for Vault generations; `age` optional (W-5) |
| Account model | `owned_accounts` = platform handle | **new component**: `vault_accounts` (persona/Vault owner); optional link to `owned_accounts`; **legacy tables not modified** (recommended, W-1) |
| Web/Tauri boundary | FastAPI `/desktop-api/v1/*`, loopback+token | **extends**: add read-mostly Vault endpoints behind the same token/loopback; ingestion stays local operator-triggered |
| Test structure | `tests/test_*` + `run_ht_evidence.py` HT-01…HT-20 | **extends**: new `tests/test_vault_*`; new invariants HT-21…HT-NN appended to the runner |
| Evidence output | `runtime/test-evidence/`, per-invariant JSON | **extends**: same directory + schema; new Vault evidence manifests |
| Config / filesystem conventions | `runtime/`, `backups/`, `restore/`, `evidence/` gitignored | **extends**: Vault roots under a gitignored `runtime/vaults/…` (or configured base) |
| Audit | global chained `audit_events` + triggers | **extends** (shared DB) or **replicated contract** (per-Vault DB): Vault mutations chain into the Vault's audit; see W-2 |
| Migration integrity | `migration_integrity.py` markers/manifest | **extends**: same guard for Vault migrations |

**Existing code that must not be bypassed:** `db.connect` (PRAGMA contract), `begin_write` (IMMEDIATE), `append_audit`/`verify_audit_chain`, `operation_keys` idempotency, `migration_runner.run_guarded_upgrade` + `verify_manifest`, `register_evidence` path-escape guard, `binding` hashing, `backup.verify_generation`. New Vault code reuses these primitives rather than reimplementing them.

---

## E. Physical SQLite Schema Proposal

**Conventions (inherited, mandatory):** every table `STRICT`; TEXT UUID primary keys; ISO-8601 UTC TEXT timestamps (`utc_now()` microsecond precision); enum columns as `CHECK (col IN (...))`; hashes `CHECK (length(x)=64)`; foreign keys `ON DELETE RESTRICT`; no destructive deletes on canonical rows (supersession only); mutations chained through `audit_events`.

**Account scoping rule.** Under the recommended **per-Vault DB** model, every canonical table still carries a redundant, non-null `vault_account_id` equal to the Vault's single account, enforced by a `CHECK` against the Vault identity marker; this makes cross-account contamination detectable even if a DB file is misplaced. Under the alternative **shared DB** model, `vault_account_id` is the tenant discriminator on every row and every query must filter by it.

Legend: **C** = canonical, **D** = derived/rebuildable, **A** = append-only audit.

### E.1 Account / Vault binding

**`vault_accounts`** (C) — the persona/Vault owner (the missing identity layer).
Columns: `id` PK; `slug TEXT NOT NULL UNIQUE CHECK(slug = lower(slug) AND slug GLOB '[a-z0-9-]*' AND length(slug) BETWEEN 2 AND 40)`; `display_name TEXT NOT NULL`; `vault_root_fingerprint TEXT NOT NULL UNIQUE` (hash of the account identity marker, §H); `policy_current_version_id TEXT` (FK → `policy_versions.id`, nullable at creation, resolved after first policy); `state TEXT NOT NULL CHECK(state IN ('active','suspended','retired'))`; `created_at`, `updated_at`.
Immutable: `id`, `slug`, `vault_root_fingerprint`. Mutable: `display_name`, `policy_current_version_id`, `state`, `updated_at`. No deletion. Index: unique on `slug`, `vault_root_fingerprint`.
*Optional* link table **`vault_account_owned_accounts`** (C): `(vault_account_id, owned_account_id)` PK, both FK, to relate a Vault to existing platform handles **without modifying `owned_accounts`**.

### E.2 Sources and occurrences

**`sources`** (C) — publisher/channel/person/org origin. `id` PK; `vault_account_id` NN FK; `source_kind TEXT NOT NULL CHECK(source_kind IN ('publisher','channel','account','feed','archive','person','organization','other'))`; `display_name TEXT NOT NULL`; `admission_stage TEXT NOT NULL CHECK(admission_stage IN ('proposed','manual_only','manual_admitted','automation_research','pilot_authorized','automated_admitted','suspended','retired','revoked','prohibited'))`; `permitted_methods_json TEXT NOT NULL` (bounded JSON array validated in service against the fixed method vocabulary, §7 correction spec); `created_at`, `updated_at`. Unique: `(vault_account_id, source_kind, display_name)`. Index: `(vault_account_id, admission_stage)`. Correction: state changes go through governed transitions + audit; no silent stage/method inference.

**`source_items`** (C) — durable external/intellectual item (post ID, video ID, article identity, GUID, report, document). `id` PK; `vault_account_id` NN FK; `source_id` NN FK; `item_kind TEXT NOT NULL`; `external_identity TEXT` (typed evidence, not authority); `title TEXT`; `created_at`. Unique: `(source_id, item_kind, external_identity)` where external_identity NOT NULL.

**`occurrences`** (C) — where/how a source item was encountered (URL locator, upload, imported package). `id` PK; `vault_account_id` NN FK; `source_item_id` NN FK; `occurrence_kind TEXT NOT NULL CHECK(occurrence_kind IN ('url_locator','upload','pasted','imported_package','youtube_locator'))`; `locator TEXT` (inert text; never fetched in M06-A); `observed_at TEXT NOT NULL`; `created_at`. Check: `locator IS NOT NULL OR occurrence_kind IN ('pasted','upload')`.

### E.3 Acquisition and artifacts

**`acquisitions`** (C) — one attempted/successful intake; failure is first-class. `id` PK; `vault_account_id` NN FK; `occurrence_id` NN FK; `method TEXT NOT NULL CHECK(method IN ('manual_paste','manual_file','manual_url_locator'))` (M06-A subset only); `outcome TEXT NOT NULL CHECK(outcome IN ('succeeded','failed','rejected','quarantined'))`; `actor_id TEXT NOT NULL`; `started_at TEXT NOT NULL`; `finished_at TEXT`; `receipt_json TEXT NOT NULL`. Index: `(vault_account_id, outcome)`.

**`original_artifacts`** (C, immutable) — exact supplied bytes/text. `id` PK; `vault_account_id` NN FK; `acquisition_id` NN FK; `content_sha256 TEXT NOT NULL CHECK(length=64)`; `byte_size INTEGER NOT NULL CHECK(>=0)`; `declared_media_type TEXT`; `sniffed_media_type TEXT`; `supplied_filename TEXT`; `stored_relpath TEXT NOT NULL` (content-addressed, §H) `CHECK(stored_relpath NOT LIKE '/%' AND stored_relpath NOT LIKE '%..%')`; `text_normalization TEXT NOT NULL CHECK(text_normalization IN ('byte_exact','utf8_normalized'))`; `rights_class TEXT NOT NULL CHECK(rights_class IN ('unspecified','owner_supplied','third_party','restricted'))`; `retention_class TEXT NOT NULL DEFAULT 'standard'`; `created_at`. Unique: `(vault_account_id, content_sha256)` (duplicate handling → relationship, not second row). No delete; restriction via `rights_class`/policy. Index: `(vault_account_id, content_sha256)`, `(vault_account_id, sniffed_media_type)`.

### E.4 Normalized document versions and elements

**`document_versions`** (C, immutable) — one parser execution output header. `id` PK; `vault_account_id` NN FK; `original_artifact_id` NN FK; `parser_id TEXT NOT NULL`; `parser_version TEXT NOT NULL`; `parser_config_sha256 TEXT NOT NULL CHECK(length=64)`; `package_relpath TEXT NOT NULL` (the JSON package, §J); `package_sha256 TEXT NOT NULL CHECK(length=64)`; `input_sha256 TEXT NOT NULL`; `execution_receipt_json TEXT NOT NULL`; `has_warnings INTEGER NOT NULL CHECK(in 0,1)`; `partial_failure INTEGER NOT NULL CHECK(in 0,1)`; `supersedes_document_version_id TEXT REFERENCES document_versions(id) ON DELETE RESTRICT`; `created_at`. Unique: `(vault_account_id, package_sha256)`; unique `(original_artifact_id, parser_id, parser_version, parser_config_sha256)` (a rerun with identical inputs is idempotent; any input change is a new version). No overwrite; supersession only.

**`elements`** (C) — addressable structural units within a document version. `id` PK; `vault_account_id` NN FK; `document_version_id` NN FK; `element_type TEXT NOT NULL`; `ordinal INTEGER NOT NULL`; `locator_json TEXT NOT NULL` (page/line/time/region/speaker/hierarchy as applicable); `normalized_text TEXT`; `text_sha256 TEXT CHECK(text_sha256 IS NULL OR length=64)`; `created_at`. Unique: `(document_version_id, ordinal)`. Index: `(vault_account_id, document_version_id)`.

**`regions`** (C) — sub-element geometric/temporal/character targets. `id` PK; `vault_account_id` NN FK; `element_id` NN FK; `region_kind TEXT NOT NULL CHECK(region_kind IN ('char_span','time_span','page_box','json_path'))`; `region_json TEXT NOT NULL`; `created_at`.

### E.5 Mentions, entities, merge/split

**`mentions`** (C) — a reference to a candidate entity within an element/region. `id` PK; `vault_account_id` NN FK; `element_id` NN FK; `region_id TEXT REFERENCES regions(id)`; `surface_text TEXT NOT NULL`; `mention_type TEXT`; `created_at`.

**`entity_candidates`** (C) — proposed but unaccepted identities. `id` PK; `vault_account_id` NN FK; `candidate_key TEXT`; `proposed_type TEXT`; `evidence_json TEXT NOT NULL`; `state TEXT NOT NULL CHECK(state IN ('proposed','accepted','rejected'))`; `created_at`.

**`entities`** (C) — human-accepted identities. `id` PK; `vault_account_id` NN FK; `entity_type TEXT NOT NULL`; `canonical_name TEXT NOT NULL`; `accepted_by TEXT NOT NULL`; `accepted_at TEXT NOT NULL`; `state TEXT NOT NULL CHECK(state IN ('active','merged_away','split_away','withdrawn'))`; `created_at`. Index `(vault_account_id, state)`.

**`entity_external_ids`** (C) — external identifiers as **evidence**. `id` PK; `vault_account_id` NN FK; `entity_id` NN FK; `scheme TEXT NOT NULL`; `value TEXT NOT NULL`; `evidence_json TEXT`. Unique `(entity_id, scheme, value)`.

**`duplicate_relationships`** (C) — non-destructive duplicate links. `id` PK; `vault_account_id` NN FK; `left_ref_type TEXT NOT NULL`; `left_ref_id TEXT NOT NULL`; `right_ref_type TEXT NOT NULL`; `right_ref_id TEXT NOT NULL`; `relationship TEXT NOT NULL CHECK(relationship IN ('exact_byte_duplicate','same_external_identity','canonical_url_candidate','syndicated_copy_candidate','near_duplicate_text','same_intellectual_work_candidate','import_replay'))`; `created_at`. No automatic collapse/delete.

**`entity_merge_split_proposals`** (C) — system- or human-proposed. `id` PK; `vault_account_id` NN FK; `proposal_kind TEXT NOT NULL CHECK(in 'merge','split')`; `proposer TEXT NOT NULL` (may be system); `evidence_json TEXT NOT NULL`; `source_entity_ids_json TEXT NOT NULL`; `state TEXT NOT NULL CHECK(state IN ('proposed','accepted','rejected','reversed'))`; `created_at`.

**`entity_merge_split_decisions`** (C, immutable) — **human acceptance only** (D09/F-3). `id` PK; `vault_account_id` NN FK; `proposal_id` NN FK; `decision TEXT NOT NULL CHECK(in 'accepted','rejected','reversed')`; `actor_id TEXT NOT NULL` (must be human actor_type in audit); `supporting_evidence_json TEXT NOT NULL` (**required, non-empty**); `decided_at TEXT NOT NULL`; `previous_state_json TEXT NOT NULL`; `resulting_state_json TEXT NOT NULL`; `reversal_of_decision_id TEXT REFERENCES entity_merge_split_decisions(id)`. Service enforces: proposer may be system, decider must be human; no automatic execution.

### E.6 Assertions and evidence

**`assertions`** (C) — umbrella governed claim record. `id` PK; `vault_account_id` NN FK; `epistemic_type TEXT NOT NULL CHECK(epistemic_type IN ('source_content_fact','attributed_claim','quotation','allegation','observation_assertion','derived_inference','relationship_candidate','accepted_conclusion','disputed_conclusion','unknown','parody_or_editorial_framing'))`; `statement_text TEXT NOT NULL`; `review_state TEXT NOT NULL CHECK(review_state IN ('candidate','human_review_needed','accepted_for_use','disputed','superseded','withdrawn','rejected'))`; `created_by TEXT NOT NULL`; `created_at`; `supersedes_assertion_id TEXT REFERENCES assertions(id) ON DELETE RESTRICT`. **No column may encode truth authority by itself**; truth/support lives in `truth_support_assessments`. Index `(vault_account_id, review_state)`.

**`assertion_evidence`** (C) — binds an assertion to exact provenance. `id` PK; `vault_account_id` NN FK; `assertion_id` NN FK; `source_item_id TEXT FK`; `original_artifact_id TEXT FK`; `document_version_id TEXT FK`; `element_id TEXT FK`; `region_id TEXT FK`; `evidence_role TEXT NOT NULL CHECK(evidence_role IN ('supports','contradicts','contextualizes','quotes'))`; `created_at`. Check: at least one of the ref columns non-null. This is where **contradictions** are recorded (role=`contradicts`), enabling the §P truncation firewall.

**`truth_support_assessments`** (C) — how well supported. `id` PK; `vault_account_id` NN FK; `assertion_id` NN FK; `support_level TEXT NOT NULL CHECK(support_level IN ('unsupported','weak','contested','corroborated','strong'))`; `rationale_text TEXT`; `actor_id TEXT NOT NULL`; `assessed_at TEXT NOT NULL`; `policy_version_id TEXT FK`. Append-only per assertion; latest is current; history preserved.

### E.7 Corrections and supersessions

**`corrections`** (C, immutable) — governed correction lineage (D06). `id` PK; `vault_account_id` NN FK; `target_ref_type TEXT NOT NULL` (`document_version|assertion|entity|policy_version|dossier|element`); `target_ref_id TEXT NOT NULL`; `correction_kind TEXT NOT NULL CHECK(in 'correction','supersession','withdrawal')`; `replacement_ref_id TEXT`; `reason_text TEXT NOT NULL`; `actor_id TEXT NOT NULL`; `decided_at TEXT NOT NULL`. No original is rewritten; the corrected record is marked non-current via its own `supersedes_*`/`state` fields and this lineage row. Drives invalidation work (§L).

### E.8 Dossiers

**`dossiers`** (C) — account-scoped organizing record. `id` PK; `vault_account_id` NN FK; `title TEXT NOT NULL`; `summary_text TEXT`; `state TEXT NOT NULL CHECK(in 'open','archived','withdrawn')`; `created_by TEXT NOT NULL`; `created_at`, `updated_at`.
**`dossier_memberships`** (C) — references, not copies. `id` PK; `vault_account_id` NN FK; `dossier_id` NN FK; `member_ref_type TEXT NOT NULL CHECK(in 'source_item','document_version','element','assertion','entity','question')`; `member_ref_id TEXT NOT NULL`; `note_text TEXT`; `created_at`. Unique `(dossier_id, member_ref_type, member_ref_id)`.

### E.9 Policy and impact

**`policy_versions`** (C, immutable) — account editorial policy snapshots. `id` PK; `vault_account_id` NN FK; `version_int INTEGER NOT NULL`; `policy_sha256 TEXT NOT NULL CHECK(length=64)`; `policy_json TEXT NOT NULL`; `effective_from TEXT NOT NULL`; `created_by TEXT NOT NULL`. Unique `(vault_account_id, version_int)`; unique `(vault_account_id, policy_sha256)`.

**`policy_impact_records`** (C) — F-2 explicit impact set. `id` PK; `vault_account_id` NN FK; `new_policy_version_id` NN FK; `superseded_policy_version_id TEXT FK`; `impacted_ref_type TEXT NOT NULL CHECK(in 'editorial_use_ruling','publication_approval')`; `impacted_ref_id TEXT NOT NULL`; `impact_state TEXT NOT NULL CHECK(impact_state IN ('flagged','re_evaluated','cleared'))`; `created_at`, `resolved_at TEXT`. **Authority may not silently inherit**: on a new policy version the service materializes one `flagged` row per dependent ruling/approval; nothing is usable-forward until `re_evaluated`/`cleared`.

### E.10 Editorial-use and publication reference

**`editorial_use_rulings`** (C) — human, account- and policy-bound (D05). `id` PK; `vault_account_id` NN FK; `assertion_id TEXT FK`; `dossier_id TEXT FK`; `policy_version_id` NN FK; `ruling TEXT NOT NULL CHECK(ruling IN ('allowed','allowed_with_framing','restricted','blocked'))`; `required_framing_text TEXT`; `public_claim_label TEXT CHECK(public_claim_label IS NULL OR public_claim_label IN ('Documented','Plausible','Disputed','Folklore','Speculative','Brain Soup'))`; `actor_id TEXT NOT NULL`; `decided_at TEXT NOT NULL`; `state TEXT NOT NULL CHECK(in 'current','invalidated','superseded')`. **Never approves exact text.** A public label here is presentation, never reverse-mapped to evidence authority.

**`publication_approval_refs`** (C) — a *reference* linking a Vault editorial-use ruling to an existing control-room approval, **only where the two DBs are related**. `id` PK; `vault_account_id` NN FK; `editorial_use_ruling_id` NN FK; `owned_account_id TEXT`; `approval_id TEXT`; `approval_binding_sha256 TEXT CHECK(... length=64)`; `created_at`. This is a pointer for provenance; it does **not** grant or infer publication authority (that stays in the M00–M05 `approvals` pipeline). Under separate-DB (W-2), `approval_id` is an opaque cross-DB reference validated only advisorily.

### E.11 Projections, search, chunks, audit, backup

**`projection_manifests`** (D→tamper-checked) — one generated Markdown/HTML view. `id` PK; `vault_account_id` NN FK; `subject_ref_type TEXT NOT NULL`; `subject_ref_id TEXT NOT NULL`; `output_relpath TEXT NOT NULL`; `output_sha256 TEXT NOT NULL CHECK(length=64)`; `source_binding_json TEXT NOT NULL` (the canonical record IDs + hashes it was generated from); `generator_version TEXT NOT NULL`; `generated_at TEXT NOT NULL`; `state TEXT NOT NULL CHECK(in 'current','stale','tampered','regenerated')`. Rebuildable; never canonical. Regeneration compares on-disk hash to `output_sha256` → `tampered` if mismatch (§O).

**`search_index_state`** (D) + FTS5 virtual table (§L). `search_index_state`: `id` PK; `vault_account_id` NN FK; `document_version_id` NN FK; `indexed_sha256 TEXT NOT NULL`; `state TEXT NOT NULL CHECK(in 'current','stale','excluded')`; `indexed_at TEXT`. The FTS5 content table (`vault_fts`, external-content) is fully rebuildable and excluded from canonical backup.

**`search_invalidation_work`** (D) — deterministic invalidation queue. `id` PK; `vault_account_id` NN FK; `reason TEXT NOT NULL CHECK(in 'correction','supersession','policy_restriction','deletion','parser_version_replacement')`; `target_ref_type TEXT`; `target_ref_id TEXT`; `state TEXT NOT NULL CHECK(in 'pending','done')`; `created_at`, `done_at TEXT`.

**`chunks`** (D) — deterministic retrieval derivative (D12). `id` PK; `vault_account_id` NN FK; `document_version_id` NN FK; `element_ids_json TEXT NOT NULL` (ordered) **or** `locator_range_json TEXT`; `chunk_strategy TEXT NOT NULL`; `chunk_strategy_version INTEGER NOT NULL`; `content_sha256 TEXT NOT NULL CHECK(length=64)`; `ordinal INTEGER NOT NULL`; `state TEXT NOT NULL CHECK(in 'current','invalidated')`; `created_at`. Unique `(document_version_id, chunk_strategy, chunk_strategy_version, ordinal)`. **Never canonical**; fully rebuildable; no embeddings.

**`vault_audit_events`** (A) — Vault mutation audit. Under **shared DB**, reuse global `audit_events` (add `vault_account_id` into payload). Under **separate Vault DB (recommended)**, replicate the exact `audit_events` contract (sequence PK, chained sha256, append-only triggers) inside each Vault DB via a Vault-scoped migration.

**`backup_generations`** (C) — per-account generation records. `id` PK; `vault_account_id` NN FK; `generation_id TEXT NOT NULL UNIQUE` (e.g. `dd-vault-<slug>-<utc>`); `manifest_sha256 TEXT NOT NULL CHECK(length=64)`; `completed INTEGER NOT NULL CHECK(in 0,1)`; `completion_marker_relpath TEXT`; `created_at`, `completed_at TEXT`; `age_recipient_fingerprint TEXT`. No-overwrite; incomplete generations retained and flagged (§Q).

**Explicitly NOT created in M06-A:** any `events`/`chronologies` table (D10); any Qdrant/embedding/graph table (D15); any live-provider/run-execution table (D16). The static context-run **schema** (§P) is a JSON contract validated in code + a thin `context_run_manifests` record only if Owner authorizes persisting definitions; default is file-based manifest with a hash, no execution.

### Canonical vs derived vs immutable vs audit (summary)

- **Canonical:** vault_accounts, sources, source_items, occurrences, acquisitions, original_artifacts, document_versions, elements, regions, mentions, entity_candidates, entities, entity_external_ids, duplicate_relationships, entity_merge_split_proposals, assertions, assertion_evidence, truth_support_assessments, dossiers, dossier_memberships, policy_versions, policy_impact_records, editorial_use_rulings, publication_approval_refs, backup_generations.
- **Immutable (append/supersede only, never rewritten):** original_artifacts, document_versions, corrections, entity_merge_split_decisions, policy_versions.
- **Mutable workflow state:** vault_accounts.state/policy pointer, assertions.review_state, entity states, dossier state, policy_impact_records.impact_state, editorial_use_rulings.state, search/projection/chunk state.
- **Append-only audit:** vault_audit_events (or global audit_events).
- **Rebuildable/derived (excluded from canonical backup):** projection_manifests + outputs, vault_fts, search_index_state, search_invalidation_work, chunks, caches, temp.

JSON is used only where bounded and non-relational (locators, receipts, policy snapshots, evidence blobs, impacted sets); all cross-record authority is relational with FKs and CHECKs.

---

## F. Transaction and Atomicity Model

**Primitive contract (inherited):** all writes use `begin_write()` (`BEGIN IMMEDIATE`) → work → `append_audit` → `commit()`, with `except: rollback(); raise`. WAL + `foreign_keys=ON` + `busy_timeout=5000`. Idempotency via `operation_keys`.

**The dual-write problem.** SQLite and the filesystem cannot be committed in one atomic unit. The invariant is: **the filesystem write completes and is fsync'd and hashed *before* the SQLite row that references it is committed; the SQLite commit is the single point of truth for "the artifact exists."** Reconciliation makes any half-state detectable and recoverable.

Exact ordering for **artifact preservation + registration**:

```text
1. write bytes to  <vault>/tmp/<uuid>.part      (never a final path)
2. fsync file; compute content_sha256 + size; sniff media type
3. atomic rename    tmp/<uuid>.part → objects/<sha2>/<sha256>   (no-overwrite;
                    if target exists → duplicate path, do NOT rewrite)
4. fsync parent directory
5. begin_write(conn); INSERT original_artifacts (stored_relpath, sha256, size...);
   append_audit; commit
6. reconciliation record: acquisition.outcome='succeeded', receipt_json bound
```

Transaction boundaries per operation:

| Operation | FS action (pre-commit) | SQLite tx |
|---|---|---|
| start acquisition | none | insert `acquisitions(outcome='failed'→updated)`; commit |
| preserve original | tmp write → fsync → rename | insert `original_artifacts`; commit |
| hashing | during FS step (before commit) | value stored in the same insert |
| register artifact | — | single tx with audit |
| parser execution | write package to `tmp/`; do not place | none until package placed |
| normalized-package publish | atomic rename package into `packages/…` | insert `document_versions` + `elements`/`regions` in **one** tx |
| element insertion | — | same tx as document_version publish |
| failure recording | keep partial in `quarantine/` | insert `acquisitions.outcome='quarantined'` / `document_versions.partial_failure=1`; commit |
| search indexing | — | derived: update FTS + `search_index_state` in one tx (rebuildable, non-fatal) |
| projection generation | write to `tmp/` → rename into `projections/` | insert/update `projection_manifests`; commit |
| correction/supersession | none | insert `corrections` + mark target non-current + enqueue `search_invalidation_work`, one tx |
| backup generation | copy into generation dir | write `backup_generations(completed=0)`; on success set `completed=1` + write completion marker |

**Crash matrix (deterministic recovery):**

- **before artifact write:** nothing on disk, no row → no-op; acquisition remains `failed`/absent.
- **during artifact write:** only `tmp/*.part` exists → startup sweep deletes stale `tmp/` files older than a threshold; no row references them.
- **after artifact write, before SQLite commit:** object exists at content-addressed path but no row → **reconciliation sweep** finds unreferenced objects; either (a) re-link if a matching pending acquisition receipt exists, or (b) move to `quarantine/orphan-objects/` and record. Never silently trusted.
- **after SQLite commit, before normalized package:** artifact row exists, no `document_versions` → valid state (unparsed artifact). Parser can run later; idempotent.
- **during parser execution:** package only in `tmp/` → discarded on sweep; `document_versions` not inserted → no partial version.
- **during projection generation:** only `tmp/` output → discarded; `projection_manifests` unchanged or left `stale`.
- **during backup creation:** `backup_generations.completed=0`, no completion marker → generation dir is incomplete; treated as invalid, never restorable, retained for inspection, superseded by a new generation.

**Rules:** temporary paths for all in-progress writes; atomic rename into final content-addressed paths; parent-dir fsync; no-overwrite on final paths; reconciliation records for every sweep action; **no silent orphan** artifacts or rows. Reconciliation is itself audited.

---

## G. Alembic Migration and Rollback Plan

**Base:** current HEAD is Alembic revision `0004` (control-room DB). All four revisions are hash-pinned in `manifest.sha256` and run only through `run_guarded_upgrade`.

**Physicality choice controls the migration shape (Owner Decision W-2):**

- **Recommended — separate per-Vault DB:** the Vault schema is a **distinct Alembic migration tree** (own `version_locations` / branch label `vault`) with its own `vault_alembic_version` in each Vault DB. This keeps M00–M05 control-room migrations (`0001`–`0004`) entirely untouched and makes cross-tenant blast radius zero. Vault revisions are `V0001`…`V0006`.
- **Alternative — shared DB:** Vault tables are appended as `0005`…`0010` on the existing single tree. Simpler tooling, but every new table needs a tenant discriminator and every existing query is now multi-tenant-adjacent — higher leakage risk. Not recommended.

**Proposed revision sequence (recommended, separate Vault tree; one revision per phase for independent rollback):**

```text
V0001  vault identity + policy primitives
       vault_accounts, vault_account_owned_accounts, policy_versions,
       policy_impact_records, vault_audit_events (+ append-only triggers)
V0002  sources/occurrences/acquisitions/original_artifacts
V0003  document_versions, elements, regions  (+ normalized-package linkage)
V0004  search_index_state, search_invalidation_work, chunks, projection_manifests
V0005  mentions, entity_candidates, entities, entity_external_ids,
       duplicate_relationships, merge/split proposals+decisions,
       assertions, assertion_evidence, truth_support_assessments,
       corrections, dossiers, dossier_memberships,
       editorial_use_rulings, publication_approval_refs
V0006  backup_generations  (+ any closure-only indexes)
```

**One vs many revisions:** **multiple** is safer. Each maps 1:1 to an implementation phase (§T), so a phase can be upgraded, validated, and — if structural-only — downgraded independently. A single mega-revision would make partial-failure recovery and per-phase review impossible.

**Dependencies on current HEAD:** the Vault tree does not depend on `0004` structurally under the separate-DB model (fresh DB per Vault). Under the shared-DB model, `0005` depends on `0004`. `vault_account_owned_accounts` references `owned_accounts(id)` only under the shared-DB model or a same-DB link; under separate-DB, the link is an advisory cross-DB reference validated in code.

**Upgrade steps:** always via `run_guarded_upgrade(vault_db, migrations_root_vault, operation_id=…, target_revision='vault@head')`: verify manifest → assert clean → write dirty marker → `alembic upgrade` → `PRAGMA integrity_check` + version check → clear marker. Each new revision file's hash is appended to a `manifest.sha256` (Vault manifest under the separate tree).

**Downgrade boundaries:** structural DDL (CREATE TABLE/INDEX) downgrades are safe **only when the Vault DB contains no ingested rows for that phase**. Following the `0003` precedent, downgrades that would drop columns/tables holding governed evidence are **declined**: `downgrade()` drops only empty, newly-created structures and otherwise raises. **Downgrade is structural-only; it never destroys ingested Vault data.** If a Vault has ingested originals/versions, the correct rollback is *restore from a pre-phase backup generation*, not `alembic downgrade`.

**Irreversible-data warnings:** every revision docstring states "downgrade is structural-only; ingested originals, document versions, assertions, corrections, and policy versions are never dropped by downgrade — use backup restore." SQLite column-drop limitations (as noted in `0003`) are reaffirmed.

**Compatibility with M00–M05 records:** under separate-DB, **guaranteed** (control-room DB not opened by Vault migrations). Under shared-DB, new tables are additive with no ALTER of existing tables; the existing `render_as_batch=True` handles any future column adds safely.

**Disposable proof-database procedure:** create a temp Vault DB in `restore/proof/`, run `run_guarded_upgrade` to `vault@head`, assert `vault_alembic_version` = head, `integrity_check=ok`, all expected tables/triggers present, then `discard_failed_empty_migration`-style cleanup (must succeed because no user rows). Emitted as evidence JSON.

**Partial-migration failure behavior:** on any exception the dirty marker persists; `assert_clean_migration_state` blocks further runs; operator uses `recover_completed_migration` (if the version advanced and integrity is ok) or `discard_failed_empty_migration` (only if no user objects). Never auto-clears.

**Dirty-schema detection:** `.migration-dirty.json` presence = fail-closed on next open; manifest hash mismatch = `MigrationIntegrityError`.

**Migration evidence outputs:** `runtime/test-evidence/vault-migration/<op_id>.json` capturing manifest verification, upgrade log, integrity result, head revision, and (for proof runs) teardown.

---

## H. Per-Account Vault Filesystem Layout

**Vault base** (gitignored; recommend under `runtime/vaults/`, configurable). Path derivation uses the account **slug** plus a random suffix bound in the identity marker, never secrets.

```text
runtime/vaults/<slug>-<vaultuid>/            # Vault root (one per account)
  VAULT_IDENTITY.json                        # account identity marker (see below)
  db/vault.sqlite3                           # Vault SQLite (separate-DB model)
  objects/<xx>/<sha256>                      # immutable originals, content-addressed
  packages/<document_version_id>.json        # normalized JSON element packages
  manifests/<...>.json                       # per-object / per-package manifests
  projections/<subject_type>/<id>.md|.html   # generated read-only projections
  tmp/                                        # staging (*.part); swept on startup
  quarantine/                                # rejected/partial inputs + orphan objects
  backups/<generation_id>/                    # immutable backup generations
  restore/<workspace_id>/                     # disposable restore workspaces
  evidence/                                   # Vault hammer/closure evidence
  logs/                                       # account-scoped logs (redacted)
```

**`VAULT_IDENTITY.json`** (the account identity marker): `{ "vault_account_id", "slug", "created_at", "vault_root_fingerprint", "schema": "m06a-vault/1" }`. `vault_root_fingerprint = sha256(vault_account_id + slug + created_at)`; the same value is stored in `vault_accounts.vault_root_fingerprint`. Any operation asserts the on-disk marker matches the DB before writing — this is the primary **wrong-account guard**.

Contract details:

- **Stable path derivation:** originals are content-addressed (`objects/<first-2-hex>/<full-sha256>`), so the path is a pure function of content → idempotent, dedup-friendly, immutable.
- **Account isolation:** every path is under exactly one Vault root; code never composes a path from another account's root; the identity marker is checked on open.
- **Path-traversal prevention:** all relative paths validated `NOT LIKE '/%' AND NOT LIKE '%..%'` (mirrors `evidence_refs` CHECK) and re-resolved with `Path.resolve()`; a resolved path whose parents do not include the Vault root is rejected (mirrors `register_evidence`).
- **Filename sanitization:** supplied filenames are **evidence only** (`original_artifacts.supplied_filename`), never used as a storage path. Stored names are hashes/UUIDs.
- **Extension handling:** no trust in extension; media type is sniffed and recorded; parser selection uses admitted type, not extension.
- **Collision handling:** content-addressed → identical bytes map to one object (recorded as `exact_byte_duplicate` relationship); different bytes cannot collide (SHA-256).
- **Immutable naming / no-overwrite:** final object/package/backup paths are written via atomic rename and never overwritten; a pre-existing final path aborts the rewrite and routes to duplicate handling.
- **Symlink/reparse policy:** reject symlinks/reparse points in Vault trees; verify with `lstat`; never follow links out of the root; deny hardlinks into `objects/`.
- **Windows concerns:** case-insensitive FS — never rely on case to distinguish files (hashes are lowercase hex, unique regardless); reject reserved device names (CON, PRN, AUX, NUL, COM1-9, LPT1-9) if ever echoed; forbid trailing dots/spaces in any derived name.
- **Max path length:** keep total path within conservative limits by using 2-level sharding and short IDs; document the Windows long-path caveat; prefer short Vault base.
- **Cleanup rules:** `tmp/*.part` older than threshold deleted on startup with an audited reconciliation record; `quarantine/` retained until owner review.
- **Temp-file recovery:** startup sweep reconciles `tmp/`, `objects/` (orphan detection), and incomplete `backups/` (§F, §Q).
- **Permissions:** Vault dirs created 0o700, files 0o600 where the platform supports it (matches `archive.py` zip perms); no world-readable evidence.
- **No secrets in paths/filenames**, ever. `age` recipients/keys never appear in a path.
- **No human-editable Markdown as authority:** projections are read-only outputs; authority is DB + `objects/` + `packages/` only.

---

## I. Original Artifact Contract

An original artifact is the **exact supplied bytes** (files, PDFs, transcripts uploaded) or **exact supplied text** (pasted). Record + FS contract:

- **byte-exact vs normalized text:** `text_normalization ∈ {byte_exact, utf8_normalized}`. Uploaded files → `byte_exact` (stored verbatim). Pasted text → both the exact received bytes (`byte_exact`) and, where a parser needs it, a separately-recorded `utf8_normalized` derivative captured as a *document version*, never as a mutation of the original.
- **hash:** SHA-256 (`content_sha256`), matching the project-wide algorithm.
- **media type:** `declared_media_type` (from upload) + `sniffed_media_type` (content sniff); mismatch is a warning surfaced at parser admission, not an authority change.
- **supplied filename / stored filename:** supplied kept as evidence; stored = content hash (§H).
- **size, timestamps, actor, account:** `byte_size`, `created_at`, `acquisitions.actor_id`, `vault_account_id`.
- **acquisition source/method:** via `acquisition_id` → `occurrence` → `source_item` → `source`; method ∈ M06-A subset.
- **rights/retention:** `rights_class`, `retention_class`; **public/private eligibility** is *not* modeled beyond `rights_class` in M06-A (the website is out of scope) — but the class field is retained so a future governed public projection is possible.
- **immutability:** row + object never modified; changes = new artifact + `corrections` lineage.
- **duplicate handling:** same `content_sha256` in an account → one object; a `duplicate_relationships` row of `exact_byte_duplicate`; no second stored object.
- **quarantine/rejection:** oversize, disallowed type, symlink, decode failure, or failed sniff → `acquisitions.outcome='rejected'|'quarantined'`, bytes (if retained) go to `quarantine/`, never `objects/`.
- **unsupported file behavior:** stored as an original artifact **only if** the format is admitted for preservation; otherwise rejected with a receipt. Preservation ≠ parseability: an admitted-for-preservation artifact may still have no admitted parser.
- **deletion/restriction:** no hard delete; restriction via `rights_class`/policy and, if required, a `corrections` withdrawal that marks it non-current while preserving lineage.
- **correction relationship:** via `corrections` (target_ref_type=`document_version`/`element`) — originals themselves are never corrected, only re-supplied.

**Four-way distinction (kept explicit in schema and docs):**

```text
original supplied artifact  → original_artifacts (+ objects/<sha256>)      canonical, immutable
normalized parser output    → document_versions + elements (+ package)     canonical derivative
generated projection        → projections/ (+ projection_manifests)        rebuildable, read-only
public-safe future projection → NOT in M06-A; boundary preserved via rights_class + read-only model
```

The website is not designed here; the model simply does not foreclose a later separately-cleared public projection.

---

## J. Normalized JSON Element-Package Contract

One package per parser execution, stored at `packages/<document_version_id>.json`, hashed into `document_versions.package_sha256`. Package fields:

```text
package_id                (= document_version_id)
schema_version            "m06a-package/1"
vault_account_id
original_artifact { id, content_sha256, byte_size, media_type }
document_version { id, supersedes_document_version_id | null }
parser { id, version, config_sha256 }
execution { started_at, finished_at, receipt: {...}, host_note }
text { raw: <string|null>, normalized: <string|null>, normalization: "..." }
elements [ { element_id, element_type, ordinal, locator: {...},
             normalized_text, text_sha256,
             regions: [ { region_kind, region_json } ] } ]
locators: page | line | time (SRT/VTT) | char_span | json_path | speaker | hierarchy
warnings [ { code, message, locator? } ]
partial_failures [ { code, message, locator? } ]     # non-empty ⇒ partial_failure=1
confidence { ... }                                    # where a parser emits it
language <bcp47 | null>
output_sha256            # sha256 of the canonical serialization of all fields above except this one
manifest { element_count, warning_count, has_partial_failures }
supersession { supersedes_document_version_id | null }
```

**Deterministic serialization + hashing:** canonical JSON — UTF-8, `sort_keys=True`, `separators=(",",":")`, `ensure_ascii=False` — exactly the project idiom in `persistence._json_bytes` / `binding.binding_bytes`. `output_sha256 = sha256(canonical_json_without_output_sha256)`. Byte-identical inputs + parser id/version/config ⇒ byte-identical package ⇒ identical hash (idempotent rerun; unique constraint blocks a duplicate `document_versions` row).

**Canonical vs derivative fields:** canonical = artifact identity/hash, parser identity/version/config, elements + exact locators, warnings, partial_failures, output_sha256, supersession. Derivative/advisory = `text.normalized` (reproducible), `confidence`, `manifest` counts (recomputable). The **raw** original remains the artifact; the package is a derivative observation, never original truth.

**Rerun / version-upgrade behavior:** a rerun with identical inputs is idempotent (same hash, blocked as duplicate). A parser-version or config change produces a **new** `document_versions` row with `supersedes_document_version_id` set; the prior package is retained and marked non-current; search/chunks for the old version are invalidated (§L). **No previous package is ever overwritten.**

---

## K. Parser Admission Records

**Global rules for every parser:** pure local computation; **no network I/O** (proved by the §R egress tests); operates on already-preserved originals only; deterministic output; bounded size/time/recursion; malformed input → explicit warning/partial-failure state, never silent truncation; partial output **without** an explicit warning/failure marker **fails closed** (the package is rejected). External references inside content (URLs in HTML/RSS/JSON/Markdown/PDF) are **inert locators/text** — never fetched. Each parser is admitted **individually** by the owner at its implementation gate; appearing here is a *candidate*, not admission.

Common admission fields are given per parser below. Recommended stack fits Python-3.11 stdlib first (no new runtime deps unless justified), consistent with the lean `pyproject.toml`.

### K.1 Plain text (pasted / uploaded)
Stage: `manual_admitted` candidate. Methods: `manual_paste`, `manual_file`. Impl: **stdlib** (`str`/`bytes`, `codecs`). Fit: zero deps. Supported subset: UTF-8/UTF-16 with BOM detection; configurable max size (default 5 MB). Exclusions: binary, control-char floods. Encoding: detect BOM; else assume UTF-8, on decode error → warning + `partial_failure` if lossy. Malformed: undecodable bytes → reject with receipt. Warnings: encoding fallback, replacement chars. Partial: any lossy decode flagged. Security: none beyond size. Egress: none. License: stdlib. Determinism: total. Adversarial tests: 5 MB+ file (reject), mixed encodings, null-byte flood, lone surrogates. **Recommend admit.**

### K.2 Markdown
Stage: candidate. Methods: `manual_paste`, `manual_file`. Impl: **`markdown-it-py`** (permissive license, pure-Python, sandboxable) *or* a minimal in-house block splitter. Fit: small, no network. Subset: CommonMark blocks/inline; **HTML-in-Markdown is treated as inert text, not rendered** (no raw-HTML passthrough execution). Exclusions: embedded HTML execution, remote image fetch (never fetched). Limits: max size 5 MB, max nesting depth. Malformed: best-effort with warnings. Partial: unclosed constructs flagged. Security: disable any URL auto-linking that could imply fetch; links stored as text. Egress: none. License: MIT-class (verify). Determinism: pin parser version in `parser_version`. Tests: deeply nested lists/blockquotes (depth cap), gigantic tables, `<script>`/`<img>` treated as text, links never resolved. **Recommend admit with raw-HTML-inert config.**

### K.3 SRT
Stage: candidate. Methods: `manual_file`, `manual_paste`. Impl: **in-house stdlib** parser (index / `HH:MM:SS,mmm --> …` / text). Fit: trivial, deterministic. Subset: standard SRT; elements = cues with `time` locators + speaker if present. Exclusions: styling tags kept as inert text. Limits: max cues, max size. Malformed: bad timestamps → warning, cue quarantined in-package (partial_failure). Determinism: total. Tests: overlapping/negative timestamps, missing indices, CRLF/LF, BOM, 100k-cue file (cap). **Recommend admit.**

### K.4 VTT
Stage: candidate. Methods: `manual_file`, `manual_paste`. Impl: **in-house stdlib** WebVTT subset. Subset: `WEBVTT` header, cues, `NOTE`; cue settings retained as metadata; styling/positioning inert. Exclusions: no external CSS/regions fetch. Same limits/malformed/tests as SRT + region/settings fields. **Recommend admit.**

### K.5 JSON
Stage: candidate. Methods: `manual_file`, `manual_paste`. Impl: **stdlib `json`** with explicit limits. Subset: UTF-8 JSON; elements addressed by `json_path` locators. **Resource-exhaustion controls:** max bytes (5 MB), max nesting depth (enforced with an incremental/bounded parse or depth guard), max total keys. Malformed: `JSONDecodeError` → reject with receipt. Partial: none (JSON is all-or-nothing) — so no silent truncation risk. Security: depth/size guards prevent billion-laughs-style blowups; no `eval`. Egress: none. Determinism: total. Tests: 10-MB doc (reject), 10k-deep nesting (reject), duplicate keys, NaN/Infinity (reject unless allowed), huge string. **Recommend admit.**

### K.6 Local RSS/Atom (files only)
Stage: candidate. Methods: `manual_file` **only** (no polling, no fetch). Impl: **`defusedxml`** (hardened XML) → element extraction; **not** `feedparser`'s network paths. Fit: `defusedxml` is the standard XML-attack mitigation. Subset: RSS 2.0 / Atom 1.0 entries → elements; `<link>`/enclosure URLs stored **inert**. Exclusions: no URL following, no enclosure download, no XSLT, no DTD. **XML entity-expansion / XXE:** disabled via `defusedxml` (no external entities, no DTD, entity-expansion bomb protection). Limits: max size, max entries. Malformed: parse error → reject; unknown elements → warning. Egress: none (enclosures never fetched). License: PSF-class (verify). Determinism: total. Tests: billion-laughs, external-entity SSRF attempt (blocked), external DTD, enclosure URL (never fetched), 50-MB feed (reject). **Recommend admit via defusedxml only.**

### K.7 Local basic HTML (files only)
Stage: candidate. Methods: `manual_file` only. Impl: **`lxml`/`html5lib` via a sanitizing, no-network configuration** or `defusedxml`-guarded parse, extracting text + structure. Fit: robust HTML handling; must be pinned to a no-fetch mode. Subset: static text, headings, lists, tables, links (inert). Exclusions: **no JavaScript execution**, no CSS/JS/image fetch, no `<iframe>`/`<object>` resolution, no remote entities. **Active-content handling:** `<script>`/`<style>`/event handlers stripped and recorded as inert; external resource URLs stored as text only. XXE/entity-expansion guarded. Limits: max size, max DOM nodes/depth. Malformed: best-effort with warnings; unrecoverable → reject. Egress: none. License: verify (lxml BSD / html5lib MIT). Determinism: pin lib version. Tests: `<script>` with fetch (inert, never run), `<img src=http…>` (never fetched), external-entity, 100-MB file (reject), deeply nested tags (depth cap), `<iframe src>` (inert). **Recommend admit in strict no-network mode.**

### K.8 Born-digital PDF
Stage: candidate. Methods: `manual_file` only. Impl: **`pypdf`** (pure-Python, no external process, no network) for text + page-level locators. Fit: pure-Python, deterministic, no shelling out. Subset: **born-digital text only**. Exclusions: **no OCR, no scanned-image claim, no rendering, no JavaScript, no network, no external references followed.** Locators: **page-level** (`page` in `locator_json`), plus char spans within page text. Warnings: pages with no extractable text → warning (candidate scanned page — **not** OCR'd, just flagged); embedded-file/annotation URLs stored inert. **Encrypted/password-protected:** if encrypted → reject with a clear `encrypted` receipt (no password prompting, no brute force). **Malformed/oversized:** parse error → reject; size cap (e.g. 50 MB) and page cap; object-stream bombs guarded by limits. Egress: none. License: pypdf BSD (verify). Determinism: pin `pypdf` version. Tests: encrypted PDF (reject), scanned-image-only PDF (warn, no OCR, empty-text flagged as partial), 500-MB / 50k-page (reject), malformed xref, PDF with `/JavaScript` (never executed), embedded remote URL (never fetched), object-stream bomb. **Recommend admit, born-digital text-only.**

### K.9 YouTube identity/context + supplied transcript
Stage: candidate. Methods: `manual_url_locator` (identity only) + `manual_paste`/`manual_file` (transcript). Impl: **no YouTube API, no yt-dlp, no network** — the URL/video ID is recorded as an `occurrence` locator (inert); the transcript is ingested via the plain-text/SRT/VTT parsers. Fit: reuses admitted text parsers; adds only the video **evidence-packet extension fields** (§ correction spec §10): stable video/channel identity, transcript provenance class, caption/ASR origin, timing precision, timestamp/speaker locators, availability lineage — as **extension fields on the general evidence packet**, not a parallel authority. Exclusions: any YouTube call, caption scraping, media download. Egress: none. Determinism: inherited from the transcript parser. Tests: attempt to fetch the URL (must be impossible/blocked), transcript with speaker labels (extension fields populated, no new authority), missing transcript (identity-only occurrence, no document version). **Recommend admit as identity + reused transcript parser.**

**Dependency/licensing note:** the only *new* runtime dependencies implied are `defusedxml`, an HTML parser (`lxml`/`html5lib`), `pypdf`, and optionally `markdown-it-py`. Each is added **only at its parser's implementation gate**, license-verified, version-pinned, and offline-installed (the network-config allows `pypi.org`/`files.pythonhosted.org` for build only, never at runtime). No parser may import a networking client.

---

## L. Search and Deterministic Chunk Plan

**FTS5 appropriateness:** SQLite FTS5 with `unicode61` (optionally `porter`) is the right local lexical engine and is available in this workspace's SQLite 3.45.1. **However**, the packaged Windows Tauri sidecar bundles its own SQLite via the PyInstaller build; FTS5 presence there **must be verified at implementation** (Owner Decision W-3). Fallback if unavailable: LIKE/`instr`-based indexed search over a normalized `elements.normalized_text`, clearly lower quality — flagged, not silently substituted.

**Search plan:**

- **Indexed content:** `elements.normalized_text` of **current** (non-superseded) document versions only, via an external-content FTS5 table `vault_fts(content, element_id UNINDEXED, document_version_id UNINDEXED)`.
- **Account filter enforcement:** under separate-DB, the FTS lives inside the Vault DB (physical isolation). Under shared-DB, **every** query is `WHERE vault_account_id = :acct` joined to `vault_fts`; a query without the account filter is a code-review/hammer failure.
- **Current vs superseded:** superseded document versions are excluded (`search_index_state.state='excluded'`); corrections/supersessions enqueue `search_invalidation_work`.
- **Restricted/deleted material:** `rights_class='restricted'` or withdrawn-by-correction content is excluded from the index; restriction re-runs invalidation.
- **Rebuild:** FTS + `search_index_state` are fully rebuildable from canonical `elements`; a rebuild command drops and repopulates deterministically; excluded from canonical backup.
- **Rank interpretation:** FTS5 `bm25()`; documented as relevance-ordering only, never evidence strength.
- **Tokenizer assumptions:** `unicode61` default; document diacritic folding; pin the choice in a `search_strategy_version`.
- **Query limits:** max query length, max results (paginated); reject pathological queries.
- **Malformed query handling:** FTS5 syntax errors caught → friendly error, never a 500; user input never interpolated (parameterized `MATCH ?`).
- **Injection safety:** all queries parameterized; no string-built SQL; FTS `MATCH` value bound.
- **Deterministic test fixtures:** a fixed corpus → fixed expected hit sets/order (with pinned tokenizer + strategy version).

**Chunk contract (D12):** `chunks` binds `account`, `document_version`, ordered `element_ids` (or a locator range), `chunk_strategy` + `chunk_strategy_version`, and `content_sha256`.

- **Boundaries:** deterministic from element ordinals (e.g. fixed element-count or normalized-length windows with defined overlap), computed purely from canonical elements.
- **Stable identifiers:** `chunk.id` UUID; stable natural key `(document_version_id, strategy, strategy_version, ordinal)`.
- **Correction invalidation:** a correction/supersession touching a document version marks its chunks `invalidated` and enqueues rebuild.
- **Parser-version invalidation:** a new document version supersedes the old; old chunks invalidated.
- **Policy-restriction invalidation:** `rights_class='restricted'`/withdrawal invalidates and excludes chunks.
- **Rebuild:** deterministic regeneration from current elements; identical inputs ⇒ identical `content_sha256`.
- **Derived, never canonical:** chunks carry no truth; they are excluded from canonical backup and reconstructed on restore. **No embeddings, no Qdrant** (D15).

---

## M. Assertions, Evidence, and Review Workflows

Physical workflow contract (four separate layers, per correction spec §3 — none reverse-maps to authority):

```text
epistemic type          → assertions.epistemic_type        (internal, Layer A)
review state            → assertions.review_state           (workflow, Layer B)
truth/support           → truth_support_assessments         (assessment, separate)
LLM output presentation → static context-run field only     (Layer C, §P; not stored as authority)
editorial-use ruling    → editorial_use_rulings             (human, account+policy bound)
public claim label      → editorial_use_rulings.public_claim_label (presentation, Layer D)
exact publication approval → publication_approval_refs → M00–M05 approvals (unchanged)
```

**No field silently becomes truth authority.** `epistemic_type='accepted_conclusion'` is a *classification*, not proof; support lives in `truth_support_assessments`; usability lives in `editorial_use_rulings`; posting authority lives only in the existing `approvals` pipeline bound to a content hash.

**Assertion binding** (via `assertion_evidence`, at least one ref required): exact **source** (`source_item_id`), **artifact** (`original_artifact_id`), **document version** (`document_version_id`), **element/locator** (`element_id`/`region_id`), **human actor** (`assertions.created_by` + audit human actor for review decisions), **review decision** (`review_state` transitions, audited), **correction** (`corrections` targeting the assertion), **contradiction** (`assertion_evidence.evidence_role='contradicts'`), **account policy** (`truth_support_assessments.policy_version_id`, `editorial_use_rulings.policy_version_id`).

Handling of hard cases:

- **unsupported claims:** `epistemic_type` retained; `truth_support.support_level='unsupported'`; may still be recorded, never promoted.
- **contradictory evidence:** both `supports` and `contradicts` evidence rows coexist; the context firewall (§P) refuses to present the claim as clean-supported if a known contradiction can't fit.
- **disputed conclusions:** `epistemic_type='disputed_conclusion'`, `review_state='disputed'`.
- **parody/editorial framing:** `epistemic_type='parody_or_editorial_framing'`; never usable as a source-content fact.
- **accepted conclusions:** `epistemic_type='accepted_conclusion'` **plus** an explicit human `review_state='accepted_for_use'`; classification alone never suffices.
- **superseded conclusions:** `review_state='superseded'` + `supersedes_assertion_id` chain; retained, visibly non-current.
- **account-specific editorial use:** `editorial_use_rulings` is per account + policy version; the same assertion may be allowed in one account and blocked in another (no cross-account transfer).

**Metrics firewall (recorded, not implemented as M11).** Metrics may **later** influence topic selection, timing, format, or framing **experiments** only. Metrics must **never** modify evidence strength, epistemic type, truth/support assessment, correction requirements, or claim labels. This is consistent with existing doctrine (`metric_snapshots` are observations; `detector_advice` is `advisory_only`). M06-A records the firewall as a schema invariant (no FK or write path from `metric_*` into `assertions`/`truth_support_assessments`/`editorial_use_rulings`) and a hammer test (§R HT-M), but adds **no M11 implementation**.

---

## N. Identity, Duplicate, Merge, and Split Plan

- **Stable internal identity:** account-scoped UUID PKs (`entities.id`), never derived from external IDs.
- **External identifiers as evidence:** `entity_external_ids` (typed, non-authoritative).
- **Duplicate candidates / relationships:** `entity_candidates` + `duplicate_relationships` (classes from correction spec §4); detection creates **relationships only** — never deletes or collapses.
- **Entity candidates → entities:** promotion is human (`entity_candidates.state='accepted'` → `entities` row created by a human actor).
- **Merge/split proposals:** `entity_merge_split_proposals` (proposer may be **system**).
- **Human acceptance:** `entity_merge_split_decisions` — decider must be a **human** actor (enforced: audit `actor_type='human'`); **no system-proposed merge executes automatically** (D09).
- **Supporting-evidence requirement (F-3):** `supporting_evidence_json` is required and non-empty; service rejects an acceptance without it.
- **Actor binding:** `actor_id` + audit chain entry.
- **Reversible lineage:** `previous_state_json`, `resulting_state_json`, `reversal_of_decision_id`; a split/reverse re-derives prior identities.
- **Correction impact:** a merge/split emits `corrections` and enqueues `search_invalidation_work` for affected chunks/index.
- **Search/chunk invalidation:** identity changes invalidate derived rows deterministically.

**No duplicate detector may delete or collapse records automatically.** **Events and chronologies remain deferred** (D10) — no such tables in M06-A.

---

## O. Read-Only Projection Plan

Generated Markdown/HTML for operator convenience only.

- **May be projected:** dossiers, document-version readouts, assertion/evidence summaries, source/entity overviews — all from **current** canonical records.
- **May never be projected:** secrets, `age` recipients/keys, another account's data, restricted (`rights_class='restricted'`) or withdrawn content, or anything that would embed authority.
- **Regeneration rules:** deterministic from canonical inputs; each output recorded in `projection_manifests` with `output_sha256` and `source_binding_json` (the exact record IDs + hashes used).
- **Projection manifests:** bind output ↔ source records ↔ hashes ↔ generator version.
- **Account isolation:** projections written only under the owning Vault's `projections/`; generator asserts the identity marker.
- **Output location:** `projections/<subject_type>/<id>.md|.html` (gitignored).
- **Tamper detection:** on regeneration, compare the on-disk file hash to `projection_manifests.output_sha256`; mismatch ⇒ mark `tampered` and either overwrite (default) or, if configured to preserve for inspection, move the edited file to `quarantine/` and regenerate.
- **Manual-edit behavior:** a hand-edited projection is **either overwritten on regeneration or rejected as tampered** — it can never be read back as authority (no code path ingests `projections/` into canonical tables).
- **Deletion/supersession:** superseded subjects regenerate to a non-current view or are removed; the manifest records the transition.
- **No authority flow back:** enforced structurally — there is **no** parser, importer, or service that reads `projections/` into SQLite or `objects/`.

The public website is **not** designed here; only the clean boundary (read-only + `rights_class`) is preserved for a future, separately-cleared public projection.

---

## P. Static LLM Context-Run Preparation Contract

M06-A **defines and validates** a static context contract and executes **no** provider call (D16). Represented as a JSON manifest (optionally recorded as a hash-bound file under the Vault) with fields:

```text
context_run_id, schema_version "m06a-context/1"
vault_account_id, vault_root_fingerprint
policy { version_id, policy_sha256 }
task_instructions
reviewed_conclusions [ {assertion_id, epistemic_type, support_level, ...} ]   # clearly separated
evidence_packets [ {packet_id, kind, document_version_id, element_id, region_id,
                    locator, text, provenance, video_extension? } ]           # untrusted
contradictions_corrections [ {assertion_id, contradiction_evidence_id | correction_id} ]
citations [ {evidence_packet_id, document_version_id, locator} ]
omissions [ {ref, reason} ]
truncation_record { budget, dropped: [ {ref, reason} ] }
retrieval_trace [ {query, selected_ids, strategy_version} ]
manifest { evidence_count, contradiction_count, output_sha256 }
authority: "none"      # no mutation, no publication
```

Retrieved/supplied content is **evidence, never instructions**. Deterministic serialization/hashing as in §J.

**Fail-closed rules (post-validation, before a run is considered valid):**

- **contradiction lost through truncation:** if a selected claim has a known `contradicts` evidence row or `correction` that does **not** fit the budget → the claim is **omitted**, or included **with a machine-bound truncation warning** that prevents clean-support presentation. Silent inclusion without the contradiction is **prohibited** and fails validation.
- **invalid/missing citation locator:** every citation must post-validate to an existing `element`/`region` in the **exact** `document_version` included in the run, within account+policy scope; otherwise the run is invalid.
- **stale policy:** `policy.version_id` must equal `vault_accounts.policy_current_version_id` (and hash match); else invalid pending re-evaluation.
- **superseded evidence:** any evidence packet whose document version is superseded/withdrawn ⇒ invalid.
- **prior model output re-ingested as evidence:** rejected — a previous model output is an untrusted derived record and cannot become an `evidence_packet` (enforced by packet `kind` whitelist excluding model output).
- **cross-account contamination:** any ref whose `vault_account_id` ≠ the run's account ⇒ invalid.

No provider is selected or integrated. Live integration is M08.

---

## Q. Backup, Restore, and Rebuild Plan

Extends `backup.py`/`archive.py` to **per-Vault** generations.

**Canonical generation contents:** consistent Vault SQLite backup (via `sqlite3 .backup()`); all `objects/` originals; all `packages/` normalized packages; `manifests/`; account `policy_versions` (in the DB); correction/supersession + review + audit lineage (in the DB); application/schema version metadata (Vault head revision, app version). **Derived excluded / rebuildable:** `vault_fts`, `search_index_state`, `chunks`, `projections/`, caches, future Qdrant/graph.

**Contract:**

- **generation identifier:** `dd-vault-<slug>-<utcstamp>` (extends the existing `dd-backup-…` scheme).
- **immutable manifest:** `manifest.json` with per-file `sha256`+`byte_size` (existing format), plus `vault_account_id`, `vault_root_fingerprint`, Vault head revision, and an explicit `originals_expected` list built from `original_artifacts` so **omission is detectable**.
- **no-overwrite:** generation directory is write-once; a second run creates a new generation.
- **atomic completion marker:** write `COMPLETE` (with manifest hash) only after all files copied + manifest written; `backup_generations.completed=1` set in the same governed step.
- **incomplete generation handling:** missing `COMPLETE`/`completed=0` ⇒ invalid, never restorable, retained for inspection.
- **encryption boundary:** reuse `archive.create_deterministic_zip` + `encrypt_with_age`; `age` remains external and **not bundled** (per `backup-encryption-prerequisites.md`); encryption is **optional** for M06-A closure unless Owner requires it (W-5). Key management deferred to **M15**.
- **wrong-account restore rejection:** restore asserts `manifest.vault_root_fingerprint` == target `VAULT_IDENTITY.json` == `vault_accounts.vault_root_fingerprint`; any mismatch ⇒ **fail closed**.
- **disposable restore location:** `restore/<workspace_id>/` (gitignored), never the live Vault.
- **tamper detection:** extends `verify_generation` — manifest hash/size checks, `PRAGMA integrity_check`, `verify_audit_chain`, DB↔`objects/` reconciliation, orphan detection; **plus** missing-required-original detection (every `original_artifacts.content_sha256` must be present) and modified-manifest/modified-artifact fail-closed.
- **cross-account derivative-contamination detection:** after restore, rebuild FTS/chunks/projections into the disposable workspace and assert **zero** rows/paths reference any other `vault_account_id`/fingerprint.
- **rebuild verification:** rebuilt search returns expected hits; chunks reproduce expected `content_sha256`; projections reproduce expected `output_sha256`.
- **cleanup:** disposable workspace deleted after proof; evidence JSON retained.

**Exact closure evidence:** `runtime/test-evidence/vault-backup/<generation_id>.json` capturing: generation manifest hash, integrity/audit-chain results, originals-present reconciliation, at least one **modified-manifest** and one **modified-artifact** fail-closed proof, one **wrong-account restore** rejection, one **missing-original** detection, and cross-account-contamination = none.

---

## R. Full Adversarial and Hammer-Test Matrix

Numbered matrix (≥60). Columns: **ID · Category · Threat/Invariant · Setup · Attempted violation · Expected fail-closed · Evidence · Phase · Closure**. Evidence = per-test JSON via the existing `run_ht_evidence.py`/`test_evidence.py` schema (new invariants **HT-21…**). "Closure" = required to close that phase.

| ID | Category | Threat / invariant | Setup | Attempted violation | Expected fail-closed | Phase |
|---|---|---|---|---|---|---|
| VT-01 | account isolation | one Vault cannot read another | 2 Vaults | query B's data via A's conn | rejected/empty; identity-marker mismatch | 1 |
| VT-02 | account isolation | wrong identity marker | mismatched `VAULT_IDENTITY.json` | write with wrong marker | abort before write | 1,2 |
| VT-03 | account scope | shared-DB tenant filter (if W-2=shared) | multi-tenant rows | query missing `vault_account_id` | code/test failure; no rows | 1 |
| VT-04 | path traversal | escape Vault root | crafted relpath `../..` | store original outside root | rejected (LIKE + resolve) | 2 |
| VT-05 | path traversal | absolute path | relpath `/etc/x` | write | rejected | 2 |
| VT-06 | Windows path | reserved device name | filename `CON` | derive storage name | never used as path (hash only) | 2 |
| VT-07 | Windows case | case-collision | `A` vs `a` supplied names | collide objects | no collision (hash keys) | 2 |
| VT-08 | symlink/reparse | symlink in Vault tree | planted symlink | follow out of root | detected/rejected (lstat) | 2 |
| VT-09 | symlink | hardlink into objects/ | planted hardlink | bypass immutability | rejected | 2 |
| VT-10 | duplicate intake | exact byte duplicate | same file twice | second stored object | one object + duplicate relationship | 2 |
| VT-11 | replay | import/acquisition replay | same op key | duplicate row | idempotent (`operation_keys`) | 2 |
| VT-12 | artifact overwrite | rewrite final object | existing `objects/<sha>` | overwrite bytes | no-overwrite abort | 2 |
| VT-13 | hash mismatch | corrupted object vs row | flip a byte | pass verification | integrity check fails closed | 2,6 |
| VT-14 | partial write | crash mid artifact write | kill after `.part` | orphan final object | sweep removes tmp; no row | 2 |
| VT-15 | orphan FS | object with no row | plant object | trusted as canonical | reconciliation quarantines | 2,6 |
| VT-16 | orphan row | row with no object | delete object | silent acceptance | detected; restore/verify fails closed | 2,6 |
| VT-17 | malformed txt | undecodable bytes | binary as text | silent lossy import | reject/partial warning | 3 |
| VT-18 | encoding attack | lone surrogates / mixed BOM | crafted file | corrupt normalized text | warning + partial_failure | 3 |
| VT-19 | oversized | 5–500 MB inputs per format | large files | resource exhaustion | size cap reject | 3 |
| VT-20 | parser crash | malformed each format | broken SRT/VTT/JSON/HTML/PDF | uncaught crash | caught → receipt, no partial version | 3 |
| VT-21 | silent truncation | parser drops tail | truncated PDF/JSON | package looks complete | partial_failure set or reject | 3 |
| VT-22 | partial w/o warning | parser emits partial, no marker | patched parser | accepted silently | **fails closed** (package rejected) | 3 |
| VT-23 | network egress | parser attempts I/O | parser given URL content | outbound connection | blocked; no socket; test proves no egress | 3 |
| VT-24 | XML entity expansion | billion laughs | crafted RSS/HTML | memory blowup | defusedxml blocks | 3 |
| VT-25 | XXE / external entity | SSRF via entity | external-entity feed | fetch/exfil | blocked (no DTD/ext entities) | 3 |
| VT-26 | HTML active content | script/iframe/img | hostile HTML | JS run / resource fetch | inert text only; never fetched | 3 |
| VT-27 | PDF encrypted | password-protected | encrypted PDF | prompt/brute force | reject with `encrypted` receipt | 3 |
| VT-28 | PDF scanned | image-only PDF | scanned PDF | claim OCR text | no OCR; empty-text flagged | 3 |
| VT-29 | PDF malformed | broken xref / bomb | crafted PDF | crash / exhaust | reject; caps enforced | 3 |
| VT-30 | inert URLs | URL in local file | HTML/RSS/JSON/MD/PDF with URL | auto-fetch | never fetched (inert locator) | 3 |
| VT-31 | wrong parser version | version mismatch | rerun old parser id | overwrite package | new superseding version; old kept | 3 |
| VT-32 | fabricated IDs | non-existent FK | forged document_version_id | insert child | FK RESTRICT rejects | 3,5 |
| VT-33 | modified evidence | altered package on disk | edit package json | pass as canonical | package_sha256 mismatch detected | 3,6 |
| VT-34 | determinism | non-deterministic output | rerun parser | differing hash | identical hash required | 3 |
| VT-35 | FTS injection | malicious MATCH | `" OR 1=1` query | SQL injection | parameterized; safe error | 4 |
| VT-36 | search leakage | cross-account hit | 2 Vaults | search returns B in A | account filter/physical isolation | 4 |
| VT-37 | stale search | superseded still indexed | supersede a version | old text returned | invalidation excludes it | 4 |
| VT-38 | restricted search | restricted content indexed | mark restricted | appears in results | excluded on invalidation | 4 |
| VT-39 | stale chunks | correction not propagated | correct a version | old chunk current | chunks invalidated | 4 |
| VT-40 | chunk determinism | unstable chunk hash | rebuild chunks | hash drift | identical `content_sha256` | 4 |
| VT-41 | projection tamper | edit generated md | hand-edit projection | read back as authority | tampered/overwritten; never canonical | 4 |
| VT-42 | projection→authority | ingest projection | feed projections/ to importer | authority flow back | no code path exists (structural) | 4 |
| VT-43 | projection isolation | cross-account projection | generate for B under A | leak | identity-marker guard | 4 |
| VT-44 | merge without evidence | empty evidence merge | accept merge, no evidence | merge accepted | rejected (F-3 required) | 5 |
| VT-45 | automatic merge | system executes merge | system proposer auto-accept | canonical merge | blocked; human-only (D09) | 5 |
| VT-46 | split reversibility | irreversible split | accept split | no lineage | lineage recorded; reversible | 5 |
| VT-47 | dup auto-collapse | detector deletes | run detector | rows removed | relationships only; no delete | 5 |
| VT-48 | correction bypass | rewrite original | mutate assertion/version | silent rewrite | immutable; supersede only | 5 |
| VT-49 | supersession bypass | current after supersede | supersede then read | old still current | marked non-current | 5 |
| VT-50 | policy impact | silent forward inherit | new policy version | ruling stays usable | `flagged` impact set required | 5 |
| VT-51 | editorial-use authority | unsupported use authority | infer publish from `allowed` | publication inferred | blocked; separate ref only | 5 |
| VT-52 | publication bypass | Vault approves post text | approve via Vault | posting authority | no authority; M00–M05 pipeline only | 5 |
| VT-53 | claim label reverse-map | label → evidence | set public label | evidence strengthened | one-way; no reverse map | 5 |
| VT-54 | metrics firewall | metric edits evidence | write metric→assertion | evidence changed | no path exists (structural) | 5 |
| VT-55 | context truncation | contradiction dropped | budget too small | clean-support output | omit or bound-warn; fail closed | 5 |
| VT-56 | citation locator | fabricated citation | cite missing locator | displayed valid | post-validation rejects | 5 |
| VT-57 | model output re-ingest | prior output as evidence | add model output packet | promoted to evidence | rejected (kind whitelist) | 5 |
| VT-58 | backup omission | missing original | drop an object from gen | verify passes | missing-original detected | 6 |
| VT-59 | manifest tamper | edit manifest hash | alter manifest | restore trusts it | hash mismatch fail closed | 6 |
| VT-60 | artifact tamper | edit backed-up object | flip byte in gen | restore ok | integrity fail closed | 6 |
| VT-61 | wrong-account restore | restore into other Vault | mismatched fingerprint | restore succeeds | rejected fail closed | 6 |
| VT-62 | dirty restore target | non-empty workspace | pre-populate restore dir | silent merge | refuse / isolate | 6 |
| VT-63 | partial backup | no completion marker | kill mid-generation | restorable | invalid; not restorable | 6 |
| VT-64 | failed rebuild | derivative won't rebuild | corrupt element text | silent pass | rebuild verification fails | 6 |
| VT-65 | cross-account derivative | rebuilt index leaks | contaminated restore | other-account rows | contamination = none asserted | 6 |
| VT-66 | detector non-detection | detector misses | disable a detector | undetected passes | non-detection fails closed (asserted) | 6 |
| VT-67 | detector error | detector throws | make detector raise | error swallowed | error fails closed, audited | 6 |
| VT-68 | empty execution list | no tests run | empty invariant set | closure claimed | empty list fails closure | 6 |
| VT-69 | fabricated evidence output | forged evidence json | hand-write pass=true | accepted | commit-SHA/version binding + rerun | 6 |
| VT-70 | secret leakage | secret in path/log/projection | inject secret | appears in artifact | secret-scan blocks; none present | 4,6 |
| VT-71 | log/cache/temp leakage | cross-account diagnostics | 2 Vaults | A's data in B's logs | account-scoped/redacted | 4,6 |
| VT-72 | rollback failure | phase rollback breaks | force downgrade w/ data | data destroyed | downgrade declines; restore path | 1–6 |
| VT-73 | migration partial | crash mid-migration | kill during upgrade | dirty state usable | dirty marker blocks; governed recovery | 1 |
| VT-74 | audit chain break | tamper audit row | edit `vault_audit_events` | chain verifies | trigger blocks update/delete; verify fails | 1,6 |
| VT-75 | oversized paste | 50 MB paste | huge paste | exhaust | size cap reject | 3 |

No happy-path theater. VT-66/VT-67/VT-68/VT-69 specifically prove that **non-detection, detector errors, empty runs, and fabricated evidence fail closed**.

---

## S. Exact Proposed Application-Repository Change Set

Grouped; paths follow existing conventions (`src/discrepancy_desk/`, `migrations/versions/`, `tests/`, `scripts/`, `docs/`). Runtime Vault data lives under gitignored `runtime/vaults/…`. **Smallest change surface: new files beside existing modules; existing M00–M05 files left untouched except three additive extension points noted as MODIFIED.**

### Persistence / domain
- **create** `src/discrepancy_desk/vault_identity.py` — Vault root + `VAULT_IDENTITY.json` derivation/verification. *Purpose:* account marker + fingerprint. *Authority:* isolation guard. *Phase 1.* *Validation:* VT-01/02.
- **create** `src/discrepancy_desk/vault_persistence.py` — governed Vault writes reusing `begin_write`/`append_audit`/`operation_keys`. *Phases 1–5.* *Validation:* VT-11/32/44–57/74.
- **create** `src/discrepancy_desk/vault_fs.py` — content-addressed store, tmp/rename/fsync, sweep/reconciliation, path guards. *Phase 2.* *Validation:* VT-04–16.

### Alembic
- **create** `migrations/versions/V0001…V0006_*.py` (separate Vault tree, W-2). *Purpose:* Vault schema. *Phases 1–6.* *Validation:* §G proof DB.
- **create** `migrations/vault/` (script_location for the Vault tree) + `migrations/vault/manifest.sha256`.
- **MODIFIED** `migrations/env.py` — parameterize `script_location`/`version_table` so the guarded runner can target the Vault tree without touching control-room behavior (additive; `render_as_batch` unchanged).

### Services / repositories
- **create** `src/discrepancy_desk/vault_service.py` — functional service (create Vault, acquire, parse, assert, dossier, correct, rule) mirroring `operator_service.py`. *Phases 2–5.*
- **create** `src/discrepancy_desk/vault_queries.py` — read-side (search, listings). *Phase 4.*

### Artifact / Vault filesystem
- covered by `vault_fs.py` + `vault_identity.py`.

### Parsers
- **create** `src/discrepancy_desk/parsers/__init__.py`, `base.py` (no-network contract, warning/partial protocol), and one module per admitted format: `text.py`, `markdown.py`, `srt.py`, `vtt.py`, `json_pkg.py`, `rss_atom.py`, `html_basic.py`, `pdf_born.py`, `youtube_transcript.py`. *Each added only at its admission gate.* *Phase 3.* *Validation:* VT-17–34.
- **create** `src/discrepancy_desk/parsers/ADMISSIONS.md` — the §K records, per-parser owner sign-off.

### Search
- **create** `src/discrepancy_desk/vault_search.py` — FTS5 (or fallback) + invalidation. *Phase 4.* *Validation:* VT-35–38.
- **create** `src/discrepancy_desk/vault_chunks.py` — deterministic chunk contract. *Phase 4.* *Validation:* VT-39/40.

### Projections
- **create** `src/discrepancy_desk/vault_projections.py` + `templates/vault/*.md.j2|*.html.j2` (Jinja2 already a dep). *Phase 4.* *Validation:* VT-41–43.

### Backup / restore
- **create** `src/discrepancy_desk/vault_backup.py` — per-Vault generation + restore proof extending `backup.py`/`archive.py`. *Phase 6.* *Validation:* VT-58–65.

### API / web / Tauri
- **MODIFIED** `src/discrepancy_desk/web.py` — add read-mostly `/desktop-api/v1/vault/*` endpoints behind the existing token/loopback middleware (ingestion stays local-operator-triggered; no new network surface). *Phase 4–5.* *Authority:* none new.
- **untouched** all `desktop/src-tauri/*` (loopback+token model already sufficient; no Rust change required for M06-A).

### Configuration
- **create** `src/discrepancy_desk/vault_config.py` — Vault base path, size caps, strategy versions (no secrets). *Phase 1.*
- **untouched** `alembic.ini`, `pyproject.toml` **except** adding parser deps (`defusedxml`, `pypdf`, HTML/markdown libs) **only** at their gates.

### Tests
- **create** `tests/test_vault_identity.py`, `test_vault_fs.py`, `test_vault_persistence.py`, `test_vault_parsers_*.py`, `test_vault_search.py`, `test_vault_chunks.py`, `test_vault_projections.py`, `test_vault_backup_restore.py`, `test_vault_authority_boundaries.py`. *Phases 1–6.*

### Hammer evidence
- **MODIFIED** `scripts/run_ht_evidence.py` — append invariants **HT-21…HT-NN** mapping to the VT-* tests. *Phase 6.*

### Documentation
- **create** `docs/m06a-vault-contract.md`, `docs/m06a-backup-restore.md`, `docs/m06a-parser-admissions.md`.
- **MODIFIED (docs repo, separate governance)** `STATUS.md` on implementation, not under this planning authorization.

**Left untouched (representative):** `persistence.py`, `binding.py`, `db.py`, `operator_service.py`, `editorial_queries.py`, `backup.py`, `archive.py`, `migration_integrity.py`, `migration_runner.py`, migrations `0001`–`0004`, all existing tests, all `desktop/` Rust/TS. (`web.py`, `env.py` are the only pre-existing Python files modified, both additively.)

---

## T. Recommended Implementation Phases

Each phase: exact scope · prerequisites · files · migration · tests · evidence · rollback · stop conditions · independent-review point · may-later-phases-start-if-failed.

**Phase 1 — Vault identity + authority primitives.**
Scope: `vault_accounts`, policy tables, impact records, Vault audit; identity marker. Prereq: accepted plan + owner authorization. Files: `vault_identity.py`, `vault_config.py`, `vault_persistence.py`(core), `V0001`. Migration: V0001. Tests: VT-01/02/03/50/72/73/74. Evidence: migration proof + isolation. Rollback: structural downgrade (empty) or discard. Stop: audit chain won't verify; identity guard bypassable. Review point: yes. Later phases: **no** if failed.

**Phase 2 — Filesystem + original artifacts + reconciliation.**
Scope: `vault_fs.py`, sources/occurrences/acquisitions/original_artifacts, dual-write + sweep. Prereq: P1 closed. Files: `vault_fs.py`, `vault_service.py`(acquire), `V0002`. Tests: VT-04–16, VT-75. Evidence: crash-matrix reconciliation proofs. Rollback: structural + `tmp`/`quarantine` cleanup; ingested rows → restore path only. Stop: any orphan not detected. Review: yes. Later: no if failed.

**Phase 3 — Normalized packages + narrow parsers.**
Scope: document_versions/elements/regions, package contract, parsers admitted **one at a time**. Prereq: P2 closed. Files: `parsers/*`, `V0003`, `ADMISSIONS.md`. Tests: VT-17–34. Evidence: per-parser fixture corpus + adversarial + no-egress. Rollback: structural; superseded packages retained. Stop: egress possible; silent partial passes. Review: yes (and per-parser owner gate). Later: search/projections (P4) may start for admitted parsers only.

**Phase 4 — Search, chunks, projections.**
Scope: FTS5 (W-3), chunks, read-only projections + tamper detection. Prereq: P3 (≥1 parser). Files: `vault_search.py`, `vault_chunks.py`, `vault_projections.py`, `templates/vault/*`, `V0004`, `web.py`(read endpoints). Tests: VT-35–43, VT-70/71. Evidence: deterministic fixtures; tamper proofs. Rollback: drop rebuildable structures freely (derived). Stop: cross-account leakage; projection→authority path. Review: yes. Later: yes even if a derivative issue (derived, non-canonical) — but leakage is a hard stop.

**Phase 5 — Assertions/entities/dossiers + review + policy.**
Scope: assertions/evidence/support, mentions/entities/merge-split, dossiers, editorial-use rulings, policy impact, context-run contract + validation. Prereq: P3 (P4 optional). Files: `vault_persistence.py`(assertions…), `vault_service.py`, `V0005`, `web.py`(review endpoints). Tests: VT-32/44–57. Evidence: authority-boundary proofs, context fail-closed proofs. Rollback: structural; governed rows retained → restore. Stop: automatic merge; publication inference; metrics→evidence path; context firewall bypass. Review: yes. Later: no if failed.

**Phase 6 — Backup/restore + full hammer closure.**
Scope: per-Vault backup/restore/rebuild + complete matrix. Prereq: P1–P5 closed. Files: `vault_backup.py`, `V0006`, `run_ht_evidence.py`(HT-21…). Tests: VT-58–69, VT-72–74 closure. Evidence: closure profile (§Q, §U). Rollback: n/a (additive). Stop: wrong-account restore succeeds; omission/tamper undetected; fabricated evidence accepted. Review: **final independent review**. Later: exit gate.

**Governance model recommendation.** Require **one owner authorization for the whole bounded package**, with a **mandatory independent-review checkpoint after each phase** (phases are already gated by pass/fail + stop conditions). Per-**parser** admission (Phase 3) additionally needs an explicit owner sign-off each. This avoids per-phase authorization ceremony while preserving hard gates. Phases 1, 2, 3, 5 are **blocking**: a failure halts forward progress. Phase 4 derivatives are non-blocking except on isolation/authority stops.

---

## U. Validation and Evidence Plan

Exact commands (project's real tooling: **uv**, ruff, pytest, alembic, PyInstaller, Rust, `age`):

```text
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
uv run pytest tests/test_vault_*.py -q
uv run python scripts/run_ht_evidence.py           # HT-01…HT-NN incl. new VT-* invariants
# migration proofs (disposable):
uv run python -c "…run_guarded_upgrade(proof_vault_db, migrations/vault, op_id, 'vault@head')…"
uv run alembic -c alembic.ini heads                # control-room tree unchanged at 0004
# disposable Vault proof: create Vault → ingest fixtures → search/chunks/projections rebuild
# backup/restore proof: create_generation → tamper cases → verify_generation → wrong-account reject
cd desktop && npm ci && npm run build             # frontend production build (unchanged surface)
cargo test --manifest-path desktop/src-tauri/Cargo.toml
uv run python scripts/build_desktop_sidecar.py    # packaged sidecar (verify FTS5 in bundle, W-3)
# secret scan + tree hygiene:
git diff --check
git status --porcelain                             # expect only intended new files
grep -RniE 'AGE-SECRET-KEY|BEGIN .*PRIVATE KEY|password=' src tests || true   # expect none
```

**Expected evidence artifacts** (append-only, hashed, existing schema):
- `runtime/test-evidence/full-suite.json` (counts, commit SHA, sqlite/python versions).
- `runtime/test-evidence/focused/*.json` per focused run.
- `runtime/test-evidence/ht/*.json` per HT invariant (via `run_ht_evidence.py`).
- `runtime/test-evidence/vault-migration/<op>.json`, `.../vault-backup/<gen>.json`.
- Backup generation `manifest.json` + `COMPLETE` marker + `.archive-manifest.json` (if encrypted).
- A closure ledger (extend `docs/ht-coverage-ledger.md`) mapping every VT-* → test → evidence file → phase.

Every evidence file carries the implementation `commit_sha`, `sqlite_version`, `python_version`. **No fabricated results**; evidence is generated by real runs at implementation time (this planning package asserts none).

---

## V. Rollback, Recovery, and Stop Conditions

- **Pre-implementation snapshot:** tag the app repo commit; back up any existing `runtime/` control-room DB; Vault work starts from empty Vaults (no legacy Vault data exists).
- **Schema rollback limits:** Vault migrations downgrade **structural-only** and **only when empty**; populated Vaults roll back via **backup restore**, never destructive `alembic downgrade` (per `0003` precedent).
- **Filesystem rollback limits:** `objects/`/`packages/` are immutable and never destructively rewound; a bad phase is undone by discarding the (empty) new structures or restoring a pre-phase generation.
- **Safely downgradable vs not:** empty new tables/indexes — yes; anything holding ingested originals/versions/assertions/corrections/policies — **no** (restore instead).
- **Partial artifact reconciliation:** startup sweep (§F) reconciles `tmp/`, orphan `objects/`, incomplete `backups/`; every action audited.
- **Failed migration detection:** persistent `.migration-dirty.json` + manifest-hash mismatch → fail-closed; governed `recover_completed_migration`/`discard_failed_empty_migration`.
- **Failed parser output:** `document_versions.partial_failure=1` or rejected acquisition → `quarantine/` + receipt; never a silent partial version.
- **Halt-on-breach:** any §C stop condition halts the phase and blocks later blocking phases.
- **Owner intervention required:** wrong-account restore success; undetected cross-account leakage; audit-chain break; fabricated evidence acceptance; any parser egress.
- **Architecture-reopening conditions (rare):** only if an accepted ruling (D01–D16) proves physically unrealizable on the packaged SQLite/FS (e.g. FTS5 truly unavailable *and* the fallback violates a ruling) — otherwise implementation adapts within the accepted architecture.

---

## W. Owner Decisions Still Required

Genuine, unresolved choices doctrine does not settle. **None reopen D01–D16.**

**W-1 — Backfill account scope onto legacy M00–M05 tables?**
*Why open:* D03 mandates per-account isolation, but existing `owned_accounts`/editorial tables predate the persona/Vault concept. *Options:* (a) **do not** modify legacy tables; relate Vaults to handles via `vault_account_owned_accounts` only (recommended); (b) add `vault_account_id` to legacy tables via batch migration. *Risks:* (b) touches accepted M00–M05 authority and its evidence, risking regression. *Recommendation:* **(a)**. *Blocks:* planning acceptance? No — implementation only.

**W-2 — Vault SQLite physicality: separate per-Vault DB file vs shared DB with tenant column.**
*Why open:* D03 says "physically separate Vaults" + "account-bound SQLite authority"; the existing app is single-DB. Doctrine implies separation but doesn't mandate a file boundary. *Options:* (a) **separate `db/vault.sqlite3` per Vault** with its own Vault migration tree/version table (recommended — strongest physical isolation, zero legacy blast radius); (b) shared control-room DB with `vault_account_id` discriminator (simpler tooling, higher leakage risk). *Risks:* (a) cross-DB references to `owned_accounts`/`approvals` become advisory; (b) every query becomes multi-tenant-sensitive. *Recommendation:* **(a)**. *Blocks:* **planning acceptance** — the schema/migration shape depends on it (this package specifies both but recommends (a)).

**W-3 — FTS5 availability in the packaged sidecar.**
*Why open:* FTS5 is present in dev SQLite 3.45.1 but the PyInstaller/Tauri-bundled SQLite on the target OS must be confirmed. *Options:* (a) require FTS5 in the packaged build (recommended); (b) ship the LIKE/`instr` fallback. *Risks:* (b) degrades search quality. *Recommendation:* **(a)** with (b) as an explicitly-flagged fallback, never a silent one. *Blocks:* implementation only (Phase 4).

**W-4 — Editorial-use → publication gating strength.**
*Why open:* synthesis §9 says an editorial-use ruling is "a prerequisite/input to publication review **where policy requires**." *Options:* (a) advisory input recorded via `publication_approval_refs` (recommended, matches "never approves exact text"); (b) hard prerequisite enforced before any approval. *Risks:* (b) couples Vault to the M00–M05 approval pipeline. *Recommendation:* **(a)**. *Blocks:* implementation only (Phase 5).

**W-5 — Is `age` encryption required for M06-A backup closure?**
*Why open:* `age` is external and not bundled until M15. *Options:* (a) encryption optional but tested-when-present (recommended); (b) required for closure. *Risks:* (b) blocks closure on M15 tooling. *Recommendation:* **(a)** — canonical backup + restore proof required; encryption tested honestly-skipped when `age` absent, per existing precedent. *Blocks:* implementation only (Phase 6).

Low-level items already resolved by repository truth (hash algorithm = SHA-256; timestamps = ISO TEXT; STRICT tables; RESTRICT FKs; idempotency via `operation_keys`; audit chaining; deterministic JSON) are **not** put to the owner.

---

## X. Planning-Package Completeness Matrix

| Requirement source | Requirement | Section | Status |
|---|---|---|---|
| authorization.md | exact physical schema | E | satisfied |
| authorization.md | Alembic sequencing + rollback | G | satisfied |
| authorization.md | per-account Vault + package layout | H, I, J | satisfied |
| authorization.md | parser admission records | K | satisfied |
| authorization.md | backup/restore/tamper | Q | satisfied |
| authorization.md | account isolation/provenance/correction/search/chunk/recovery | E,F,H,L,N,Q | satisfied |
| authorization.md | full adversarial matrix | R | satisfied |
| authorization.md | bounded package, files, validation, evidence | S, T, U | satisfied |
| authorization.md | implementation-still-blocked statement | Z | satisfied |
| outline | physical schema (no M06-B/Qdrant/graph/LLM) | E | satisfied |
| outline | migration + rollback + proof DB + M00–M05 compat | G | satisfied |
| outline | Vault FS + immutable originals + packages + manifests + protections | H,I,J | satisfied |
| outline | admission records for all 8 formats (+YouTube) | K | satisfied |
| outline | backup/restore/rebuild + wrong-account + omission + tamper + contamination | Q | satisfied |
| outline | adversarial matrix (all listed categories) | R | satisfied |
| outline | bounded implementation package + external review prompt + owner gate | S,T,Y,Z | satisfied |
| synthesis | canonical authority (no 2nd truth store) | E, O, §B | satisfied |
| synthesis | universal envelope + admission (2 dims) + terminal semantics | E.2, E.3, §C | satisfied |
| synthesis | artifact/normalization contract | I, J | satisfied |
| synthesis | 4-layer assertion/claim model | M | satisfied |
| synthesis | separate authority workflows + policy impact | M, E.9, E.10 | satisfied |
| synthesis | identity/correction/dedup; events deferred | N, E.7, §C | satisfied |
| synthesis | search + chunk contract | L | satisfied |
| synthesis | static context-run + fail-closed rules | P | satisfied |
| synthesis | logging/observability isolation | H, R(VT-71), §C | satisfied |
| synthesis | backup/restore/rebuild | Q | satisfied |
| F-1 | `revoked` + distinct terminal semantics | E.2 (`sources.admission_stage`), §C | satisfied |
| F-2 | policy-version impact set (no silent forward) | E.9 `policy_impact_records`, M, VT-50 | satisfied |
| F-3 | merge/split acceptance records actor + evidence | E.5 `entity_merge_split_decisions`, N, VT-44 | satisfied |
| F-4 | Obsidian plan historical | §A (verified header) | satisfied |
| F-5 | parser network-egress implementation test | K, VT-23 | satisfied |
| add'l req | test backup omission of an original | Q, VT-58 | satisfied |
| add'l req | test restored derivative cross-account contamination | Q, VT-65 | satisfied |
| add'l req | video/transcript = evidence-packet extensions | K.9, P | satisfied |
| impl-only | parsers cannot perform network I/O | K, VT-23 | satisfied |
| impl-only | edited projections never authority; detected/overwritten | O, VT-41/42 | satisfied |
| impl-only | partial output w/o warning fails closed | K, VT-22 | satisfied |
| M06-D01 | hybrid SQLite/FS authority | E, §B | satisfied |
| M06-D02 | universal ingestion envelope | E.2–E.4 | satisfied |
| M06-D03 | separate physical Vaults | H, W-2 | satisfied (physicality fixed by W-2) |
| M06-D04 | generated read-only projections | O | satisfied |
| M06-D05 | separate research/editorial/publication | M, E.10 | satisfied |
| M06-D06 | immutable correction/supersession | E.7, N | satisfied |
| M06-D07 | versioned normalized JSON packages | J | satisfied |
| M06-D08 | static structured context runs | P | satisfied |
| M06-D09 | human-only merge/split | N, E.5, VT-45 | satisfied |
| M06-D10 | events/chronologies deferred | §C, E (none created) | satisfied |
| M06-D11 | narrow parser scope | K | satisfied |
| M06-D12 | lexical search + chunk contract | L | satisfied |
| M06-D13 | per-account backup + restore proof | Q | satisfied |
| M06-D14 | fully manual | §C | satisfied |
| M06-D15 | Qdrant/graph deferred | §C, L, E (none) | satisfied |
| M06-D16 | no live LLM integration | P, §C | satisfied |

No item is unsatisfied. The three **blocked-by-owner-decision** items (W-2 physicality, W-3 FTS5, W-4 gating, W-5 encryption, W-1 backfill) are *implementation* choices with recommendations; only **W-2** is flagged as potentially affecting planning acceptance because the migration shape depends on it — and this package specifies both shapes with a recommendation, so it does not leave the plan incomplete.

---

## Y. Final Verdict

```text
M06-A PLANNING PACKAGE READY WITH OWNER DECISIONS REQUIRED
```

The package is internally consistent, grounded in verified live repository truth from both repositories at the expected checkpoints, faithful to M06-D01–D16 and to synthesis findings F-1–F-5 (F-4 already resolved in-repo), and specifies exact schema, migrations, filesystem layout, parser admissions, a 75-row adversarial matrix, backup/restore, an exact change set, phases, and validation/evidence. It reopens no accepted ruling and admits no parser, provider, network, or implementation.

Five genuine owner decisions remain (W-1…W-5). Only **W-2** (Vault-DB physicality) could shape the migration structure; both shapes are specified with a clear recommendation (separate per-Vault DB). The remaining four are implementation-time choices with recommendations. Because a genuine architecture-adjacent decision (W-2) is outstanding, the verdict is *ready with owner decisions required* rather than unconditionally ready. After the owner resolves W-1…W-5, this package is ready for independent review with no further planning work.

---

## Z. Explicit Authority Block

```text
No M06-A implementation authority exists.

This planning package may define the exact proposed schema, migrations,
filesystem layout, parser admissions, tests, evidence outputs, and
implementation phases.

It does not authorize application-code changes, migrations, database
creation, parser execution, Vault creation, dependency installation,
runtime changes, M06-B work, network retrieval, providers, monitoring,
live LLM integration, Qdrant, graph work, or cross-account transfer
execution.

Implementation requires independent review, finding disposition, owner
review, and explicit owner authorization.
```
