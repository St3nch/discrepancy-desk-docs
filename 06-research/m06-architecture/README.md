# M06 Architecture Research Program

## Status

Authorized research workspace. No M06 implementation authority.

This directory contains the deep-research source material for the future Records, Dossiers, Anomaly Vault, ingestion, retrieval-context, and correlation architecture.

The controlling plan is:

```text
05-implementation-planning/m05-to-m06-transition-audit-and-research-plan.md
```

## Research Tracks

### Track A — Governed Vault and Research Memory

Determine how the system should preserve and organize:

- immutable originals;
- source occurrences and observations;
- normalized documents and transcripts;
- entities, events, claims, assertions, contradictions, and questions;
- dossiers, chronologies, and connection candidates;
- provenance, correction, supersession, and deletion;
- account isolation, backup, restore, and complete reconstruction;
- human-readable views without duplicate operational truth;
- future deterministic retrieval chunks and Qdrant invalidation.

### Track B — Manual-First and Future-Automated Ingestion

Determine how the system should accept:

- pasted text and transcripts;
- YouTube URLs, metadata, transcript files, and future transcript providers;
- public webpages;
- RSS/Atom and other feeds;
- PDFs, HTML, Markdown, text, images, screenshots, and structured files;
- human-triggered browser captures;
- official APIs and later admitted SaaS providers;
- future monitored channels, feeds, and websites.

The same governed ingestion package must support manual acquisition now and automated connectors later.

## Required Reports

```text
R-M06-01-source-universe-and-admission-policy.md — completed 2026-07-20
R-M06-02-youtube-audiovisual-ingestion.md — completed 2026-07-20
R-M06-03-website-feed-and-change-monitoring.md — completed 2026-07-20
R-M06-04-document-normalization-and-parser-provenance.md
R-M06-05-canonical-vault-knowledge-model.md
R-M06-06-identity-versioning-correction-and-deduplication.md
R-M06-07-llm-context-assembly-and-governed-reasoning.md
R-M06-08-retrieval-chunking-graph-and-qdrant-readiness.md
R-M06-09-automation-security-backup-and-rebuild.md
```

## Required Report Structure

Each report must use this structure:

```text
# Title

## Research status
## Question and project boundary
## Executive findings
## Verified facts
## Standards and provider capabilities
## Constraints and failure modes
## Project implications
## Candidate architecture options
## Recommended direction
## Rejected or unsafe shortcuts
## Open questions
## Inputs required by later reports
## Primary sources and access dates
```

## Evidence Labels

Every material statement must be distinguishable as one of:

```text
VERIFIED FACT
SOURCE/PROVIDER CLAIM
INFERENCE
PROJECT RECOMMENDATION
OPEN QUESTION
```

Do not convert a provider's advertised capability into project approval.

Do not interpret technical feasibility as legal, policy, retention, or automation permission.

## Research Rules

- Prefer current primary sources: official API documentation, standards, original research, official product terms, and authoritative security guidance.
- Record exact access dates.
- Distinguish official APIs from unofficial endpoints and browser extraction.
- Record pricing, quotas, retention, authentication, and export limitations when relevant.
- Treat transcript, OCR, extraction, entity, relationship, and correlation outputs as fallible derivatives.
- Preserve the project's human-authority and account-isolation doctrine.
- Do not design database tables or implementation code in individual research reports.
- Do not admit a provider, parser, connector, Qdrant, graph engine, monitor, or automation merely because research finds it feasible.

## Synthesis Gate

No report is doctrine by itself.

After all reports are reviewed, their accepted findings must be synthesized into the exact M06 architecture, canonical record model, ingestion contract, future connector interface, LLM context contract, adversarial matrix, and implementation plan.

Independent Claude review and explicit owner acceptance are required before M06 implementation begins.

## Current Progress

```text
R-M06-01  Source Universe and Admission Policy  complete
R-M06-02  YouTube and Audiovisual Ingestion    complete
R-M06-03  Website, Feed, and Change Monitoring complete
R-M06-04  Document Normalization and Provenance complete
R-M06-05  Canonical Vault and Knowledge Model  complete
R-M06-06  Identity, Versioning, Correction, and Deduplication next
R-M06-07 through R-M06-09                      not started
```

R-M06-01 is research material only. Its recommendations require architecture synthesis and owner acceptance before implementation.
