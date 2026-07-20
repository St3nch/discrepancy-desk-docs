# Milestone 08 — Agent-Neutral Governed Interface

## Status
Not started.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `00-doctrine/human-approval-policy.md`
- `00-doctrine/account-rules-and-boundaries.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- M04 completion record in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- accepted D023/D024 and later accepted agent-authority decisions in `99-decisions/decision-log.md`
- `08-audits/m08-agent-interface-architecture-review.md` (must be created before entry)

## Goal
Expose safe, versioned business operations to connected LLMs without exposing persistence or human authority.

## Entry Gate
M04/M05 contracts stable; agent identity, scopes, provenance, budgets, revocation, and mock-agent hammer plan approved.

## Scope
- canonical HTTP/service operations with optional thin MCP wrapper
- agent identities, credentials, per-operation scopes
- candidate reads/writes only
- idempotency, input hashes, provenance, provider/model/session/job metadata
- cost/token observations, per-agent/global limits, kill switch
- typed rejection envelopes, pause/revocation, dry-run where useful
- mock-agent test harness

## Forbidden
Direct DB/SQL/filesystem persistence, approval issuance, accepted-truth promotion, publication, evidence deletion, account ownership, autonomous platform actions.

## Exit Gate
A mock agent can submit attributable candidates and read admitted context while all human-only and direct-persistence paths fail closed; replay, malformed input, budget exhaustion, and revocation tests pass.