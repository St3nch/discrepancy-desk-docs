# Milestone 11 — Hardening, Backup, and Recovery

## Status
Not started.

## Goal
Prove the project can survive operator error, failed migrations, corrupted evidence, lost local state, and dependency failure.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/hammer-test-strategy.md`
- all completed milestone records
- accepted subsystem-specific hammer-test contracts
- current architecture and operations docs

## Entry Gate
Core operational features are stable enough to preserve; owner approves backup and recovery posture.

## Scope
Secret review, database backup, Vault backup, restore proof, migration recovery, evidence hashing/integrity, dependency-failure behavior, regression suite, operator runbook.

## Out of Scope
Unproven cloud complexity, unattended external actions, claiming recoverability without an actual restore.

## Required Evidence
Restored disposable environment; migration recovery test; evidence-integrity failure test; clean-secret scan; full regression and hammer-test results; SQLite restore verification; Anomaly Vault note/link/hash verification; Qdrant rebuild from canonical records when admitted; dependency-outage fallback proof; documented recovery commands and ownership.

## Exit Gate
A fresh operator can restore a verified working project from documented backups, and critical failures stop safely.

## Completion Record
Record date, backup IDs/hashes, restore location, commands/results, defects, and next action.