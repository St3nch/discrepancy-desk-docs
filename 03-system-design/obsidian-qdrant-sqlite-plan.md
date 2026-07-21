# Status — Historical Research and Superseded Architecture

This document is retained as historical research and early planning context. It is **not** the controlling M06 architecture.

The accepted architecture is governed by:

- `05-implementation-planning/m06-architecture-synthesis.md`;
- `99-decisions/m06-owner-architecture-rulings.md`;
- `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`.

Where this document describes editable Obsidian notes, Markdown-centered authority, shared Vault behavior, or earlier Qdrant assumptions, the accepted M06 rulings supersede it. Markdown and HTML are generated read-only projections in M06-A; SQLite and governed filesystem records remain canonical; Qdrant remains deferred to M14.

---

# Obsidian + Qdrant + SQLite Plan

## Core Model

```text
Obsidian Vault = human-readable notes
SQLite = structured operational source of truth
Qdrant = semantic search index
LLM = context retrieval + drafting assistant
```

## Obsidian Use

The Anomaly Vault can be an Obsidian-compatible Markdown folder.

Suggested structure:

```text
Anomaly Vault/
  00 Inbox/
  01 Topics/
  02 Sources/
  03 Claims/
  04 Entities/
  05 Connection Candidates/
  06 Post Ideas/
  07 Drafts/
  08 Published Notes/
  09 Templates/
```

## Qdrant Use

Qdrant should not be the source of truth.

It indexes chunks and returns pointers.

Payload example:

```json
{
  "vault_path": "02 Sources/smithsonian-giants-claim.md",
  "note_type": "source",
  "topic": "giants",
  "claim_label": "disputed",
  "entities": ["Smithsonian Institution", "giants", "newspaper archives"],
  "sqlite_source_id": 42
}
```

## When to Add Qdrant

Do not add Qdrant immediately.

Add it when:

- enough notes exist
- search by keyword becomes insufficient
- “what does this remind us of?” becomes valuable
- No Coincidences needs semantic similarity

## SQLite Role

SQLite stores:

- exact IDs
- state machine status
- approvals
- source metadata
- published URLs
- metrics
- raw captures
- audit log

## Good Principle

SQLite holds the files. Qdrant remembers where the bodies are alphabetized.
