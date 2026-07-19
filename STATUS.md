# Project Status — Discrepancy Desk

*Last updated: 2026-07-19*

## Current Mode

Planning.

## Current Focus

Complete M00 planning-foundation review and establish the initial Git baseline. The implementation roadmap now uses milestone contracts with required reading, entry gates, evidence, tests, exit gates, and mandatory completion updates.

Before any database work, the Project Steward must stop and confirm the Discrepancy Desk hammer-test contract against `05-implementation-planning/hammer-test-strategy.md`. No schema, migration, or persistence implementation is admitted until M02 refines that baseline into project-specific invariants, fixtures, deliberate violation cases, real-engine tests, and evidence requirements.

## Completed / Decided

- Public brand selected: **The Discrepancy Desk**.
- Exact `.com` domain purchased: `discrepancydesk.com`.
- X/social handle selected: `@DiscrepancyDesk`.
- X bio chosen around B1-L7 / commentary/parody / Quinton Clearance.
- Persona selected: **Quinton Clearance**.
- Lore location selected: **Basement 1, Level 7** / **B1-L7**.
- Canon hobby selected: bread baking / sourdough.
- Public tone selected: deadpan bureaucratic weird-history/conspiracy archive clerk.
- Project folders created:
  - `C:\dev\x\discrepancy-desk-docs`
  - `C:\dev\x\discrepancy-desk`
- Docs repo role decided: planning, doctrine, specs, implementation instructions.
- App repo role decided: actual coded project.
- MCP helper repo path selected: `C:\dev\gpt-mcp`.
- Initial technical direction selected: local-first, raw-first, human-approved.
- Cross-system hammer-test strategy documented for SQLite, Anomaly Vault, No Coincidences, Qdrant, mock fixtures, and recovery; each subsystem contract must be refined during its milestone.
- Platform sequence accepted: **X first**, then Truth Social only after the X workflow is operational and stable (D013).
- Truth Social platform admission accepted as manual-only (D014): official UI/share tools, URLs, human notes, owned-content records, manual metrics, bookmarks, and owner export.
- Truth Social DOM/browser-extension capture is not admitted. See `06-research/truth-social-capture-boundaries.md`.
- Chrome extension plan is now X-only; no Truth Social content script or parser may be built under the current decision.

## Pending

- Initialize `C:\dev\x\discrepancy-desk-docs` as a Git repo.
- Add this document pack to the docs repo.
- Create GitHub repo `discrepancy-desk-docs`.
- Commit initial planning docs.
- Validate and document the operational `gpt-mcp` / Go10 tool boundary for this project (Go10 is online with 26 tools and has successfully accessed project roots — remaining work is documenting the boundary, not standing up the server).
- Finish X profile details, avatar, banner, pinned post.
- Prepare API probe plan before app schema.

## Not Started

- Actual app implementation.
- X API probe.
- Local dashboard.
- Chrome extension build (planning complete; no code yet).
- Truth Social manual-publishing workflow build (planning complete; no code yet).
- Obsidian vault integration.
- Qdrant integration.
- Metrics capture.
- No Coincidences implementation.

## Hard Boundaries

No autonomous posting (X or Truth Social).
No autonomous replies (X or Truth Social).
No auto-likes.
No auto-follows.
No engagement pods.
No calling undocumented/internal API endpoints on either platform.
No real-government impersonation.
No claims of real classified access.
No declaring speculation as confirmed truth.

## Open Verification Items

These are known-unverified facts that specific plans currently depend on. Re-check before the relevant milestone starts:

- X: whether PCF-compliant keywords are still required at the start of the account **display name** (not just bio) — see `04-platform-rules/x-account-rules.md` and source notes.
- X: current pay-per-use API pricing and account-level daily post caps.
- Truth Social: re-confirm no public, self-service developer program has launched since July 2026 research (`06-research/truth-social-platform-research.md`); note Truth API (institutional B2B) already exists and is out of scope regardless.
- Truth Social: parody-account enforcement consistency — legal review recommended before launch.
- Truth Social: recheck the manual-only capture boundary by 2026-10-19 or earlier if official policy changes, written permission is received, or professional legal guidance is obtained.
