# Decision Log

## D001 — Public Brand

Accepted:

```text
The Discrepancy Desk
```

Reason:

- exact `.com` available and purchased
- `@DiscrepancyDesk` fits X 15-character handle limit
- strong bureaucratic weirdness
- more ownable than Anomaly Archive / Redacted Archive

## D002 — Public Persona

Accepted:

```text
Quinton Clearance
```

Fictional records custodian associated with Basement 1, Level 7 / B1-L7.

## D003 — Brand Safety

Accepted:

The account must be commentary/parody/fictional and must not imply real agency employment or classified access.

## D004 — Core Doctrine

Accepted:

```text
AI drafts. Human clears. Database remembers. Metrics judge.
```

## D005 — Repo Split

Accepted:

```text
C:\dev\x\discrepancy-desk-docs = planning/docs/specs
C:\dev\x\discrepancy-desk = app/code repo
```

## D006 — Local-First Technical Direction

Accepted:

Early stack should be local-first and simple:

```text
FastAPI + SQLite + Alembic + Jinja/HTMX
```

Do not start with React/Postgres/Redis/Qdrant.

## D007 — X API Probe Before Schema

Accepted:

Run read-only X API probes and store raw JSON before final schema decisions.

## D008 — Manual Posting First

Accepted:

No autonomous posting/replies in early system. Manual copy-paste posting with published URL recording.

## D009 — Anomaly Vault Direction

Accepted:

Research memory can be Obsidian-compatible Markdown, with SQLite as structured operational truth and Qdrant later for semantic retrieval.

## D010 — No Coincidences

Accepted:

No Coincidences is a future pattern-candidate module. It flags weird overlaps but does not declare truth.

## D011 — Bread Baker Canon

Accepted:

Quinton bakes bread/sourdough/old recipes. This humanizes the persona and creates “crumbs” wordplay.

## D012 — MCP Helper Repo

Accepted:

Local ChatGPT MCP helper repo path:

```text
C:\dev\gpt-mcp
```

Purpose: safe filesystem/Git/project-steward tools for ChatGPT.

## D013 — Dual-Platform Brand Presence and Sequence

Accepted: 2026-07-19.

The Discrepancy Desk will launch and stabilize on **X first**. Truth Social is a secondary platform under the same brand and persona, considered only after the X workflow is operational and stable.

Truth Social must not delay the X MVP or cause X-specific policy, API, capture, publishing, or metrics assumptions to be reused without separate review.

## D014 — Truth Social Manual-Only Admission

Accepted: 2026-07-19.

Truth Social is admitted only for:

- manual publishing through the official UI
- official share-composer assistance
- official bookmarks
- manually entered URLs and human-authored notes
- owned-post text, URLs, IDs, timestamps, and manually observed metrics
- official owner-data exports

Not admitted:

- browser-extension DOM capture
- Truth Social content scripts or parsers
- automated reads or writes
- undocumented endpoints or API probing
- scraping, background collection, or browser automation
- thread, account, timeline, search-result, or bulk archival
- automated screenshots or recurring metrics polling

The boundary may be reopened only after written permission, materially revised official policy, or owner-accepted professional legal guidance. Source: `06-research/truth-social-capture-boundaries.md`.

## D015 — Approved Quinton Avatar and B1-L7 X Banner

Accepted: 2026-07-19.

The owner approved and applied the current Quinton Clearance avatar and B1-L7 X banner to the live `@DiscrepancyDesk` profile.

The avatar establishes the canonical animation-friendly visual identity for Quinton: approximately 55, short and compact, lean-to-average build, messy gray hair, rectangular glasses, tired eyes, expressive eyebrows, pale shirt, loosened dark tie, brown-gray cardigan, and a dry skeptical expression.

The banner establishes the current B1-L7 environment: a dusty modern records office, an open circular vault, Quinton walking through with his face hidden, and an unexpectedly high-technology archive interior beyond it. The supporting wall includes a single portrait of President Donald Trump and a respectfully displayed American flag.

Approved image masters live in the main project repo under `assets/brand/`. Only owner-approved, manually saved assets belong there; rejected generations are not retained. The governing visual specification and exact paths/hashes are recorded in `01-brand/avatar-banner-direction.md`.

## D016 — M02 Hammer-Test Doctrine

Accepted: 2026-07-19.

Every persistence rule must be attacked with deliberate bad data, invalid state transitions, fabricated identifiers, modified evidence, stale approvals, duplicate operations, partial failures, detector failures, migration failures, and recovery failures.

Tests must run against the real SQLite engine where database behavior matters. Any ambiguity, missing evidence, integrity mismatch, or unrecognized failure must fail closed. Happy-path tests alone prove nothing.

The doctrine specifically requires protection of exact approved text from platform-returned text drift, approval invalidation after edits, rejection of fabricated IDs, hash-based modified-evidence detection, blocking promotion when raw evidence is missing, lifecycle enforcement, deliberate duplicate/replay behavior, timestamped metric provenance, distinct missing/withheld/error states, preservation of original mention evidence, no auto-promotion after classifier or detector failure, and migration/backup/restore proof that preserves identifiers, relationships, exact text, provenance, and evidence hashes.

The detailed minimum contract is governed by `05-implementation-planning/hammer-test-strategy.md`. M02 may strengthen it but may not silently weaken it.

## D017 — Main Assets/Evidence Directory Remains Non-Git Pending Governance

Accepted: 2026-07-19.

