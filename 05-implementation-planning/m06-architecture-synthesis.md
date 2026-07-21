# M06 Architecture Synthesis — Governed Local Research Vault

## Status

Owner-approved architecture synthesis candidate prepared from:

- the nine R-M06 research reports;
- the multi-account research/editorial-policy note;
- Claude's independent review verdict `RESEARCH PACKAGE READY WITH CORRECTIONS`;
- AC-02 correction requirements;
- owner rulings M06-D01 through M06-D16.

This document authorizes independent review of the synthesis only. It does not authorize implementation.

## 1. Milestone Split

M06 is divided into two separately gated packages:

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

M06-A is local-only. M06-B introduces the first new generic network-retrieval surface and therefore requires its own SSRF, redirect, DNS/IP, response-limit, content-policy, evidence, and hammer-test gate.

## 2. Canonical Authority

```text
SQLite
  identity, account scope, workflow, review state,
  policy bindings, assertions, dossiers, provenance manifests

Governed filesystem
  immutable original artifacts
  versioned normalized JSON element packages

Generated read-only projections
  Markdown and HTML

Rebuildable derivatives
  lexical indexes, chunks, caches, Qdrant, graph outputs
```

No projection, index, cache, provider store, graph, or LLM output may become a second canonical truth system.

## 3. Physical Multi-Account Model

Each account that uses research memory has a physically separate Vault root and account-bound SQLite authority.

Cross-account reuse is not direct sharing. Any future transfer must use a hash-bound export/import package, independent receiving-account review, and explicit provenance. Accepted conclusions, editorial-use rulings, approvals, entity merges, and account policy never transfer automatically.

Cross-account import execution is not in M06-A.

## 4. Universal Ingestion Envelope

Every admitted source class converges on one governed envelope with source-specific extensions.

Core concepts:

```text
source
source item
occurrence
observation
acquisition
original artifact
document version
element
mention
entity candidate
assertion
dossier
```

An acquisition records attempted or successful intake and produces a receipt. Failure is a first-class result. Source-specific implementations may add fields but may not create separate authority models.

## 5. Admission Model

Source admission uses two orthogonal dimensions:

```text
admission stage
  proposed
  manual_only
  manual_admitted
  automation_research
  pilot_authorized
  automated_admitted
  suspended
  retired
  revoked
  prohibited

permitted methods
  explicit set of allowed acquisition methods for that source
```

A method is not authorized merely because a source is admitted. Automation, recurrence, provider use, and network retrieval remain separately gated.

Terminal and interruption semantics are distinct:

```text
suspended
  temporary operational stop; prior authority remains recorded but cannot be exercised

retired
  intentionally ended; historical authority and evidence remain preserved

revoked
  previously granted authority explicitly withdrawn by governance action

prohibited
  source or method is ineligible for admission unless a later owner ruling changes doctrine
```

## 6. M06-A Intake Scope

M06-A accepts only local, human-triggered intake:

- pasted plain text;
- uploaded plain text;
- Markdown;
- SRT;
- VTT;
- JSON;
- RSS/Atom files supplied locally;
- basic HTML files supplied locally;
- born-digital PDF files supplied locally;
- YouTube URL recorded as identity/context plus a human-supplied transcript file or pasted transcript.

Video, transcript, caption, timing, speaker-label, and audiovisual metadata fields are source-specific extensions of the universal evidence-packet contract. They do not create a parallel authority or context model.

M06-A excludes:

- URL fetching;
- feeds or monitoring;
- provider calls;
- media downloading;
- OCR and scanned PDFs;
- office documents;
- rendered-browser capture;
- recurring work;
- live LLM/provider integration;
- Qdrant and graph work.

## 7. Artifact and Normalization Contract

The exact supplied bytes or text are immutable originals with hashes and media-type evidence.

Each parser execution creates a new versioned JSON element package containing:

- artifact identity and hash;
- parser/tool identity, version, and configuration;
- execution timestamp and receipt;
- elements and exact locators;
- raw and normalized text where applicable;
- warnings, partial-failure states, and confidence metadata;
- language and timing/page/region information where available;
- output hash;
- supersession lineage.

