# Milestone 15 — Hardening, Backup, Recovery, and Operator Runbook

## Status
Not started.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/m02-migration-and-hammer-execution-plan.md`
- `05-implementation-planning/m03-work-package-d-guarded-migrations-encrypted-archives-and-evidence-return.md`
- `05-implementation-planning/m03-work-package-e-recovery-authority-mismatch-and-evidence-return.md`
- completion records for installed components in their canonical milestone files: `05-implementation-planning/milestone-03-local-dashboard.md`, `05-implementation-planning/milestone-04-x-operations-and-metrics.md`, `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`, `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`, `05-implementation-planning/milestone-07-human-triggered-x-capture.md`, `05-implementation-planning/milestone-08-agent-neutral-interface.md`, `05-implementation-planning/milestone-09-hermes-release-watch-pilot.md`, `05-implementation-planning/milestone-10-truth-social-secondary.md`, `05-implementation-planning/milestone-11-metrics-ledger-learning.md`, `05-implementation-planning/milestone-12-rich-editorial-workspaces.md`, `05-implementation-planning/milestone-13-no-coincidences.md`, and `05-implementation-planning/milestone-14-qdrant-retrieval.md`
- `03-system-design/multi-account-model.md`
- accepted D024 in `99-decisions/decision-log.md`
- `06-research/backup-recovery-current-options.md` and `08-operations/m15-backup-recovery-operating-plan.md` (both must be created or refreshed before M15 entry)
- `08-audits/m15-recovery-readiness-review.md` (must be created before closure)

## Goal
Prove whole-product recovery and safe operation across all admitted components.

## Entry Gate
Final local component set known; recovery objectives and disaster matrix owner-approved.

## Scope
- SQLite, evidence, vault, Tauri configuration, capture helper, agent configuration, and optional Qdrant backup/restore
- audit-chain and evidence verification
- secret review and credential rotation
- migration/interrupted-write/corruption recovery
- provider outage, agent disable/revoke, and offline operation
- machine-loss replacement recovery
- dependency/update pinning, routine integrity checks, regression suite, operator runbook

## Required Drills
Database loss/corruption; interrupted migration; modified backup/evidence/audit chain; lost Tauri config; provider outage; compromised agent credential; duplicate job; Hermes permanently disabled; Qdrant unavailable; malformed capture; restore onto a replacement Windows machine.

## Exit Gate
Clean restore succeeds; modified artifacts fail closed; secrets remain protected; compromised agents revoke without DB work; the core product operates without Hermes or Qdrant; documented recovery steps are reproducible and owner-accepted.