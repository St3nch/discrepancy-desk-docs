# Architecture Overview

## High-Level Architecture

```text
Public X Account
    ↑ manual posting
B1-L7 Control Room
    ↑ ↓
SQLite operational database
    ↑ ↓
Anomaly Vault / Obsidian notes
    ↑ ↓
Qdrant semantic index later
    ↑ ↓
LLM context builder / drafting assistant
```

## Early Stack

```text
Python
FastAPI
SQLite
Alembic
Jinja/HTMX
plain JS only where needed
```

## Why Local-First

- cheaper
- faster
- controllable
- private
- easy to inspect
- less infrastructure drag
- less overbuilt goblin machine

## Database Role

SQLite is the structured source of truth for operational data:

- accounts
- captures
- sources
- posts
- drafts
- reviews
- approvals
- published URLs
- metrics snapshots
- raw API responses
- audit log

## Obsidian Role

Obsidian-compatible Markdown is the human-readable research vault:

- topic notes
- source notes
- claim notes
- entity notes
- connection candidates
- future post ideas

## Qdrant Role

Qdrant is delayed until there is enough content.

Qdrant should index chunks from:

- Obsidian notes
- source summaries
- claims
- entities
- published posts
- high-performing drafts

Qdrant returns IDs; SQLite/Markdown remain the source of truth.

## LLM Role

The LLM drafts, summarizes, compares, retrieves, and formats.

The LLM does not decide truth or publish content.
