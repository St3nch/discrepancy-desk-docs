# Milestone 07 — Human-Triggered X Capture Helper

## Status
Not started.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/chrome-extension-plan.md`
- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/automation-boundaries.md`
- `06-research/x-policy-api-verification-2026-07-19.md` (must be refreshed or explicitly superseded before M07 entry; any replacement path must be synchronized into this file and the roadmap)
- `03-system-design/multi-account-model.md`
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- `99-decisions/decision-log.md`

## Goal
Reduce manual X intake friction without creating a scraper or autonomous browser agent.

## Entry Gate
Stable X workflow; current policy review; capture contract and provenance hammer plan approved.

## Scope
- explicit human-triggered capture only
- current URL, selected text, human note, optional screenshot, content hash, capture time
- operator confirmation and governed intake submission
- safe malformed/duplicate handling

## Out of Scope
Continuous monitoring, crawling, scrolling, DOM harvesting, hidden collection, platform actions, credential extraction, Truth Social DOM capture.

## Exit Gate
A narrow helper is proven attributable, explicit, policy-compliant, non-recurring, and fail-closed—or the project records an owner-approved decision to retain manual URL/note capture only.