# Research Log

Tracks accepted research findings that other docs depend on, separate from product/brand decisions in `decision-log.md`.

Each entry should be re-checked at or before the "Recheck by" point. Findings sourced from official documentation still drift — platforms change rules and endpoints without notice.

## R001 — Historical X API Pricing Note — Superseded

Status: Superseded by R007.

This entry originally recorded third-party reporting about X's 2026 pricing transition. It must not be used for current planning, cost estimates, or access claims.

Current official-source verification is recorded in:

```text
R007 — X API Pay-Per-Use and Owned-Read Pricing Verified
06-research/x-policy-api-verification-2026-07-19.md
```

Retained only to preserve research history and show why re-verification was necessary.

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

## Resolved X Verification Items

The former X PCF display-name and API-pricing questions were resolved on 2026-07-19 through official X sources and are now recorded as R006 and R007.

The project does not rely on a generic social-account daily-post estimate for M01. API endpoint rate limits, billing, and ordinary account anti-spam limits are distinct controls; the read-only probe follows the official endpoint limits and its much smaller owner-approved budget.

## R005 — Truth Social Browser/DOM Capture Not Admitted

Accepted: 2026-07-19.

Truth Social's current Terms prohibit automated/non-human access, scripts, extraction tools, and systematic retrieval used to build collections or databases without written permission. A human click does not remove the scripted/extraction character of a browser content script.

Current boundary: URLs, human-authored notes, owned-content records, official share tools/bookmarks, manual metrics, and official owner export only. No Truth Social DOM parser, content script, background capture, automated screenshotting, or third-party content archive.

Source: `06-research/truth-social-capture-boundaries.md`.

Confidence: High for the conservative product boundary; legal interpretation remains non-final and may be revisited only with written permission, revised official policy, or professional legal guidance.

Recheck by: 2026-10-19, or earlier upon a relevant policy change.

## R006 — X PCF Display-Name and Profile Requirements Verified

Accepted:

As of 2026-07-19, X's official Authenticity policy requires compliant Parody, Commentary, and Fan accounts that depict another entity to use a PCF label, avoid an identical avatar, place a term such as `parody`, `fake`, `fan`, or `commentary` at the beginning of the display name, and include a matching disclosure in the bio.

Project implication:

The conservative M01 posture is `Commentary | The Discrepancy Desk`, the Commentary PCF label, and explicit fictional/non-affiliation language in the bio.

Source: `06-research/x-policy-api-verification-2026-07-19.md`.

Confidence: High — official X policy.

Recheck by: before final public profile approval or after any relevant X policy change.

## R007 — X API Pay-Per-Use and Owned-Read Pricing Verified

Accepted:

As of 2026-07-19, X API v2 uses prepaid credit-based pay-per-use pricing. Official documentation lists Post reads at $0.005 per returned Post, User reads at $0.010 per returned User, and qualifying owned reads at $0.001 per resource. Relevant endpoint rate limits are documented separately and are not spending permissions. Pay-per-use plans are documented with a 2 million monthly Post-read cap.

The official X marketing page and detailed pricing documentation have shown inconsistent write-price figures. M01 is read-only, so this does not block the probe. The live Developer Console remains the final authority before any credit purchase.

Source: `06-research/x-policy-api-verification-2026-07-19.md`.

Confidence: High for read and owned-read pricing; Moderate for irrelevant write pricing.

Recheck by: immediately before buying credits and before probe execution.
