# Product Overview

## Product Name

Private/internal system name:

```text
B1-L7 Control Room
```

Public brand:

```text
The Discrepancy Desk
```

## Purpose

The B1-L7 Control Room is a local human-approved system for collecting leads, organizing sources, generating drafts, approving content, publishing manually, and tracking performance for The Discrepancy Desk.

## Main Workflow

```text
Capture → Store → Draft → Review → Approve → Manually Post → Record → Measure → Learn
```

## Early User Experience

The user should be able to:

- save a topic/source/reply/article/video lead
- see it in an inbox
- add notes
- ask for research/draft help
- review claim labels
- approve or reject drafts
- copy approved content to X manually
- paste back the published X URL
- track basic metrics later

## System Modules

- Capture Inbox
- Source Library
- Anomaly Vault
- Draft Desk
- Human Review Queue
- Published Log
- Metrics Ledger
- No Coincidences
- LLM Context Builder

## Early Stack Direction

```text
FastAPI
SQLite
Alembic
Jinja/HTMX
Manual copy-paste posting
Read-only X API probe before schema finalization
```

## Delayed Until Later

- React SPA
- Postgres
- Redis/Celery
- Qdrant
- browser automation
- auto-posting
- X write API
- analytics prediction
- multi-account UI
