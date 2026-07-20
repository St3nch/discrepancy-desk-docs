# M05-to-M06 Transition — Independent Audit and Architecture Research Plan

## Status

Owner-authorized on 2026-07-20.

M05 implementation is complete and owner-accepted. M06 implementation is not authorized. The project is in a bounded transition phase consisting of:

1. independent audit of all work since the last Claude audit;
2. correction and revalidation of every accepted audit finding;
3. deep research into the governed Vault architecture;
4. deep research into manual-first and future-automated ingestion architecture;
5. synthesis into an exact M06 architecture and adversarial plan;
6. independent review of that complete M06 proposal before implementation entry.

## Governing Boundary

Do not implement the M06 Vault, ingestion connectors, monitors, Qdrant, graph retrieval, or autonomous acquisition during this transition.

Research findings are source material. They are not doctrine, schema approval, provider admission, automation approval, or implementation authority.

## Phase 1 — Independent Claude Audit

The audit must inspect live truth from both repositories:

```text
C:\dev\x\discrepancy-desk
C:\dev\x\discrepancy-desk-docs
```

The last preserved independent audit is:

```text
08-audits/claude-audit-through-m01-hammer-review-acceptance.md
```

The new audit therefore covers all accepted work after that audit, including:

- M02 persistence contract, lifecycle model, migration/hammer plan, and adversarial matrix;
- M03 SQLite/Alembic implementation, authority boundaries, evidence, audit chain, recovery, service loop, web harness, and executable hammer closure;
- roadmap and decisions D023 and D024;
- M04 editorial organization, scheduling, Command Center, realistic week scenario, and M04 closure;
- M05 Tauri security foundation, backend lifecycle, desktop API parity, native evidence import, packaged Python sidecar, NSIS installation lifecycle, and final evidence binding;
- current status, roadmap, decision-log, and milestone consistency.

### Audit goals

The reviewer must look for:

- unsupported completion or validation claims;
- mismatch between docs and application truth;
- security or authority regressions;
- direct or accidental database authority in React/Rust;
- token, port, process-lifecycle, installer, and evidence-import defects;
- generated artifacts or secrets admitted to source control;
- incomplete account isolation;
- duplicate truth between SQLite, files, Markdown, and UI state;
- untested or weakly tested failure paths;
- evidence that does not bind to the claimed commit;
- misleading UX or disabled features presented as functional;
- roadmap/milestone numbering or status drift;
- technical debt that materially threatens M06.

### Required audit output

The audit must return:

1. exact repositories, branches, working-tree states, and HEAD SHAs inspected;
2. findings ordered by severity;
3. exact file and line references;
4. proof or reproduction steps;
5. required corrections;
6. explicit non-findings for major security/authority surfaces;
7. final verdict:
   - accept;
   - accept with corrections;
   - reject pending correction.

The reviewer must not edit either repository.

## Phase 2 — Correction Loop

For each accepted finding:

1. record the finding and owner ruling;
2. implement the smallest bounded correction;
3. add or strengthen tests that fail before the correction where practical;
4. run focused validation;
5. run the full regression suite;
6. rerun executable hammer evidence when authority, lifecycle, persistence, or packaging is affected;
7. commit and push the correction only after owner acceptance;
8. request independent re-review of the corrected surface when materially significant.

No audit finding may be silently ignored. Rejected findings require an explicit owner ruling and rationale.

## Phase 3 — M06 Deep Research Program

Two coordinated research tracks are required.

### Track A — Governed Vault and Research-Memory Architecture

Research questions include:

- canonical roles of raw files, SQLite, validated JSON/JSONL, Markdown projections, and future Qdrant indexes;
- source, occurrence, observation, artifact, document, document version, transcript, segment, entity, event, claim, assertion, contradiction, dossier, chronology, question, and connection-candidate vocabulary;
- stable identity, versioning, deduplication, correction, supersession, deletion, and provenance;
- account-specific physical isolation and shared-evidence references;
- human-readable projections without duplicate operational truth;
- deterministic chunk identity and future index invalidation;
- backup, restore, broken-link, orphan, and rebuild behavior;
- retrieval evaluation and correlation-candidate boundaries.

### Track B — Manual-First and Future-Automated Ingestion Architecture

Research questions include:

- universal ingestion contract across pasted text, transcripts, URLs, files, PDFs, HTML, screenshots, YouTube, RSS/Atom, APIs, browser helpers, and future monitors;
- original artifact preservation and acquisition receipts;
- review/admission states and fail-closed rejection;
- YouTube metadata, transcript providers, transcript files, timestamps, local speech-to-text, and channel monitoring;
- website retrieval, extraction, feeds, sitemaps, conditional requests, meaningful-change detection, and monitoring controls;
- provider APIs, costs, retention, terms, substitution, rate limits, and failure handling;
- parser provenance, OCR uncertainty, hostile documents, prompt injection, and secret/private-data handling;
- one ingestion path that works manually now and accepts automated connector output later.

## Research Report Sequence

Create the following reports under `06-research/m06-architecture/`:

```text
R-M06-01-source-universe-and-admission-policy.md
R-M06-02-youtube-audiovisual-ingestion.md
R-M06-03-website-feed-and-change-monitoring.md
R-M06-04-document-normalization-and-parser-provenance.md
R-M06-05-canonical-vault-knowledge-model.md
R-M06-06-identity-versioning-correction-and-deduplication.md
R-M06-07-llm-context-assembly-and-governed-reasoning.md
R-M06-08-retrieval-chunking-graph-and-qdrant-readiness.md
R-M06-09-automation-security-backup-and-rebuild.md
```

Each report must distinguish:

```text
verified fact
provider or standards guidance
inference
project-specific recommendation
open question
```

Each report must use current primary sources wherever available and record access dates.

## Phase 4 — Architecture Synthesis

After all research reports are reviewed, prepare:

```text
03-system-design/m06-information-acquisition-and-vault-architecture.md
03-system-design/m06-canonical-record-and-provenance-model.md
03-system-design/m06-manual-ingestion-contract.md
03-system-design/m06-future-connector-interface.md
03-system-design/m06-llm-context-and-correlation-contract.md
05-implementation-planning/m06-adversarial-test-matrix.md
05-implementation-planning/m06-exact-implementation-plan.md
08-audits/m06-vault-ingestion-architecture-review.md
```

These synthesis documents must resolve:

- canonical storage authority;
- exact record vocabulary;
- account isolation;
- manual ingestion UX and lifecycle;
- future automation compatibility;
- LLM retrieval/context boundaries;
- connection-candidate authority;
- Qdrant readiness without implementing Qdrant;
- backup and complete rebuild behavior;
- implementation packages and stop conditions.

## M06 Entry Gate

M06 implementation may begin only when all of the following are true:

- the post-M01 independent audit is complete;
- accepted audit findings are corrected and revalidated;
- all nine M06 research reports are complete and reviewed;
- the architecture synthesis and adversarial matrix are prepared;
- an independent Claude review inspects the complete proposal against live repo truth;
- review findings are resolved or explicitly ruled on by the owner;
- the owner explicitly accepts the exact M06 implementation plan.

## Current Ruling

```text
M05: owner-accepted and closed
Post-M05 independent audit: active
M06 deep research: authorized after audit baseline capture
M06 architecture synthesis: blocked pending research
M06 implementation: blocked
M07 and later implementation: blocked by their own gates
M14/Qdrant implementation: blocked
```
