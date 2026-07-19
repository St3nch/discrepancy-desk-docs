# Milestone 01 — X Identity, Policy, and Read-Only API Probe

## Status

Not started.

## Goal

Finish X-facing identity and policy readiness, then inspect real X API payloads before persistence design.

## Required Reading

- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `01-brand/brand-identity.md`
- `01-brand/quinton-clearance-persona.md`
- `01-brand/voice-and-style-guide.md`
- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/x-api-probe-plan.md`
- `04-platform-rules/automation-boundaries.md`
- `04-platform-rules/platform-risk-register.md`
- relevant current X research and verification items

## Entry Gate

- M00 complete.
- X account identity and PCF/commentary posture approved.
- Current X API pricing, limits, and terms rechecked from official sources.
- Owner approves any spend before credits are purchased.

## Scope

- finish X account profile and disclosure posture
- read-only API setup
- bounded endpoint list
- raw JSON capture
- probe manifest
- payload field inventory
- schema implications report
- unknowns and retention notes

## Out of Scope

- X write API
- posting or replying through API
- automation
- database schema or migrations
- dashboard implementation
- Chrome extension
- Truth Social probe

## Required Evidence

Store raw probe evidence under an approved app-repo evidence path, including:

- immutable raw responses
- request context with secrets redacted
- `probe_manifest.json`
- timestamps and endpoint/version information
- hashes where appropriate
- schema-implications document
- exact commands or controlled procedure used

## Exit Gate

- account posture is documented
- raw samples and manifest exist
- evidence contains no exposed secrets
- payload fields and missing fields are inventoried
- schema implications and unknowns are documented
- no write capability was exercised
- M02 planning input is bounded
- milestone and `STATUS.md` updated

## Completion Record

Record date, evidence paths, costs, official-doc recheck date, deviations, and next milestone.