`C:\dev\x\discrepancy-desk` remains intentionally non-Git. Do not initialize it casually.

Before any future Git initialization, the owner must approve:

- third-party-data and personally identifiable information retention policy;
- whether raw API evidence may be committed or published;
- `.gitignore` rules covering credentials, `.env`, caches, generated files, and OS artifacts;
- evidence-size and binary-storage policy;
- secret scanning and pre-commit verification;
- whether evidence belongs in a private repository, external archive, or another governed store.

Approved brand assets and local evidence may remain in this directory under current manual controls. This decision does not authorize publication or remote synchronization.

## D018 — Claude Audit Accepted as PASS WITH CORRECTIONS

Accepted: 2026-07-19.

The independent read-only audit through M01 is accepted with verdict `PASS WITH CORRECTIONS`.

M01 remains accepted and M02 planning may continue. Physical schema, migrations, SQLite creation, ORM/application models, CRUD, and persistence implementation remain blocked until the accepted correction package and M02 exit gate are satisfied.

The root-level attempt-001 evidence will not be moved or rewritten. Its historical layout must be documented, and verification must use only the paths explicitly recorded in each `hashes.sha256` manifest.

The accepted audit review is recorded in `08-audits/claude-audit-through-m01-hammer-review-acceptance.md`.


## D019 — M02 Adversarial Persistence Matrix Baseline Approved

Accepted: 2026-07-19.

The owner approved `05-implementation-planning/m02-adversarial-test-matrix.md` as the governing M02 baseline.

The baseline contains 20 named persistence invariants and deliberate violation classes covering exact authored-text approval binding, approval freshness, lifecycle legality, external identity, canonical evidence, database/filesystem reconciliation, connection-level foreign-key enforcement, uniqueness and idempotency, transaction atomicity, concurrency and busy handling, audit integrity, metric observations, explicit unknown states, mention classification, platform isolation, dirty and interrupted migrations, backup and disposable restore, three-way post-restore reconciliation, and detector/classifier non-authority.

The matrix may be strengthened as the conceptual persistence contract and owner decisions are reconciled, but it may not be silently weakened, omitted, or treated as implementation authority.

This decision does not authorize SQL schema, migrations, SQLite creation, ORM models, CRUD code, application persistence, dashboard implementation, or platform writes.


## D020 — M02 Persistence Contract and UD-01 Through UD-12 Approved

Accepted: 2026-07-19.

The owner approved the smallest operational persistence contract, lifecycle/state model, and the Project Steward recommendations resolving UD-01 through UD-12.

The accepted decisions govern canonical external-file evidence, immutable exact-component revision binding, lifecycle reopening and publication history, append-only hash-chained audit events, operation-specific idempotency, explicit observation states, bounded third-party retention, manual-publication confirmation, immutable metric snapshots, SQLite WAL/foreign-key/busy-timeout/write-transaction settings, Alembic migration authority with dirty-state refusal, and encrypted three-generation backup with disposable restore proof.

The detailed accepted choices are recorded in `05-implementation-planning/m02-owner-approved-persistence-decisions.md`. UD-13 through UD-18 remain deferred within their stated later-milestone boundaries.

This approval remains planning authority only. It does not authorize SQL schema, migration files, SQLite creation, ORM models, CRUD code, application persistence, dashboard implementation, or platform writes.


## D021 — M02 Closed and Bounded M03 Package Authorized

Accepted: 2026-07-19.

The owner approved M02 closure and authorized preparation and execution of the bounded first M03 physical-design and persistence implementation package governed by `m02-migration-and-hammer-execution-plan.md`.

M03 is active. Physical execution remains blocked at the repository-initialization entry gate because D017 separately reserved third-party-data, raw-evidence publication, ignore, binary, secret, and remote-governance decisions. Authorization to execute M03 does not silently waive D017.

The next owner gate is the repository-governance package in `m03-work-package-a-repository-governance-and-bootstrap.md`.


## D022 — M03 Repository Governance Approved

Accepted: 2026-07-19.

The owner approved the repository-governance defaults in `05-implementation-planning/m03-work-package-a-repository-governance-and-bootstrap.md`.

The application directory `C:\dev\x\discrepancy-desk` may operate as a Git repository on `main` under these controls:

- raw evidence, runtime databases, backups, restores, virtual environments, credentials, and secret-bearing files remain outside Git;
- approved production brand assets may be tracked;
- synthetic, fictional, owned, or public-domain fixtures may be tracked;
- ordinary tracked files are limited to 10 MiB unless separately approved;
- Git LFS is not admitted yet;
- staged scope and secret review are required before commits;
- repository URL and visibility require owner confirmation before first remote push.

This decision admitted repository initialization and the already-authorized M03 implementation package. It did not authorize raw-evidence publication or credential access.


## D023 — Full Editorial Product Roadmap

Accepted by the owner on 2026-07-20.

The research report `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md` has been reviewed and retained as research rather than promoted wholesale into doctrine.

The coordinated accepted ruling is recorded in `05-implementation-planning/editorial-control-room-roadmap-ruling.md`, with the complete destination and gates recorded in `05-implementation-planning/implementation-roadmap.md`.

The accepted decisions are:

