# X API Probe Plan

## Purpose

Capture a deliberately small set of real X API payloads before persistence design.

Do not schema-from-vibes. Do not turn a probe into a collector.

## Admission

Read-only only.

No write actions.
No posting.
No replying.
No liking, following, reposting, bookmarking, or DMs.
No recurring polling.
No database implementation.

Current policy, pricing, and rate-limit verification:

```text
06-research/x-policy-api-verification-2026-07-19.md
```

Recheck the Developer Console immediately before buying credits or executing the probe.

## Cost Boundary

- Owner approval is required before purchasing credits.
- Disable automatic recharge.
- Set a low spending limit.
- Prefer owned-read endpoints priced at the current documented owned-read rate.
- Request the smallest useful result counts.
- Do not repeat requests merely for convenience.
- Stop on unexpected charges, undocumented access behavior, or permission drift.

## Probe Packages

### Package A — Identity

```text
GET /2/users/me
```

Purpose:

- verify authenticated account identity;
- inspect the current User object;
- establish the account ID used by owned-read endpoints.

### Package B — Owned Posts

```text
GET /2/users/:id/tweets
```

Purpose:

- inspect original posts, replies, quotes, URLs, and later media records;
- preserve pagination and response metadata;
- test owned-read pricing behavior.

### Package C — Mentions

```text
GET /2/users/:id/mentions
```

Purpose:

- inspect mention and reply shapes;
- understand author, conversation, and referenced-post relationships.

### Package D — Direct Post Lookup

```text
GET /2/tweets?ids=...
GET /2/tweets/:id
```

Purpose:

- compare lookup payloads against timeline payloads;
- inspect expansions and missing-field behavior.

### Package E — Conversation and Quote Context

Only when sample posts exist and cost remains inside the approved budget:

```text
GET /2/tweets/search/recent?query=conversation_id:<OUR_POST_ID>
GET /2/tweets/:id/quote_tweets
```

Purpose:

- inspect reply-thread and quote relationships;
- determine whether these endpoints add enough information to justify inclusion.

Do not probe full-archive search, streaming, follower graphs, trends, DMs, or write endpoints in M01.

## Fields to Request

### Post fields

```text
attachments
author_id
conversation_id
created_at
edit_controls
edit_history_tweet_ids
entities
geo
id
in_reply_to_user_id
lang
non_public_metrics
organic_metrics
possibly_sensitive
promoted_metrics
public_metrics
referenced_tweets
reply_settings
source
text
withheld
```

Private or owner-only metrics may be unavailable or permission-dependent. Their absence is evidence and must be recorded; do not widen permissions casually.

### User fields

```text
created_at
description
entities
id
location
name
pinned_tweet_id
profile_image_url
protected
public_metrics
url
username
verified
verified_type
withheld
```

### Expansions

```text
author_id
attachments.media_keys
referenced_tweets.id
referenced_tweets.id.author_id
in_reply_to_user_id
```

Request only supported fields for each endpoint. Record rejected fields rather than repeatedly guessing.

## Required Sample Types

Use owned/public project posts only:

- plain original post;
- original post with article URL;
- reply;
- quote post;
- image/media post when available;
- replies under an owned URL/article post when available.

Do not create junk posts solely to satisfy a schema fantasy. If a sample type does not yet exist, record it as deferred.

## Evidence Location

Probe evidence belongs in the application repo, not the planning repo:

```text
C:\dev\x\discrepancy-desk\evidence\api-probes\x\m01\
```

Suggested layout:

```text
README.md
probe_manifest.json
raw/
  001_users_me.json
  002_owned_posts.json
  003_mentions.json
  004_post_lookup.json
  005_conversation_replies.json
  006_quote_posts.json
requests/
  001_users_me.request.json
  ...
hashes.sha256
payload-field-inventory.md
schema-implications.md
unknowns.md
```

No credentials, authorization headers, access tokens, refresh tokens, API secrets, or unredacted environment dumps may enter evidence.

## Manifest Requirements

For every request, record:

- probe ID;
- UTC timestamp;
- endpoint and API version;
- authentication class, without secrets;
- requested fields, expansions, and result count;
- response HTTP status;
- rate-limit headers;
- returned resource counts;
- actual or observed credit usage when available;
- raw response path;
- SHA-256 hash;
- redaction status;
- operator note;
- whether the response is complete, partial, empty, rejected, or errored.

Empty and error responses are evidence. Do not delete them because they are ugly.

## Fail-Closed Rules

Stop the probe when:

- a request would require write permissions;
- credentials appear in output;
- the endpoint behavior differs materially from current official documentation;
- spending exceeds or approaches the approved limit;
- the app or account permissions are broader than necessary;
- the response cannot be preserved without exposing secrets;
- X changes pricing or access terms mid-probe.

## Required Outputs

After the probe, produce:

- immutable raw samples;
- manifest and hashes;
- payload summary;
- field and relationship inventory;
- endpoint/access/cost observations;
- missing-field and unknowns report;
- schema implications;
- explicit list of conclusions that cannot yet be drawn;
- bounded input for M02.

The output may suggest candidate structures. It does not approve database tables.