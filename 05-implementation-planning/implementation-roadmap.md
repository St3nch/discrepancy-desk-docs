# Implementation Roadmap — The Discrepancy Desk

## Purpose

This roadmap is the execution spine for the complete product. It preserves the full destination while enforcing entry, authority, validation, documentation, and owner-acceptance gates.

The roadmap controls sequence. Milestone files control bounded execution. Doctrine and accepted decisions control behavior.

## Steward Execution Rule

Plan broadly, implement substantial coherent capabilities, validate hard, and keep moving.

A work package should be large enough to deliver a complete operator capability and small enough to test, audit, and recover safely. Do not split ordinary related implementation into approval theater. Stop only for a real owner decision, new authority, new provider/platform, destructive action, unresolved contract, failed validation, or audit finding.

## Mandatory Milestone Documentation Rule

Every milestone entry in this roadmap must reference:

1. its canonical milestone file;
2. every governing doctrine, decision, architecture, research, audit, and prior-milestone handoff document required to execute it safely;
3. any proposed decision that must be accepted before implementation;
4. any external-policy or provider research that must be refreshed at entry;
5. the evidence or completion record that closes the preceding dependency.

The milestone file must repeat and may strengthen its own **Required Reading** list. It may not omit any roadmap-listed governing document. The roadmap and milestone file must be updated together whenever scope, order, gates, or governing documents change.

Unless a separate exact completion-record document is explicitly named, the canonical milestone file contains that milestone's completion record. References to a milestone completion record therefore use the exact canonical milestone-file path. Future governing documents must be assigned an exact canonical path and the milestone gate by which they must be created.

A Project Steward may not begin milestone planning or implementation from the milestone title, summary, chat history, or this roadmap alone. It must read the milestone file and every document listed under that milestone's **Governing documents**. Missing, stale, contradictory, renamed, or superseded references are a blocking defect and must be corrected before work proceeds.

This rule exists to prevent downstream milestones from drifting into unstated assumptions or "vibes" as project context grows.

## Non-Negotiable Boundaries

- AI drafts; the human clears.
- No autonomous posting, replying, liking, following, reposting, or direct messaging.
- No agent or LLM may access the database directly.
- External agents use governed business operations, never SQL/table operations.
- Accepted truth, approvals, publication authority, evidence deletion, and account ownership remain human-controlled.
- The web control room is a functional contract harness; Tauri is the product-grade operator surface.
- All new operational contracts from M04 onward must be multi-account-capable even when the first operator experience defaults to one active account.
- Major architecture proposals involving vault truth boundaries, semantic retrieval, agent authority, or other high-risk cross-system behavior require an independent Claude AI review before owner acceptance. The review is advisory and cannot replace owner authority.
- A milestone closes only after its exit gate, tests, evidence, docs, clean diff, and explicit owner acceptance.

# Full Milestone Sequence

## M00 — Planning Foundation and Repository Baseline — COMPLETE

Established doctrine, brand, repositories, platform boundaries, roadmap discipline, and steward synchronization.

**Milestone file**

- `05-implementation-planning/milestone-00-foundation.md`

**Governing documents**

- `PROJECT_BRIEF.md`
- `README.md`
- `LLM_MAP.md`
- `00-doctrine/operating-doctrine.md`
- `00-doctrine/human-approval-policy.md`
- `00-doctrine/account-rules-and-boundaries.md`
- `01-brand/brand-identity.md`
- `01-brand/quinton-clearance-persona.md`
- `02-product/product-overview.md`
- `03-system-design/architecture-overview.md`
- `99-decisions/decision-log.md`

**Required outcome**

A fresh Project Steward can reconstruct the project, authority boundaries, repository split, and next milestone from repository truth.

## M01 — X Identity, Policy, and Controlled API Probe — COMPLETE

Established the X account, developer boundary, preserved bounded read-only probe evidence, and recorded real payload/cost findings.

**Milestone file**

- `05-implementation-planning/milestone-01-api-probe.md`

**Governing documents**

- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/x-api-probe-plan.md`
- `04-platform-rules/automation-boundaries.md`
- `06-research/x-policy-api-verification-2026-07-19.md`
- `05-implementation-planning/m01-work-package-b-developer-cost-boundary.md`
- `05-implementation-planning/m01-work-package-b-redacted-configuration-record.md`
- `05-implementation-planning/m01-work-package-c-controlled-probe-procedure.md`
- `05-implementation-planning/m01-work-package-d-probe-analysis-and-m02-handoff.md`
- `99-decisions/research-log.md`
- `99-decisions/decision-log.md`

**Required outcome**

Bounded, preserved read-only X evidence and a documented handoff into persistence planning.

## M02 — Persistence Contract and Hammer-Test Plan — COMPLETE

Approved lifecycle, exact-text authority, evidence, idempotency, audit, migration, recovery, and adversarial test contracts before implementation.

**Milestone file**

- `05-implementation-planning/milestone-02-persistence-contract.md`

**Governing documents**

- `03-system-design/data-model-planning.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/m02-adversarial-test-matrix.md`
- `05-implementation-planning/m02-operational-persistence-contract.md`
- `05-implementation-planning/m02-lifecycle-state-model.md`
- `05-implementation-planning/m02-owner-approved-persistence-decisions.md`
- `05-implementation-planning/m02-unresolved-decision-register.md`
- `05-implementation-planning/m02-migration-and-hammer-execution-plan.md`
- `08-audits/claude-audit-through-m01-hammer-review-acceptance.md`
- `99-decisions/decision-log.md`

**Required outcome**

An owner-approved persistence contract and adversarial baseline that admits physical implementation without schema-from-vibes.

## M03 — Governed Local Control Room Foundation — COMPLETE

Implemented SQLite/Alembic persistence, governed service operations, exact approvals, publication reconciliation, successor/replacement lineage, recovery protections, thin FastAPI/Jinja UI, and executable hammer evidence.

**Milestone file**

- `05-implementation-planning/milestone-03-local-dashboard.md`

**Governing documents**

- `03-system-design/data-model-planning.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/m02-adversarial-test-matrix.md`
- `05-implementation-planning/m02-operational-persistence-contract.md`
- `05-implementation-planning/m02-lifecycle-state-model.md`
- `05-implementation-planning/m02-owner-approved-persistence-decisions.md`
- `05-implementation-planning/m02-unresolved-decision-register.md`
- `05-implementation-planning/m02-migration-and-hammer-execution-plan.md`
- `08-audits/claude-audit-through-m01-hammer-review-acceptance.md`
- `99-decisions/decision-log.md`
- `05-implementation-planning/m03-work-package-a-repository-governance-and-bootstrap.md`
- `05-implementation-planning/m03-work-package-b-initial-persistence-implementation-return.md`
- `05-implementation-planning/m03-work-package-c-idempotency-concurrency-and-reconciliation-return.md`
- `05-implementation-planning/m03-work-package-d-guarded-migrations-encrypted-archives-and-evidence-return.md`
- `05-implementation-planning/m03-work-package-e-recovery-authority-mismatch-and-evidence-return.md`
- `05-implementation-planning/m03-work-package-f-minimal-operator-service-loop-return.md`
- `05-implementation-planning/m03-work-package-g-thin-local-control-room-return.md`
- `05-implementation-planning/m03-work-package-h-successor-and-replacement-lineage-return.md`
- `05-implementation-planning/m03-work-package-i-hammer-closure-review-return.md`
- application repo: `docs/ht-coverage-ledger.md`
- application repo: `docs/m03-exit-gate-review.md`
- application repo: `scripts/run_ht_evidence.py`

**Required outcome**

The technical closure evidence remains 54 tests passed, Ruff clean, 19/19 admitted HT invariants passed, and HT-14 deferred by approved scope. The owner accepted and closed M03 on 2026-07-20.

## M04 — Editorial Control Room and X Operating Workflow

Build the substantial functional web operating system: Command Center, rolling 90-day scheduler, Unscheduled Reserve, Archive/Docket/Flash Release lanes, WIP pipeline, Ready-to-Post, Need-a-Post, exact review, manual X publication/reconciliation, and manual metrics.

**Milestone file**

- `05-implementation-planning/milestone-04-x-operations-and-metrics.md`

**Governing documents**

- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- `02-product/product-overview.md`
- `02-product/workflow-overview.md`
- `02-product/module-map.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/multi-account-model.md`
- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/automation-boundaries.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/m03-work-package-i-hammer-closure-review-return.md`
- application repo: `docs/m03-exit-gate-review.md`
- accepted decisions D023 and D024 in `99-decisions/decision-log.md`

**Required outcome**

The owner can plan and operate a realistic editorial week end-to-end without SQL, raw IDs, or ambiguous authority. Multi-account isolation is proven with at least two synthetic accounts even if the UI defaults to one active account.

## M05 — Tauri Desktop Foundation and Product Parity