- records-first editorial operation;
- Archive, Docket, and Flash Release as editorial lanes rather than lifecycle states;
- rolling 90-day active scheduling with a first-class Unscheduled Reserve;
- Command Center focused on immediate operator action;
- anti-filler behavior allowing an empty schedule slot;
- FastAPI/Jinja web control room as functional contract harness;
- Tauri as product-grade operator surface;
- connected LLMs/agents limited to governed candidate operations;
- no direct agent database access;
- HTTP/service business operations as canonical, with optional thin MCP wrappers;
- Hermes treated as a replaceable restricted sibling runtime after the agent-neutral interface passes its gate;
- explicit milestone destinations for dossiers, capture, Release Watch, Truth Social, metrics learning, Replies, Articles, Assets, No Coincidences, Qdrant, and recovery.

The owner accepted this roadmap direction on 2026-07-20. This acceptance authorizes coordinated planning and milestone sequencing. M04 code implementation remains blocked until the exact M04 work package and added adversarial matrix are prepared and owner-reviewed.

## D024 — Multi-Account Vault and Qdrant Isolation Direction

Accepted by the owner on 2026-07-20 as the governing direction for M06 and M14 planning. Implementation remains subject to the named milestone gates and independent reviews.

Leading direction:

- each account that needs research memory receives its own physical Obsidian-compatible vault;
- account-specific interpretations, claims, conclusions, voice notes, and editorial memory remain isolated in that account's vault;
- shared canonical evidence may be referenced from more than one vault without sharing account-specific interpretation;
- the initial Qdrant proposal is one collection per account vault within a shared local Qdrant service;
- ordinary retrieval defaults to one active account and fails closed when account scope is absent;
- cross-account retrieval requires an explicit owner-admitted operation and permission;
- Qdrant remains derived, disposable, and deterministically rebuildable from governed canonical records and vaults.

Before M06 vault architecture or M14 Qdrant architecture receives owner acceptance, Claude AI must independently review the complete proposal against live repository truth and current product capabilities. The review must challenge cross-account leakage, duplicate truth stores, collection topology, filters, deletion/reindex behavior, backup/rebuild assumptions, and recovery. The independent review is advisory; final authority remains with the owner.

## D025 — M06 Governed Vault Architecture Direction

Accepted by the owner on 2026-07-20 for architecture synthesis. Implementation remains blocked pending independent synthesis review, owner acceptance of the reviewed synthesis, and a separately authorized M06-A work package.

The owner accepted M06-D01 through M06-D16 in `99-decisions/m06-owner-architecture-rulings.md`, including:

- hybrid SQLite/filesystem canonical authority;
- one universal governed ingestion envelope;
- physically separate account Vaults with governed imports only;
- generated read-only Markdown/HTML projections;
- separate research admission, truth/support, editorial-use, and exact publication authority;
- immutable correction and supersession lineage;
- versioned normalized JSON element packages;
- static structured LLM context-run contracts without live provider integration;
- human-only entity merge/split acceptance;
- deferral of first-class events and chronologies from the first M06-A package;
- narrow local parser scope;
- SQLite lexical search and deterministic chunk contract;
- per-account canonical backup and disposable restore proof;
- fully manual M06-A operation;
- Qdrant and graph work deferred;
- M06 split into M06-A Local Manual Vault and M06-B Bounded Static Webpage Retrieval.

The controlling synthesis candidate is `05-implementation-planning/m06-architecture-synthesis.md`.


## D026 — Accept Corrected M06 Synthesis and Authorize M06-A Planning

Accepted by the owner on 2026-07-21.

The owner accepted the corrected M06 architecture synthesis after independent review and correction commit:

```text
191fc8a — Apply M06 synthesis audit corrections
```

The owner directed:

```text
close AC-02
authorize preparation of the exact M06-A planning package
```

Consequences:

- `05-implementation-planning/m06-architecture-synthesis.md` is the accepted M06 architecture baseline;
- AC-02 is closed through `08-audits/ac02-m06-research-correction-closure.md`;
- preparation of the exact M06-A planning package is authorized under `05-implementation-planning/m06a-planning-authorization.md`;
- M06-A implementation remains blocked pending the completed planning package, independent review, owner review, and explicit implementation authorization;
- M06-B planning and implementation remain blocked.


## D027 — Resolve M06-A Vault Grain, SQLite Topology, and Retention Boundary

Accepted by the owner on 2026-07-21.

The owner resolved OD-1 through OD-3 for the M06-A corrected planning candidate.

Vault grain:

```text
one editorial/public brand identity per Vault
```

For the current project, one physical Vault represents **The Discrepancy Desk**. Platform-specific owned accounts remain separate central control-room records and may be explicitly bound to that Vault. An X account, future Truth Social account, or later platform-owned account does not automatically receive a separate physical Vault.

SQLite topology:

```text
central control-room SQLite database
+
one SQLite database per physical Vault
```

The central database remains authoritative for platform-owned accounts, exact revision approvals, publication records and attempts, metrics, and the central Vault registry and bindings. Each physical Vault owns its own SQLite authority database, audit chain, operation-key ledger, migration state, backup-generation history, and canonical research and dossier records. Cross-database operations are not atomic and must use correlation receipts and reconciliation.

Retention boundary:

```text
reject material requiring timed deletion in first M06-A
destructive purge authority deferred
```

M06-A may record rights and retention metadata and operation-specific restrictions, but timed-deletion-required or unknown-retention material must fail before canonical byte admission. M06-A gains no automatic deletion, deadline scheduler, destructive purge operation, or promise to delete later. Any future purge authority requires a separately planned, independently reviewed, owner-authorized package.

These rulings resolve the planning decisions only. The corrected M06-A planning candidate remains unaccepted, independent resolved-package review is still required, M06-A implementation remains blocked, and M06-B planning and implementation remain blocked.


