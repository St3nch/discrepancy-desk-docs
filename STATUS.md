# Project Status — Discrepancy Desk

*Last updated: 2026-07-20*

## Current Mode

M05 planning and exact technical-package preparation.

## Active Milestone

```text
M05 — Tauri Desktop Foundation and Product Parity
```

## Current Focus

M03 is owner-accepted and closed. The full editorial-product roadmap, D023, and D024 were accepted by the owner on 2026-07-20.

M04 is owner-accepted and closed as of 2026-07-20. M05 implementation is authorized under the owner-accepted exact technical plan and 77-invariant adversarial matrix. The first Package A batch is now in the application working tree: token-gated versioned desktop API mode, account-scoped health/query endpoints, deny-by-default Tauri configuration and capabilities, React/Vite shell, Rust backend-supervisor skeleton, reproducible npm lock resolution, security contract, and 12 focused adversarial tests. Ruff passes, the full Python suite reports 79 passed, and the production frontend build passes. Native Cargo/Tauri compilation remains unverified because the approved repository tool does not expose Cargo. The batch is uncommitted pending owner review.

Application repository truth:

```text
HEAD 770a6bb — Bind M04 closure evidence to implementation
Implementation evidence bound to 1455433ddee69d52b1dc67367a532b594a4a1a6c
main synchronized with origin/main
```

M03 validation:

```text
uv run ruff check .          → passed
uv run pytest                → 54 passed
scripts/run_ht_evidence.py   → 19 executed, 19 passed, 0 failed
HT-14                        → deferred by approved scope
```

## Accepted Roadmap

```text
M00  Planning Foundation — complete
M01  X Identity/Policy/API Probe — complete
M02  Persistence Contract/Hammer Plan — complete
M03  Governed Local Control Room Foundation — complete
M04  Editorial Control Room and X Operating Workflow — complete
M05  Tauri Desktop Foundation and Product Parity — planning active
M06  Records, Dossiers, and Anomaly Vault
M07  Human-Triggered X Capture Helper
M08  Agent-Neutral Governed Interface
M09  Restricted Release Watch and Hermes Pilot
M10  Truth Social Secondary Manual Workflow
M11  Metrics Ledger and Editorial Learning
M12  Rich Editorial Workspaces
M13  No Coincidences Pattern Candidates
M14  Qdrant Semantic Retrieval
M15  Hardening, Backup, Recovery, and Operator Runbook
```

Canonical ruling: `05-implementation-planning/editorial-control-room-roadmap-ruling.md`.

## Accepted Product Direction

- records-first editorial operation;
- Archive, Docket, and Flash Release editorial lanes;
- rolling 90-day active schedule;
- first-class Unscheduled Reserve;
- Command Center answering what matters now;
- Ready-to-Post and anti-filler Need-a-Post;
- functional web contract harness followed by product-grade Tauri;
- bounded LLM context and candidate operations only;
- no direct agent database access;
- agent-neutral interface before Hermes;
- one restricted Release Watch pilot before any broader agent role;
- separate physical vault per account under D024;
- account-scoped Qdrant collections and fail-closed retrieval under D024;
- M06 and M14 implementation still require their named independent reviews and entry gates.

## M03 Closure Evidence

- physical SQLite/Alembic implementation;
- guarded migrations and dirty-state recovery;
- exact revision and approval binding;
- verified evidence references;
- idempotency and transaction behavior;
- append-only hash-chained audit events;
- manual-ready and publication reconciliation;
- mismatch, successor revision, and replacement publication lineage;
- manual metric observation states;
- backup/restore and three-way reconciliation proof;
- thin FastAPI/Jinja operator harness;
- executable HT evidence bound to the implementation commit.

## Hard Boundaries

- No autonomous posting, replies, likes, follows, reposts, or DMs.
- No undocumented/internal platform endpoints.
- No credentials in Git, Markdown, screenshots, manifests, or evidence.
- No real-government impersonation or claims of classified access.
- No speculation promoted as confirmed truth.
- No agent/LLM direct database access.
- No agent self-approval, accepted-truth promotion, publication authority, or evidence deletion.
- No provider/platform/source watcher admitted without its milestone gate and owner approval.
- Truth Social remains manual-only under D014.

## Docs Working Tree

The accepted roadmap/M03 closure package is committed and pushed at `c54706b214249313c0dda9aae18d2d5fb6efebb5`. The accepted M04 planning package is committed and pushed at `ca99dcf`. All M04 implementation and evidence-binding checkpoints are pushed through `770a6bb`. The accepted M05 planning package is committed and pushed at `479a4b6`; the current docs working tree records the first Package A implementation batch pending review.

## Next Bounded Action

Review the first M05 Package A implementation batch. If accepted, checkpoint the application and synchronized docs changes, then continue with executable sidecar startup/health/shutdown and native-compilation validation.