# Project Status — Discrepancy Desk

*Last updated: 2026-07-20*

## Current Mode

M05 technical completion and owner closure review.

## Active Milestone

```text
M05 — Tauri Desktop Foundation and Product Parity
```

## Current Focus

M03 is owner-accepted and closed. The full editorial-product roadmap, D023, and D024 were accepted by the owner on 2026-07-20.

M04 is owner-accepted and closed as of 2026-07-20. M05 Packages A through C are technically complete under the owner-accepted exact technical plan and 77-invariant adversarial matrix. Accepted checkpoints are pushed through `384cb03`; the complete desktop parity and packaging implementation is pushed at `31d7a70`; and the final evidence-binding record is pushed at `529052b`. The desktop now has token-gated M04 workflow parity, account-scoped Command Center/Calendar/Work/Records/Metrics/System surfaces, bounded native evidence import, a Rust-owned packaged Python backend, current-user NSIS packaging, installed launch/restart/shutdown/uninstall proof, and no updater or autonomous platform authority. Final validation reports Ruff passed, 97 Python tests passed, 3 Rust tests passed, the React/Vite production build passed, NSIS packaging passed, and executable hammer evidence reports 29 executed, 29 passed, 0 failed, with one inherited scope deferral. M05 awaits explicit owner closure acceptance before M06 planning begins.

Application repository truth:

```text
HEAD 529052b — Bind M05 closure evidence
Implementation commit: 31d7a7001e72e7477e6a38cb2e7c3ee9d099197c
Hammer evidence SHA-256: e3c59481d3fd7b8163828e5cc97f6f26ac359d086c481e41ec5cfb5bc2c6fc20
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
M05  Tauri Desktop Foundation and Product Parity — technically complete; owner closure pending
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

The accepted roadmap/M03 closure package is committed and pushed at `c54706b214249313c0dda9aae18d2d5fb6efebb5`. The accepted M04 planning package is committed and pushed at `ca99dcf`. All M04 implementation and evidence-binding checkpoints are pushed through `770a6bb`. The accepted M05 planning package is pushed at `479a4b6`; all M05 implementation, packaging, installed lifecycle proof, and evidence-binding checkpoints are pushed through application commit `529052b`. The current docs working tree records M05 technical completion pending owner closure acceptance.

## Next Bounded Action

Owner reviews and explicitly accepts M05 closure. After closure, prepare the exact M06 Records, Dossiers, and Anomaly Vault planning package without beginning M06 implementation.