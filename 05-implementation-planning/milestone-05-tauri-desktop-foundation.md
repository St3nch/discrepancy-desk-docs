# Milestone 05 — Tauri Desktop Foundation and Product Parity

## Status
Implementation active. M04 is owner-accepted and closed. The owner accepted the exact M05 technical plan and 77-invariant adversarial matrix on 2026-07-20. Implementation must remain inside their fixed change surface, security boundary, packaging boundary, and stop conditions.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/multi-account-model.md`
- `02-product/workflow-overview.md`
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- application repo: `docs/m04-service-api-contract.md` (must be created before M05 entry)
- accepted D023 in `99-decisions/decision-log.md`

## Goal
Build the product-grade daily operator interface around M04-proven contracts.

## Entry Gate
M04 owner-accepted; service/authority/scheduling contracts stable; Tauri technical plan and security boundary approved.

## Scope
- Tauri shell and local backend connection
- Command Center, Calendar, Work, Records, Release Watch, Library, Metrics, System navigation
- resizable panes, side inspector, command palette, keyboard navigation
- native file selection/drag-drop, window restoration, local notifications, secure credential handling
- offline/error/health states
- full M04 workflow parity
- B1-L7 operational-records-desk visual identity

## Out of Scope
New business authority, direct database access, Hermes, autonomous platform actions, multi-user collaboration, major dossier logic.

## Exit Gate
The complete M04 loop works through Tauri; all mutations use governed service/API operations; credentials and failures are handled safely; web harness remains a test surface; owner accepts desktop foundation.