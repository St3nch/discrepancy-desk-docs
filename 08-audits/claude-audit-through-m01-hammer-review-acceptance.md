# Claude Audit Through M01 — Review and Acceptance Record

## Date

2026-07-19

## Source

Owner-provided Claude AI read-only audit generated from the live docs and main project directories using the governing prompt in:

```text
07-prompts/claude-project-audit-through-m01-hammer-focus.md
```

## Verdict Accepted

```text
PASS WITH CORRECTIONS
```

M01 evidence remains accepted. M02 planning is admitted. Physical schema, migrations, SQLite creation, ORM/application models, CRUD, and persistence implementation remain blocked.

## Independently Confirmed Findings

- attempt 001 remains at the M01 root while the successful retry is under `attempt-002/`;
- moving or rewriting attempt 001 would alter historical evidence, so the accepted correction is explicit immutable-layout documentation rather than relocation;
- a verifier must check only paths explicitly named in each `hashes.sha256` file;
- the earlier candidate `response_body_json` concept conflicts with canonical exact-byte evidence requirements;
- the main assets/evidence directory is intentionally non-Git today and needs a deliberate policy before any future initialization;
- SQLite-specific foreign-key, locking, idempotency, audit-integrity, cross-store, dirty-migration, restore, and Unicode mutation requirements were missing as explicit categories.

## Accepted Corrections

1. Preserve original attempt-001 and attempt-002 evidence without moving or editing hashed files.
2. Add explicit layout documentation, a bounded verifier, and preserved verifier output.
3. Rewrite candidate data-model planning to make exact bytes or path-plus-hash canonical and parsed JSON derivative only.
4. Move the plaintext credential file outside `C:\dev` by manual owner action without exposing its contents. Completed on 2026-07-19: the old path no longer exists and the new file exists at `C:\Users\Stench\.discrepancy-desk\discrepancy-desk-x.env`; contents were not inspected.
5. Keep the main assets/evidence directory non-Git until owner-approved PII, publication, ignore, binary, and secret policies exist.
6. Add all eight SQLite-specific hammer categories to the governing strategy and M02 gate.
7. Record the audit without treating it as implementation authorization.

## Deferred Observations

The probe runner's cost classification and post-request ceiling behavior did not invalidate M01. They are later worker-design requirements and do not justify modifying preserved probe evidence.

## Non-Authorization

This record does not authorize schema, migrations, database creation, implementation, platform calls, publication, credential inspection, or Git initialization of the main assets/evidence directory.
