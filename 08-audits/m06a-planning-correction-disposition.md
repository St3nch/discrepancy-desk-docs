# M06-A Planning Correction Disposition

## Status

```text
Correction cycle: closed
Owner decisions: resolved
Claude resolved-package review: completed
Focused correction verification: completed
Planning package: owner-accepted
Implementation authority: none
```

This record dispositions every material finding in:

```text
08-audits/m06a-local-manual-vault-planning-package-independent-review.md
```

Source planning input:

```text
08-audits/m06a-local-manual-vault-planning-package.md
```

Independent-review verdict:

```text
M06-A PLAN READY WITH CORRECTIONS
```

Owner-accepted corrected planning package:

```text
05-implementation-planning/m06a-local-manual-vault-canonical-plan.md
05-implementation-planning/m06a-parser-admission-plan.md
05-implementation-planning/m06a-adversarial-closure-matrix.md
```

Closure record:

```text
08-audits/m06a-planning-correction-closure.md
```

The source report remains preserved unchanged. Divergence is recorded here rather than patched into the historical report.

---

# 1. Disposition Rules

Allowed dispositions:

```text
accepted
accepted with modification
rejected
deferred
owner decision required
deferred to acceptance/navigation package
```

A finding closes only when its required result exists in the named candidate section, independent review confirms the result, all dependent owner decisions are resolved, and the owner accepts the corrected package. `Lands in` identifies candidate text, not completed implementation.

---

# 2. Finding Dispositions

## M06A-PC-01 — Historical live-tree statement differs from current repository state

**Source finding:** F-01
**Classification:** documentation inconsistency
**Severity:** Low
**Disposition:** accepted with modification

### Required result

Preserve Claude's historical statement unchanged in the source report, but do not repeat it as current truth. The corrected candidate must rely on the actual clean post-preservation checkpoint and treat ephemeral-clone inspection as incapable of proving local untracked-file state.

### Lands in

- this disposition record, status and source-history language;
- `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`, Status and governing-input sections.

### Closure evidence

Independent corrected-package review confirms no historical report was edited and current repository claims are accurately scoped.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-02 — Missing observations and unresolved questions

**Source finding:** F-02
**Classification:** accepted-architecture conflict
**Severity:** High
**Disposition:** accepted

### Required result

Make `observations` and `unresolved_questions` first-class canonical objects with same-Vault identity, evidence, actor, state, and correction lineage. Typed dossier memberships must reference real tables.

### Lands in

- core plan §6, Canonical Object and Provenance Model;
- core plan §7, Observation, Acquisition, and Artifact Contract;
- core plan §16, Assertions, Entities, Questions, Dossiers, and Policy;
- matrix M06A-HT-018 and M06A-HT-060.

### Closure evidence

Schema/migration review plus executable same-Vault observation/question/dossier tests.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-03 — Artifact and acquisition cardinality collapses repeated encounters

**Source finding:** F-03
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted

### Required result

Separate one content-addressed immutable `artifact_objects` row per Vault/content hash from immutable encounter-specific acquisitions and `acquisition_artifact_links`. Repeated identical bytes must preserve independent source, occurrence, observation, actor, filename, policy, timestamp, and receipt.

### Lands in

- core plan §6.1, object families;
- core plan §7.3, Artifact objects versus encounters;
- matrix M06A-HT-021 and M06A-HT-022.

### Closure evidence

Real SQLite/filesystem duplicate-acquisition proof shows one object and multiple immutable receipts.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-04 — Acquisition lifecycle is false and locator-only intake is misclassified

**Source finding:** F-04
**Classification:** accepted-architecture conflict
**Severity:** High
**Disposition:** accepted with modification

### Required result

Acquisition starts as `started` with null outcome and finalizes truthfully. The candidate intentionally uses separate lifecycle states (`started`, `finalized`, `interrupted`, `reconciled`) and terminal outcomes rather than copying the review's suggested labels; this preserves truthful non-terminal and terminal semantics while making reconciliation explicit. A URL/YouTube locator alone creates identity/occurrence/observation records, not successful remote acquisition or artifact records. `no_artifact` is legal only for a completed governed operation where no byte artifact was expected or admitted. It cannot represent failed acquisition, missing expected content, parser failure, retention rejection, interruption, quarantined temporary material, or silent artifact loss.

