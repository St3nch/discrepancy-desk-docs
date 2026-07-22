# Independent Implementation Review — The Discrepancy Desk M06-A Phase 2

*Review completed: 2026-07-22*

## A. Live Repository Truth

### Application repository

```text
C:\dev\x\discrepancy-desk
```

Verified state at review start and after validation:

```text
branch: main
HEAD: eaf7b5dcd46c61654ec0320e9db19aec0a3fe962
origin/main: eaf7b5dcd46c61654ec0320e9db19aec0a3fe962
ahead/behind: 0/0
working tree: clean
```

Commit `eaf7b5d` (`Implement M06-A Phase 2 artifact foundation`) sits directly on `5f0f9ae`. The diff `5f0f9ae..eaf7b5d` changes exactly the 22 paths declared by the authorized package.

### Documentation repository

```text
C:\dev\x\discrepancy-desk-docs
```

Verified state:

```text
branch: main
HEAD: d614198e7e84d5a8363c3d905cafc3f9c24df955
origin/main: d614198e7e84d5a8363c3d905cafc3f9c24df955
working tree: clean
```

D033 closes Phase 1. D034 authorizes the exact Phase 2 package and its 33-invariant profile.

## B. Independent Verdict

```text
M06-A PHASE 2 INDEPENDENTLY VERIFIED
```

Every required validation command, including packaged-executable exercise, was executed. No Critical, High, or Medium findings were reported.

## C. Findings

### L-03 — Empty V0002 Downgrade Drops Inherited V0001 `vault_metadata`

**Severity:** Low

**Location:** `vault_migrations/versions/V0002_ingestion_artifacts_policy_and_backup.py`, `downgrade()`

The governed-occupancy check correctly includes `vault_metadata`, so every real Vault with its identity singleton refuses downgrade. On a never-provisioned identity-less database, however, the final drop loop also dropped `vault_metadata`, leaving a database stamped `V0001` without the full V0001 schema.

No governed Vault exploit was identified. The issue affected only synthetic empty-database schema parity.

Bounded correction identified by the review:

- keep `vault_metadata` in the occupancy check;
- exclude it from the V0002 table-drop loop;
- add an empty-database downgrade schema-parity test.

### L-04 — Nested Control-Name Files Evade Backup Unmanifested-File Detection

**Severity:** Low

**Location:** `src/discrepancy_desk/vault_backup.py`, `verify_vault_generation()`

The final actual-file set excluded files by basename:

```text
manifest.json
COMPLETE
```

This allowed an unmanifested nested file with either name to evade the set comparison. Such a file gained no database, audit, artifact, or policy authority, and the proof workspace remained disposable, but the unmanifested-file guarantee contained a blind spot.

Bounded correction identified by the review:

- exclude only generation-root-relative `manifest.json` and `COMPLETE`;
- reject nested files with either name;
- add tamper tests.

The Project Steward later confirmed that the restore copy loop already used the correct root-relative comparison. Only `verify_vault_generation()` required the code correction.

## D. V0002 and Schema Verdict

The V0002 schema was found sound, subject to L-03.

Verified characteristics included:

- fourteen STRICT tables;
- composite same-Vault foreign keys;
- exact acquisition state/outcome constraints;
- append-only triggers on authority tables;
- no-delete triggers and field-level lifecycle update guards;
- one-time upload authorization consumption;
- guarded backup-generation transitions;
- immutable policy-binding lineage;
- retention `allow` structurally incompatible with a deadline;
- fail-closed downgrade on governed identity or Phase 2 rows;
- fresh creation and V0001→V0002 upgrade behavior;
- valid central and Vault migration manifests.

## E. Retention and Artifact-Authority Verdict

The retention and artifact authority boundary was found faithful.

Verified behavior included:

- timed-deletion, unknown, missing, and contradictory classifications reject before upload authorization or byte intake;
- rejection receipts contain no content bytes, label, filename, locator, note, or content-derived hash;
- the upload endpoint rechecks preservation-compatible retention;
- artifact upload streams in bounded chunks;
- the 64 MiB ceiling is enforced during streaming;
- canonical paths derive only from lowercase SHA-256;
- no-overwrite finalization;
- existing-object size/hash verification;
- actual SHA-256 and byte size participate in idempotency;
- exact replay returns the original result;
- conflicting byte replay fails without producing a new object;
- duplicate bytes preserve separate acquisition encounters;
- failures enter typed interruption and reconciliation handling;
- database and canonical filesystem inventory must reconcile exactly;
- health blocks on reconciliation, temp bytes, or inventory divergence;
- only `manual_file` and `manual_locator` are admitted server-side.

## F. Backup and Restore Verdict

The foundational backup and disposable-restore design was found faithful, subject to L-04.

Verified behavior included:

