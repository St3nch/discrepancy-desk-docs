# Project Status — Discrepancy Desk

*Last updated: 2026-07-20*

## Current Mode

M06 Vault and ingestion architecture research.

## Active Milestone

```text
M06 — Records, Dossiers, and Anomaly Vault (architecture research only)
```

## Current Focus

M00 through M05 are owner-accepted and closed. AC-01 is closed through application correction commit `17f40e3d18a58ac47b48933551a4044586d940aa`, application evidence-binding commit `6cbd036`, and docs closure commit `982bcf0`.

The complete nine-report M06 research program received independent Claude review with verdict:

```text
RESEARCH PACKAGE READY WITH CORRECTIONS
```

No critical or high findings were reported. The review is preserved at `08-audits/claude-m06-research-program-independent-review.md`. AC-02 records nine bounded pre-synthesis corrections at `08-audits/ac02-m06-research-correction-disposition.md`, with the consolidated correction specification at `05-implementation-planning/m06-pre-synthesis-correction-specification.md`.

The owner accepted one package-boundary decision:

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

The boundary is recorded at `05-implementation-planning/m06a-m06b-package-boundary.md`. M06-A remains local-only. M06-B is a later separately gated network/SSRF package. Neither is authorized for implementation.

Two owner decision sets are approved and recorded at `99-decisions/m06-owner-architecture-rulings.md`:

```text
M06-D01  Hybrid SQLite/filesystem canonical authority         approved
M06-D02  Universal governed ingestion envelope                approved
M06-D03  Separate physical Vaults with governed imports       approved
M06-D04  Generated read-only Markdown/HTML projections        approved
M06-D05  Separate research/editorial/publication workflows    approved
M06-D06  Immutable correction and supersession lineage        approved
M06-D07  Versioned normalized JSON element package            approved
M06-D08  Static structured LLM context runs                   approved
```

The active work is owner option review for the remaining architecture and scope decisions. Architecture synthesis, M06-A implementation, M06-B planning, provider admission, parser admission, monitoring, live LLM integration, Qdrant, graph work, and cross-account transfer execution remain blocked.

Application repository truth:

```text
HEAD 6cbd036 — Bind AC-01 correction evidence
AC-01 implementation commit: 17f40e3d18a58ac47b48933551a4044586d940aa
Full-suite evidence SHA-256: 78fc6073c087eb35f404b57030137d8a2c9e51b28f8da4c903812f2a64b40796
Hammer evidence SHA-256: baaba75a25125e9dde53bbf8255e13d1c4a6e4df66c20985b3857bad1c898dbf
main synchronized with origin/main
```

Current AC-01 correction baseline remains documentary project evidence:

```text
uv run ruff check .                                      → passed
uv run pytest -o addopts= --disable-warnings -q          → 104 passed
Rust tests                                               → 3 passed
frontend production build                               → passed
packaged sidecar build                                   → passed
scripts/run_ht_evidence.py                               → 31 executed, 31 passed, 0 failed
HT-14                                                    → deferred by approved scope
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

## Hard Boundaries

- No autonomous posting, replies, likes, follows, reposts, or DMs.
- No undocumented/internal platform endpoints.
- No credentials in Git, Markdown, screenshots, manifests, or evidence.
- No real-government impersonation or claims of classified access.
- No speculation promoted as confirmed truth.
- No agent/LLM direct database access.
- No agent self-approval, accepted-truth promotion, publication authority, or evidence deletion.
- No provider/platform/source watcher admitted without its milestone gate and owner approval.
- No arbitrary YouTube media downloading or generic caption scraping.
- Truth Social remains manual-only under D014.

## Docs Repository Truth

```text
AC-01 closure: 982bcf0
R-M06-01 through R-M06-06 and multi-account note: d5d8b1d through f1ce71f
R-M06-07 through R-M06-09: 7f44f9c
Audit-prompt boundary correction: 91146e4
Claude M06 review, AC-02 correction package, and M06-A/M06-B split: pending current commit
```

## Next Bounded Action

Conduct owner option review for the before-synthesis decisions in small groups. Record each ruling without beginning implementation. After the required rulings and AC-02 correction closure, author the M06 architecture synthesis for independent review. Do not begin M06-A or M06-B implementation.
