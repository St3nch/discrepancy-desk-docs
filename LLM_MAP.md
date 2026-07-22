# LLM Map — Discrepancy Desk Docs

This file is the entry point for any LLM, coding agent, audit agent, or project steward working on The Discrepancy Desk.

Do not treat this project as a generic social media bot, SEO dashboard, conspiracy spam system, scraper farm, autonomous engagement engine, or fake government insider persona.

## Project Summary

The Discrepancy Desk is a human-approved, AI-assisted anomaly archive, research, drafting, review, and publishing-support system for a fictional/parody public account centered on weird history, conspiracy lore, disputed claims, anomalies, hidden timelines, and bureaucratic deadpan storytelling.

The Desk may archive and publish clearly labeled speculative, disputed, conspiratorial, folkloric, implausible, and unresolved material. It is not a formal fact-checking or truth-adjudication product and does not need to prove or disprove every theory. It must preserve provenance, distinguish source material from Desk inference where material, and never fabricate the supporting record. The controlling clarification is `00-doctrine/editorial-anomaly-archive-direction.md` and D029.

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

M00 through M05 are owner-accepted and closed. AC-01 and AC-02 are closed. The M06 architecture synthesis and all sixteen owner architecture rulings are accepted.

The resolved M06-A Local Manual Vault planning baseline is owner-accepted through D028, and its planning correction cycle is closed. D027 fixes one editorial/public brand identity per physical Vault, one SQLite database per Vault, and rejection of timed-deletion material in first M06-A. D029 clarifies the editorial anomaly archive. D030 through D033 implement, independently verify, and close Phase 1. D034 authorized the exact Phase 2 package and 33-invariant profile. Application commit `eaf7b5d` implemented Phase 2; the required independent Claude review returned `M06-A PHASE 2 INDEPENDENTLY VERIFIED`; exact five-path commit `1e8cba8` corrected both Low findings. D035 accepts the correction, defers only the optional second spot review due usage limits, and closes Phase 2. No parser is admitted. Phase 3 and M06-B remain separately gated. Providers, monitoring, live LLM integration, Qdrant, graph work, purge, publication automation, and cross-Vault transfer execution remain blocked.

## Read Order for LLMs

Read these first:

1. `PROJECT_BRIEF.md`
2. `STATUS.md`
3. `00-doctrine/operating-doctrine.md`
   - then `00-doctrine/editorial-anomaly-archive-direction.md`
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
15. `05-implementation-planning/m06-architecture-synthesis.md`

   Then read the owner-accepted M06-A planning continuation in this exact order:

   - `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`
   - `05-implementation-planning/m06a-parser-admission-plan.md`
   - `05-implementation-planning/m06a-adversarial-closure-matrix.md`
   - `08-audits/m06a-planning-correction-disposition.md`
   - `08-audits/m06a-planning-correction-closure.md`
   - D027 and D028 in `99-decisions/decision-log.md`

