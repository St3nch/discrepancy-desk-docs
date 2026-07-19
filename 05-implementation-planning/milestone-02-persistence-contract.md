# Milestone 02 — Persistence Contract and Hammer-Test Plan

## Status

Active — all planning artifacts and UD-01 through UD-12 decisions reconciled; M02 is ready for owner closure review and explicit M03 authorization.

## Goal

Define the smallest operational persistence contract and the tests that must attack it before any database implementation begins.

## Mandatory Owner Clarification

Satisfied on 2026-07-19.

The owner approved the following doctrine:

> Every persistence rule must be attacked with deliberate bad data, invalid state transitions, fabricated identifiers, modified evidence, stale approvals, duplicate operations, partial failures, detector failures, migration failures, and recovery failures. Tests must run against the real SQLite engine where database behavior matters. Any ambiguity, missing evidence, integrity mismatch, or unrecognized failure must fail closed. Happy-path tests alone prove nothing.

The detailed minimum requirements are governed by `05-implementation-planning/hammer-test-strategy.md` under **Owner-Approved M02 Doctrine**.

Do not infer the exact persistence contract from The Observatory. Its fail-closed, adversarial, real-database testing doctrine is relevant context, but Discrepancy Desk invariants must be justified by this project's real workflows and M01 evidence.

## Required Reading

- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `00-doctrine/human-approval-policy.md`
- `02-product/mvp-scope.md`
- `02-product/workflow-overview.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/data-model-planning.md`
- `04-platform-rules/automation-boundaries.md`
- `05-implementation-planning/milestone-01-api-probe.md`
- M01 payload evidence and schema-implications report

## Entry Gate

- M01 completed or explicitly waived by owner with rationale.
- Real workflow and payload evidence available.
- Owner has defined the required hammer-test doctrine.

## Scope

- bounded entity and lifecycle model
- ownership and provenance fields
- exact-text approval binding
- draft/review/published-state transitions
- raw evidence preservation rules
- migration strategy
- rollback and recovery expectations
- adversarial database test plan
- evidence requirements for migration and integrity testing

## Out of Scope

- migrations
- SQLite file creation
- application models
- CRUD implementation
- dashboard implementation
- platform writes

## Independent Audit Gate

A Claude AI read-only audit completed after M01 with verdict **PASS WITH CORRECTIONS**. The accepted correction package was completed and committed on 2026-07-19. M02 planning remains admitted, but physical schema and persistence implementation remain blocked until the M02 exit gate is satisfied.

Accepted audit requirements include:

- immutable documentation of the root-level attempt-001 layout and explicit-manifest hash verification;
- reconciliation of candidate data-model language with exact-byte raw evidence;
- removal of the plaintext credential file from the broad `C:\dev` connector boundary by owner action;
- an explicit non-Git and third-party-data policy for the main assets/evidence directory;
- SQLite-specific hammer coverage for connection pragmas, locking, idempotency, audit integrity, cross-store evidence, dirty migrations, restore reconciliation, and Unicode/text mutation.

## Required Hammer-Test Categories

Use all operational-database and **M02 Required SQLite-Specific Categories** in `05-implementation-planning/hammer-test-strategy.md`. M02 must refine each into a named invariant, valid fixture, deliberate violation fixture, expected fail-closed result, real-engine requirement, and preserved evidence requirement before any migration work.

## Active Planning Artifacts

- `05-implementation-planning/m02-adversarial-test-matrix.md` — owner-approved 20-invariant M02 baseline.
- `05-implementation-planning/m02-operational-persistence-contract.md` — owner-approved smallest conceptual operational persistence contract.
- `05-implementation-planning/m02-lifecycle-state-model.md` — owner-approved lifecycle, legal-transition, approval-invalidation, and fail-closed model.
- `05-implementation-planning/m02-owner-approved-persistence-decisions.md` — owner-approved resolutions for UD-01 through UD-12.
- `05-implementation-planning/m02-unresolved-decision-register.md` — resolved UD-01 through UD-12 history plus deferred UD-13 through UD-18.
- `05-implementation-planning/m02-migration-and-hammer-execution-plan.md` — planning-only migration, recovery, fixture, test-evidence, and M03 bounding plan.

## Required Evidence

- owner-approved hammer-test contract
- persistence-contract document
- lifecycle/state diagram
- migration plan
- test matrix with deliberate negative cases
- resolved UD-01 through UD-12 decision record and explicitly deferred UD-13 through UD-18

## Exit Gate

- owner approves the persistence contract
- hammer-test plan is explicit and bounded
- every proposed table/entity has a demonstrated MVP workflow need
- no unsupported X or Truth Social payload assumptions remain
- implementation batch for M03 is bounded
- `STATUS.md` updated

## Completion Record

Complete when the exit gate is verified. Record date, evidence paths, deviations, and owner approval here.