### Lands in

- core plan §7.1 and §7.2;
- parser plan §15;
- matrix M06A-HT-019, M06A-HT-020, and M06A-HT-104.

### Closure evidence

Lifecycle constraints, locator-only behavior, and a deliberate proof that `no_artifact` cannot mask failure, interruption, retention rejection, or missing expected content.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-05 — Deterministic package claim includes run-specific values

**Source finding:** F-05
**Classification:** factual error
**Severity:** High
**Disposition:** accepted

### Required result

Separate run-specific `parser_executions` from deterministic `normalized_packages`. Exclude timestamps, host data, process IDs, random UUIDs, execution IDs, and absolute paths from package bytes/hash. Identical input/parser/config/schema must reproduce byte-identical output or fail determinism.

### Lands in

- core plan §8;
- parser plan §5;
- matrix M06A-HT-039 and M06A-HT-040.

### Closure evidence

Independent-process deterministic rerun evidence and failure test for injected run metadata.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-06 — Human-only actor authority is unenforceable

**Source finding:** F-06
**Classification:** authority or safety blocker
**Severity:** Critical
**Disposition:** accepted

### Required result

Add an authoritative actor registry and trusted server-supplied `ActorContext`. Request bodies cannot choose actor identity/class. The desktop launch token authenticates the local backend process but does not itself establish a human decision identity. Unknown, disabled, revoked, system, model, import, or wrong-Vault actors must fail human-only operations.

### Lands in

- core plan §9;
- core plan §13;
- core plan Phase 1 and application change surface;
- matrix M06A-HT-009 through M06A-HT-015.

### Closure evidence

Actor impersonation, disabled actor, model/system authority, desktop-token, audit, and idempotency hammer evidence.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21; the identified implementation proof remains a Critical phase-closure requirement.

---

## M06A-PC-07 — Authority-bearing polymorphic references lack integrity

**Source finding:** F-07
**Classification:** authority or safety blocker
**Severity:** High
**Disposition:** accepted

### Required result

Replace authority-bearing `ref_type/ref_id` designs with typed junctions, typed target tables, or exact-one-target columns with real SQLite foreign keys. Generic targets are allowed only in derived non-authoritative queues and require validation before dispatch.

### Lands in

- core plan §10;
- core plan §6.1 typed membership/evidence/correction families;
- matrix M06A-HT-026 and M06A-HT-060.

### Closure evidence

Schema inspection and fabricated/dangling/generic-reference rejection tests.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-08 — Same-account and provenance-chain consistency is not enforced

**Source finding:** F-08
**Classification:** authority or safety blocker
**Severity:** High
**Disposition:** accepted

### Required result

Carry `vault_account_id` through canonical tables, use composite same-Vault foreign keys, validate Vault singleton identity on every connection, and enforce semantic parent-chain continuity within service transactions. Individually valid IDs from unrelated branches must not compose into a valid record.

### Lands in

- core plan §6.2 and §6.3;
- core plan §7 and §10;
- matrix M06A-HT-001, M06A-HT-018, M06A-HT-025, and M06A-HT-026.

### Closure evidence

Two-Vault real-engine and cross-provenance composition tests.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-09 — Vault identity and create/open behavior are not enforceable

**Source finding:** F-09
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted with modification

### Required result

Apply resolved OD-1: one physical Vault represents one editorial/public brand identity, currently The Discrepancy Desk. Use a random `vault_instance_id`, singleton `vault_metadata`, matching `VAULT_IDENTITY.json`, registry fingerprint, explicit `create_vault()`, and non-creating `open_existing_vault()`. Bind multiple platform-owned accounts explicitly in the central database without creating platform-specific Vaults or granting publication authority. Reject wrong root/database/marker, wrong-brand bindings, path ambiguity, symlinks, junctions, and reparse points before migration/write. SQLite does not claim to validate a filesystem marker through `CHECK`.

### Lands in

- core plan §3.1, resolved OD-1;
- core plan §4, §5, §11, and §14;
- matrix M06A-HT-001 through M06A-HT-008;
- matrix M06A-HT-088 through M06A-HT-092.

