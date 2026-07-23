# Milestone 06 — Records, Dossiers, and Anomaly Vault

## Status

The corrected M06 architecture synthesis and resolved M06-A planning baseline are owner-accepted. AC-02 and the M06-A planning correction cycle are closed. D027 through D038 govern the accepted planning baseline, editorial identity, Tauri-only boundary, Phase 1 closure, exact Phase 2 authorization and closure, Phase 3A implementation, independent review, correction, and owner closure.

M06-A Phase 1 is closed through application commits `8fe3be4` and `5f0f9ae` plus D033. Phase 2 is independently verified, corrected through `1e8cba8`, and owner-closed through D035. D036 authorized Phase 3A, implemented at `251b3ca841af46e63485b9ab5bf292cbae55a418`; independent review required correction. D037 authorized the exact Phase 3A-C1 package, implemented and clean-evidence-bound at `1337b5ac450ae82664aa1ad9667a85af41c4351e`. D038 accepts the disposition and closes Phase 3A. D039 authorized the exact `m06a.text.v1` owner-admission and canonical-execution package and exact 28-invariant profile. Application commit `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9` implements it with clean commit-bound evidence. D040 defers but does not waive D039 independent review and owner closure, and authorized the exact `m06a.srt.v1` under-test candidate package. Application commit `529165c19da30185dd9833fab608d6dc28dfed88` implemented it. D041 Project-Steward self-review required two corrections, both implemented and clean-evidence-bound at `6a8082253a52a601291efaf3ed85ee411b04be20`. No independent-review claim is made. D042 accepts the disclosed self-review basis and closes D039 and corrected D040. Fresh V0004 Vaults install SRT only as `under_test`. D044 prepared the exact `m06a.vtt.v1` under-test candidate package; D045 accepts its exact implementation and 28-invariant profile. SRT admission/canonical execution, VTT implementation/admission/canonical execution, JSON, Phase 3B, later M06-A phases, and M06-B remain blocked.

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
- `05-implementation-planning/m13-governed-discrepancy-detection-planning-direction.md` — D043 Phase 4/5 planning dependencies only
- `05-implementation-planning/m06a-phase2-exact-implementation-package.md`
- `05-implementation-planning/m06a-phase3a-exact-implementation-package.md` — owner-closed through D038 at corrected commit `1337b5a`
- `05-implementation-planning/m06a-text-v1-owner-admission-and-canonical-execution-package.md` — implemented at `7980b1e`; owner-closed through D042
- `05-implementation-planning/m06a-srt-v1-under-test-candidate-package.md` — implemented at `529165c`, corrected at `6a80822`, and owner-closed through D042; no SRT admission/canonical or later-parser authority
- `05-implementation-planning/m06a-vtt-v1-under-test-candidate-package.md` — prepared through D044 and accepted for exact implementation through D045
- `08-audits/m06a-planning-correction-disposition.md`
- `08-audits/m06a-planning-correction-closure.md`
- `08-audits/claude-m06a-phase1-independent-implementation-review.md`
- `08-audits/m06a-phase1-independent-review-disposition.md`
- `08-audits/claude-m06a-phase3a-independent-static-review.md`
- `08-audits/m06a-phase3a-independent-review-disposition.md`
- `08-audits/m06a-phase3a-implementation-return.md`
- `08-audits/m06a-text-v1-implementation-return.md`
- `08-audits/m06a-srt-v1-implementation-return.md`
- `08-audits/m06a-srt-v1-c1-correction-return.md`
- `08-audits/m06a-d040-project-steward-self-review-disposition.md`
- `08-audits/m06a-d039-d040-owner-closure.md`
- `08-audits/m06a-phase3a-closure.md`
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
- accepted D024 and D027 through D038 in `99-decisions/decision-log.md`

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


## D043 M13 Planning Dependencies

D043 does not open M13 or authorize detector implementation. It preserves two requirements that future
M06 packages must address without expanding their authority:

1. Before the exact Phase 4 package, decide whether and how the governed search/current-version/policy
   boundary can produce a corpus/coverage snapshot suitable for later detector evaluation. No Tier 1
   detector or candidate store is implied without separate authorization.
2. Before the exact Phase 5 package, define stable assertion comparison dimensions in addition to assertion
   types, including time, unit, scope, population/included class, definition or counting method, and
   uncertainty where applicable.

These comparison dimensions do not create first-class event or chronology objects and do not alter M06-D10.
The authoritative planning direction is
`05-implementation-planning/m13-governed-discrepancy-detection-planning-direction.md`.

## M06-A Implementation Entry Gate

Before M06-A implementation:

1. independent review of `m06-architecture-synthesis.md` is complete — satisfied;
2. accepted architecture and planning findings are corrected or dispositioned — satisfied;
3. AC-02 and the M06-A planning correction cycle are closed — satisfied;
4. the exact physical schema, migrations, file layout, package schema, parser admission records, backup procedure, and adversarial matrix are prepared and owner-accepted — satisfied;
5. the exact bounded implementation package for Phase 1 is identified and owner-reviewed — satisfied through the accepted canonical phase definition;
6. the owner explicitly authorizes Phase 1 implementation — satisfied through D030.

Current result: Phases 1 and 2 implementation, review, correction disposition, and owner closure are complete through D035. D036 authorized Phase 3A; independent review required correction. D037 authorized Phase 3A-C1, implemented and clean-evidence-bound at `1337b5ac450ae82664aa1ad9667a85af41c4351e`. D038 accepts the review disposition and closes Phase 3A. D039 plain-text admission/canonical execution is implemented and clean-evidence-bound at `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9`; D040 defers but does not waive its independent review and owner closure. The exact SRT under-test candidate was implemented at `529165c19da30185dd9833fab608d6dc28dfed88`; D041 self-review corrections are implemented and clean-evidence-bound at `6a8082253a52a601291efaf3ed85ee411b04be20`. No independent-review claim is made. D042 accepts the disclosed self-review basis and closes D039 and corrected D040. D044 prepared the exact VTT under-test package; D045 accepts exact bounded implementation. Admission and canonical execution remain blocked. Fresh Vaults remain under-test until explicit per-Vault human admission.

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
