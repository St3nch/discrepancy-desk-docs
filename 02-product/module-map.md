# Module Map

## Public Layer

### The Discrepancy Desk

The public-facing social account and eventual site.

### Quinton Clearance

The fictional persona/voice.

## Private Local System

### B1-L7 Control Room

The local dashboard/app for operations.

Functions:

- capture inbox
- draft queue
- approval queue
- published log
- metrics tracking
- source/research connection

### Anomaly Vault

Research memory layer.

Stores:

- topics
- sources
- claims
- entities
- notes
- source summaries
- future leads
- post ideas

### No Coincidences

Pattern-candidate detector.

It flags weird overlaps but does not declare truth.

### Drawer 17

Recurring series/format for especially weird pattern candidates or “this drawer opened itself” content.

### The Humming

Running gag. Never fully explained.

## Supporting Tooling

### gpt-mcp

Separate local MCP helper repo intended to expose safe filesystem/Git tools to ChatGPT.

Local path:

```text
C:\dev\gpt-mcp
```

## Data Layer Roles

```text
SQLite = operational source of truth
Obsidian vault = human-readable research notes
Qdrant = semantic retrieval/index later
Raw JSON = provider/API evidence
```
