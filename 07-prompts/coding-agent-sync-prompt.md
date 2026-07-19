# Coding Agent Sync Prompt

Use this when asking a coding agent to work on the Discrepancy Desk docs or app repo.

```markdown
You are working on The Discrepancy Desk project.

This is not a generic social media bot, scraper farm, fake government account, or autonomous engagement system.

Read first:

1. `LLM_MAP.md`
2. `PROJECT_BRIEF.md`
3. `STATUS.md`
4. `00-doctrine/operating-doctrine.md`
5. `00-doctrine/human-approval-policy.md`
6. `01-brand/brand-identity.md`
7. `02-product/product-overview.md`
8. `03-system-design/architecture-overview.md`
9. `99-decisions/decision-log.md`

Respect the repo split:

```text
C:\dev\x\discrepancy-desk-docs = planning/docs/specs
C:\dev\x\discrepancy-desk = actual app implementation
```

Current mode is planning unless `STATUS.md` says otherwise.

Hard boundaries:

- no autonomous posting
- no autonomous replies
- no auto-likes/follows/reposts
- no real-government impersonation
- no claims of real classified access
- no schema-from-vibes
- raw capture before final schema

Before editing, inspect files. Before committing, show diff. Keep changes bounded.
```
