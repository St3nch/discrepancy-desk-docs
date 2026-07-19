# Truth Social Account Rules

Distilled from `06-research/truth-social-platform-research.md` (researched July 2026). Re-verify before launch and periodically after, since Truth Social states its rules and endpoints "may change without notice."

## Working Policy

Truth Social platform rules are a hard boundary, same as X.

## No Public Developer Access

There is no public developer registration, API key process, or written-permission process for general developers. The only official API ("Truth API," announced July 2026) is a paid, institutional-only, read-only data feed for financial services partners — not a developer platform, and out of scope for this project.

## Disallowed Behavior

The system must not:

- automate reading, scraping, or systematic retrieval of Truth Social content
- call undocumented or internal `/api/v1/...` endpoints directly
- reuse session cookies or embedded first-party client credentials
- auto-post, auto-reply, auto-like, auto-follow, auto-repost, or auto-DM
- build a compiled database of Truth Social content without written permission from TMTG
- reproduce or aggregate content for commercial purposes without written permission
- impersonate a real government agency or official

## AI Use

Allowed direction:

```text
AI drafts → human reviews → human manually posts via Truth Social's own UI
```

Danger direction:

```text
AI or extension calls a Truth Social endpoint directly
```

Do not build the danger direction, even for reads.

## Sanctioned Manual Publishing Path

- Official share composer: `https://truthsocial.com/share?title=...&url=...` (documented, prefills up to 500 characters).
- Official share-button and follow-button embeds.
- A human always presses "post" inside Truth Social's own composer. No extension or backend submits on the human's behalf.

## Profile Safety

Use "commentary/parody," "fictional records custodian," and "not affiliated with any government agency" framing, consistent with X account rules. Truth Social nominally permits parody accounts but enforcement is inconsistent — professional legal review is recommended before launch given the government-adjacent persona.

## Content Limits (documented)

- Post text: 1,000 characters.
- Share-composer prefill text: 500 characters.
- Up to 4 photos or 1 video per post.
- Video: up to 450 MB, max 15 minutes.

## Data Handling

May store: the project's own published post URLs, platform post IDs, timestamps, approved source text, manually observed metrics, source attribution.

Must not store: scraped raw API payloads, bulk search results, follower/replier lists, access tokens, or reused session cookies.

## First-Day Presence

No specific Truth Social first-follow strategy has been decided yet. Apply the same "avoid mass-follow, build character not bot" instinct used for X (see `x-account-rules.md`) unless a Truth Social-specific reason emerges to differ.