## D028 — Accept Resolved M06-A Planning Baseline

Accepted by the owner on 2026-07-21.

The owner accepted the resolved M06-A planning package after:

- preservation of the original planning report and independent technical review;
- resolution of OD-1 through OD-3 in D027;
- preparation of the corrected core plan, parser-admission plan, and 108-invariant adversarial matrix;
- independent Claude review with verdict `M06-A RESOLVED PLAN READY WITH CORRECTIONS`;
- correction of R-01 through R-05 and explicit deferral of R-06 to the acceptance/navigation package;
- focused verification with verdict `M06-A FOCUSED CORRECTIONS VERIFIED — READY FOR OWNER ACCEPTANCE`.

The owner-accepted controlling M06-A planning baseline is:

```text
05-implementation-planning/m06a-local-manual-vault-canonical-plan.md
05-implementation-planning/m06a-parser-admission-plan.md
05-implementation-planning/m06a-adversarial-closure-matrix.md
08-audits/m06a-planning-correction-disposition.md
08-audits/m06a-planning-correction-closure.md
```

Consequences:

- the M06-A planning correction cycle is closed;
- D027 remains the governing Vault-grain, SQLite-topology, and retention-boundary decision;
- R-06 navigation and roadmap reconciliation is included in the same acceptance package;
- no parser is admitted;
- no matrix test is claimed to exist or to have executed;
- no application code, migration, database, Vault, dependency, parser, or runtime implementation is authorized;
- M06-A implementation remains blocked pending a separate explicit owner authorization;
- M06-B planning and implementation remain blocked.


## D029 — Clarify Editorial Anomaly Archive Direction

Accepted by the owner on 2026-07-22.

The owner established the governing editorial rule:

```text
Record the crazy.
Label what it is.
Connect what emerges.
Never fabricate the file.
```

The controlling doctrine is `00-doctrine/editorial-anomaly-archive-direction.md`.

Consequences:

- The Discrepancy Desk may archive and publish clearly labeled speculative, disputed, conspiratorial, folkloric, anomalous, implausible, and unresolved material.
- Editorial use does not require proving or disproving every claim, settling every allegation, or completing a full investigation before an interesting fragment may be used.
- Source statements, public-record contents, allegations, secondhand reports, repeated online claims, folklore, Desk observations or inferences, and unknowns remain distinguishable where the distinction matters.
- A source or provenance link proves where material came from; it does not automatically prove the underlying proposition.
- Connections and recurring patterns may be editorially useful without proving causation, intent, or a complete theory.
- Truth/support assessment remains a separate optional authority layer. A verified human operation is required when a final support assessment is recorded, but such an assessment is not a universal prerequisite for storage, dossier membership, editorial usefulness, or publication.
- Editorial-use rulings may allow unresolved or speculative material with attribution, labels, warnings, or required framing.
- The fictional agency and Quinton Clearance remain presentation devices and do not authorize invented sources, documents, quotations, screenshots, evidence, or corroboration.
- The database and Vault are an archive, memory, provenance, relationship, editorial-support, publication-history, and metrics-history system—not a formal fact-checking service or truth machine.
- Provenance, immutable originals, source/Desk-inference separation, correction lineage, audit history, human approval, exact-content binding, manual publishing, and anti-fabrication safeguards remain unchanged.
- Roadmap sequencing, the M06 object model, source-admission policy, manual-publication workflow, and metrics workflow are unchanged by this clarification.

This decision concerns editorial identity only. It grants no new platform, automation, implementation, parser, provider, publication, deletion, or destructive-migration authority.


## D030 — Authorize M06-A Phase 1 Implementation

Accepted by the owner on 2026-07-22.

The owner authorized the exact bounded M06-A Phase 1 implementation package identified in the owner-accepted canonical plan:

```text
identity
actors
central and per-Vault migration foundations
Vault registry and platform-account bindings
safe Vault create/open routing
selected-Vault health
cross-database receipts and reconciliation
Phase 1 adversarial tests and evidence runner mappings
```

Consequences:

- application changes are authorized only for M06-A Phase 1;
- M06-A Phases 2 through 6 remain blocked pending separate phase authorization;
- no intake artifacts, parser implementation or admission, search, chunks, projections, assertions, dossiers, backup execution, live LLM integration, M06-B network work, Qdrant, graph work, monitoring, provider calls, automated publishing, or destructive purge authority is granted;
- Phase 1 must preserve existing M00–M05 regression authority and produce real SQLite/filesystem adversarial evidence;
- Phase 1 implementation remains subject to independent review, correction, commit-bound evidence, and explicit owner closure before Phase 2;
- planning acceptance D028 remains the governing technical baseline, as clarified by D029 and D031.


## D031 — Tauri Is the Sole Supported Operator Interface

Accepted by the owner on 2026-07-22.

The owner directed the project to stop spending implementation time on parallel browser-interface development and focus on the packaged Tauri desktop client.

The governing product-surface boundary is:

```text
Tauri desktop client
  sole supported human operator interface

FastAPI loopback backend
  retained governed local API/service host required by Tauri

legacy FastAPI/Jinja pages
  frozen historical regression scaffolding only
  no new product features
  no browser/Tauri feature-parity requirement
```

Consequences:

- new M06 and later operator capabilities are implemented through Tauri consuming the token-gated loopback API and governed service operations;
- FastAPI remains required for local desktop API routing, health, authority enforcement, and packaged sidecar operation;
- existing M03/M04 Jinja routes and tests may remain where useful as inherited regression coverage, but they are not a supported daily operator product and receive no new feature parity work;
- new browser forms, Vault pages, browser file-intake workflows, browser search/dossier/backup views, and browser/Tauri parity tests are removed from M06-A scope;
- security and authority proof must instead establish `Tauri client → authenticated loopback API → governed service → persistence` with no direct frontend database access or independent frontend authority decisions;
- roadmap sequencing, M06 object model, human approval, manual publication, provenance, audit, and recovery boundaries are unchanged;
- this ruling does not remove FastAPI, authorize remote API exposure, authorize autonomous behavior, or weaken existing regression tests.


## D032 — Defer Independent Phase 1 Audit Without Blocking the Commit Path

Accepted by the owner on 2026-07-22.

Claude MCP/client failures and Claude subscription usage limits prevented the planned independent implementation review from completing before the M06-A Phase 1 commit gate. The owner directed the project to continue rather than leave the validated implementation indefinitely uncommitted.

Consequences:

- the Project Steward may commit, produce clean commit-bound evidence for, and push the exact authorized M06-A Phase 1 implementation;
- the absence of the immediate Claude review is recorded explicitly and must not be represented as an independent-review pass;
- the independent Claude implementation audit is deferred, not permanently waived;
- the audit must review the committed Phase 1 implementation and any evidence-binding follow-up when Claude access and usage permit;
- any later Claude findings must be corrected and evidence-bound before M06-A Phase 2 may be authorized;
- Phase 2 and all later M06-A phases remain blocked;
- parser admission, intake/artifact/search/dossier/projection/backup capability, M06-B, network retrieval, providers, monitoring, live LLM integration, Qdrant, graph work, destructive purge, and automated publishing remain blocked;
- this decision changes review timing only and grants no additional product, schema, platform, provider, automation, or publication authority.


## D033 — Accept M06-A Phase 1 Closure and Open Phase 2 Package Review

Accepted by the owner on 2026-07-22.

After the committed Phase 1 implementation, clean commit-bound evidence, deferred independent audit, Project Steward verification, and disposition of the two non-blocking Low observations, the owner directed the project to proceed.

Consequences:

- M06-A Phase 1 is owner-accepted and closed;
- the independent verdict `M06-A PHASE 1 INDEPENDENTLY VERIFIED` satisfies the obligation deferred under D032;
- no Critical, High, or Medium findings require correction;
- L-01 reparse-chain recheck and L-02 explicit audit-column selection remain bounded hardening candidates and do not reopen Phase 1;
- the mutable `latest` evidence-file observation becomes a required evidence-hardening item in the next implementation package;
- preparation, review, validation, commit, and push of the exact Phase 2 implementation package are authorized as documentation work;
- Phase 2 application code, `V0002`, intake, artifact storage, backup, and restore remain blocked until the owner explicitly accepts the exact Phase 2 package;
- parsers and Phase 3+, M06-B, network retrieval, providers, monitoring, live LLM integration, Qdrant, graph work, purge, and automated publishing remain blocked;
- this decision closes Phase 1 and opens the Phase 2 authorization review only; it is not Phase 2 implementation authority.


## D034 — Authorize M06-A Phase 2 Implementation

Accepted by the owner on 2026-07-22.

The owner explicitly approved `05-implementation-planning/m06a-phase2-exact-implementation-package.md` and its exact 33-invariant Phase 2 profile for implementation.

Consequences:

- application implementation is authorized only for the exact Phase 2 observation, acquisition, immutable artifact, rights/retention, foundational backup/restore, Tauri-only operator, evidence-hardening, and bounded Phase 1 hardening surface defined in the accepted package;
- Vault migration `V0002_ingestion_artifacts_policy_and_backup.py` is authorized;
- the exact Phase 2 profile consists of 28 Phase 2 closure invariants plus 5 inherited authority/recovery regressions;
- the legacy and Phase 1 suites must remain independently green;
- the 64 MiB server-owned intake ceiling is accepted for this package;
- no parser implementation or admission is authorized;
- no Phase 3 through Phase 6 implementation is authorized;
- M06-B, network retrieval, providers, monitoring, live LLM integration, Qdrant, graph work, purge, and automated publishing remain blocked;
- implementation must receive clean commit-bound evidence, independent Claude review, finding disposition, and explicit owner closure before Phase 2 closes or Phase 3 may be considered;
- application paths outside the exact package surface require an explicit owner-approved amendment.


## D035 — Accept Corrected M06-A Phase 2 Closure and Defer Optional Spot Review

Accepted by the owner on 2026-07-22.

The owner accepted the verified five-path correction package, deferred the optional second Claude spot review because Claude subscription usage limits were reached, and closed M06-A Phase 2.

Governing facts:

- the required full independent Phase 2 implementation audit completed against application commit `eaf7b5dcd46c61654ec0320e9db19aec0a3fe962` with verdict `M06-A PHASE 2 INDEPENDENTLY VERIFIED`;
- the audit reported no Critical, High, or Medium findings and concluded that D034's independent-review obligation was satisfied;
- L-03 and L-04 were independently reproduced by the Project Steward;
- the owner authorized an exact five-path correction package;
- application correction commit `1e8cba8f0ef88c2e05b9617956872be26753993e` corrected both findings and was pushed to `origin/main`;
- the correction produced 183 passing Python tests, Phase 2 33/33, Phase 1 31/31, legacy 31/31 plus the approved HT-14 deferral, successful native/package validation, and clean commit-bound evidence;
- the second Claude spot review applied only to the already-corrected Low findings and was optional additional assurance, not the required independent Phase 2 audit.

