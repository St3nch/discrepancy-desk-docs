# Multi-Account Model

## Future Direction

The system may eventually support multiple social accounts, but not in early MVP UI.

Add `account_id` to relevant tables from the beginning, but do not build complex multi-account workflows early.

## Policy

Each account must have:

- distinct purpose
- distinct voice
- distinct audience
- distinct content lane

Accounts must not coordinate for fake engagement.

## Early Example Accounts

Potential future accounts:

```text
Discrepancy Desk = weird history / conspiracy lore / anomalies
GoingBulk = fitness / meal prep / body project
SearchClarity = SEO / marketplace visibility business
AI/project account = coding agents / systems work
```

## Forbidden Coordination

Do not build:

- “all accounts repost this” button
- automatic cross-amplification
- fake reply chains
- same content across multiple accounts
- engagement pod scheduler

## Data Model Hint

Include `account_id` early on:

- posts
- drafts
- captures
- sources, if account-specific
- reviews
- metrics snapshots
- published posts

But keep UI single-account at first.