### Closure evidence

D027/OD-1 decision record plus independent resolved-package review and identity, binding, open/create, publication-boundary, and Windows-path hammer evidence.

### Status

Closed for planning by D027, focused verification, and owner acceptance on 2026-07-21.

---

## M06A-PC-10 — Proposed migration environment is not executable

**Source finding:** F-10
**Classification:** implementation-design defect
**Severity:** Critical
**Disposition:** accepted with modification

### Required result

Apply resolved OD-2: retain the separate central control-room SQLite database, keep existing central migrations 0001–0004, add one additive central registry revision, and create one exact `vault_migrations/` Alembic environment used independently by each physical Vault SQLite file. Each Vault owns its own audit chain, operation ledger, migration state, and backup history. Generalize the runner so it does not infer `migrations_root.parent/alembic.ini`; strengthen manifest, identity, dirty-state, recovery, packaging, downgrade, and per-Vault isolation contracts. Cross-database work uses receipts and reconciliation, never atomicity claims.

### Lands in

- core plan §3.2, resolved OD-2;
- core plan §4, §12, §13, §14, and §20;
- core plan Phase 1 and §22;
- matrix M06A-HT-016 and M06A-HT-062 through M06A-HT-074;
- matrix M06A-HT-093 through M06A-HT-097.

### Closure evidence

D027/OD-2 decision record, independent resolved-package review, real fresh/populated/interrupted/recovery proofs, per-Vault migration and backup isolation, cross-database reconciliation, and packaged resource proof.

### Status

Closed for planning by D027, focused verification, and owner acceptance on 2026-07-21; per-Vault migration and recovery proof remains a Critical implementation phase-closure requirement.

---

## M06A-PC-11 — Audit and idempotency topology is contradictory

**Source finding:** F-11
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted

### Required result

Use `audit_events`, `operation_keys`, and `alembic_version` inside each physical authority database. Define one audit chain per database. Cross-database operations use correlation IDs and separate central/Vault receipts with reconciliation; they never claim atomicity.

### Lands in

- core plan §12.2;
- core plan §13;
- matrix M06A-HT-014 through M06A-HT-017 and M06A-HT-074.

### Closure evidence

Per-database audit/idempotency proof and deliberate cross-database partial-failure reconciliation.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-12 — Desktop/backend multi-Vault routing is absent

**Source finding:** F-12
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted

### Required result

Add registry-driven opaque Vault routing, verified on-demand open, selected Vault state separate from selected platform account, Vault health/migration/backup routing, bounded connection lifetime, and desktop/API changes. Requests never supply arbitrary paths.

### Lands in

- core plan §14;
- core plan Phase 1;
- core plan §22.4 and §22.5;
- matrix M06A-HT-001, M06A-HT-008, M06A-HT-013, M06A-HT-075, and M06A-HT-077.

### Closure evidence

Source and packaged desktop API/routing/security parity tests with two Vaults.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-13 — Backup model falsely excludes derived rows and implies atomic completion

**Source finding:** F-13
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted

### Required result

State honestly that full SQLite snapshots may contain derived rows; classify them non-authoritative and clear/invalidate/rebuild them on restore. Establish foundational backup in Phase 2. Use start receipt, snapshot, object/package copy, manifest, `COMPLETE`, final receipt, and crash reconciliation without claiming one filesystem/SQLite atomic transaction.

### Lands in

- core plan §20;
- core plan Phase 2 and Phase 6;
- matrix M06A-HT-066 through M06A-HT-074.

### Closure evidence

Foundational and complete backup/restore evidence including partial completion and derived rebuild.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-14 — Rights, retention, retrieval, export, and public eligibility are conflated

**Source finding:** F-14
**Classification:** authority or safety blocker
**Severity:** High
**Disposition:** accepted with modification

### Required result

Apply resolved OD-3: separate retention eligibility/deadline, internal retrieval, context, export, internal/public projection, and quotation/redistribution eligibility. Use immutable policy versions and superseding artifact-policy bindings; unknown defaults to deny. Timed-deletion-required, missing, contradictory, or unknown retention eligibility is rejected before canonical byte admission and parser execution. Preserve only content-free rejection metadata. M06-A contains no destructive purge authority or delete-later bypass.

