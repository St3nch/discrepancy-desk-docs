# Independent Review — The Discrepancy Desk M06-A Phase 1

*Review completed: 2026-07-22*

## A. Live Repository Truth

**Application repository:** `C:\dev\x\discrepancy-desk`

- branch: `main`;
- HEAD: `5f0f9ae139a6c2dece123b503a90a656c13671fe`;
- upstream: `origin/main`;
- ahead/behind: `0/0`;
- working tree: clean;
- no staged, modified, deleted, or untracked paths.

Recent commits:

```text
5f0f9ae  Bind M06-A Phase 1 closure evidence
8fe3be4  Implement M06-A Phase 1 Vault foundation
6cbd036  Bind AC-01 correction evidence
```

The review verified that `5f0f9ae` adds only `docs/m06a-phase1-implementation-closure.md`; working-tree source content is byte-identical to implementation commit `8fe3be4cc9da3183da88cee4bf4b19e2979b901d`.

**Documentation repository:** `C:\dev\x\discrepancy-desk-docs`

- branch: `main`;
- HEAD: `eb405c483db1ee15814d84f6288920dbe498de68`;
- upstream: `origin/main`;
- ahead/behind: `0/0`;
- working tree: clean.

The evidence-binding structure—clean evidence bound to `8fe3be4`, recorded by follow-up commit `5f0f9ae`—is represented accurately and consistently.

## B. Independent Verdict

```text
M06-A PHASE 1 INDEPENDENTLY VERIFIED
```

Verified within the validation scope executed during the review. No Critical, High, or Medium findings.

## C. Findings

No Critical, High, or Medium findings.

### L-01 — Residual TOCTOU Window in Vault Root Creation

**Severity:** Low

**Location:** `src/discrepancy_desk/vault_identity.py`, `create_vault()` / `resolve_vault_root()`

Reparse, case-collision, and existence checks precede `root.mkdir()`, so a local process could theoretically alter the filesystem between validation and creation. Practical exploitation is constrained because creation is fail-closed: `root.mkdir(parents=True, exist_ok=False)` and the marker opened with mode `"x"` both refuse pre-existing entries, and failures enter `reconciliation_required` handling.

Current tests do not exercise the race itself; HT-004 and HT-016 prove fail-closed create semantics.

Optional bounded hardening: re-run `_reject_reparse_chain()` on the fully created root before migration begins. This is not required for Phase 1 closure.

### L-02 — Audit Verification Uses Positional `SELECT *`

**Severity:** Low

**Location:** `src/discrepancy_desk/vault_persistence.py`, `verify_vault_audit_chain()`

The verifier relies on the current V0001 `audit_events` column order. It is correct against the exact manifest-pinned schema, but a future column reorder could make the positional logic fragile.

Optional bounded hardening: select the required columns explicitly. This is not required for Phase 1 closure.

## D. Tauri-Only Verdict

D031 is faithfully implemented.

- no new browser Vault product surface exists;
- no `/vaults` Jinja route exists;
- no `templates/vaults.html` exists;
- all Vault operations are under `/desktop-api/v1/vaults[...]`;
- desktop API paths are protected by launch-token middleware;
- `desktop/src/api/client.ts` calls the loopback API and never SQLite;
- the request body carries no actor selector;
- HT-108 proves request-supplied `actor_id` does not select the trusted owner actor;
- `provision_vault()` receives `owner_actor_id="owner-local"` from server state;
- legacy M03/M04 Jinja routes remain frozen regression scaffolding;
- selected-Vault state remains separate from selected platform-account state.

## E. Phase-Boundary Verdict

No unauthorized authority leaked into Phase 1.

The V0001 schema creates only identity, actor, audit, operation-key, receipt, and reconciliation structures. Empty directories such as `objects/sha256`, `packages/sha256`, and `projections/*` are inert placeholders with no executable intake, parser, search, dossier, provider, network, purge, or publication capability.

Destructive downgrade is refused when governed rows exist.

## F. Validation Results

Executed:

- Git status and history inspection for both repositories—clean and synchronized at expected commits;
- `uv run ruff check .`—passed;
- `uv run pytest -o addopts= --disable-warnings -q`—139 tests passed, no failed/error/skipped tests; the process later encountered an environmental `PermissionError` while cleaning a stale pytest temp symlink;
- `pnpm --dir desktop test`—2 passed;
- `git diff --check`—passed;
- central Alembic head—`0005`;
- Vault Alembic head—`V0001`;
- direct recomputation of central and Vault migration manifests—8/8 central and 4/4 Vault entries matched;
- direct recomputation of closure evidence hashes—matched.

Not re-executed during the independent session at owner direction due usage limits:

- `run_ht_evidence.py --suite m06a-phase1`;
- `run_ht_evidence.py --suite legacy`;
- desktop production build;
- Rust tests;
- sidecar build;
- packaged loopback smoke.

Their clean commit-bound evidence was independently hash-verified and the underlying code and mappings were inspected.

The original Claude workbench migration-manifest helper initially reported a false mismatch because it assumed classic `sha256sum` column order and versions-only coverage. The helper was later corrected to understand the project's manifest format and full migration environment. After restart it reported `all_ok: true`, with 8/8 central and 4/4 Vault entries matching and zero parse errors. The workbench Git-diff tools were also corrected to use `--no-ext-diff` so the user's configured external diff driver cannot break audit inspection.

## G. Evidence Verdict

All evidence claims were verified.

- 31 exact Phase 1 invariant IDs;
- no duplicates or missing executable mappings;
- zero-test, skip, xfail/xpass, collection-error, missing-evidence, ID-divergence, exit-status-divergence, and commit-divergence conditions fail closed;
- HT-108 maps to `test_m06a_ht_108_tauri_api_uses_governed_service`;
- all evidence identifies implementation commit `8fe3be4cc9da3183da88cee4bf4b19e2979b901d` and records `working_tree_dirty: false`.

Recomputed hashes matched the closure record:

```text
Full suite:
81b91a84e4acae96e31bb1e1236338f4d32cf9c034a4507a5241d01afc716b2b

Phase 1 hammer:
34896d63cdaafc2b71f9742354c43c844ed0bf58a2bf86d0e533a00839643647

Legacy hammer:
6bb8dde95ea9c0c542e600ee6b31e60644709810e63c3267caa8d628f2da25b8

Sidecar evidence:
8454400f8ae71d9f41280aab82b3171fddab8a15e7db80894fbc72b06c9133f7
```

## H. D032 Disposition

The review concludes that this audit satisfies the independent-review obligation deferred under D032.

The hammer suites, desktop/Rust/sidecar builds, and packaged smoke were not re-executed live during the independent session because of owner-directed usage limits. Their commit-bound evidence was independently hash-verified and inspected. Nothing found suggests live re-execution would change the verdict.

## I. Exact Correction Package

No corrections required.

L-01 and L-02 are optional hardening candidates confined to:

```text
src/discrepancy_desk/vault_identity.py
src/discrepancy_desk/vault_persistence.py
```

No files were edited.

## J. Phase 2 Recommendation

The Phase 1 implementation is bounded, fail-closed, evidence-bound, and faithful to D027 through D031.

The project may proceed to owner consideration of a separately bounded Phase 2 authorization. This review does not authorize Phase 2.

## Stop Boundary

Nothing was edited, staged, committed, or pushed during the independent review. No Phase 2 work began.
