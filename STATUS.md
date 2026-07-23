# Project Status — Discrepancy Desk

*Last updated: 2026-07-23*

## Current Mode

M06-A Phases 1 and 2 are owner-accepted and closed through D035. D036 Phase 3A was implemented at `251b3ca841af46e63485b9ab5bf292cbae55a418`; independent review returned `M06-A PHASE 3A REQUIRES CORRECTION`. D037 authorized the exact correction package, implemented and clean-evidence-bound at `1337b5ac450ae82664aa1ad9667a85af41c4351e`. D038 accepts the independent review disposition and closes Phase 3A. D039 authorized the exact `m06a.text.v1` owner-admission and canonical-execution package and its exact 28-invariant profile. Application commit `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9` implements that package and is clean-evidence-bound. D040 defers its independent review because Claude usage limits currently block that review; the obligation is not waived and owner closure remains pending. D040 also authorized the exact `m06a.srt.v1` under-test candidate package and exact 24-invariant profile. Application commit `529165c19da30185dd9833fab608d6dc28dfed88` implemented it. D041 records Project-Steward self-review after repeated Claude crashes: D039 had no material findings, while D040 required two exact corrections. Application commit `6a8082253a52a601291efaf3ed85ee411b04be20` corrects both findings with clean commit-bound evidence. These reviews are explicitly not represented as independent, and owner closure remains pending. SRT admission/canonical execution, VTT, JSON, Phase 3B, later M06-A phases, and M06-B remain blocked.

## Active Milestone

