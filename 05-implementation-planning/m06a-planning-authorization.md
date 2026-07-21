# M06-A Planning Authorization

## Status

Owner-approved on 2026-07-21.

The owner accepted the corrected M06 architecture synthesis, closed AC-02, and authorized preparation of the exact M06-A planning package.

## Authorized Work

Planning may now prepare, reconcile, and independently review:

- the exact M06-A physical data model and SQLite schema proposal;
- Alembic migration sequencing and rollback boundaries;
- the per-account Vault filesystem and normalized-package layout;
- parser admission records for each proposed local parser and format;
- backup-generation, disposable-restore, and tamper-proof procedures;
- account-isolation, provenance, correction, search, chunk, and recovery contracts;
- the complete M06-A adversarial/hammer-test matrix;
- the bounded implementation package, file set, validation plan, and evidence outputs.

Planning may inspect both repositories and propose exact implementation paths. It may not change application code, migrations, runtime configuration, database files, parsers, or operational state.

## Still Blocked

This authorization does not admit or authorize:

- M06-A implementation;
- M06-B planning or implementation;
- URL retrieval or any network acquisition;
- provider calls, credentials, OAuth, or paid services;
- feeds, monitoring, recurring jobs, or background ingestion;
- OCR, scanned-PDF handling, office formats, or browser-rendered capture;
- live LLM/provider integration;
- Qdrant, embeddings, graph, or GraphRAG work;
- cross-account import execution;
- autonomous truth, approval, publication, or entity-merge authority.

## Required Return Before Implementation

The completed planning package must return to the owner with:

1. exact scope and exclusions;
2. exact files/components proposed for change;
3. schema and migration plan;
4. filesystem/package contract;
5. parser admission records;
6. backup/restore and rebuild plan;
7. full adversarial matrix;
8. validation commands and evidence outputs;
9. rollback and failure boundaries;
10. independent review disposition;
11. an explicit statement that implementation remains blocked pending owner authorization.

## Authority Boundary

```text
architecture accepted
→ planning authorized
→ implementation still blocked
```
