# Decision Log

## D001 — Public Brand

Accepted:

```text
The Discrepancy Desk
```

Reason:

- exact `.com` available and purchased
- `@DiscrepancyDesk` fits X 15-character handle limit
- strong bureaucratic weirdness
- more ownable than Anomaly Archive / Redacted Archive

## D002 — Public Persona

Accepted:

```text
Quinton Clearance
```

Fictional records custodian associated with Basement 1, Level 7 / B1-L7.

## D003 — Brand Safety

Accepted:

The account must be commentary/parody/fictional and must not imply real agency employment or classified access.

## D004 — Core Doctrine

Accepted:

```text
AI drafts. Human clears. Database remembers. Metrics judge.
```

## D005 — Repo Split

Accepted:

```text
C:\dev\x\discrepancy-desk-docs = planning/docs/specs
C:\dev\x\discrepancy-desk = app/code repo
```

## D006 — Local-First Technical Direction

Accepted:

Early stack should be local-first and simple:

```text
FastAPI + SQLite + Alembic + Jinja/HTMX
```

Do not start with React/Postgres/Redis/Qdrant.

## D007 — X API Probe Before Schema

Accepted:

Run read-only X API probes and store raw JSON before final schema decisions.

## D008 — Manual Posting First

Accepted:

No autonomous posting/replies in early system. Manual copy-paste posting with published URL recording.

## D009 — Anomaly Vault Direction

Accepted:

Research memory can be Obsidian-compatible Markdown, with SQLite as structured operational truth and Qdrant later for semantic retrieval.

## D010 — No Coincidences

Accepted:

No Coincidences is a future pattern-candidate module. It flags weird overlaps but does not declare truth.

## D011 — Bread Baker Canon

Accepted:

Quinton bakes bread/sourdough/old recipes. This humanizes the persona and creates “crumbs” wordplay.

## D012 — MCP Helper Repo

Accepted:

Local ChatGPT MCP helper repo path:

```text
C:\dev\gpt-mcp
```

Purpose: safe filesystem/Git/project-steward tools for ChatGPT.

## D013 — Dual-Platform Brand Presence and Sequence

Accepted: 2026-07-19.

The Discrepancy Desk will launch and stabilize on **X first**. Truth Social is a secondary platform under the same brand and persona, considered only after the X workflow is operational and stable.

Truth Social must not delay the X MVP or cause X-specific policy, API, capture, publishing, or metrics assumptions to be reused without separate review.

## D014 — Truth Social Manual-Only Admission

Accepted: 2026-07-19.

Truth Social is admitted only for:

- manual publishing through the official UI
- official share-composer assistance
- official bookmarks
- manually entered URLs and human-authored notes
- owned-post text, URLs, IDs, timestamps, and manually observed metrics
- official owner-data exports

Not admitted:

- browser-extension DOM capture
- Truth Social content scripts or parsers
- automated reads or writes
- undocumented endpoints or API probing
- scraping, background collection, or browser automation
- thread, account, timeline, search-result, or bulk archival
- automated screenshots or recurring metrics polling

The boundary may be reopened only after written permission, materially revised official policy, or owner-accepted professional legal guidance. Source: `06-research/truth-social-capture-boundaries.md`.

## D015 — Approved Quinton Avatar and B1-L7 X Banner

Accepted: 2026-07-19.

The owner approved and applied the current Quinton Clearance avatar and B1-L7 X banner to the live `@DiscrepancyDesk` profile.

The avatar establishes the canonical animation-friendly visual identity for Quinton: approximately 55, short and compact, lean-to-average build, messy gray hair, rectangular glasses, tired eyes, expressive eyebrows, pale shirt, loosened dark tie, brown-gray cardigan, and a dry skeptical expression.

The banner establishes the current B1-L7 environment: a dusty modern records office, an open circular vault, Quinton walking through with his face hidden, and an unexpectedly high-technology archive interior beyond it. The supporting wall includes a single portrait of President Donald Trump and a respectfully displayed American flag.

Approved image masters live in the main project repo under `assets/brand/`. Only owner-approved, manually saved assets belong there; rejected generations are not retained. The governing visual specification and exact paths/hashes are recorded in `01-brand/avatar-banner-direction.md`.

## D016 — M02 Hammer-Test Doctrine

Accepted: 2026-07-19.

Every persistence rule must be attacked with deliberate bad data, invalid state transitions, fabricated identifiers, modified evidence, stale approvals, duplicate operations, partial failures, detector failures, migration failures, and recovery failures.

Tests must run against the real SQLite engine where database behavior matters. Any ambiguity, missing evidence, integrity mismatch, or unrecognized failure must fail closed. Happy-path tests alone prove nothing.

The doctrine specifically requires protection of exact approved text from platform-returned text drift, approval invalidation after edits, rejection of fabricated IDs, hash-based modified-evidence detection, blocking promotion when raw evidence is missing, lifecycle enforcement, deliberate duplicate/replay behavior, timestamped metric provenance, distinct missing/withheld/error states, preservation of original mention evidence, no auto-promotion after classifier or detector failure, and migration/backup/restore proof that preserves identifiers, relationships, exact text, provenance, and evidence hashes.

The detailed minimum contract is governed by `05-implementation-planning/hammer-test-strategy.md`. M02 may strengthen it but may not silently weaken it.
