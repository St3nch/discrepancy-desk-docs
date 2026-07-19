# Human Approval Policy

## Policy

No content is posted publicly unless a human approves it.

This includes:

- original posts
- replies
- quote posts
- threads
- profile changes
- pinned posts
- image captions
- generated memes
- cross-posts

## Approval State Machine

Initial planning state machine:

```text
idea_captured
→ research_needed
→ researched
→ draft_generated
→ fact_source_review_needed
→ human_review_needed
→ approved
→ scheduled_or_manual_ready
→ published
```

Early implementation may simplify this, but should not remove the human review gate.

## Exact Text Binding

Approval applies to the exact content approved.

If the content is changed after approval:

```text
approved → human_review_needed
```

## Manual Posting First

Early versions should use manual copy/paste to X.

The app may record:

- approved final text
- copy timestamp
- manual published URL
- publication status

But it must not write to X automatically in early milestones.

## Never Allowed Without Explicit Future Reapproval

- autonomous X posting
- autonomous X replies
- automatic likes
- automatic follows
- automatic reposts
- engagement-pod behavior
- AI keyword-triggered replies