Consequences:

- M06-A Phase 2 is owner-accepted and closed;
- L-03 and L-04 are resolved and do not remain open hardening backlog items;
- the optional correction spot review is deferred and must not be represented as completed;
- the deferred spot review may be performed later but does not block Phase 2 closure;
- Phase 3 remains unauthorized and requires a separately prepared exact package, adversarial profile, owner acceptance, implementation evidence, and review gate;
- parser implementation or admission remains blocked;
- M06-B, network retrieval, providers, monitoring, live LLM integration, Qdrant, graph work, purge, cross-Vault transfer execution, and automated publishing remain blocked;
- this decision closes Phase 2 only and grants no later-phase authority.


## D036 — Authorize Exact M06-A Phase 3A Parser Framework and Plain-Text Under-Test Implementation

**Date:** 2026-07-23
**Status:** Accepted
**Owner ruling:** The owner approved the exact `05-implementation-planning/m06a-phase3a-exact-implementation-package.md` package and its exact 35-invariant profile.

D036 authorizes implementation of only:

- the common M06-A parser framework;
- Vault migration `V0003_parser_packages_and_documents.py`;
- parser definitions, immutable configuration/admission versions, execution receipts, normalized packages, document versions, elements, and regions;
- normalized-package schema `m06a.normalized-package.v1` and deterministic canonical JSON handling;
- the fixed-entrypoint short-lived parser worker and exact denial controls;
- `m06a.text.v1` as an `under_test` candidate only;
- synthetic fixtures, source and packaged-sidecar resources, exact tests, the exact 35-invariant profile, and commit-bound evidence;
- the read-only parser status surface through the supported Tauri/loopback path;
- exact package backup/restore and packaged no-egress proof.

D036 does **not** authorize:

- any production `owner_admitted` parser state;
- canonical parsing of real Vault material;
- an admission, execution, suspension, revocation, or parser-configuration mutation endpoint or UI;
- SRT, VTT, JSON, Markdown, XML, HTML, PDF, OCR, browser rendering, or media work;
- Phase 3B, Phases 4 through 6, or M06-B;
- FTS, chunks, projections, assertions, entities, dossiers, context runs, providers, network retrieval, monitoring, live LLM use, Qdrant, graph work, purge, or publication automation;
- any dependency change to `pyproject.toml` or `uv.lock`.

Implementation must stop and return to owner review if any exact package stop condition is reached. Successful implementation or testing does not admit `m06a.text.v1`; parser admission remains a separate future owner gate.


## D037 — Authorize M06-A Phase 3A-C1 Independent Review Corrections

**Date:** 2026-07-23
**Status:** Accepted

The owner authorized preparation and implementation of the exact `05-implementation-planning/m06a-phase3a-c1-independent-review-correction-package.md` package after independent static review and Project-Steward reproduction of the Phase 3A findings.

D037 authorizes only:

- bounded Vault migration `V0004_phase3a_c1_lineage_and_identity.py` without editing V0001 through V0003;
- restoration of current-head inherited regression proof while preserving explicitly historical V0002 fixtures;
- worker filesystem, process, and packaged security-boundary hardening;
- independently reconciled complete locator coverage;
- immutable tuple-versioned parser-definition identity and exact execution/package/document lineage;
- fail-closed packaged implementation and dependency-lock verification;
- correction of governing security-boundary wording;
- complete clean commit-bound evidence regeneration and finding disposition.

D037 does **not** authorize:

- any production `owner_admitted` parser;
- canonical parsing;
- parser execution, admission, or mutation endpoints or UI;
- Phase 3B, Phases 4 through 6, or M06-B;
- additional parser formats;
- providers, network retrieval, monitoring, live LLM use, Qdrant, graph work, purge, or publication automation;
- dependency changes, central migrations, or edits to prior Vault migrations.

Phase 3A remains open. Successful correction requires clean evidence, synchronized governance records, independent finding disposition, and explicit owner closure. Parser admission remains a separate future owner gate.


## D038 — Accept M06-A Phase 3A Closure

**Date:** 2026-07-23
**Status:** Accepted

The owner accepted the M06-A Phase 3A independent review disposition and the corrected application implementation at `1337b5ac450ae82664aa1ad9667a85af41c4351e`.

Governing facts:

- D036 authorized the exact Phase 3A parser-framework package and 35-invariant profile;
- application commit `251b3ca841af46e63485b9ab5bf292cbae55a418` implemented the original package;
- independent static review returned `M06-A PHASE 3A REQUIRES CORRECTION`;
- D037 authorized the exact Phase 3A-C1 correction package;
- application commit `1337b5ac450ae82664aa1ad9667a85af41c4351e` corrected every reproduced material finding;
- clean commit-bound evidence records 215 passing Python tests, 55 focused tests, Phase 3A 35/35 invariants with 53/53 mapped tests, Phase 2 33/33, Phase 1 31/31, legacy 31/31 executed plus the approved HT-14 deferral, Tauri 4/4, Rust 3/3, and migration heads `0005`/`V0004`;
- the corrected product state remains `under_test`, contains zero production `owner_admitted` parser records, and exposes no canonical parser by default.

Consequences:

