# Milestone 06 — Records, Dossiers, and Anomaly Vault

## Status

The corrected M06 architecture synthesis is owner-accepted. AC-02 is closed. Preparation of the exact M06-A planning package is authorized under `05-implementation-planning/m06a-planning-authorization.md`.

M06-A implementation remains blocked pending completion and independent review of the exact planning package, owner review, and explicit implementation authorization. M06-B planning and implementation remain blocked.

## Package Sequence

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

M06-A and M06-B are separate gates. M06-B does not inherit implementation authority from M06-A.

## Required Reading

- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/m06-architecture-synthesis.md`
- `05-implementation-planning/m06a-m06b-package-boundary.md`
- `05-implementation-planning/m06-pre-synthesis-correction-specification.md`
- `05-implementation-planning/m06a-planning-authorization.md`
- `05-implementation-planning/m06a-planning-package-outline.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/m06-architecture/README.md`
- all reports `R-M06-01` through `R-M06-09`
- `06-research/m06-architecture/multi-account-research-and-editorial-policy-layer.md`
- `08-audits/claude-m06-research-program-independent-review.md`
- `08-audits/ac02-m06-research-correction-disposition.md`
- `08-audits/ac02-m06-research-correction-closure.md`
- `08-audits/claude-m06-architecture-synthesis-review.md`
- `99-decisions/m06-owner-architecture-rulings.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- accepted D024 in `99-decisions/decision-log.md`

## Goal

Create a governed, per-account, local research-memory foundation that preserves exact originals, parser provenance, typed assertions, dossiers, correction lineage, lexical retrieval, and recoverability without creating a second truth store.

## Accepted Architecture

- SQLite owns identity, account scope, workflow, review state, policy bindings, assertions, dossiers, and manifests.
- The governed filesystem owns immutable originals and versioned normalized JSON element packages.
- Markdown/HTML projections are generated and read-only.
- Each account has a physically separate Vault.
- Every source class converges on one universal governed ingestion envelope.
- Research admission, truth/support assessment, editorial-use ruling, and exact publication approval are separate authority layers.
- Corrections and supersessions are immutable lineage.
- Entity merges/splits require human acceptance.
- Static LLM context-run contracts are defined, but no live LLM/provider integration ships in M06-A.
- SQLite lexical search is included; deterministic chunks are defined; Qdrant and graph work remain deferred.

## M06-A Scope — Local Manual Vault

- human-triggered local intake only;
- pasted/uploaded text and supported local files;
- TXT, Markdown, SRT, VTT, JSON, local RSS/Atom, local basic HTML, and born-digital PDF;
- YouTube URL identity plus human-supplied transcript;
- immutable originals and acquisition receipts;
- versioned normalized element packages with exact locators;
- source/item/occurrence/observation/artifact/document-version/element records;
- typed assertions, unresolved questions, and dossiers;
- account-policy and editorial-use bindings;
- immutable correction/supersession lineage;
- proposal-only entity merge/split candidates;
- local lexical/full-text search;
- deterministic chunk and invalidation contract;
- generated read-only Markdown/HTML projections;
- per-account canonical backup and disposable restore proof.

## M06-A Exclusions

- any URL fetch or generic network retrieval;
- monitoring, feeds, WebSub, scheduled jobs, or background ingestion;
- provider calls or credentials;
- OCR and scanned PDFs;
- office formats;
- media downloading;
- browser-rendered capture;
- first-class event and chronology objects;
- cross-account import execution;
- live LLM/provider integration;
- Qdrant, embeddings, graph, or GraphRAG;
- autonomous truth, approval, or publication.

## M06-B Scope — Bounded Static Webpage Retrieval

M06-B may later add one human-supplied public URL per operation under a separate network and SSRF contract.

It requires a fresh bounded plan, explicit owner approval, policy/retention review, and adversarial tests for DNS, redirects, private networks, rebinding, response limits, content types, decompression, parser failure, evidence preservation, and partial responses.

M06-B excludes crawling, monitoring, feeds, browser rendering, authenticated retrieval, and provider-assisted extraction unless separately admitted later.

## Entry Gate

Before M06-A implementation:

1. independent review of `m06-architecture-synthesis.md` is complete;
2. accepted findings are corrected or dispositioned;
3. AC-02 is closed;
4. the exact physical schema, migrations, file layout, package schema, parser admission records, backup procedure, and adversarial matrix are prepared;
5. the coherent implementation package is owner-reviewed;
6. the owner explicitly authorizes implementation.

## M06-A Exit Gate

- every durable assertion is typed, attributable, evidence-linked, account-scoped, and correctable;
- no candidate promotes to accepted conclusion or editorial ruling without human authority;
- originals, packages, manifests, and database records reconcile exactly;
- parser reruns and corrections preserve prior versions;
- entity merge/split bypass fails closed;
- lexical retrieval and invalidation are proven;
- generated projections rebuild and never become authority;
- two synthetic account Vaults prove physical and logical isolation;
- logs, traces, caches, temp paths, and restore operations do not leak account data;
- per-account backup/restore and tamper detection pass in a disposable location;
- evidence is bound to the implementation commit;
- docs, roadmap, status, map, decisions, and completion records are synchronized;
- the owner explicitly accepts closure.

## M06-B Entry Gate

M06-A must be stable and owner-accepted. M06-B requires its own plan, SSRF/network contract, hammer matrix, policy review, and explicit owner authorization.
