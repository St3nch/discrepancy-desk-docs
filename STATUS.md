# Project Status — Discrepancy Desk

*Last updated: 2026-07-20*

## Current Mode

Post-M05 independent audit and M06 Vault/ingestion architecture research.

## Active Milestone

```text
M06 — Records, Dossiers, and Anomaly Vault (architecture research only)
```

## Current Focus

M03 is owner-accepted and closed. The full editorial-product roadmap, D023, and D024 were accepted by the owner on 2026-07-20.

M04 and M05 are owner-accepted and closed as of 2026-07-20. M05 closure is bound to application commits `31d7a70` and `529052b` and docs commit `9b9d770`. The project is now in the owner-authorized M05-to-M06 transition: an independent Claude audit must inspect all accepted work since the preserved M01 audit, accepted findings must be corrected and revalidated, and a nine-report deep-research program must establish the governed Vault and manual-first/future-automated ingestion architecture. M06 implementation, Qdrant, source monitors, and automated connectors remain blocked. The controlling transition plan is `05-implementation-planning/m05-to-m06-transition-audit-and-research-plan.md`.

Application repository truth:

```text
HEAD 529052b — Bind M05 closure evidence
Implementation commit: 31d7a7001e72e7477e6a38cb2e7c3ee9d099197c
Hammer evidence SHA-256: e3c59481d3fd7b8163828e5cc97f6f26ac359d086c481e41ec5cfb5bc2c6fc20
main synchronized with origin/main
```

Current AC-01 correction baseline:

```text
Application implementation commit                       → 17f40e3d18a58ac47b48933551a4044586d940aa
Application evidence-binding commit                     → 6cbd036
uv run ruff check .                                      → passed
uv run pytest -o addopts= --disable-warnings -q          → 104 passed
Rust tests                                               → 3 passed
frontend production build                               → passed
packaged sidecar build                                   → passed
scripts/run_ht_evidence.py                               → 31 executed, 31 passed, 0 failed
HT-14                                                    → deferred by approved scope
full-suite evidence SHA-256                              → 78fc6073c087eb35f404b57030137d8a2c9e51b28f8da4c903812f2a64b40796
hammer evidence SHA-256                                  → baaba75a25125e9dde53bbf8255e13d1c4a6e4df66c20985b3857bad1c898dbf
```

## Accepted Roadmap

```text
M00  Planning Foundation — complete
M01  X Identity/Policy/API Probe — complete
M02  Persistence Contract/Hammer Plan — complete
M03  Governed Local Control Room Foundation — complete
M04  Editorial Control Room and X Operating Workflow — complete
M05  Tauri Desktop Foundation and Product Parity — complete; AC-01 corrections closed
M06  Records, Dossiers, and Anomaly Vault — architecture research active; implementation blocked
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

The accepted roadmap/M03 closure package is committed and pushed at `c54706b214249313c0dda9aae18d2d5fb6efebb5`. The accepted M04 planning package is committed and pushed at `ca99dcf`. All M04 implementation and evidence-binding checkpoints are pushed through `770a6bb`. The accepted M05 planning package is pushed at `479a4b6`; all M05 implementation, packaging, installed lifecycle proof, and evidence-binding checkpoints are pushed through application commit `529052b`, with owner closure recorded in the current transition package. The docs working tree now establishes the mandatory independent audit and M06 dual-track deep-research program.

## Next Bounded Action

Run the independent Claude audit from `08-audits/claude-post-m01-through-m05-independent-audit-prompt.md`. Record and correct accepted findings, rerun affected validation, and then execute the nine-report M06 Vault and ingestion research program. Do not begin M06 implementation.