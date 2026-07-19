# M01 Work Package C — Controlled Read-Only Probe Procedure

## Status

Prepared for owner review and separate execution approval.

This document does not authorize an X API request. Work Package C execution begins only after the owner explicitly approves the exact procedure and cost ceiling.

## Purpose

Run the smallest useful read-only X API probe needed to inspect real response shapes before M02 persistence planning.

The probe is evidence collection, not application implementation and not recurring collection.

## Fixed Boundary

Allowed:

- one authenticated identity request;
- one owned-post timeline request;
- one owned-mentions request;
- one direct lookup request for the two already-published project posts;
- preservation of raw responses, request metadata, headers, hashes, errors, empty results, and observed cost.

Not allowed:

- writes of any kind;
- posting, replying, liking, following, reposting, bookmarking, or DMs;
- recent search, full-archive search, streaming, webhooks, subscriptions, follower graphs, or recurring polling;
- database, schema, migration, dashboard, or persistence implementation;
- broad field guessing or repeated requests for convenience;
- any credential in source code, files, screenshots, command history, evidence, or chat.

## Authentication

Use the approved read-only OAuth 1.0a User Context credentials stored in Bitwarden.

Required session-only environment-variable names:

```text
DD_X_API_KEY
DD_X_API_KEY_SECRET
DD_X_ACCESS_TOKEN
DD_X_ACCESS_TOKEN_SECRET
```

Rules:

- values are entered by the owner into a local session only;
- values are never written into either repository;
- no `.env` file is created;
- no command that echoes or prints a secret is allowed;
- the probe process reads the variables but never logs them;
- close the terminal session after evidence preservation and verification;
- if any secret appears in output, stop, revoke/regenerate the exposed credential, and preserve only a redacted incident record.

## Execution Method

Use a one-shot local Python procedure executed from the main project directory.

The procedure must:

- use OAuth 1.0a request signing;
- issue only the four admitted GET requests in the fixed order below;
- apply a connection/read timeout;
- disable automatic retries;
- never follow an unexpected redirect containing authorization material;
- capture response body bytes before parsing;
- preserve response status and selected non-secret headers;
- write raw bodies exactly as returned;
- fail closed on an unexpected endpoint, method, permission, redirect, charge, or credential leak;
- exit after the fixed request list completes or a stop condition occurs.

The exact script or commands must be reviewed before execution and preserved in the evidence directory. Script creation is part of the separately approved execution batch, not this planning batch.

## Approved Evidence Location

Evidence belongs in the main project directory:

```text
C:\dev\x\discrepancy-desk\evidence\api-probes\x\m01\
```

Required layout:

```text
README.md
probe_manifest.json
controlled-procedure.py
requests/
  001_users_me.request.json
  002_owned_posts.request.json
  003_mentions.request.json
  004_post_lookup.request.json
raw/
  001_users_me.json
  002_owned_posts.json
  003_mentions.json
  004_post_lookup.json
headers/
  001_users_me.headers.json
  002_owned_posts.headers.json
  003_mentions.headers.json
  004_post_lookup.headers.json
hashes.sha256
execution-summary.md
```

Do not create this evidence directory until Work Package C execution is explicitly approved.

## Fixed Request Order

### P01 — Authenticated Identity

```text
GET https://api.x.com/2/users/me
```

Purpose:

- verify OAuth 1.0a user context;
- establish the authenticated account ID;
- inspect the live User object.

Requested user fields:

```text
created_at,description,entities,id,location,name,pinned_tweet_id,profile_image_url,protected,public_metrics,url,username,verified,verified_type,withheld
```

No expansion is required unless the endpoint supports and justifies `pinned_tweet_id` expansion without widening the request.

### P02 — Owned Posts

```text
GET https://api.x.com/2/users/{authenticated_user_id}/tweets
```

Parameters:

```text
max_results=5
exclude=retweets
```

Requested post fields:

```text
attachments,author_id,conversation_id,created_at,edit_controls,edit_history_tweet_ids,entities,in_reply_to_user_id,lang,possibly_sensitive,public_metrics,referenced_tweets,reply_settings,source,text,withheld
```

Requested expansions:

```text
author_id,attachments.media_keys,referenced_tweets.id,referenced_tweets.id.author_id,in_reply_to_user_id
```

Requested user fields:

```text
created_at,description,id,location,name,profile_image_url,protected,public_metrics,url,username,verified,verified_type
```

Requested media fields:

```text
alt_text,duration_ms,height,media_key,preview_image_url,public_metrics,type,url,width
```

Purpose:

- capture the first and pinned posts through the owned timeline;
- inspect text, media, alt text, public metrics, edit-history, and reference shapes;
- confirm owned-read billing behavior.

### P03 — Mentions

```text
GET https://api.x.com/2/users/{authenticated_user_id}/mentions
```

Parameters:

```text
max_results=5
```

Use the same conservative post, user, media fields, and expansions as P02.

Purpose:

- inspect mention response shape;
- preserve an empty result as valid evidence if no mentions exist;
- avoid manufacturing test mentions.

### P04 — Direct Lookup of Published Posts

```text
GET https://api.x.com/2/tweets
```

IDs:

```text
2078860810688848010
2078869974622392424
```

Use the same conservative fields and expansions as P02.

Purpose:

- compare direct-lookup response shape with owned timeline shape;
- confirm the image post's media and alt-text representation;
- observe standard post-read billing for known resources.