```text
M06-A — Local Manual Vault (D039 self-review verified; D040 corrected and self-review verified; owner closure pending; no later parser authorized)
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

The boundary is recorded at `05-implementation-planning/m06a-m06b-package-boundary.md`. M06-A remains local-only. Phases 1 and 2 are complete and closed through D035. D036 authorized Phase 3A; D037 authorized the independently required correction; D038 closes Phase 3A. D039 authorizes only the exact per-Vault plain-text admission and human-triggered canonical-execution capability, implemented at `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9`. D041 Project-Steward self-review found no material D039 finding; this is not an independent-review claim, and owner closure remains required. Phase 3B, Phases 4 through 6, other parser admission, automatic parsing, and M06-B remain blocked.

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

D030 authorized the exact Phase 1 identity, actor, migration, registry, routing, selected-Vault health, reconciliation, and adversarial package. Application commit `8fe3be4cc9da3183da88cee4bf4b19e2979b901d` implements that boundary, and evidence-binding commit `5f0f9ae` preserves the clean commit-bound closure record. The independent Claude audit returned `M06-A PHASE 1 INDEPENDENTLY VERIFIED`, and D033 closed Phase 1.

D034 authorized the exact Phase 2 observation, acquisition, immutable-artifact, rights/retention, foundational backup/restore, Tauri-only, and 33-invariant package. Application commit `eaf7b5dcd46c61654ec0320e9db19aec0a3fe962` implemented it. The required independent Claude review returned `M06-A PHASE 2 INDEPENDENTLY VERIFIED`, reported no Critical, High, or Medium findings, and concluded that D034's review gate was satisfied. The two Low findings were reproduced and corrected through exact five-path commit `1e8cba8f0ef88c2e05b9617956872be26753993e`. D035 accepts the correction, defers only the optional second spot review, and closes Phase 2.

D036 authorized the Phase 3A common parser framework, V0003, plain-text `under_test` candidate, package recovery, read-only status surface, and 35-invariant profile. Application commit `251b3ca841af46e63485b9ab5bf292cbae55a418` implemented it. Independent review returned `M06-A PHASE 3A REQUIRES CORRECTION`; D037 authorized the exact correction package. Commit `1337b5ac450ae82664aa1ad9667a85af41c4351e` adds V0004, current-head inherited proof, worker security profile v2, locator-reconciled coverage, tuple-versioned identity, exact package/document lineage, and fail-closed packaged identity verification. Clean evidence records 215/215 full-suite tests, 55/55 focused tests, Phase 3A 35/35 invariants with 53/53 mapped tests, Phase 2 33/33, Phase 1 31/31, legacy 31/31 executed plus the approved HT-14 deferral, Tauri 4/4, Rust 3/3, heads `0005`/`V0004`, packaged-sidecar proof, zero production `owner_admitted` rows, and canonical parsing unavailable. D038 accepts the independent review disposition and closes Phase 3A. The implementation return, finding disposition, and closure record are preserved under `08-audits/`.

D039 implementation is complete at `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9`. Fresh Vaults remain `under_test`; implementation creates no production `owner_admitted` row, and each physical Vault still requires the explicit human ceremony before canonical plain-text use. D041 Project-Steward self-review found no material D039 finding. The exact `m06a.srt.v1` under-test candidate was implemented at `529165c19da30185dd9833fab608d6dc28dfed88`; D040 self-review reproduced two Medium findings, both corrected and regression-proved at `6a8082253a52a601291efaf3ed85ee411b04be20`. Neither review is labeled independent, and owner closure remains pending. SRT admission/canonical execution, VTT, JSON, Phase 3B, Phases 4 through 6, M06-B planning, automatic/background parsing, monitoring, live LLM integration, Qdrant, graph work, and cross-Vault transfer execution remain blocked.

D029 records the owner clarification that The Discrepancy Desk is an editorial anomaly archive rather than a formal fact-checking or truth-adjudication product. Clearly labeled speculative, disputed, conspiratorial, folkloric, implausible, anomalous, and unresolved material may be archived and published without a universal prove-or-disprove requirement. Provenance, source/Desk-inference separation, correction lineage, exact human approval, manual publication, and the non-fabrication boundary remain unchanged. The controlling doctrine is `00-doctrine/editorial-anomaly-archive-direction.md`.

D031 makes Tauri the sole supported human operator interface. FastAPI remains the token-gated loopback backend/API and packaged sidecar host. Existing Jinja pages remain frozen historical regression scaffolding only; no new M06 browser features or browser/Tauri parity work is admitted.

D032 records the owner decision to continue the Phase 1 commit and clean-evidence path after Claude MCP/client failures and subscription usage limits prevented the immediate independent implementation audit. That deferred audit is now complete and preserved at `08-audits/claude-m06a-phase1-independent-implementation-review.md`; its disposition is `08-audits/m06a-phase1-independent-review-disposition.md`. D033 accepts Phase 1 closure and opens the exact Phase 2 package for owner review without granting implementation authority.

Application repository truth:

```text
HEAD/origin: 6a8082253a52a601291efaf3ed85ee411b04be20
Phase 1 implementation: 8fe3be4cc9da3183da88cee4bf4b19e2979b901d
Phase 1 evidence binding: 5f0f9ae
Phase 2 implementation: eaf7b5dcd46c61654ec0320e9db19aec0a3fe962
Phase 2 correction: 1e8cba8f0ef88c2e05b9617956872be26753993e
Phase 3A original implementation: 251b3ca841af46e63485b9ab5bf292cbae55a418
Phase 3A corrected implementation: 1337b5ac450ae82664aa1ad9667a85af41c4351e
Plain-text admission/canonical implementation: 7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9
SRT under-test implementation: 529165c19da30185dd9833fab608d6dc28dfed88
SRT self-review correction: 6a8082253a52a601291efaf3ed85ee411b04be20
Branch: main
Remote: synchronized with origin/main
Working tree: clean
```

The accepted AC-01 baseline remains documentary project evidence:

```text
Full-suite evidence SHA-256: 78fc6073c087eb35f404b57030137d8a2c9e51b28f8da4c903812f2a64b40796
Hammer evidence SHA-256: baaba75a25125e9dde53bbf8255e13d1c4a6e4df66c20985b3857bad1c898dbf
```

Clean commit-bound M06-A Phase 1 validation:

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
duplicate-root path leakage                               → absent
full-suite evidence SHA-256                               → 81b91a84e4acae96e31bb1e1236338f4d32cf9c034a4507a5241d01afc716b2b
Phase 1 hammer evidence SHA-256                           → 34896d63cdaafc2b71f9742354c43c844ed0bf58a2bf86d0e533a00839643647
legacy hammer evidence SHA-256                            → 6bb8dde95ea9c0c542e600ee6b31e60644709810e63c3267caa8d628f2da25b8
packaged sidecar artifact SHA-256                         → 3c589baee8a2bebc534c167fa334371386c59dfbab6fe4b76a62e253c01021b1
```

All full-suite and hammer evidence identifies implementation commit `8fe3be4cc9da3183da88cee4bf4b19e2979b901d` and records `working_tree_dirty: false`. The application closure record is `docs/m06a-phase1-implementation-closure.md`.

Independent Phase 1 review result:

```text
Verdict: M06-A PHASE 1 INDEPENDENTLY VERIFIED
Critical findings: 0
High findings: 0
Medium findings: 0
Low observations: 2 non-blocking hardening candidates
D032 obligation: satisfied
Owner closure: accepted through D033
```

