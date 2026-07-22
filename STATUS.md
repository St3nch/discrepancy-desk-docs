# Project Status — Discrepancy Desk

*Last updated: 2026-07-22*

## Current Mode

M06-A Phase 1 implementation authorized under D030; local uncommitted implementation is being aligned to the Tauri-only D031 boundary and prepared for independent review.

## Active Milestone

```text
M06-A — Local Manual Vault (Phase 1 implementation authorized; uncommitted review candidate in progress)
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

The boundary is recorded at `05-implementation-planning/m06a-m06b-package-boundary.md`. M06-A remains local-only. D030 authorizes Phase 1 implementation only; Phases 2 through 6 remain blocked. M06-B is a later separately gated network/SSRF package and is not authorized for planning or implementation.

All four owner decision sets are approved and recorded at `99-decisions/m06-owner-architecture-rulings.md`:

```text
M06-D01  Hybrid SQLite/filesystem canonical authority         approved
M06-D02  Universal governed ingestion envelope                approved
M06-D03  Separate physical Vaults with governed imports       approved
M06-D04  Generated read-only Markdown/HTML projections        approved
M06-D05  Separate research/editorial/publication workflows    approved
M06-D06  Immutable correction and supersession lineage        approved
M06-D07  Versioned normalized JSON element package            approved
M06-D08  Static structured LLM context runs                   approved
M06-D09  Human-only entity merge/split acceptance             approved
M06-D10  Events and chronologies deferred from first M06-A    approved
M06-D11  Narrow initial parser scope                           approved
M06-D12  SQLite lexical search plus chunk contract            approved
M06-D13  Per-Vault canonical backup and restore proof         approved
M06-D14  M06-A remains fully manual                           approved
M06-D15  Qdrant and graph work deferred                       approved
M06-D16  No live LLM/provider integration in M06-A            approved
```

The owner accepted the resolved M06-A planning baseline after the preserved independent review, D027 owner rulings, Claude resolved-package review, bounded corrections, and focused verification. D028 records acceptance, and `08-audits/m06a-planning-correction-closure.md` closes the correction cycle. The accepted package contains the core plan, parser-admission plan, and 108-invariant adversarial matrix.

D030 authorizes the exact Phase 1 identity, actor, migration, registry, routing, selected-Vault health, reconciliation, and Phase 1 adversarial package. A local uncommitted implementation candidate exists and is being revalidated after the D031 Tauri-only correction. No parser is admitted. Phases 2 through 6, M06-B planning, provider admission, parser admission, monitoring, live LLM integration, Qdrant, graph work, and cross-Vault transfer execution remain blocked.

D029 records the owner clarification that The Discrepancy Desk is an editorial anomaly archive rather than a formal fact-checking or truth-adjudication product. Clearly labeled speculative, disputed, conspiratorial, folkloric, implausible, anomalous, and unresolved material may be archived and published without a universal prove-or-disprove requirement. Provenance, source/Desk-inference separation, correction lineage, exact human approval, manual publication, and the non-fabrication boundary remain unchanged. The controlling doctrine is `00-doctrine/editorial-anomaly-archive-direction.md`.

D031 makes Tauri the sole supported human operator interface. FastAPI remains the token-gated loopback backend/API and packaged sidecar host. Existing Jinja pages remain frozen historical regression scaffolding only; no new M06 browser features or browser/Tauri parity work is admitted.

Application repository truth:

```text
HEAD/origin: 6cbd0366e55bfba0c9687201615b72e70bf485d5
Commit:      Bind AC-01 correction evidence
Branch:      main
Remote:      synchronized with origin/main
Working tree: dirty by design with 39 exact M06-A Phase 1 source/test/migration paths
Staged:      none
Committed:   no Phase 1 commit
Pushed:      no Phase 1 push
```

The accepted AC-01 baseline remains documentary project evidence:

```text
Full-suite evidence SHA-256: 78fc6073c087eb35f404b57030137d8a2c9e51b28f8da4c903812f2a64b40796
Hammer evidence SHA-256: baaba75a25125e9dde53bbf8255e13d1c4a6e4df66c20985b3857bad1c898dbf
```

Current uncommitted M06-A Phase 1 validation after the D031 correction:

```text
uv run ruff check .                                      → passed
uv run pytest -o addopts= --disable-warnings -q          → 139 passed
M06-A Phase 1 hammer                                     → 31/31 passed, 0 deferred
Legacy hammer                                             → 31/31 passed, HT-14 approved deferral
Tauri Vitest                                              → 2 passed
Tauri production build                                    → passed
Rust tests                                                → 3 passed
packaged sidecar build                                    → passed
packaged loopback smoke                                   → central 0005; Vault V0001; audit valid
new /vaults browser product route                         → absent / 404
sidecar SHA-256                                           → bb8bc3496195b0a58759eb8b17c8727b79726c0d072b29d5f4edfd919bc01c02
```

These are provisional dirty-tree results. They are not commit-bound Phase 1 closure evidence.

## Accepted Roadmap

```text
M00  Planning Foundation — complete
M01  X Identity/Policy/API Probe — complete
M02  Persistence Contract/Hammer Plan — complete
M03  Governed Local Control Room Foundation — complete
M04  Editorial Control Room and X Operating Workflow — complete
M05  Tauri Desktop Foundation and Product Parity — complete; AC-01 corrections closed
M06-A  Local Manual Vault — Phase 1 implementation authorized; review candidate in progress
M06-B  Bounded Static Webpage Retrieval — deferred; separately gated
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
- editorial anomaly archive under D029: record the crazy, label what it is, connect what emerges, never fabricate the file;
- unresolved or speculative material may be editorially useful without a mandatory final conclusion;
- Archive, Docket, and Flash Release editorial lanes;
- rolling 90-day active schedule;
- first-class Unscheduled Reserve;
- Command Center answering what matters now;
- Ready-to-Post and anti-filler Need-a-Post;
- Tauri as the sole supported operator interface, with FastAPI retained as the governed loopback backend/API and legacy Jinja pages frozen as regression scaffolding;
- bounded LLM context and candidate operations only;
- no direct agent database access;
- agent-neutral interface before Hermes;
- one restricted Release Watch pilot before any broader agent role;
- one physical Vault per editorial/public brand identity under D027, with platform-owned accounts bound centrally;
- account-scoped Qdrant collections and fail-closed retrieval under D024;
- M06-A Phase 1 is authorized under D030; later phases still require separate explicit owner authorization; M14 retains its named review and entry gate.

