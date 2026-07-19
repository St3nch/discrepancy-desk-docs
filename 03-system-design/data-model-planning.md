# Data Model Planning

## Rule

Do not finalize schema from vibes.

Before final schema, preserve real sample payloads from X API probes and manual captures.

## Initial Conceptual Tables

Potential early SQLite tables:

```text
accounts
raw_captures
sources
source_notes
claims
entities
entity_mentions
posts
drafts
human_reviews
published_posts
metrics_snapshots
model_outputs
api_calls
audit_log
connection_candidates
connection_evidence
```

## Important Relationships

Article/source flow:

```text
sources → posts → replies/comments → reply drafts/human reviews → metrics_snapshots
```

Generic content flow:

```text
raw_captures → sources/ideas/posts → drafts → human_reviews → published_posts → metrics_snapshots
```

## Post Fields to Consider

- source_id
- contains_url
- url_count
- primary_url
- content_angle
- claim_label
- parent_post_id
- conversation_id
- published_url
- platform_post_id
- final_text_hash
- approval_status

## Raw Capture Table

Early raw-capture table:

```text
api_probe_raw(
  provider,
  endpoint,
  request_url,
  request_params_json,
  response_status,
  response_headers_json,
  response_body_json,
  captured_at
)
```

## Approval Binding

Approval should bind to exact final content hash.

If final text changes, approval resets.
