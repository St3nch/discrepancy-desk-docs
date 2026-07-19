# Milestone 05 — Anomaly Vault Foundation

## Status
Not started.

## Goal
Add an Obsidian-compatible research-memory layer after the core X operating loop is stable.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `02-product/module-map.md`
- `00-doctrine/operating-doctrine.md`
- M04 completion record

## Entry Gate
M04 complete; real research workflow pain points documented.

## Scope
Source, claim, entity, topic, and research-note conventions; provenance; templates; links between Vault notes and operational records; search-before-create rules.

## Out of Scope
Qdrant, automated truth judgments, bulk imports, autonomous research agents, replacing SQLite operational truth.

## Required Evidence
Example notes from real project material; clean, conflicting, malformed, and adversarial mock fixtures; provenance and claim-label review; broken-link/orphan/alias/frontmatter tests; mutation and partial-restore tests; clear promotion boundary from research notes to operational content; exact commands and results recorded under the hammer-test evidence rule.

## Exit Gate
A human and fresh LLM can store, find, relate, and cite research without confusing Markdown memory with database state.

## Completion Record
Record date, evidence paths, accepted templates, deviations, and follow-up work.