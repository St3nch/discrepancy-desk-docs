# Milestone 02 — Persistence Contract and Hammer-Test Plan

## Status

Not started.

## Goal

Define the smallest operational persistence contract and the tests that must attack it before any database implementation begins.

## Mandatory Owner Clarification

Before doing database work, stop and ask the owner what **hammer tests** must mean for The Discrepancy Desk. Record the answer here before proceeding.

Do not infer the exact contract from The Observatory, although its fail-closed, adversarial, real-database testing doctrine is relevant context.

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

## Required Hammer-Test Categories

Use the operational-database categories in `05-implementation-planning/hammer-test-strategy.md` as the baseline. M02 must refine them into project-specific invariants, fixtures, deliberate violation cases, real-engine tests, and evidence requirements after the owner clarification and before any migration work.

## Required Evidence

- owner-approved hammer-test contract
- persistence-contract document
- lifecycle/state diagram
- migration plan
- test matrix with deliberate negative cases
- unresolved schema questions

## Exit Gate

- owner approves the persistence contract
- hammer-test plan is explicit and bounded
- every proposed table/entity has a demonstrated MVP workflow need
- no unsupported X or Truth Social payload assumptions remain
- implementation batch for M03 is bounded
- `STATUS.md` updated

## Completion Record

Complete when the exit gate is verified. Record date, evidence paths, deviations, and owner approval here.