Build the real desktop product around proven M04 contracts: native shell, navigation, panes, command palette, file handling, secure local credentials, notifications, health, and core workflow parity.

**Milestone file**

- `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`

**Governing documents**

- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/multi-account-model.md`
- `02-product/workflow-overview.md`
- M04 milestone completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- application repo: `docs/m04-service-api-contract.md` (must be created before M05 entry)
- accepted D023 in `99-decisions/decision-log.md`

**Required outcome**

The complete M04 operator loop works through Tauri without direct database access, preserving account scope and authority boundaries.

## M06 — Records, Dossiers, and Anomaly Vault

Prepare governed research memory under accepted D024: one separate vault per account that needs research memory, with topic dossiers, chronology, entities, typed assertions, unresolved questions, related filings, and Obsidian-compatible records without creating a second uncontrolled truth store. Implementation remains subject to the M06 architecture review and entry gate.

**Milestone file**

- `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`

**Governing documents**

- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- M05 milestone completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- accepted D024 in `99-decisions/decision-log.md`
- independent Claude AI vault-architecture review required at `08-audits/m06-vault-architecture-review.md` (must be created before M06 entry)

**Required outcome**

Every durable assertion has type, provenance, evidence linkage, and human promotion/correction behavior; vault/account isolation is proven; the complete proposal receives independent Claude AI review before owner acceptance.

## M07 — Human-Triggered X Capture Helper

Reduce intake friction with a narrow, human-triggered X capture helper or record a deliberate decision to retain manual URL/note intake.

**Milestone file**

- `05-implementation-planning/milestone-07-human-triggered-x-capture.md`

**Governing documents**

- `05-implementation-planning/chrome-extension-plan.md`
- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/automation-boundaries.md`
- `06-research/x-policy-api-verification-2026-07-19.md` or its current replacement
- `03-system-design/multi-account-model.md`
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- `99-decisions/decision-log.md`

**Required outcome**

Capture is attributable, explicit, policy-reviewed, account-scoped, non-recurring, and non-scraping—or manual URL/note intake is deliberately retained.

## M08 — Agent-Neutral Governed Interface

Create the versioned HTTP/service business-operation surface, optional thin MCP wrapper, agent identities/scopes, provenance, idempotency, budgets, revocation, and mock-agent hammer tests.

**Milestone file**

- `05-implementation-planning/milestone-08-agent-neutral-interface.md`

**Governing documents**

- `00-doctrine/human-approval-policy.md`
- `00-doctrine/account-rules-and-boundaries.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- accepted D023/D024 and later accepted agent-authority decisions in `99-decisions/decision-log.md`
- independent Claude AI agent-interface architecture review at `08-audits/m08-agent-interface-architecture-review.md` (must be created before M08 entry)

**Required outcome**

A mock agent can submit account-scoped candidates but cannot reach human authority, another account's workspace, or direct persistence.

## M09 — Restricted Release Watch and Hermes Pilot

Connect one tightly restricted Hermes profile to one admitted official-source workflow. Fresh sessions, memory/skills constrained, candidate submission only, hard cost limit, red-team injection tests, and immediate revocation.

**Milestone file**

- `05-implementation-planning/milestone-09-hermes-release-watch-pilot.md`

**Governing documents**

- M08 completion record in `05-implementation-planning/milestone-08-agent-neutral-interface.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- current official Hermes documentation/repository research package at `06-research/hermes-agent-capability-and-security-review.md` (must be created or refreshed before M09 entry)
- independent Claude AI Hermes architecture/security review at `08-audits/m09-hermes-pilot-architecture-review.md` (must be created before M09 entry)
- `04-platform-rules/automation-boundaries.md`
- `03-system-design/multi-account-model.md`
- accepted agent identity, memory, skill, source-admission, and cost decisions in `99-decisions/decision-log.md`

**Required outcome**

Hermes is proven suitable under restriction or deliberately rejected without affecting core operation. Every job is bound to one explicit account and candidate authority only.

## M10 — Truth Social Secondary Manual Workflow

Extend the stable workflow to Truth Social using manual publishing, owned-post records, platform-specific variants, and manual metrics only.

**Milestone file**

- `05-implementation-planning/milestone-10-truth-social-secondary.md`

**Governing documents**