- M06-A Phase 3A is owner-accepted and closed;
- the independent-review and correction obligations under D036 and D037 are satisfied;
- `m06a.text.v1` remains unadmitted and unavailable for canonical parsing;
- parser admission remains a separate future owner gate;
- Phase 3B, Phases 4 through 6, and M06-B remain unauthorized;
- additional parser formats, providers, network retrieval, monitoring, live LLM integration, Qdrant, graph work, purge, cross-Vault transfer execution, and publication automation remain blocked;
- D038 closes Phase 3A only and grants no implementation authority for the next phase.


## D039 — Authorize `m06a.text.v1` Owner Admission and Canonical Execution

**Date:** 2026-07-23
**Status:** Accepted

The owner approved the exact `05-implementation-planning/m06a-text-v1-owner-admission-and-canonical-execution-package.md` package and its exact 28-invariant profile.

D039 authorizes only:

- correction of the supported Tauri Vault gate and migration text from stale `V0003` assumptions to the live `V0004` head;
- one explicit human-only, per-physical-Vault owner-admission ceremony for the exact `m06a.text.v1` tuple and exact evidence manifest recorded in the package;
- creation of one immutable `owner_admitted` successor to the exact current `under_test` admission row, without updating or deleting the predecessor;
- human-triggered canonical plain-text parsing of one already admitted, retention-eligible, same-Vault artifact at a time;
- append-only parser execution, deterministic package storage/reuse, initial document version, elements, regions, audit, idempotency, backup/restore, reconciliation, packaged-sidecar, and exact 28-invariant evidence proof;
- the exact application change surface and validation contract stated in the accepted package.

D039 does **not** authorize:

- automatic, startup, migration, package-install, bulk, cross-Vault, or every-Vault parser admission;
- parser configuration editing or parser lifecycle controls beyond the exact admission successor;
- automatic, background, scheduled, watcher, agent, provider, or post-intake parsing;
- SRT, VTT, JSON, Markdown, RSS/Atom, HTML, PDF, OCR, FTS, chunks, projections, assertions, dossiers, or later parser modules;
- Phase 3B, Phases 4 through 6, or M06-B;
- providers, network retrieval, monitoring, live LLM integration, Qdrant, graph work, purge, or publication automation;
- any migration, dependency, parser implementation, parser contract, resource-manifest, package-schema, or security-profile change.

Implementation must stop and return to owner review if the exact parser tuple or evidence hashes change, if V0004 proves insufficient, if a forbidden path must change, or if safe canonical execution requires document supersession authority. Successful implementation does not admit the parser in every Vault; each physical Vault still requires its own explicit human admission action. Independent review, finding disposition, clean evidence, and explicit owner closure remain required.

## D040 — Defer D039 Independent Review and Authorize `m06a.srt.v1` Under-Test Candidate

**Date:** 2026-07-23
**Status:** Accepted

Because Claude AI usage limits currently prevent the planned D039 independent review, the owner authorized continued bounded work without treating that review as waived or satisfied.

D040 records that:

- D039 remains implemented and clean-evidence-bound at `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9`;
- D039 independent review, finding disposition, and explicit owner closure remain open obligations;
- review may be attempted later when independent-review capacity returns;
- later findings must be reproduced against live repository truth and corrected or explicitly dispositioned;
- no document may describe D039 as independently verified before that review occurs.

D040 also authorizes preparation and implementation of the exact `05-implementation-planning/m06a-srt-v1-under-test-candidate-package.md` package and its exact 24-invariant profile.

D040 authorizes only:

- parser-scoped immutable SRT resources that do not change the exact D039 plain-text tuple;
- one strict internal `m06a.srt.v1` parser implementation;
- a separate fixed SRT source/packaged worker entrypoint using the proven denial controls;
- synthetic fixtures, deterministic candidate/package proof, and exact source/packaged receipts;
- immutable `under_test` SRT candidate installation in fresh V0004 Vaults;
- read-only SRT candidate status;
- clean commit-bound evidence for the exact 24-invariant profile and all inherited regressions.

D040 does **not** authorize:

- SRT owner admission or canonical SRT execution;
- existing-Vault SRT retrofit;
- automatic, background, bulk, scheduled, watcher, agent, or provider parsing;
- VTT, JSON, Markdown, RSS/Atom, HTML, PDF, OCR, or Phase 3B;
- Phase 4 through 6 or M06-B;
- dependency or migration changes;
- changes to the exact plain-text parser implementation, contract, worker, resource manifest, config, schema, admission material, or canonical route behavior;
- providers, network retrieval, monitoring, live LLM integration, Qdrant, graph work, purge, or publication automation;
- closure or waiver of D039 or any later independent-review obligation.

Implementation must stop and return to owner review if the exact D039 tuple would change, an unlisted path is needed, a migration/dependency is required, the plain-text worker/contract must change, or safe SRT structure cannot be represented inside the exact parser-scoped package.

## D041 — Replace Unstable Claude Review with Project-Steward Self-Review and Authorize D040-C1

**Date:** 2026-07-23
**Status:** Accepted

Repeated Claude AI review sessions crashed, reset, and consumed usage without producing a complete verdict. The owner directed: `We need to do this our self.`

D041 records that:

- the Project Steward may perform the current D039/D040 adversarial reviews directly against live repository truth;
- those records must be labeled Project-Steward self-review and must not be represented as independent;
- D039 self-review found no material finding and preserves owner closure as a separate decision;
- D040 self-review reproduced two Medium closure-blocking findings;
- the exact `05-implementation-planning/m06a-srt-v1-c1-self-review-correction-package.md` package and exact eight-invariant correction profile are authorized for implementation.

