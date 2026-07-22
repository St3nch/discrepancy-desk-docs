# Milestone 06 — Records, Dossiers, and Anomaly Vault

## Status

The corrected M06 architecture synthesis and resolved M06-A planning baseline are owner-accepted. AC-02 and the M06-A planning correction cycle are closed. D027 through D035 govern the accepted planning baseline, editorial identity, Tauri-only boundary, Phase 1 closure, exact Phase 2 authorization, and corrected Phase 2 closure.

M06-A Phase 1 is closed through application commits `8fe3be4` and `5f0f9ae` plus D033. Phase 2 is implemented through `eaf7b5d`, independently verified, corrected through `1e8cba8`, and owner-closed through D035. The optional second correction spot review is deferred, while the required Phase 2 audit is complete. An exact Phase 3A candidate package is prepared for owner review, limited to the common parser framework plus `m06a.text.v1` in `under_test` state. It has no implementation or parser-admission authority. Phases 3 through 6 remain blocked pending separate explicit authorization. No parser is admitted. M06-B planning and implementation remain blocked.

## Package Sequence

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

M06-A and M06-B are separate gates. M06-B does not inherit implementation authority from M06-A.

## Required Reading

- `LLM_MAP.md`
- `STATUS.md`
- `00-doctrine/editorial-anomaly-archive-direction.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/m06-architecture-synthesis.md`
- `05-implementation-planning/m06a-m06b-package-boundary.md`
- `05-implementation-planning/m06-pre-synthesis-correction-specification.md`
- `05-implementation-planning/m06a-planning-authorization.md`
- `05-implementation-planning/m06a-planning-package-outline.md`
- `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`
- `05-implementation-planning/m06a-parser-admission-plan.md`
- `05-implementation-planning/m06a-adversarial-closure-matrix.md`
- `05-implementation-planning/m06a-phase2-exact-implementation-package.md`
- `05-implementation-planning/m06a-phase3a-exact-implementation-package.md` — review candidate only until owner acceptance
- `08-audits/m06a-planning-correction-disposition.md`
- `08-audits/m06a-planning-correction-closure.md`
- `08-audits/claude-m06a-phase1-independent-implementation-review.md`
- `08-audits/m06a-phase1-independent-review-disposition.md`
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
- accepted D024 and D027 through D035 in `99-decisions/decision-log.md`

## Goal

Create a governed, brand-level local anomaly archive and research-memory foundation that preserves exact originals, parser provenance, typed assertions, dossiers, correction lineage, lexical retrieval, and recoverability without creating a second truth store. The Vault preserves the trail and supports editorial connections; it is not a formal fact-checking or truth-adjudication system.

## Accepted Architecture

- SQLite owns identity, account scope, workflow, review state, policy bindings, assertions, dossiers, and manifests.
- The governed filesystem owns immutable originals and versioned normalized JSON element packages.
- Markdown/HTML projections are generated and read-only.
- Each editorial/public brand identity has one physically separate Vault; platform-owned accounts remain central records bound explicitly to that Vault.
- Every source class converges on one universal governed ingestion envelope.
- Research admission, optional truth/support assessment, editorial-use ruling, and exact publication approval are separate authority layers. Speculative or unresolved status does not by itself block admission or editorial use.
- Corrections and supersessions are immutable lineage.
- Entity merges/splits require human acceptance.
- Static LLM context-run contracts are defined, but no live LLM/provider integration ships in M06-A.
- SQLite lexical search is included; deterministic chunks are defined; Qdrant and graph work remain deferred.
- Tauri is the sole supported human operator interface. FastAPI remains the governed loopback API/service host; legacy Jinja pages receive no new M06 feature work.

## M06-A Scope — Local Manual Vault

- human-triggered local intake only;
- pasted/uploaded text and supported local files;
- TXT, Markdown, SRT, VTT, JSON, local RSS/Atom, local basic HTML, and born-digital PDF;
- YouTube URL identity plus human-supplied transcript;
- immutable originals and acquisition receipts;
- versioned normalized element packages with exact locators;
- source/item/occurrence/observation/artifact/document-version/element records;
- typed assertions, allegations, source claims, folklore, Desk inferences, contradictions, unresolved questions, and dossiers;
- account-policy and editorial-use bindings;
- immutable correction/supersession lineage;
- proposal-only entity merge/split candidates;
- local lexical/full-text search;
- deterministic chunk and invalidation contract;
- generated read-only Markdown/HTML projections;
- per-Vault canonical backup and disposable restore proof.

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

## M06-A Implementation Entry Gate

Before M06-A implementation:

1. independent review of `m06-architecture-synthesis.md` is complete — satisfied;
2. accepted architecture and planning findings are corrected or dispositioned — satisfied;
3. AC-02 and the M06-A planning correction cycle are closed — satisfied;
4. the exact physical schema, migrations, file layout, package schema, parser admission records, backup procedure, and adversarial matrix are prepared and owner-accepted — satisfied;
5. the exact bounded implementation package for Phase 1 is identified and owner-reviewed — satisfied through the accepted canonical phase definition;
6. the owner explicitly authorizes Phase 1 implementation — satisfied through D030.

Current result: Phases 1 and 2 implementation, clean commit-bound evidence, required independent reviews, correction disposition, and owner closure are complete through D035. The exact Phase 3A candidate package is prepared but not accepted. Phase 3 implementation and every parser admission remain closed until separately authorized.

## M06-A Exit Gate

- every durable assertion is typed, attributable, source/provenance-linked as applicable, account-scoped, and correctable; the link proves where the statement came from, not automatically that the underlying proposition is true;
- no candidate promotes to a final support assessment, accepted conclusion, or editorial ruling without human authority; unresolved material may remain editorially usable without such promotion;
- originals, packages, manifests, and database records reconcile exactly;
- parser reruns and corrections preserve prior versions;
- entity merge/split bypass fails closed;
- lexical retrieval and invalidation are proven;
- generated projections rebuild and never become authority;
- two synthetic account Vaults prove physical and logical isolation;
- logs, traces, caches, temp paths, and restore operations do not leak account data;
- per-Vault backup/restore and tamper detection pass in a disposable location;
- evidence is bound to the implementation commit;
- docs, roadmap, status, map, decisions, and completion records are synchronized;
- the owner explicitly accepts closure.

## M06-B Entry Gate

M06-A implementation must be stable and milestone closure must be owner-accepted. M06-B then requires its own plan, SSRF/network contract, hammer matrix, policy review, and explicit owner authorization. Planning acceptance alone does not open M06-B.
