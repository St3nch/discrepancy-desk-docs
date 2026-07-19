# Research Log

Tracks accepted research findings that other docs depend on, separate from product/brand decisions in `decision-log.md`.

Each entry should be re-checked at or before the "Recheck by" point. Findings sourced from official documentation still drift — platforms change rules and endpoints without notice.

## R001 — X API Is Pay-Per-Use, No Free Tier (as of research date)

Accepted:

X moved to pay-per-use pricing for new developers on February 6, 2026. No free tier for new signups; Basic/Pro closed to new signups. Enterprise starts at ~$42,000/month.

Source: third-party trackers citing X's pricing page (which defers exact figures to the Developer Console).

Confidence: Moderate (exact current rate card not independently re-verified at time of this log entry).

Recheck by: before Milestone 01 (X API Probe) begins, and again before any API spend is committed.

## R002 — Truth Social Has No Public Developer Program

Accepted:

As of July 19, 2026, Truth Social has no public developer registration, no API keys, no third-party OAuth client registration process. The only official API ("Truth API," announced July 16, 2026) is a paid, institutional-only, read-only data feed for financial services partners — not a general developer platform.

Source: `06-research/truth-social-platform-research.md` (full citations there).

Confidence: High for "no public program exists today." Unresolved whether any non-financial written-permission path exists.

Recheck by: before any Truth Social API integration is considered; otherwise recheck opportunistically (e.g., every few months) since this is a fast-moving area.

## R003 — Truth Social Automation Is Prohibited by ToS

Accepted:

Truth Social's Terms of Service (§4(6), §7) prohibit automated/non-human access, bots, scripts, scrapers, and systematic retrieval/compilation without written permission.

Source: `06-research/truth-social-platform-research.md`, §6.

Confidence: High (primary source, quoted directly).

Recheck by: before any capture or publishing tooling ships; ToS documents change without notice per the platform's own disclaimer.

## R004 — Truth Social Content and Media Limits

Accepted:

Post text limit: 1,000 characters (official help docs). Share-composer prefill: 500 characters. Up to 4 photos or 1 video per post; video up to 450 MB / 15 minutes.

Source: `06-research/truth-social-platform-research.md`, §11.

Confidence: High, from official help documentation. Note: some third-party sources claim a 500-character post limit rather than 1,000 — the documented figure (1,000) is preferred here, but this residual discrepancy is unresolved.

Recheck by: before finalizing any draft-length UI constraints in the dashboard or extension.

## Open Items Not Yet Logged as Findings

- X: PCF-compliant keyword requirement in account **display name** (not just bio) — flagged as needing verification, not yet independently confirmed either way.
- X: current exact account-level daily post caps for Premium vs. unverified accounts.

These remain open questions in `STATUS.md` under "Open Verification Items" until resolved into a numbered entry here.

## R005 — Truth Social Browser/DOM Capture Not Admitted

Accepted: 2026-07-19.

Truth Social's current Terms prohibit automated/non-human access, scripts, extraction tools, and systematic retrieval used to build collections or databases without written permission. A human click does not remove the scripted/extraction character of a browser content script.

Current boundary: URLs, human-authored notes, owned-content records, official share tools/bookmarks, manual metrics, and official owner export only. No Truth Social DOM parser, content script, background capture, automated screenshotting, or third-party content archive.

Source: `06-research/truth-social-capture-boundaries.md`.

Confidence: High for the conservative product boundary; legal interpretation remains non-final and may be revisited only with written permission, revised official policy, or professional legal guidance.

Recheck by: 2026-10-19, or earlier upon a relevant policy change.