Parser output is a derivative observation, not original truth. A parser rerun never overwrites a prior package.

## 8. Assertion and Claim Model

Four layers are distinct:

```text
epistemic type
review state
LLM/output presentation status
public account claim label
```

Candidate epistemic types include:

- source-content fact;
- attributed claim;
- quotation;
- allegation;
- observation assertion;
- derived inference;
- relationship candidate;
- accepted conclusion;
- disputed conclusion;
- unknown;
- parody/editorial framing.

Review state and account-specific editorial usability are separate from epistemic type.

Public labels such as `Documented`, `Plausible`, `Disputed`, `Folklore`, `Speculative`, or `Brain Soup` are presentation policy, not canonical evidence classes.

## 9. Separate Authority Workflows

```text
research admission
  may this material enter this account Vault?

truth/support assessment
  how well supported is the proposition?

editorial-use ruling
  may this account use it and under what framing?

publication approval
  is this exact content revision approved for posting?
```

An LLM may propose an editorial-use assessment. Only a human may create the governed ruling. An editorial-use ruling is a prerequisite/input to publication review where policy requires it; it never approves exact post text.

A new account-policy version creates an explicit impact set for dependent editorial-use rulings and publication approvals. Affected records are flagged for re-evaluation and may not silently inherit authority from the superseded policy version.

## 10. Identity, Correction, and Deduplication

- Internal IDs are stable and account-scoped.
- External IDs are typed evidence, not primary authority.
- Duplicate detection creates relationships; it does not delete or collapse records automatically.
- Entity merges and splits may be proposed by the system but accepted only by a human. Acceptance records the actor, supporting evidence, decision time, and reversible lineage.
- Corrections and supersessions create immutable lineage.
- No original, parser output, assertion, policy version, or approved revision is silently rewritten.

First-class event and chronology objects are deferred from the first M06-A implementation.

## 11. Dossiers

A dossier is a governed account-scoped organizing record that references canonical sources, observations, document versions, elements, assertions, questions, and related editorial work.

A dossier does not duplicate the underlying evidence. Its generated Markdown/HTML view is read-only in M06-A.

## 12. Search and Chunk Contract

M06-A includes local SQLite lexical/full-text search.

A deterministic chunk contract is defined now even though semantic embeddings remain deferred. Chunk identity binds:

- account;
- document version;
- ordered element set or exact locator range;
- chunk strategy and version;
- normalized content hash.

Corrections, supersession, policy restriction, deletion, or parser-version replacement create deterministic invalidation work.

Qdrant remains M14. Graph and GraphRAG remain after stable identity, merge/split, correction, and retrieval contracts.

## 13. Static LLM Context-Run Contract

M06-A defines but does not execute a provider integration.

A future static context run contains:

- exact account and Vault identity;
- account policy version and hash;
- task instructions;
- reviewed conclusions, clearly separated;
- untrusted evidence packets;
- corrections and contradictions;
- canonical locators and citations;
- omissions and truncation record;
- retrieval trace;
- tool/provider/model metadata when later used;
- no mutation or publication authority.

Retrieved or supplied content is never an instruction layer.

### Required fail-closed rules

- If a known contradiction or correction cannot fit with its associated claim, the claim is excluded or carries a bound truncation warning.
- Every displayed citation must be post-validated to an existing locator in the exact document version that was supplied to the run.
- Previous model outputs remain untrusted derived records and cannot be promoted to evidence merely by re-ingestion.

Live LLM/provider integration belongs in M08.

## 14. Import Minimums

Any future cross-account import package must contain:

- source and item identities;
- occurrence/acquisition receipts;
- manifest and hashes;
- original artifact when rights and retention permit;
- normalized package and parser provenance when included;
- explicit omissions;
- export account identity and export timestamp;
- no accepted conclusions, editorial rulings, approvals, policy, or entity-merge authority.

If the original artifact is absent, the import is labeled `reference_only` or `derivative_only`, cannot claim local original possession, and may be restricted from stronger evidence roles by receiving-account policy.

## 15. Logging and Observability