The authorized correction is limited to:

- full exact D040 packaged resource-tuple validation;
- rejection of coherent or partial config/schema/manifest/dependency/implementation tamper;
- safe per-parser status isolation so SRT resource failure cannot hide healthy D039 status;
- exact focused, packaged, inherited, and clean commit-bound evidence.

D041 does **not** authorize SRT owner admission, canonical SRT execution, existing-Vault retrofit, VTT, JSON, Phase 3B, later M06 phases, providers, agents, live LLM integration, Qdrant, graph work, purge, or publication automation.

## D042 — Accept D041 Disposition and Close D039 and D040

**Date:** 2026-07-23
**Status:** Accepted

The owner accepted the D041 Project-Steward adversarial self-review and correction disposition.

D042 closes:

- D039 — `m06a.text.v1` Owner Admission and Canonical Execution; and
- D040 — the `m06a.srt.v1` Under-Test Candidate, as corrected at application commit `6a8082253a52a601291efaf3ed85ee411b04be20`.

The owner explicitly acknowledged that the D039 and D040 reviews were Project-Steward self-reviews and are not represented as independent reviews. The previously deferred independent-review obligation is therefore no longer an open closure gate for these two packages, but no independent-review claim may be made.

D042 does **not** automatically admit `m06a.text.v1` in any Vault, admit SRT, authorize canonical SRT execution, authorize existing-Vault SRT retrofit, authorize automatic or bulk parsing, authorize VTT or JSON, authorize Phase 3B or later M06-A phases, authorize M06-B, or authorize providers, agents, live LLM integration, Qdrant, graph work, purge, or publication automation.

The authoritative closure record is `08-audits/m06a-d039-d040-owner-closure.md`.

## D043 — Accept Governed Discrepancy-Detection Planning Direction

**Date:** 2026-07-23
**Status:** Accepted

The owner directed the Project Steward to produce and preserve the revised governed discrepancy-detection
proposal, integrate it into the development roadmap, and complete any documentation work needed now so the
future No Coincidences capability is not forgotten.

D043 accepts the planning direction at:

```text
05-implementation-planning/m13-governed-discrepancy-detection-planning-direction.md
```

D043 adopts for future planning:

- governed corpus/coverage manifests with explicit searched denominators and known gaps;
- stable assertion comparison dimensions in addition to assertion types;
- one generic immutable candidate/successor lifecycle;
- separate epistemic and editorial dispositions with blind first-pass epistemic review;
- exact candidate fingerprints and delta-explained successors;
- detector-specific admission, suspension, and retirement;
- structured detector calibration captured from the beginning but advisory-only;
- a bounded Tier 1 deterministic pilot with reviewer-time, noise, yield, and stop criteria;
- initial governed expectation contracts limited to annual/periodic continuation and page/document continuity;
- early deterministic `common_origin` candidates while broader claim mutation remains deferred.

D043 reserves planning dependencies for M06 Phase 4 corpus/coverage shape and M06 Phase 5 assertion
comparison dimensions. It places any future rich competing-explanations workbench in a separately admitted
M12 subpackage and keeps full assertion-level detection in M13.

D043 explicitly defers permutation/null models, paraphrase-based mutation chains, automated threshold
tuning, broad public dismissal-ledger publication, semantic/graph assistance, background execution, and
cross-Vault detection behind separate future gates.

D043 authorizes documentation and roadmap synchronization only. It does **not** authorize application
changes, migrations, dependencies, schema, Tier 1 implementation, detector admission, Phase 4, Phase 5,
M12 implementation, M13 entry, VTT or JSON implementation, Phase 3B, M06-B, providers, agents, live LLM
integration, Qdrant, graph work, purge, or publication automation.

After this documentation package, active development returns to the M06-A parser sequence. The next bounded
action is preparation of the exact `m06a.vtt.v1` under-test candidate package for owner review; VTT
implementation is not authorized by D043.

## D044 — Prepare Exact `m06a.vtt.v1` Under-Test Candidate Package

**Date:** 2026-07-23
**Status:** Accepted

After D042 closed D039 and corrected D040 and D043 preserved the future governed discrepancy-detection
direction, the owner directed the Project Steward to continue the active M06-A parser sequence.

D044 authorizes documentation-only preparation of:

```text
05-implementation-planning/m06a-vtt-v1-under-test-candidate-package.md
```

The prepared package defines:

- a strict internal UTF-8/UTF-8-BOM WebVTT subset;
- exact `WEBVTT` header framing and blank separation;
- optional cue identifiers and strict timestamp ordering;
- validated inert cue settings;
- inert NOTE preservation;
- terminal rejection of STYLE, REGION, region settings, timeline mapping, rendering, CSS, and DOM semantics;
- parser-scoped full-tuple resource verification from the first implementation;
- a neutral parser-status aggregator that does not extend SRT-owned coupling;
- fresh-V0004 `under_test` installation only;
- an exact changed-path surface and 28-invariant profile;
- exact validation, review, stop, and owner-closure gates.

D044 does **not** authorize application changes, VTT implementation, VTT admission, canonical VTT parsing,
existing-Vault retrofit, automatic or bulk parsing, SRT authority changes, JSON, Phase 3B, Phase 4 through 6,
M06-B, providers, agents, live LLM integration, Qdrant, graph work, purge, or publication automation.

The next bounded action is owner review of the exact VTT package. Implementation requires a separate explicit
owner decision.
