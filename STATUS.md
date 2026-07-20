# Project Status — Discrepancy Desk

*Last updated: 2026-07-20*

## Current Mode

M06 Vault and ingestion architecture research.

## Active Milestone

```text
M06 — Records, Dossiers, and Anomaly Vault (architecture research only)
```

## Current Focus

M00 through M05 are owner-accepted and closed. Claude's independent post-M01 through M05 audit returned `ACCEPT WITH CORRECTIONS` with no blocking authority, account-isolation, loopback/token, audit-integrity, installer-preservation, or secret-hygiene defect.

AC-01 is closed through application correction commit `17f40e3d18a58ac47b48933551a4044586d940aa`, application evidence-binding commit `6cbd036`, and docs closure commit `982bcf0`.

The M06 nine-report research program is active:

```text
R-M06-01  Source Universe and Admission Policy       complete
R-M06-02  YouTube and Audiovisual Ingestion         complete
R-M06-03  Website, Feed, and Change Monitoring      complete
R-M06-04  Document Normalization and Provenance     complete
R-M06-05  Canonical Vault and Knowledge Model       next
R-M06-06 through R-M06-09                           not started
```

R-M06-02 recommends a manual-first YouTube workflow consisting of a URL plus a pasted or uploaded transcript, exact preservation, explicit provenance, normalized timestamps/segments, and human review. R-M06-03 compares paste-only webpage intake, bounded single-page retrieval, feeds, monitoring, provider-assisted extraction, and rendered-browser capture. R-M06-04 compares Markdown-first, common JSON element packages, parser-native outputs, database-only normalization, and a hybrid governed-package model. None is selected or approved. Every option remains subject to owner review during architecture synthesis before implementation authority can exist.

M06 implementation, Qdrant, source monitors, media downloading, transcript-provider integration, and automated connectors remain blocked.

Application repository truth:

```text
HEAD 6cbd036 — Bind AC-01 correction evidence
AC-01 implementation commit: 17f40e3d18a58ac47b48933551a4044586d940aa
Full-suite evidence SHA-256: 78fc6073c087eb35f404b57030137d8a2c9e51b28f8da4c903812f2a64b40796
Hammer evidence SHA-256: baaba75a25125e9dde53bbf8255e13d1c4a6e4df66c20985b3857bad1c898dbf
main synchronized with origin/main
```

Current AC-01 correction baseline:

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
R-M06-01 research: d5d8b1d
R-M06-02 research: e268fc8
R-M06-03 research: 156519b
R-M06-04 research: pending current commit
```

## Next Bounded Action

Execute `R-M06-05 — Canonical Vault and Knowledge Model`. Do not begin M06 implementation, crawling, monitoring, transcript-provider integration, media acquisition, parser selection, schema adoption, or Qdrant work.