### Lands in

- core plan §3.3, resolved OD-3;
- core plan §7.4, §15, and §20;
- parser plan §1.4, §2.4, §2.5, §4.1, and §18;
- matrix M06A-HT-027 through M06A-HT-031;
- matrix M06A-HT-098 through M06A-HT-101.

### Closure evidence

D027/OD-3 decision record plus independent resolved-package review and policy-dimension, pre-byte rejection, safe receipt, no-downstream-presence, parser exclusion, backup exclusion, and no-hidden-purge tests.

### Status

Closed for planning by D027, focused verification, and owner acceptance on 2026-07-21.

---

## M06A-PC-15 — Parser admission records are not exact or machine-enforceable

**Source finding:** F-15
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted

### Required result

Define exact parser IDs, implementations or explicitly unresolved choices, version/license/limits/subsets/warnings/failures/fixtures/packaging, and machine-enforceable definition/config/admission records. Runtime invocation requires an exact owner-admitted hash tuple; prose never grants authority.

### Lands in

- `05-implementation-planning/m06a-parser-admission-plan.md`, §§1–18;
- core plan §8 and Phase 3A/3B;
- matrix M06A-HT-032 and M06A-HT-044.

### Closure evidence

Per-parser admission record, dependency/lock/license inventory, fixture/evidence manifest, and owner admission.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21; no parser is admitted.

---

## M06A-PC-16 — Parser no-egress boundary is not enforceable

**Source finding:** F-16
**Classification:** authority or safety blocker
**Severity:** Critical
**Disposition:** accepted

### Required result

Use a fixed-entrypoint parser worker with deny controls for socket creation/connect, DNS, urllib/HTTP clients, subprocess/shell, external XML/HTML resolvers, arbitrary filesystem access, and unapproved native behavior. Prove malicious canaries fail in source and packaged sidecar. If proof fails, parser remains unadmitted.

### Lands in

- parser plan §4;
- parser-specific resolver sections;
- core plan Phase 3A/3B;
- matrix M06A-HT-033 through M06A-HT-038 and M06A-HT-044.

### Closure evidence

Source and packaged no-egress evidence with malicious parser/resolver fixtures and no partial canonical output.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21; the identified implementation proof remains a Critical phase-closure requirement.

---

## M06A-PC-17 — FTS, chunk, and projection contracts are insufficiently executable

**Source finding:** F-17
**Classification:** implementation-design defect
**Severity:** Medium
**Disposition:** accepted

### Required result

Use an explicit integer row mapping plus external-content FTS5, fixed tokenizer, current/policy/right filtering, transactional synchronization, no silent LIKE fallback, deterministic chunk identity/invalidation, and hash-bound read-only projections with tamper detection and no path back to authority.

### Lands in

- core plan §17, §18, and §19;
- core plan Phase 4;
- matrix M06A-HT-045 through M06A-HT-052.

### Closure evidence

Source and packaged FTS5 proof, stale-index/chunk invalidation, projection tamper, and cross-Vault derivative tests.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-18 — Hammer matrix is not executable and runner can misstate success

**Source finding:** F-18
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted

### Required result

Provide requirement source, phase, valid/violation fixtures, exact planned pytest mapping, real-engine need, fail-closed result, evidence output, and closure for every invariant. Strengthen the existing runner to fail on zero execution, missing mappings, collection failure, absent/fabricated/wrong-commit evidence, detector errors, unapproved skips, and matrix/runner divergence.

### Lands in

- `05-implementation-planning/m06a-adversarial-closure-matrix.md`, all sections;
- matrix M06A-HT-078 through M06A-HT-087;
- core plan §23.

### Closure evidence

Runner unit tests plus complete per-invariant and aggregate evidence bound to the implementation commit.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-19 — Cross-database references can be fabricated or stale

**Source finding:** F-19
**Classification:** authority or safety blocker
**Severity:** High
**Disposition:** accepted

### Required result

