# Implementation Roadmap — The Discrepancy Desk

## Purpose

This roadmap is the execution spine for building a working project from planning through stable operation.

A Project Steward LLM must not rely on this roadmap alone. Before starting or continuing a milestone, it must read that milestone file and every document listed under **Required Reading**.

The roadmap controls sequence. Milestone files control bounded execution. Doctrine and accepted decisions control behavior.

## Steward Operating Rules

1. Sync from repo truth before making claims or edits.
2. Read `LLM_MAP.md`, `STATUS.md`, this roadmap, the active milestone file, and all milestone-required planning docs.
3. Do not start a milestone until its entry gate is satisfied.
4. Do not silently expand scope. Record newly discovered work as a later milestone, research gap, or explicit amendment.
5. Read before edit; search before create; inspect changes before commit.
6. Preserve evidence for probes, migrations, tests, and milestone completion.
7. Human approval remains mandatory for public publishing and any newly admitted external action.
8. No autonomous posting, replying, liking, following, reposting, or direct messaging.
9. Do not schema-from-vibes. Real payloads and real workflows must inform structured design.
10. Update the active milestone file and `STATUS.md` when work completes, blocks, or materially changes.
11. Record accepted project decisions in `99-decisions/decision-log.md`; record dated external findings in `99-decisions/research-log.md`.
12. A milestone is not complete because code exists. It is complete only when its exit gate, tests, evidence, and documentation updates are satisfied.

## Mandatory Pre-Database Stop

Before any database schema, migration, persistence implementation, or database-backed feature work begins, the Project Steward must stop and ask the owner:

> You said you want hammer tests added before database work. What exact failure modes, invariants, adversarial cases, and evidence requirements do you want included for this project?

The current cross-system doctrine is preserved in `05-implementation-planning/hammer-test-strategy.md`. It covers the operational database, Anomaly Vault, No Coincidences, Qdrant, mock-data requirements, fail-closed behavior, and milestone evidence. M02 must still refine the database portion against the real persistence contract before implementation.

No database implementation is admitted until the owner clarification and refined M02 hammer-test matrix are recorded in planning docs.

## Milestone Sequence

### M00 — Planning Foundation and Repository Baseline

**Goal:** establish a governed, readable planning repository.

**Milestone file:** `05-implementation-planning/milestone-00-foundation.md`

**Required outcome:** docs repo initialized, reviewed, committed, and usable by a fresh Project Steward.

### M01 — X Identity, Policy, and Read-Only API Probe

**Goal:** finish the X-facing identity and inspect real X API payloads before data-model decisions.

**Milestone file:** `05-implementation-planning/milestone-01-api-probe.md`

**Required outcome:** compliant account posture, bounded read-only probe evidence, payload inventory, and schema implications.

Truth Social remains secondary and must not delay M01.

### M02 — Persistence Contract and Hammer-Test Plan

**Goal:** define the smallest operational data contract and its adversarial test requirements before writing migrations.

**Milestone file:** `05-implementation-planning/milestone-02-persistence-contract.md`

**Required outcome:** owner-approved hammer-test contract, bounded data model, lifecycle rules, exact-text approval binding, and migration/test plan.

No database implementation occurs in this milestone.

### M03 — Local Control Room MVP

**Goal:** build the first complete local capture-to-publication-record loop.

**Milestone file:** `05-implementation-planning/milestone-03-local-dashboard.md`

**Required outcome:** tested FastAPI + SQLite + Alembic + Jinja/HTMX application supporting manual capture, sources, drafts, human review, and manual published-post recording.

This is the core MVP.

### M04 — X Operating Workflow and Manual Metrics

**Goal:** operate the MVP against real X work without autonomous publishing.

**Milestone file:** `05-implementation-planning/milestone-04-x-operations-and-metrics.md`

**Required outcome:** repeatable drafting/review/manual-publish workflow, owned-post records, manual metrics snapshots, and operating evidence.

### M05 — Anomaly Vault Foundation

**Goal:** add a governed Obsidian-compatible research-memory layer after the operational loop works.