- `06-research/truth-social-platform-research.md`
- `06-research/truth-social-capture-boundaries.md`
- `04-platform-rules/truth-social-account-rules.md`
- `04-platform-rules/truth-social-automation-boundaries.md`
- `04-platform-rules/automation-boundaries.md`
- `03-system-design/multi-account-model.md`
- D013 and D014 in `99-decisions/decision-log.md`
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- current Truth Social policy refresh at `06-research/truth-social-policy-refresh-current.md` (must be created or refreshed before M10 entry)

**Required outcome**

A complete human-cleared Truth Social loop with no undocumented API, scraping, or browser automation, and no cross-account/platform authority leakage.

## M11 — Metrics Ledger and Editorial Learning

Turn trustworthy owned-post observations into human-reviewed editorial learning, rolling 90-day monetization tracking, and bounded hypotheses/experiments without autonomous strategy.

**Milestone file**

- `05-implementation-planning/milestone-11-metrics-ledger-learning.md`

**Governing documents**

- `02-product/workflow-overview.md`
- `02-product/module-map.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- current X monetization/policy research at `06-research/x-monetization-current.md` (must be created or refreshed before M11 entry)
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- M10 completion record in `05-implementation-planning/milestone-10-truth-social-secondary.md` if Truth Social has been admitted
- `99-decisions/research-log.md`
- `99-decisions/decision-log.md`

**Required outcome**

Observations, correlations, hypotheses, experiments, and accepted conclusions remain distinct, account-scoped, and auditable.

## M12 — Rich Editorial Workspaces

Add deliberately admitted product packages: Reply Desk, Article Room, Asset Library, advanced calendar/pipeline interactions, richer search, saved views, and native operator productivity.

**Milestone file**

- `05-implementation-planning/milestone-12-rich-editorial-workspaces.md`

**Governing documents**

- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- `02-product/module-map.md`
- `02-product/workflow-overview.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/multi-account-model.md`
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- M06 completion record in `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md` where relevant
- M11 completion record in `05-implementation-planning/milestone-11-metrics-ledger-learning.md`
- `99-decisions/decision-log.md`

**Required outcome**

Each content package preserves account scope, exact approval, dependency, correction, publication, asset/evidence, and human-action boundaries.

## M13 — No Coincidences Pattern Candidates

Flag explainable entity/date/source/phrase/chronology overlaps without declaring truth.

**Milestone file**

- `05-implementation-planning/milestone-13-no-coincidences.md`

**Governing documents**

- `02-product/no-coincidences.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `05-implementation-planning/hammer-test-strategy.md`
- M06 completion record in `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`
- `03-system-design/multi-account-model.md`
- D010 in `99-decisions/decision-log.md`
- independent review at `08-audits/m13-no-coincidences-architecture-review.md` (must be created before accepting detector authority or cross-vault behavior)

**Required outcome**

Every candidate is explainable, provenance-bound, account-scoped, human-reviewed, and tested for false positives, non-detection, and detector failure.

## M14 — Qdrant Semantic Retrieval

Add semantic/hybrid retrieval only after a sufficient governed corpus and measured retrieval need exist.

**Milestone file**

- `05-implementation-planning/milestone-14-qdrant-retrieval.md`

**Governing documents**

- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- M06 completion record in `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`
- accepted D024 in `99-decisions/decision-log.md`
- current Qdrant official documentation/research package at `06-research/qdrant-capability-and-boundary-review.md` (must be created or refreshed before M14 entry)
- fixed retrieval evaluation plan at `05-implementation-planning/m14-retrieval-evaluation-plan.md` and governed dataset at `05-implementation-planning/m14-retrieval-evaluation-dataset.md` (both must be created before M14 implementation)
- independent Claude AI Qdrant architecture review at `08-audits/m14-qdrant-architecture-review.md` (must be created before M14 entry)

**Required outcome**

Retrieval quality is evaluated; every result traces to canonical records; account/vault isolation, deletion, correction, reindex, and fallback behavior are proven.

## M15 — Hardening, Backup, Recovery, and Operator Runbook

Prove whole-product recovery across SQLite, evidence, Tauri, capture, agent configuration, credentials, optional retrieval services, and any per-account vault architecture that has been owner-accepted and installed.

**Milestone file**

- `05-implementation-planning/milestone-15-hardening-recovery.md`

**Governing documents**

- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/m02-migration-and-hammer-execution-plan.md`
- `05-implementation-planning/m03-work-package-d-guarded-migrations-encrypted-archives-and-evidence-return.md`
- `05-implementation-planning/m03-work-package-e-recovery-authority-mismatch-and-evidence-return.md`
- completion records for installed components in their canonical milestone files: `05-implementation-planning/milestone-03-local-dashboard.md`, `05-implementation-planning/milestone-04-x-operations-and-metrics.md`, `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`, `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`, `05-implementation-planning/milestone-07-human-triggered-x-capture.md`, `05-implementation-planning/milestone-08-agent-neutral-interface.md`, `05-implementation-planning/milestone-09-hermes-release-watch-pilot.md`, `05-implementation-planning/milestone-10-truth-social-secondary.md`, `05-implementation-planning/milestone-11-metrics-ledger-learning.md`, `05-implementation-planning/milestone-12-rich-editorial-workspaces.md`, `05-implementation-planning/milestone-13-no-coincidences.md`, and `05-implementation-planning/milestone-14-qdrant-retrieval.md`
- `03-system-design/multi-account-model.md`
- accepted D024 in `99-decisions/decision-log.md`
- current recovery research at `06-research/backup-recovery-current-options.md` and operating plan at `09-operations/m15-backup-recovery-operating-plan.md` (both must be created before M15 entry)
- independent final recovery review at `08-audits/m15-recovery-readiness-review.md` (must be created before M15 closure)

**Required outcome**

Documented disaster drills succeed, modified evidence/backups fail closed, account boundaries survive restore, secrets remain protected, and the product operates without Hermes or Qdrant.

# Feature Destination Map

| Capability | Destination |
|---|---|
| Command Center, WIP, Ready-to-Post, Need-a-Post | M04; polished in M05/M12 |
| Rolling 90-day scheduler and Reserve | M04; native drag/drop and advanced views M05/M12 |
| Archive, Docket, Flash Release lanes | M04 |
| Manual X publishing, reconciliation, manual metrics | M04 |
| Tauri desktop product | M05 |
| Per-account dossiers and Anomaly Vault under accepted D024 | M06, subject to M06 review and entry gate |
| Human-triggered X capture | M07 |
| Governed agent API/MCP | M08 |
| Hermes and official-source Release Watch | M09 |
| Truth Social | M10 |
| Full metrics learning and monetization analysis | M11 |
| Reply Desk, Article Room, Assets, advanced calendar/pipeline | M12 |
| No Coincidences | M13 |
| Account-scoped Qdrant retrieval under accepted D024 | M14, subject to M14 review and entry gate |
| Full-system recovery | M15 |

# Standard Entry Gate

Before starting a milestone:

1. previous required milestones are owner-accepted;
2. the milestone file and every roadmap-listed governing document have been read;
3. every listed path exists and superseded/stale references have been corrected;
4. required research and policies are current;
5. milestone scope and exclusions are explicit;
6. authority, persistence, provider, platform, and destructive-action changes are owner-approved;
7. hammer/adversarial requirements are defined for new durable state or authority;
8. the repository is synchronized and the working tree is understood.

# Standard Implementation Gate

Before code changes:

1. the coherent work package and files/components are identified;
2. tests and evidence outputs are identified;
3. newly discovered work is assigned to a named milestone or explicit amendment;
4. no unrelated provider, platform, authority, or schema expansion is bundled silently;
5. the milestone's Required Reading list still matches this roadmap.

# Standard Exit Gate

A milestone closes only when:

1. the complete admitted operator capability works;
2. happy-path and adversarial tests pass;
3. authority bypass, malformed input, duplicate/retry, and failure behavior are proven;
4. evidence is preserved and bound to the implementation commit;
5. milestone, roadmap, status, map, decisions, and governing-document references are synchronized;
6. Git status/diff are reviewed;
7. the owner explicitly accepts closure.

# Immediate Authorized Sequence

1. M03 is accepted and closed;
2. the roadmap revision, D023, and D024 are accepted;
3. prepare the substantial M04 package and added adversarial matrix from the complete governing-document chain;
4. obtain owner review of that exact M04 package before code implementation;
5. implement M04 in a small number of coherent batches after authorization;
6. validate and close M04;
7. start Tauri M05.

# D024 Roadmap Reference

`D024 — Multi-Account Vault and Qdrant Isolation Direction` in `99-decisions/decision-log.md` is accepted as the governing direction for M06 and M14. Separate physical vaults per account, account-scoped collections, and fail-closed retrieval scope are accepted planning doctrine. Implementation remains blocked until each milestone's named independent review, exact design, hammer plan, and owner-controlled entry gate are satisfied.

# Final Rule

The complete product vision is preserved here. Sequencing is dependency control, not abandonment. Build meaningful capabilities, not ceremonial fragments, and never continue past a milestone boundary without its documented context chain.