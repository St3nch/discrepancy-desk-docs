# Milestone 09 — Restricted Release Watch and Hermes Pilot

## Status
Not started.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- M08 completion record in `05-implementation-planning/milestone-08-agent-neutral-interface.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- `06-research/hermes-agent-capability-and-security-review.md` (must be current before entry)
- `08-audits/m09-hermes-pilot-architecture-review.md` (must be created before entry)
- `04-platform-rules/automation-boundaries.md`
- `03-system-design/multi-account-model.md`
- accepted agent identity, memory, skill, source, and cost decisions in `99-decisions/decision-log.md`

## Goal
Prove one low-authority background-agent workflow against an admitted official source.

## Entry Gate
M08 closed; installed Hermes version independently reviewed; memory/skill behavior runtime-verified; source/provider/model/budget/red-team plan approved; kill switch tested.

## Scope
- one separately administered Hermes profile
- fresh scheduled sessions and pinned provider/model
- external memory off; built-in memory/tooling disabled and verified where supported
- autonomous skills disabled; skill writes approval-gated
- minimal tools: admitted source read + governed Desk candidate operation
- authority/novelty/relevance/urgency candidate classification
- hard cost/rate limits, full provenance, immediate revocation

## Forbidden
Database/SQL, terminal, unrestricted filesystem, messaging gateway, social posting, approval, lifecycle mutation, schedule mutation, accepted-truth promotion.

## Exit Gate
A bounded pilot proves attribution, dedupe, fail-closed provider/cost behavior, injection and memory-poisoning containment, no authority bypass, and working revocation. Owner either accepts restricted Hermes use or records rejection; either outcome is valid.