The two Low observations are a defense-in-depth reparse-chain recheck before canonical byte work and explicit audit-event column selection. They do not reopen Phase 1. The next exact package also requires immutable commit-SHA-named aggregate evidence copies so later test runs cannot replace the only local copy of closure evidence.

M06-A Phase 2 closure:

```text
Required independent verdict: M06-A PHASE 2 INDEPENDENTLY VERIFIED
Original implementation: eaf7b5dcd46c61654ec0320e9db19aec0a3fe962
Corrected implementation: 1e8cba8f0ef88c2e05b9617956872be26753993e
Critical findings: 0
High findings: 0
Medium findings: 0
Low findings: 2, both corrected
Optional correction spot review: deferred due Claude usage limits
Owner closure: accepted through D035
```

Preserved evidence:

- `08-audits/claude-m06a-phase2-independent-implementation-review.md`;
- `08-audits/m06a-phase2-correction-and-closure.md`;
- `05-implementation-planning/m06a-phase2-exact-implementation-package.md`.

Clean corrected validation includes 183 Python tests, Phase 2 33/33, Phase 1 31/31, legacy 31/31 plus the approved HT-14 deferral, Tauri 4/4, Rust 3/3, exact migration heads `0005`/`V0002`, sidecar build, packaged nested-control tamper refusal, no path leakage, and no proof residue.

## Accepted Roadmap

```text
M00  Planning Foundation — complete
M01  X Identity/Policy/API Probe — complete
M02  Persistence Contract/Hammer Plan — complete
M03  Governed Local Control Room Foundation — complete
M04  Editorial Control Room and X Operating Workflow — complete
M05  Tauri Desktop Foundation and Product Parity — complete; AC-01 corrections closed
M06-A  Local Manual Vault — D039 self-review verified at `7980b1e`; SRT corrected and self-review verified at `6a80822`; owner closure and all later authority separately gated
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
- M06-A Phases 1 through 3A are independently reviewed, corrected as required, and owner-closed through D038; D039 plain-text admission/canonical execution is implemented at `7980b1e`, and D040 SRT under-test is corrected at `6a80822`; Project-Steward self-review is complete without an independent-review claim, owner closure remains pending, and all parser admission and later phases remain separately gated and blocked; M14 retains its named review and entry gate.

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
Deferred independent-audit timing decision: D032
M06-A Phase 1 owner closure and Phase 2 package-review opening: D033
M06-A Phase 2 implementation authorization: D034
M06-A Phase 2 corrected owner closure: D035
M06-A Phase 3A implementation authorization: D036
M06-A Phase 3A correction authorization: D037
M06-A Phase 3A owner closure: D038
Plain-text admission/canonical implementation authorization: D039
Deferred D039 review and SRT under-test authorization: D040
M06-A Phase 1 implementation commit: `8fe3be4cc9da3183da88cee4bf4b19e2979b901d`
M06-A Phase 1 evidence-binding commit: `5f0f9ae`
Application closure record: `docs/m06a-phase1-implementation-closure.md`
Independent Phase 1 review: `08-audits/claude-m06a-phase1-independent-implementation-review.md`
Independent-review disposition: `08-audits/m06a-phase1-independent-review-disposition.md`
Phase 2 exact package: `05-implementation-planning/m06a-phase2-exact-implementation-package.md`
Phase 3A exact package: `05-implementation-planning/m06a-phase3a-exact-implementation-package.md` (owner-closed through D038)
Plain-text admission/canonical package: `05-implementation-planning/m06a-text-v1-owner-admission-and-canonical-execution-package.md` (implemented at `7980b1e`; independent review and owner closure pending)
Plain-text implementation return: `08-audits/m06a-text-v1-implementation-return.md`
Independent Phase 2 review: `08-audits/claude-m06a-phase2-independent-implementation-review.md`
Phase 2 correction and closure: `08-audits/m06a-phase2-correction-and-closure.md`
Phase 2 implementation commit: `eaf7b5dcd46c61654ec0320e9db19aec0a3fe962`
Phase 2 correction commit: `1e8cba8f0ef88c2e05b9617956872be26753993e`
M06-A planning correction closure: `08-audits/m06a-planning-correction-closure.md`
```

## Next Bounded Action

Present D039 and corrected D040 for explicit owner closure consideration. Do not infer closure from Project-Steward self-review or clean evidence. Do not admit SRT, authorize canonical SRT execution, prepare VTT/JSON implementation, enter Phase 3B, or authorize any later capability until the owner records the next exact decision.
