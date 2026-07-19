# X API Probe Plan

## Purpose

Before designing final schema, capture real X API payloads and inspect them.

Do not schema-from-vibes.

## Probe Type

Read-only.

No write actions.
No posting.
No replying.
No liking/following/reposting.

## Target Probes

Potential endpoints to inspect:

```text
GET /2/users/me
GET /2/users/:id/tweets
GET /2/users/:id/mentions
GET /2/tweets/search/recent?query=conversation_id:<OUR_POST_ID>
GET /2/tweets/:id/quote_tweets
GET /2/tweets?ids=...
```

## Fields to Request

Tweet fields to inspect:

```text
attachments
author_id
conversation_id
created_at
entities
geo
id
in_reply_to_user_id
lang
possibly_sensitive
public_metrics
referenced_tweets
reply_settings
source
text
withheld
```

User fields:

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

Expansions:

```text
author_id
attachments.media_keys
referenced_tweets.id
referenced_tweets.id.author_id
in_reply_to_user_id
```

## Raw Files

Save samples under:

```text
api-probes/x/
```

Suggested files:

```text
001_users_me.json
002_my_recent_posts.json
003_mentions.json
004_conversation_replies.json
005_quote_posts.json
006_tweet_lookup_by_ids.json
probe_manifest.json
```

## Probe Post Types

Need samples for:

- plain original post
- original post with article URL
- reply
- quote post
- image/media post later
- replies under URL/article post

## Output

After probes, produce:

- payload summary
- field inventory
- schema implications
- unknowns
- recommended tables/columns
