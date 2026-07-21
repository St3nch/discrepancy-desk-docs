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