Treat cross-database identifiers in the Vault as non-authoritative verified receipts only. Any current approval dependency is checked live through the central governed service against account, revision, binding, decision, and consumption/supersession state. No Vault FK or editorial-use ruling grants publication authority.

### Lands in

- core plan §11;
- core plan §13 and §16.5;
- matrix M06A-HT-016, M06A-HT-017, M06A-HT-056, and M06A-HT-057.

### Closure evidence

Fabricated, stale, wrong-account, consumed, and superseded central-reference tests.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

## M06A-PC-20 — Phases and application change surface are understated

**Source finding:** F-20
**Classification:** implementation-design defect
**Severity:** High
**Disposition:** accepted

### Required result

Use corrected phases beginning with identity/actors/migrations/routing, add foundational backup when canonical artifacts begin, split stdlib and external parser admissions, and include migration runner/integrity, backup, packaging, hammer runner, dependencies, desktop TypeScript/Rust, health/routes, migrations, and tests. Do not claim general non-destructive downgrade precedent; define revision-specific rollback/refusal/restore.

### Lands in

- core plan §12.5;
- core plan §20.1;
- core plan §21 and §22;
- core plan §23.4;
- matrix migration, backup, packaging, and runner rows.

### Closure evidence

Independent file-surface review, exact implementation return, phase evidence, and no unplanned authority expansion.

### Status

Closed for planning by focused verification and owner acceptance on 2026-07-21.

---

# 3. Resolved Owner Decisions

The owner resolved all three planning decisions on 2026-07-21. They are recorded together in `99-decisions/decision-log.md` as D027. The recommendation in each candidate decision was adopted. These rulings constrain the planning candidate but do not grant canonical acceptance or implementation authority.

## OD-1 — Canonical Vault-account meaning

**Decision ID:** OD-1, recorded under D027

**Question:** Does one physical Vault represent an editorial/public brand identity or one platform-owned account?

**Options considered:**

```text
one Vault per editorial/public brand identity
one Vault per platform-owned account
```

**Owner ruling:**

```text
One physical Vault represents one editorial/public brand identity.
The current Vault identity is The Discrepancy Desk.
```

**Recommendation adopted:** Yes.

**Affected findings:** M06A-PC-09; also M06A-PC-08, M06A-PC-12, and M06A-PC-19.

**Affected canonical sections:** core plan §§3.1, 4, 5, 6, 11, 14, 16, 20, 21, and 22; matrix M06A-HT-001 through M06A-HT-008 and M06A-HT-088 through M06A-HT-092.

**Date:** 2026-07-21.

**Authority effect:** The candidate must use one The Discrepancy Desk brand Vault that may bind multiple central platform-owned accounts. A platform account binding creates no separate Vault and grants no publication authority.

**Remaining block:** Focused correction verification, owner acceptance, and separate implementation authorization.

## OD-2 — Physical SQLite topology

**Decision ID:** OD-2, recorded under D027

**Question:** Does the Vault system use one SQLite file per physical Vault or one shared account-scoped Vault database?

**Options considered:**

```text
one SQLite file per physical Vault
one shared account-scoped SQLite database
```

**Owner ruling:**

```text
The central control-room SQLite database remains separate.
Each physical Vault has its own SQLite database file.
```

**Recommendation adopted:** Yes.

**Affected findings:** M06A-PC-10; also M06A-PC-11, M06A-PC-12, M06A-PC-13, and M06A-PC-19.

**Affected canonical sections:** core plan §§3.2, 4, 5, 6, 11, 12, 13, 14, 20, 21, and 22; matrix M06A-HT-001 through M06A-HT-017, M06A-HT-062 through M06A-HT-074, and M06A-HT-093 through M06A-HT-097.

**Date:** 2026-07-21.

**Authority effect:** The candidate has one controlling per-Vault SQLite design. Every physical authority database has its own audit, idempotency, migration, and backup state. Cross-database operations use correlation receipts and reconciliation and are never atomic.

**Remaining block:** Focused correction verification, owner acceptance, and separate implementation authorization.

## OD-3 — Retention and destructive purge boundary

**Decision ID:** OD-3, recorded under D027

**Question:** Does first M06-A reject timed-deletion material or implement governed destructive purge authority?

