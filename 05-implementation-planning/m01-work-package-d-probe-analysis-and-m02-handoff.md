# M01 Work Package D — Probe Analysis and M02 Handoff

## Status

Complete.

The successful controlled probe executed on 2026-07-19 and the final Developer Console billing check are both recorded. M01 handoff inputs are complete.

## Evidence

Successful run:

```text
C:\dev\x\discrepancy-desk\evidence\api-probes\x\m01\attempt-002\
```

Attempt 001 remains preserved separately. It stopped fail-closed on P01 with HTTP 401 because the first credential-loading method placed only one character in each environment variable. No retry occurred in that run.

Attempt 002 results:

| Probe | Purpose | HTTP | Resources |
|---|---|---:|---:|
| P01 | authenticated identity | 200 | 1 |
| P02 | owned posts | 200 | 2 |
| P03 | mentions | 200 | 1 |
| P04 | direct post lookup | 200 | 2 |

```text
Estimated attributable cost: $0.023
Hard ceiling: $0.10
Automatic retries: 0
Hash verification: HASHES_OK
```

## Key Findings

### Account identity

The API returned a stable numeric account ID distinct from username and display name. Mutable profile observations included username, name, description, location, verification state, pinned-post ID, and public metrics.

Implications:

- external account ID is the candidate identity anchor;
- profile fields are time-bound observations;
- public metrics require capture timestamps;
- empty fields must remain empty rather than being silently invented.

### Owned posts

The owned timeline returned both published project posts. Observed fields included post ID, author ID, conversation ID, created time, text, language, sensitivity flag, reply settings, edit controls, edit-history IDs, public metrics, attachments, and entities.

The pinned image post returned a media-key relationship and an expanded media object containing media type, URL, dimensions, and exact alt text.

Implications:

- media is a separate referenced resource;
- API-returned post text may include a generated `t.co` media URL not present in the human-approved authored copy;
- exact approved authored text must remain distinct from platform-returned text;
- metrics are snapshots, not permanent post attributes;
- edit-history array presence alone does not prove an edit occurred.

### Direct lookup

Direct lookup returned substantially the same post, author, media, alt-text, and metric shapes as the owned timeline.

Implications:

- endpoint and request provenance still matter even when payloads overlap;
- similar payloads do not justify discarding raw endpoint evidence;
- direct lookup may support targeted future refreshes, but recurring collection is not admitted by M01.

### Mentions

The mentions endpoint returned one unsolicited invitation-style mention from an unrelated account. The content had spam-like risk indicators.

This report does not claim confirmed malicious intent.

Implications:

- a mention is evidence of a platform event, not proof of meaningful engagement;
- later workflow may need human states such as `unreviewed`, `relevant`, `spam_candidate`, `ignored`, and `actionable`;
- original mention evidence must not be deleted merely because a human classifies it as spam;
- no automated reply, link-following, or engagement action is justified.

## Observed Relationships

```text
User.id -> Post.author_id
User.pinned_tweet_id -> Post.id
Post.attachments.media_keys[] -> Media.media_key
Post.entities.urls[].media_key -> Media.media_key
Mention.entities.mentions[].id -> mentioned User.id
Mention.author_id -> included author User.id
```

Observed original posts had `conversation_id == id`, but this is not yet a universal invariant.

## Candidate Resource Fields

### User

```text
id, username, name, description, location, url, created_at,
protected, verified, verified_type, profile_image_url,
pinned_tweet_id, public_metrics
```

### Post

```text
id, author_id, conversation_id, created_at, text, lang,
possibly_sensitive, reply_settings, edit_controls,
edit_history_tweet_ids, attachments, entities, public_metrics,
referenced_tweets when present, in_reply_to_user_id when present
```

### Media

```text
media_key, type, url, preview_image_url when present,
width, height, duration_ms when present, alt_text when present,
public_metrics when present
```

### Observation and provenance