## Hard Boundaries

- No autonomous posting, replies, likes, follows, reposts, or DMs.
- No undocumented/internal platform endpoints.
- No credentials in Git, Markdown, screenshots, manifests, or evidence.
- No real-government impersonation or claims of classified access.
- No speculation promoted as confirmed truth.
- No fabricated sources, documents, quotations, screenshots, evidence, or corroboration.
- No hiding the difference between source material and Desk-created inference.
- No agent/LLM direct database access.
- No agent self-approval, accepted-truth promotion, publication authority, or evidence deletion.
- No provider/platform/source watcher admitted without its milestone gate and owner approval.
- No arbitrary YouTube media downloading or generic caption scraping.
- Truth Social remains manual-only under D014.

## Docs Repository Truth

```text
AC-01 closure: 982bcf0
M06 research program: d5d8b1d through 7f44f9c
Audit-prompt boundary correction: 91146e4
Claude M06 research review and AC-02 package: b675014
Owner rulings M06-D01 through M06-D16: 44f5a57 through d8105e0
M06 synthesis and roadmap reconciliation: c90f1af
M06 synthesis audit corrections: 191fc8a
AC-02 closure and M06-A audit preservation: fb3b865
M06-A owner rulings: D027
M06-A planning owner acceptance: D028
Editorial anomaly archive clarification: D029 and `00-doctrine/editorial-anomaly-archive-direction.md` (docs commit `740cc22`)
M06-A Phase 1 implementation authorization: D030
Tauri-only supported operator interface: D031
M06-A planning correction closure: `08-audits/m06a-planning-correction-closure.md`
```

## Next Bounded Action

Complete validation of the D031 Tauri-only correction against the uncommitted M06-A Phase 1 implementation candidate, then obtain independent implementation review of the exact live application tree. Correct any findings before owner authorization to commit. Do not begin Phase 2, admit parsers, add intake/artifact/search/dossier/backup capability, or begin M06-B planning.