**Options considered:**

```text
reject material requiring timed deletion in first M06-A
implement governed purge authority in M06-A
```

**Owner ruling:**

```text
First M06-A rejects material requiring timed deletion.
Destructive purge authority is deferred.
```

**Recommendation adopted:** Yes.

**Affected findings:** M06A-PC-14; also M06A-PC-15, M06A-PC-16, and M06A-PC-20.

**Affected canonical sections:** core plan §§3.3, 7.4, 15, 20, 21, and 23; parser plan §§1.4, 2.4, 2.5, 4.1, and 18; matrix M06A-HT-027 through M06A-HT-032 and M06A-HT-098 through M06A-HT-101.

**Date:** 2026-07-21.

**Authority effect:** Timed-deletion-required or unknown-retention material is rejected before canonical bytes or parser execution. Only safe content-free rejection metadata may remain. M06-A gains no purge implementation or delayed-deletion promise.

**Remaining block:** Focused correction verification, owner acceptance, and separate implementation authorization.

---

# 4. Claude Resolved-Package Review Dispositions

The completed external Claude review returned:

```text
M06-A RESOLVED PLAN READY WITH CORRECTIONS
```

Claude reported one Medium and five Low findings, with no High or Critical findings. The review was supplied outside the repository and is treated as advisory input until independently verified and dispositioned here. No review prompt is stored in the repository.

## R-01 — Phase 2 foundational backup conflicts with implied migration-revision content

**Claude classification and severity:** documentation inconsistency with sequencing impact; Medium.

**Independent verification result:** verified.

**Disposition:** accepted

**Exact correction:** Rename the Phase 2 Vault revision to `V0002_ingestion_artifacts_policy_and_backup.py`, move `backup_generations`, `backup_generation_files`, and generation start/final receipt schema into that revision family, rename `V0006` to closure-only support, and add an exact phase-to-revision mapping. Phase 2 backup is blocked unless the V0002 foundational backup schema is present.

**Affected files and sections:** core plan §§12.1, 12.2, 20.1, 20.3, and 21; matrix M06A-HT-102.

**Status:** closed by focused verification and owner acceptance on 2026-07-21.

**Blocking scope:** no longer blocks planning acceptance; Phase 2 implementation remains unauthorized.

## R-02 — Quarantine storage location is undefined

**Claude classification and severity:** missing implementation detail; Low.

**Independent verification result:** verified.

**Disposition:** accepted

**Exact correction:** Define `quarantined` as a governed database state over immutable objects, packages, executions, or projection manifests at their ordinary paths. No second canonical quarantine root exists. Define `temp/<operation-id>/temporary-quarantine/` separately as non-canonical, operation-scoped, excluded from backup/search, and required to be admitted or destroyed/reconciled before clean operation closure.

**Affected files and sections:** core plan §§4.1, 8.3, 8.4, and 19; parser plan §§2.6, 4.1, and 12; matrix M06A-HT-024, M06A-HT-039, M06A-HT-050, M06A-HT-100, M06A-HT-105, and M06A-HT-106.

**Status:** closed by focused verification and owner acceptance on 2026-07-21.

**Blocking scope:** no longer blocks planning acceptance; no runtime quarantine authority is granted.

## R-03 — Content-derived hashes on rejection path are not explicitly bounded

**Claude classification and severity:** missing implementation detail; Low.

**Independent verification result:** verified.

**Disposition:** accepted

**Exact correction:** Prohibit any rejection receipt, audit event, operation key, log, diagnostic, or retained metadata from containing a hash derived from rejected bytes or pasted text. State that a digest can confirm possession of known content and is not content-free. Limit rejection idempotency to Vault/actor identity, retention declaration, policy-basis reference, safe source-kind classification, client-generated operation nonce, and sanitized non-content descriptor.

**Affected files and sections:** core plan §§3.3, 13.2, and 15.1; parser plan §§1.4 and 2.5; matrix M06A-HT-103.

**Status:** closed by focused verification and owner acceptance on 2026-07-21.

**Blocking scope:** no longer blocks planning acceptance; later Phase 2 intake implementation remains unauthorized.