```text
probe ID, UTC capture time, endpoint, API version, HTTP status,
requested fields and expansions, authentication class,
resource count, safe rate-limit headers, raw path, SHA-256,
redaction state, billing class, cost, completion/error state
```

These are candidate concepts for M02, not approved tables or columns.

## Access, Cost, and Retention Notes

All four attempt-002 requests returned HTTP 200. Endpoint-specific rate-limit headers were preserved. The probe remained far below the documented limits; rate-limit capacity is not spending permission.

The runner estimated `$0.023`. The Developer Console later showed `$4.98` remaining from the `$5.00` purchased balance, so the recorded actual billed cost is `$0.02` after console rounding. Auto-recharge remained off/unavailable and the `$5.00` spend cap remained in place.

Raw responses may include third-party usernames, IDs, text, links, profile metadata, and external media locators. Later retention must remain bounded to legitimate project use and platform policy.

Media and profile-image URLs are locators, not guaranteed permanent archives or unrestricted republication rights.

## M02 Schema Implications

M02 should evaluate, without implementing:

1. Stable external identifiers separated from mutable display attributes.
2. Raw evidence separated from normalized operational records.
3. Endpoint, request, capture-time, and hash provenance retained.
4. User, post, media, metric snapshot, and observation concepts kept distinct enough to preserve optional relationships.
5. Exact approved authored text separated from API-returned text.
6. Human review states for mentions and external interactions.
7. Explicit handling of missing, empty, withheld, unavailable, and errored values.
8. No assumption that timeline, lookup, mention, reply, quote, and search payloads are interchangeable.

## Candidate Hammer-Test Inputs

The roadmap-required owner clarification remains mandatory. Probe-derived candidate tests include:

- reject fabricated account, post, media, and evidence IDs;
- reject normalized records linked to missing raw evidence;
- detect modified raw bytes through SHA-256 mismatch;
- reject approval binding when API-returned text replaces exact approved authored text;
- preserve empty fields instead of inventing values;
- reject metric snapshots lacking capture time and source context;
- preserve original mention evidence after human classification;
- fail closed on HTTP errors, malformed JSON, scope drift, redirects, and credential exposure;
- prevent duplicate runs from overwriting earlier evidence;
- prevent detector or classifier failure from auto-promoting a mention or pattern candidate;
- prove migration and recovery preserve hashes and identifier relationships.

These are inputs, not the final M02 contract.

## Explicit Non-Conclusions

The probe does not prove:

- every X response will contain the same fields;
- current pricing and rate limits will remain stable;
- the console's rounded `$0.02` charge reveals the exact per-endpoint billing allocation;
- the observed mention was definitively harmful;
- metrics are complete or comparable across platforms;
- every original post always has `conversation_id == id`;
- edit-history arrays prove that an edit occurred;
- media URLs are permanent archives;
- private metrics are available;
- the final schema is known;
- recurring collection or write access is justified.

## M02 Handoff Questions

Before persistence design, resolve:

1. What exact failure modes, invariants, adversarial cases, and evidence requirements does the owner require?
2. What is the smallest operational record for capture, drafting, exact-text approval, manual publication, and metrics?
3. Which raw payloads are retained, for how long, and under what deletion and recovery rules?
4. How are authored text, platform-returned text, rendered text, and edited versions distinguished?
5. How are external IDs validated without treating API data as infallible?
6. What lifecycle applies to mentions, spam candidates, ignored items, and actionable interactions?
7. What evidence proves migration, rollback, backup, restore, and hash integrity?
8. Which deliberately broken fixtures must fail before implementation is admitted?

## M01 Closeout Result

Developer Console evidence supplied by the owner showed:

```text
Purchased balance: $5.00
Remaining balance: $4.98
Actual billed cost recorded for M01: $0.02
Auto-recharge: off/unavailable
Billing-cycle spend cap: $5.00
```

The console rounds the displayed balance to cents, while the runner estimated `$0.023` from returned-resource counts. The console-observed `$0.02` is the authoritative closeout amount.
