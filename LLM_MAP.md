# LLM Map — Discrepancy Desk Docs

This file is the entry point for any LLM, coding agent, audit agent, or project steward working on The Discrepancy Desk.

Do not treat this project as a generic social media bot, SEO dashboard, conspiracy spam system, scraper farm, autonomous engagement engine, or fake government insider persona.

## Project Summary

The Discrepancy Desk is a human-approved, AI-assisted research, drafting, review, and publishing support system for a fictional/parody public account centered on weird history, conspiracy lore, anomalies, hidden timelines, and bureaucratic deadpan storytelling.

Public persona:

- Brand: The Discrepancy Desk
- Handle: `@DiscrepancyDesk`
- Persona: Quinton Clearance
- Lore: Basement 1, Level 7 / B1-L7
- Domain: `discrepancydesk.com`

The brand publishes on two platforms: **X** and **Truth Social**. The two platforms have very different official access levels — X has a paid developer API; Truth Social's only official API ("Truth API") is an institutional B2B data feed with no general-purpose developer program identified as of July 2026. Do not assume rules, capabilities, or research findings from one platform apply to the other. See `04-platform-rules/` for platform-specific docs.

### Name Disambiguation

These four names refer to different things and are sometimes used loosely — this is the canonical mapping:

```text
The Discrepancy Desk  = public brand / account name
Quinton Clearance     = fictional persona / voice
B1-L7                 = lore location (Basement 1, Level 7)
B1-L7 Control Room    = the private local system/dashboard (internal tool name)
```

Core line:

> Per attached memorandum, reality appears to have been poorly disclosed.

Operating doctrine:

> AI drafts. Human clears. Database remembers. Metrics judge.

## Repo Split

Planning/docs repo:

```text
C:\dev\x\discrepancy-desk-docs
```

Application/code repo:

```text
C:\dev\x\discrepancy-desk
```

The docs repo defines what should be built. The app repo contains implementation.

## Current Project Mode

Planning mode.

No app implementation has started unless `STATUS.md` says otherwise.

## Read Order for LLMs

Read these first:

1. `PROJECT_BRIEF.md`
2. `STATUS.md`
3. `00-doctrine/operating-doctrine.md`
4. `00-doctrine/human-approval-policy.md`
5. `00-doctrine/account-rules-and-boundaries.md`
6. `01-brand/brand-identity.md`
7. `01-brand/quinton-clearance-persona.md`
8. `01-brand/voice-and-style-guide.md`
9. `01-brand/x-launch-post-package.md`
10. `02-product/product-overview.md`
11. `03-system-design/architecture-overview.md`
12. `04-platform-rules/truth-social-account-rules.md`
13. `04-platform-rules/truth-social-automation-boundaries.md`
14. `05-implementation-planning/implementation-roadmap.md`
15. `05-implementation-planning/m01-work-package-b-developer-cost-boundary.md`
16. `05-implementation-planning/m01-work-package-b-redacted-configuration-record.md`
17. `05-implementation-planning/m01-work-package-c-controlled-probe-procedure.md`
18. `05-implementation-planning/m01-work-package-d-probe-analysis-and-m02-handoff.md`
19. `03-system-design/data-model-planning.md`
20. `05-implementation-planning/hammer-test-strategy.md`
21. `05-implementation-planning/milestone-02-persistence-contract.md`
22. `05-implementation-planning/m02-adversarial-test-matrix.md`
23. `05-implementation-planning/m02-operational-persistence-contract.md`
24. `05-implementation-planning/m02-lifecycle-state-model.md`
25. `05-implementation-planning/m02-owner-approved-persistence-decisions.md`
26. `05-implementation-planning/m02-unresolved-decision-register.md`
27. `05-implementation-planning/m02-migration-and-hammer-execution-plan.md`
28. `05-implementation-planning/m03-work-package-a-repository-governance-and-bootstrap.md`
29. `05-implementation-planning/m03-work-package-b-initial-persistence-implementation-return.md`
30. `05-implementation-planning/m03-work-package-c-idempotency-concurrency-and-reconciliation-return.md`
31. `08-audits/claude-audit-through-m01-hammer-review-acceptance.md`
32. `07-prompts/claude-project-audit-through-m01-hammer-focus.md`
33. `05-implementation-planning/chrome-extension-plan.md`
34. `06-research/truth-social-platform-research.md`
35. `06-research/truth-social-capture-boundaries.md`
36. `99-decisions/decision-log.md`
37. `99-decisions/research-log.md`