16. `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`
17. `99-decisions/m06-owner-architecture-rulings.md`
18. `08-audits/claude-m06-research-program-independent-review.md`
19. `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
20. `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
21. `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
22. `05-implementation-planning/m01-work-package-b-developer-cost-boundary.md`
23. `05-implementation-planning/m01-work-package-b-redacted-configuration-record.md`
24. `05-implementation-planning/m01-work-package-c-controlled-probe-procedure.md`
25. `05-implementation-planning/m01-work-package-d-probe-analysis-and-m02-handoff.md`
26. `03-system-design/data-model-planning.md`
27. `05-implementation-planning/hammer-test-strategy.md`
28. `05-implementation-planning/milestone-02-persistence-contract.md`
29. `05-implementation-planning/m02-adversarial-test-matrix.md`
30. `05-implementation-planning/m02-operational-persistence-contract.md`
31. `05-implementation-planning/m02-lifecycle-state-model.md`
32. `05-implementation-planning/m02-owner-approved-persistence-decisions.md`
33. `05-implementation-planning/m02-unresolved-decision-register.md`
34. `05-implementation-planning/m02-migration-and-hammer-execution-plan.md`
35. `05-implementation-planning/m03-work-package-a-repository-governance-and-bootstrap.md`
36. `05-implementation-planning/m03-work-package-b-initial-persistence-implementation-return.md`
37. `05-implementation-planning/m03-work-package-c-idempotency-concurrency-and-reconciliation-return.md`
38. `05-implementation-planning/m03-work-package-d-guarded-migrations-encrypted-archives-and-evidence-return.md`
39. `05-implementation-planning/m03-work-package-e-recovery-authority-mismatch-and-evidence-return.md`
40. `05-implementation-planning/m03-work-package-f-minimal-operator-service-loop-return.md`
41. `05-implementation-planning/m03-work-package-g-thin-local-control-room-return.md`
42. `05-implementation-planning/m03-work-package-h-successor-and-replacement-lineage-return.md`
43. `05-implementation-planning/m03-work-package-i-hammer-closure-review-return.md`
44. `08-audits/claude-audit-through-m01-hammer-review-acceptance.md`
45. `07-prompts/claude-project-audit-through-m01-hammer-focus.md`
46. `05-implementation-planning/chrome-extension-plan.md`
47. `06-research/truth-social-platform-research.md`
48. `06-research/truth-social-capture-boundaries.md`
49. `99-decisions/decision-log.md`
50. `99-decisions/research-log.md`
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
- archive and publish clearly labeled speculative, disputed, folkloric, conspiratorial, anomalous, or unresolved material
- connect recurring people, organizations, places, dates, claims, records, symbols, language, and timelines without treating the connection as automatic proof

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
- fabricate sources, documents, quotations, screenshots, evidence, or corroboration
- hide the difference between source material and Desk-created inference
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

Research memory layer and editorial connection engine. It is an archive and provenance system, not a truth machine. It may preserve unresolved allegations, folklore, speculation, contradictions, and pattern candidates when their source and status are represented honestly. Its M06-A local-only canonical formats, ingestion boundary, projections, parser-admission contract, and recovery plan are defined in the owner-accepted M06-A planning baseline. Obsidian is not an architectural requirement. Phases 1 and 2 are independently verified and owner-closed through D035. Phase 3, parser work, and every later capability remain blocked until separately authorized.

Docs:

- `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`
- `05-implementation-planning/m06a-parser-admission-plan.md`
- `05-implementation-planning/m06a-adversarial-closure-matrix.md`
- `05-implementation-planning/m06a-phase2-exact-implementation-package.md`
- `08-audits/m06a-planning-correction-closure.md`
- `08-audits/claude-m06a-phase1-independent-implementation-review.md`
- `08-audits/m06a-phase1-independent-review-disposition.md`
- `08-audits/claude-m06a-phase2-independent-implementation-review.md`
- `08-audits/m06a-phase2-correction-and-closure.md`
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

- `09-operations/mcp-tooling-plan.md`
- `07-prompts/claude-mcp-research-prompt.md`

## Important Implementation Rule

Do not schema-from-vibes.

Raw capture first. Inspect real payloads. Then design structured tables.

## Decision Log

All accepted major decisions belong in:

```text
99-decisions/decision-log.md
```


## Editorial Control Room and Full Product Destination

Canonical planning:

- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `05-implementation-planning/milestone-04-x-operations-and-metrics.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md` (research input, not doctrine)

Tauri is the sole supported human operator interface. FastAPI remains the token-gated loopback backend/API and packaged sidecar host. Legacy Jinja pages remain frozen historical regression scaffolding and receive no new product features or parity requirements. Dossiers, capture, agent API, Hermes, Truth Social, metrics learning, rich editorial workspaces, No Coincidences, Qdrant, and recovery each have named milestone destinations and gates.

## Connected LLM and Agent Boundary

Connected LLMs receive bounded task/context packets and may produce candidates only. No LLM or agent may directly access SQLite or exercise human approval, accepted-truth promotion, publication, evidence deletion, account ownership, or autonomous social actions.

The canonical future path is:

```text
LLM/agent → optional thin MCP wrapper → governed HTTP/service business operations → Desk persistence
```

Hermes is a possible replaceable sibling runtime admitted only after the agent-neutral interface passes its mock-agent and authority gates.