## Deferred Requests

Do not execute in the first probe run:

- recent search by `conversation_id`;
- quote-post lookup;
- reply-thread expansion;
- private or organic metrics follow-up;
- a second page of timelines;
- any request for a sample type that does not yet exist.

These may be proposed later only if the first four responses leave a specific unresolved question and the owner approves the additional request and cost.

## Cost Boundary

Current official pricing recheck records:

- User read: `$0.010` per returned user resource;
- standard Post read: `$0.005` per returned post resource;
- qualifying owned timeline reads: `$0.001` per returned resource.

Expected first-run cost, assuming one User object, two owned posts, no mentions, and two direct post resources:

```text
P01: approximately $0.010
P02: approximately $0.002
P03: $0.000 if empty; up to $0.005 for five owned mentions
P04: approximately $0.010
Expected total: approximately $0.022 to $0.027
```

Cost assumptions are estimates, not guarantees.

Hard Work Package C execution ceiling:

```text
$0.10 total observed or reasonably attributable API cost
```

Stop immediately if:

- estimated or observed cost reaches `$0.10`;
- the console shows unexpected unit pricing;
- an endpoint appears not to qualify for the expected billing class;
- duplicate or automatic retries occur;
- an unexpected number of resources is returned.

The existing Developer Console billing-cycle cap remains `$5.00`; the `$0.10` probe ceiling is the tighter operational limit.

## Request Evidence Record

Each request record must contain only:

- probe ID;
- UTC timestamp;
- HTTP method;
- endpoint path and API version;
- non-secret query parameters;
- authentication class: `OAuth 1.0a User Context`;
- app ID: `33215850`;
- requested fields and expansions;
- HTTP status;
- response byte count;
- returned primary-resource count;
- returned included-object counts by type;
- response classification: complete, partial, empty, rejected, or errored;
- raw response path;
- selected header evidence path;
- SHA-256 hash;
- redaction status;
- observed or estimated cost;
- operator note.

Do not record:

- Authorization headers;
- OAuth signatures, nonces, or timestamps from the signed header;
- keys, tokens, secrets, cookies, payment details, or Bitwarden metadata;
- complete environment listings;
- unredacted exception dumps that may include request headers.

## Selected Response Headers

Preserve only headers useful for evidence and rate-limit/billing review, such as:

```text
content-type
content-length
date
x-rate-limit-limit
x-rate-limit-remaining
x-rate-limit-reset
x-response-time
x-transaction-id
```

Preserve only headers actually returned. Exclude cookies, authorization material, tracing data that contains request URLs with secrets, and any header not reviewed as safe.

## Raw Evidence and Hashing

- save the response body exactly as received, including error and empty bodies;
- do not pretty-print or normalize the raw file before hashing;
- parsed summaries may be separate files later;
- compute SHA-256 for every request record, raw body, safe-header record, controlled procedure, manifest, and summary;
- write hashes using relative paths;
- regenerate `hashes.sha256` only through a reviewed deterministic command;
- after hashing, do not modify evidence files in place;
- corrections create a new file or a clearly recorded new run, never silent replacement.

## Preflight Checklist

Before the first request:

- owner explicitly approves Work Package C execution and `$0.10` ceiling;
- app remains active and read-only;
- OAuth 1.0a access token still shows `Read` for `@DiscrepancyDesk`;
- Bitwarden contains all four current OAuth 1.0a values;
- no bearer token or OAuth 2.0 token is used;
- Developer Console credit balance and `$5.00` spend cap are confirmed;
- auto-recharge remains off/unavailable;
- official endpoint fields, limits, and prices are rechecked the same day;
- evidence directory does not already contain a prior run that would be overwritten;
- exact script and dependency choices are reviewed;
- terminal history and logging behavior will not expose secrets;
- no unrelated files are dirty or included in evidence.

## Fail-Closed Stop Conditions

Stop without continuing to the next request when:

- authentication fails in a way that suggests stale or mismatched credentials;
- any secret is displayed, logged, serialized, or written;
- the request method is not GET;
- the resolved host is not `api.x.com`;
- a redirect occurs unexpectedly;
- X requires write or DM permission;
- a field or expansion is rejected and repeating the request would be guesswork;
- the response cannot be preserved safely;
- the returned resource count exceeds the planned maximum;
- unexpected billing or rate-limit behavior appears;
- the procedure attempts a retry;
- an evidence path already exists and would be overwritten;
- the app permission has drifted from read-only;
- the `$0.10` operational ceiling would be reached.

A failed or rejected request remains evidence. Do not delete it and do not automatically retry it.

## Execution Approval Required

The owner must explicitly approve all of the following before execution:

```text
Four-request sequence: P01 through P04
OAuth 1.0a User Context
Maximum five owned posts
Maximum five mentions
Two fixed direct-lookup IDs
$0.10 hard execution ceiling
Evidence directory and no-overwrite rule
No retries and fail-closed stop conditions
```

## Completion Conditions

Work Package C execution is complete only when:

- the approved fixed sequence has completed or stopped safely;
- every attempted request has a request record, raw body, safe-header record, and SHA-256 hash;
- no credential appears in evidence;
- actual/observed cost is recorded;
- Developer Console balance and usage are checked after execution;
- evidence integrity is verified;
- exact procedure and result summary are preserved;
- no write capability was exercised;
- Work Package D analysis is authorized as the next bounded batch.