**Milestone file:** `05-implementation-planning/milestone-05-anomaly-vault.md`

**Required outcome:** source, claim, entity, topic, and research-note conventions with clear boundaries between Markdown research memory and SQLite operational truth.

### M06 — Human-Triggered Capture Helper

**Goal:** reduce manual capture friction without becoming a scraper.

**Milestone file:** `05-implementation-planning/milestone-06-capture-helper.md`

**Required outcome:** an admitted, human-triggered X capture helper or a documented decision to retain URL/manual-note capture only.

Truth Social capture remains excluded unless its active research and policy gate explicitly admit a narrow feature.

### M07 — Truth Social Secondary Launch

**Goal:** extend the stable brand workflow to Truth Social without duplicating X assumptions.

**Milestone file:** `05-implementation-planning/milestone-07-truth-social-secondary.md`

**Required outcome:** manual publishing workflow, owned-post recordkeeping, platform-specific copy variants, and manual metrics. No undocumented API or automated capture.

Entry requires a stable X workflow.

### M08 — Metrics Ledger and Editorial Learning

**Goal:** turn owned-post observations into human-reviewed editorial learning.

**Milestone file:** `05-implementation-planning/milestone-08-metrics-ledger.md`

**Required outcome:** comparable-but-not-falsely-equated platform metrics, content tags, review cadence, and bounded recommendations.

### M09 — No Coincidences Prototype

**Goal:** flag explainable pattern candidates without declaring truth.

**Milestone file:** `05-implementation-planning/milestone-09-no-coincidences.md`

**Required outcome:** exact/entity/date/source-overlap candidates, provenance, human review states, and non-detection/failure tests.

Not MVP.

### M10 — Qdrant Semantic Retrieval

**Goal:** add semantic retrieval only after enough high-quality Anomaly Vault material exists.

**Milestone file:** `05-implementation-planning/milestone-10-qdrant-retrieval.md`

**Required outcome:** evaluated retrieval quality, provenance-preserving context assembly, deletion/reindex behavior, and fallback when vector retrieval fails.

Not MVP. Do not build merely because embeddings are shiny.

### M11 — Hardening, Backup, and Recovery

**Goal:** prove the project can survive failure and be safely operated.

**Milestone file:** `05-implementation-planning/milestone-11-hardening-recovery.md`

**Required outcome:** backup/restore proof, secret review, migration recovery, evidence integrity, regression suite, and operator runbook.

## MVP Boundary

The MVP is complete at the end of **M04**, provided M00–M04 exit gates are satisfied.

MVP includes:

- governed planning baseline
- X identity and policy readiness
- bounded X read-only payload research
- owner-approved persistence and hammer-test contract
- local Control Room
- capture, source, draft, review, approval, rejection, and manual published-post records
- exact-text approval binding
- manual X publishing
- manual owned-post metrics

MVP does not require:

- Truth Social launch
- browser extension
- Anomaly Vault
- Qdrant
- No Coincidences
- autonomous platform actions
- React, Postgres, Redis, Celery, or multi-agent orchestration

## Milestone Completion Protocol

At completion of every milestone, the Project Steward must:

1. Verify every exit-gate item with evidence.
2. Update the milestone document:
   - status
   - completion date
   - evidence paths
   - accepted deviations
   - unresolved follow-up work
3. Update `STATUS.md`:
   - completed milestone
   - active milestone
   - current blockers
   - next bounded action
4. Update `LLM_MAP.md` if new canonical docs were added.
5. Update decision/research logs when applicable.
6. Run relevant tests and record exact commands/results.
7. Inspect Git status and diff.
8. Commit only with explicit owner approval.
9. Never push unless explicitly asked.

## Roadmap Change Rule

Changing milestone order, scope, entry gates, exit gates, or platform admission requires:

- a written rationale;
- affected-doc review;
- owner approval;
- roadmap, milestone, status, and decision-log updates in the same bounded change.

## Final Rule

Build the smallest complete, tested, recoverable system first.

The Desk does not need every drawer installed before it can file its first case.