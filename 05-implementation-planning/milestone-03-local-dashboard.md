# Milestone 03 — Local Control Room MVP

## Status

Not started.

## Goal

Build the smallest complete, tested local workflow from capture through human-approved publication record.

## Required Reading

- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/milestone-02-persistence-contract.md`
- `02-product/mvp-scope.md`
- `02-product/workflow-overview.md`
- `03-system-design/architecture-overview.md`
- accepted persistence contract, hammer-test plan, and M01 schema implications

## Entry Gate

- M02 completed and owner-approved.
- Hammer-test contract recorded.
- App repo initialized under `C:\dev\x\discrepancy-desk`.

## Scope

- FastAPI
- SQLite
- Alembic
- Jinja/HTMX
- capture inbox
- source records
- drafts
- human approve/reject flow
- exact-text approval binding
- manual published URL record
- basic local status screen
- migration, integrity, and hammer tests

## Out of Scope

- X write API
- Truth Social integration
- Chrome extension
- Anomaly Vault
- Qdrant
- No Coincidences
- complex analytics
- React SPA

## Required Evidence

- migration history
- focused and full test results
- hammer-test results against a real SQLite database
- manual operator walkthrough
- proof changed text invalidates prior approval
- proof forbidden transitions fail closed
- clean recovery from a deliberately failed migration/test fixture

## Exit Gate

- complete local loop works without autonomous platform action
- database constraints and application rules are tested
- no critical hammer-test failure remains
- operator can create, review, approve/reject, and record a manual publication
- docs and `STATUS.md` updated

## Completion Record

Record date, commit, test commands/results, evidence paths, deviations, and follow-up work.