## R-04 — Application change surface omits evidence plugin and browser-template scope

**Claude classification and severity:** missing implementation detail; Low.

**Independent verification result:** verified.

**Disposition:** accepted

**Exact correction:** Add `tests/conftest.py` to the evidence/hammer surface because the live pytest plugin consumes the runner-selected evidence destination and emits session counts/commit data. Fix the UI boundary: M06-A remains available through both the local FastAPI/Jinja browser application and packaged desktop shell, using the same service operations and authority outcomes. Add browser templates/routes/tests and cross-surface parity coverage.

**Affected files and sections:** core plan §§14.7, 22.5, 22.6, and 23; matrix M06A-HT-107 and M06A-HT-108.

**Status:** closed by focused verification and owner acceptance on 2026-07-21.

**Blocking scope:** no longer blocks planning acceptance; no UI or test code is authorized.

## R-05 — PC-04 disposition and `no_artifact` legality are imprecise

**Claude classification and severity:** documentation inconsistency; Low.

**Independent verification result:** verified with modification.

**Disposition:** accepted with modification

**Exact correction:** Change M06A-PC-04 to `accepted with modification` because the candidate deliberately strengthens the review's suggested lifecycle vocabulary by separating lifecycle state from terminal outcome and adding reconciliation. Define `no_artifact` as legal only for completed locator/identity-only work where no byte artifact was expected or admitted; prohibit its use for failure, missing expected content, parser failure, retention rejection, interruption, temporary quarantine, or silent loss.

**Affected files and sections:** core plan §7.2; M06A-PC-04 in this disposition; matrix M06A-HT-104.

**Status:** closed by focused verification and owner acceptance on 2026-07-21.

**Blocking scope:** no longer blocks planning acceptance; no lifecycle implementation is authorized.

## R-06 — Navigation and status records lag D027 and the candidate package

**Claude classification and severity:** documentation inconsistency; Low.

**Independent verification result:** verified.

**Disposition:** deferred to acceptance/navigation package

**Exact correction:** No technical-design change was required. This authorized acceptance/navigation package updates:

```text
STATUS.md
LLM_MAP.md
05-implementation-planning/implementation-roadmap.md
05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md
05-implementation-planning/m06a-planning-package-outline.md
```

This package records D027 and D028, the preserved audit record, the resolved planning baseline and disposition, the focused-verification result, the closure record, and controlling-document progression. The roadmap update records progress and controlling documents without altering the accepted M06 direction.

**Affected files and sections:** the five navigation/governance files listed above, modified in this acceptance package.

**Status:** closed by the acceptance/navigation reconciliation on 2026-07-21.

**Blocking scope:** navigation lag is closed; no implementation authority follows.

---

# 5. Finding Coverage Summary

```text
Independent-review findings:       20
Mapped:                            20
Accepted:                          15
Accepted with modification:        5
Rejected:                           0
Deferred:                           0
Owner decision required:            0
```

OD-1 through OD-3 are resolved and incorporated. R-01 through R-05 passed focused verification. R-06 is closed by the acceptance/navigation package. The owner accepted the resolved planning baseline on 2026-07-21, and the correction cycle is closed through `08-audits/m06a-planning-correction-closure.md`.

---

# 6. Correction Closure

The planning correction cycle closed after:

1. the corrected planning files passed internal validation;
2. the Claude resolved-package review was completed and R-01 through R-06 were dispositioned;
3. R-01 through R-05 were corrected and passed focused verification;
4. R-06 navigation, milestone, roadmap, status, map, and outline updates were reconciled;
5. the owner explicitly accepted the resolved M06-A planning baseline;
6. `08-audits/m06a-planning-correction-closure.md` recorded closure.

Planning closure does not authorize implementation. A separate explicit owner authorization is still required before any application or runtime change.

---

# 7. Explicit Authority Block

```text
Correction cycle: closed
Owner decisions: resolved
Claude resolved-package review: completed
Focused correction verification: completed
Planning package: owner-accepted
Implementation authority: none

This disposition records planning closure only. It does not authorize
application code, migrations, database creation, Vault creation, parser
implementation, dependency installation, runtime changes, M06-B work,
or implementation.
```