- active human `vault_admin` authority;
- bounded operation keys;
- exact replay and conflict refusal;
- one selected Vault only;
- SQLite backup API use;
- authoritative admitted objects only;
- no temp, rejected, prior-backup, or other-Vault material;
- deterministic manifest and create-only `COMPLETE` marker;
- database receipts for generation files;
- reconciliation-required state on failure;
- exact generation-ID validation;
- manifest, completion marker, database receipt, hash, size, migration-head, identity, audit-chain, artifact, and policy reconciliation;
- wrong-Vault, dirty-target, nonempty-target, tamper, missing-file, and incomplete-generation refusal;
- proof workspace removal on API success and failure;
- active-human, audited, idempotent verification operation.

No impossible cross-database atomicity claim was identified.

## G. Tauri-Only Verdict

D031 remained faithfully implemented.

- `/vaults` returned 404 from the packaged executable;
- no Vault Jinja template exists;
- legacy Jinja scaffolding remains untouched;
- Tauri obtains a user-selected `File`;
- bytes travel through the token-gated multipart loopback API;
- the frontend does not supply filesystem paths, actor identity, canonical paths, retention authority, or SQLite operations;
- trusted actors are resolved server-side.

## H. Phase-Boundary Verdict

No unauthorized capability leaked into Phase 2.

The implementation did not add executable authority for:

```text
parsers
parser admission or execution
normalized packages
document versions
search or FTS
chunks
assertions
entities
questions
dossiers
projections
URL retrieval
network access
providers
monitoring
background jobs
live LLM integration
Qdrant or embeddings
graph work
cross-Vault import execution
purge or delete-later authority
automated publication
new browser Vault product routes
```

Empty package/projection directories remained inert placeholders.

## I. Validation Results

All required commands were executed.

```text
uv sync --frozen --extra dev                         passed
uv run ruff check .                                  passed
full Python suite                                    180 passed
Phase 2 hammer                                       33/33 passed
Phase 1 hammer                                       31/31 passed
legacy hammer                                        31/31 passed + approved HT-14 deferral
central migration head                               0005
Vault migration head                                 V0002
Tauri tests                                          4 passed
Tauri production build                               passed
Rust tests                                           3 passed
sidecar build                                        passed
git diff --check                                     passed
packaged executable exercise                         14/14 checks passed
```

The Python suite completed all 180 tests before an environmental cleanup failure involving the stale `pytest-current` junction under the user's temporary directory. Redirecting pytest temporary storage allowed the hammer suites to complete cleanly. The evidence runner correctly failed closed when subprocess exit status and evidence status initially diverged.

The packaged exercise proved:

- central `0005`;
- `/vaults` 404;
- Vault creation;
- duplicate-root refusal without path leakage;
- selected-Vault `V0002` health;
- content-free timed-deletion rejection before bytes;
- preservation-compatible intake and content-addressed artifact admission;
- backup generation;
- verification/disposable restore;
- byte-identical verification replay;
- proof-directory removal;
- post-workflow health.

## J. Evidence and Hash Verdict

All expected evidence hashes recomputed exactly for application commit:

```text
eaf7b5dcd46c61654ec0320e9db19aec0a3fe962
```

All records identified that commit and recorded `working_tree_dirty: false`.

```text
Full suite:
7b3c47a44286656387654cb6259a27992a017c4737f38c7b90df82c8383761a0

Phase 2 hammer:
8bd30f2be189a809d027f476deb0e62b71e924d42f45068b7328b9e68004cc0f

Phase 1 hammer:
b44ad4ba21444dd57b60ee0bca6ae0dffe1a72af763dd77280924cb0524136d6

Legacy hammer:
183d1c048a08b11ab2b9da5a78a1561ccdd030170d1129c0f26ae176bc1f133b

Sidecar evidence file:
43bc17266d55767ca152369c49a9fba465c65b58f0f2c457ac8fc8c3cc11bbca

Recorded sidecar artifact:
08f54a646d1332931f1143b3d6799cbd0d7c6fcfe824829ebb5071f16bc52289
```

The review independently demonstrated deterministic same-commit aggregate evidence and create-only by-commit refusal behavior.

## K. Correction Package

No Critical, High, or Medium correction was required.

The review identified the optional bounded L-03/L-04 hardening surface:

```text
src/discrepancy_desk/vault_backup.py
tests/test_m06a_backup_restore.py
tests/test_m06a_migrations.py
vault_migrations/manifest.sha256
vault_migrations/versions/V0002_ingestion_artifacts_policy_and_backup.py
```

No files were edited during the independent review.

## L. D034 Independent-Review Gate

The review concluded:

```text
D034 independent-review gate: satisfied
```

The review re-established repository truth, reviewed the complete declared diff, executed every required command, exercised the packaged workflow, and recomputed the evidence hashes.

## M. Phase 2 Closure Recommendation

The review concluded:

```text
The owner may consider M06-A Phase 2 closure.
```

The review did not authorize Phase 3.

## Stop Boundary

Nothing was edited, staged, committed, or pushed during the review. No closure documentation, Phase 3 work, parser admission, or M06-B work was begun.