## Core Boundaries

The system may:

- store research leads
- store source notes
- store public posts/replies captured manually or through approved API reads
- help generate drafts
- help classify claims
- organize topic/context memory
- support human review
- record approved/published content
- track performance metrics
- help find related source material
- flag pattern candidates for human review

The system must not:

- autonomously post to X or Truth Social
- autonomously reply on X or Truth Social
- auto-like
- auto-follow
- auto-repost
- call undocumented/internal API endpoints on either platform
- run engagement pods
- coordinate multiple accounts (or multiple platforms) for fake amplification
- impersonate a real government employee
- claim real classified access
- present speculation as confirmed fact
- run “wake up sheeple” panic content as doctrine

## Major Components

### Public Brand

Docs:

- `01-brand/brand-identity.md`
- `01-brand/quinton-clearance-persona.md`
- `01-brand/voice-and-style-guide.md`
- `01-brand/profile-bio-options.md`
- `01-brand/avatar-banner-direction.md`
- `01-brand/x-launch-post-package.md`
- `01-brand/bread-baker-canon.md`

### B1-L7 Control Room

Private local dashboard for capture, drafting, approval, publishing support, and metrics.

Docs:

- `02-product/product-overview.md`
- `02-product/module-map.md`
- `02-product/workflow-overview.md`
- `03-system-design/architecture-overview.md`

### Anomaly Vault

Research memory layer. Likely Obsidian-compatible. Stores sources, claims, entities, notes, topic trails, and future post ideas.

Docs:

- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `02-product/module-map.md`

### No Coincidences

Future pattern-candidate detector. Flags weird overlaps; does not declare truth.

Docs:

- `02-product/no-coincidences.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`

### X API Probe

Before final schema, collect real X API JSON samples to understand payload shapes.

Docs:

- `06-research/x-policy-api-verification-2026-07-19.md`
- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/x-api-probe-plan.md`
- `05-implementation-planning/milestone-01-api-probe.md`

### Truth Social Integration

Truth Social has an official licensed product called **Truth API** (announced July 16, 2026), but it is an institutional B2B data feed for financial-services partners — not a general-purpose developer platform, and not admitted for this project. As of July 19, 2026, no public, self-service, general-purpose developer API or third-party integration program has been identified. The platform is admitted for **manual publishing support only** — human-operated posting via the official share composer, with manual recording of published URLs and metrics. No API probe is planned or authorized.

Docs:

- `06-research/truth-social-platform-research.md` (platform-admission research)
- `06-research/truth-social-capture-boundaries.md` (accepted capture boundary: URL/note/owned-record only)
- `04-platform-rules/truth-social-account-rules.md`
- `04-platform-rules/truth-social-automation-boundaries.md`

### Chrome Extension

Later manual capture helper for **X only**. Truth Social is not an admitted DOM-capture target. Truth Social support remains URL-and-human-note entry, owned-post records, manual metrics, official bookmarks/share tools, and official owner export. No Truth Social content script or parser may be built without a later owner-approved change supported by written permission, revised official policy, or professional legal guidance.

Docs:

- `05-implementation-planning/chrome-extension-plan.md`

### Local MCP Tooling

A separate helper repo, `C:\dev\gpt-mcp`, is planned to expose safe filesystem/Git tools to ChatGPT through FastMCP v3+ HTTP and ngrok.

Docs:

- `08-operations/mcp-tooling-plan.md`
- `07-prompts/claude-mcp-research-prompt.md`

## Important Implementation Rule

Do not schema-from-vibes.

Raw capture first. Inspect real payloads. Then design structured tables.

## Decision Log

All accepted major decisions belong in:

```text
99-decisions/decision-log.md
```