Logs, traces, caches, temporary files, and error reports are account-scoped or strongly redacted. One account's source universe, locators, filenames, or content may not become discoverable through another account's diagnostics or context assembly.

Secrets never enter canonical evidence, logs, manifests, projections, or test fixtures.

## 16. Backup, Restore, and Rebuild

M06-A requires immutable per-account backup generations containing:

- consistent SQLite backup;
- original artifacts;
- normalized packages;
- manifests and hashes;
- account policy versions;
- correction/supersession and review lineage;
- audit records;
- application/schema version metadata.

Each generation has a unique immutable manifest and no-overwrite semantics.

Qdrant, graph data, caches, indexes, and projections are rebuildable and non-canonical.

Closure requires a disposable restore into a separate location proving account identity, hash reconciliation, no orphan canonical records, no omitted required original artifacts, policy availability, projection regeneration, no cross-account contamination in rebuilt indexes or other derivatives, and fail-closed detection of at least one modified manifest or artifact. The existing `age` mechanism may protect archives; packaged key-management UX remains M15.

## 17. M06-B Boundary

M06-B may later add bounded static retrieval of one human-supplied public URL.

Before M06-B planning is accepted, it requires:

- allowed schemes and ports;
- DNS resolution and redirect revalidation;
- private/local/link-local/metadata-network denial;
- request, response, timeout, and redirect caps;
- content-type and size validation;
- no authentication/cookies by default;
- immutable response artifacts and receipts;
- policy/terms and retention review;
- SSRF, rebinding, redirect, decompression, parser, and partial-response hammer tests;
- separate owner authorization.

M06-B does not imply crawling, monitoring, feeds, browser rendering, or provider-assisted extraction.

## 18. Explicit Deferrals

- recurring acquisition and monitoring;
- feeds/WebSub/channel notifications;
- transcript or extraction SaaS;
- OCR/scanned document handling;
- human-triggered X helper (M07);
- live agent-neutral LLM access (M08);
- restricted source watch (M09);
- events/chronologies until after M06-A foundation proves stable;
- pattern/connection candidates (M13);
- Qdrant semantic/hybrid retrieval (M14);
- graph/GraphRAG after stable identity/retrieval proof;
- packaged key management and full-system recovery (M15).

## 19. M06-A Adversarial Baseline

The implementation plan must include at minimum:

- wrong-account access and path tests;
- duplicate/import replay;
- MIME mismatch and hostile file limits;
- path traversal and archive escape;
- malformed/partial parser outputs;
- parser version changes;
- fabricated IDs and locators;
- modified original/package/manifest detection;
- correction/supersession invalidation;
- entity merge/split authority bypass;
- policy version changes;
- editorial-use/publication approval confusion;
- claim-without-known-contradiction context failure;
- unsupported citation validation failure;
- backup omission, tamper, wrong-account restore, and dirty recovery;
- local parser network-egress attempts fail closed;
- manual edits to generated projections cannot become authority and are detected or overwritten on regeneration;
- parser partial output without an explicit warning/failure state fails closed;
- account leakage through search, logs, cache keys, projections, and temp files;
- detector non-detection and detector-error fail-closed cases.

## 20. Entry and Exit Gates

### Synthesis review gate

This synthesis must receive independent review. Findings must be dispositioned, and the owner must accept the final architecture before implementation planning.

### M06-A implementation entry gate

Requires:

- accepted synthesis;
- exact physical schema and migration plan;
- exact file/package layout;
- parser candidates and admission records;
- full adversarial matrix;
- backup/restore procedure;
- bounded work package;
- explicit owner implementation authorization.

### M06-A exit gate

Requires:

- admitted local formats work end-to-end;
- originals and packages reconcile exactly;
- account isolation and authority boundaries are proven;
- lexical search and invalidation are proven;
- generated projections rebuild correctly;
- disposable restore and tamper tests pass;
- evidence is bound to the implementation commit;
- independent review findings are closed;
- explicit owner acceptance.

## 21. Authority Statement

The system records what was observed and what sources claim. It preserves uncertainty, contradictions, and editorial usefulness without converting them into truth. AI proposes; humans admit, rule, approve, merge, correct, and publish.
