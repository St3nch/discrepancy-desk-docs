# Claude Independent Review — M06 Vault and Ingestion Research Program

## Record status

Completed independent review supplied by the owner on 2026-07-20.

This repository record preserves the review's verdict, findings, required corrections, owner-decision set, and package recommendation. The reviewer performed read-only filesystem inspection and did not execute Git, builds, or tests on the host.

## Live repository truth reported by reviewer

Application repository:

```text
C:\dev\x\discrepancy-desk
branch: main
HEAD: 6cbd0366e55bfba0c9687201615b72e70bf485d5
origin/main: synchronized
```

Documentation repository at review time:

```text
C:\dev\x\discrepancy-desk-docs
branch: main
HEAD: 91146e44143d05065d1c3f539d8ac94ad0fb35b0
origin/main: synchronized
```

The reviewer explicitly labeled test counts, build results, and evidence hashes as documentary rather than independently executed.

## Final verdict

```text
RESEARCH PACKAGE READY WITH CORRECTIONS
```

The reviewer found no critical or high findings. The research package was judged coherent, doctrine-aligned, honestly labeled, and safe for owner option review. Architecture synthesis remains blocked until bounded terminology, state-model, and rule corrections are resolved and the owner rules on the load-bearing architecture options.

## Positive findings

The review confirmed:

- evidence labels are applied substantively rather than decoratively;
- technical feasibility is consistently separated from permission and provider admission;
- the Telescope Rule and human-authority doctrine survive the nine-report program;
- source claims, extracted candidates, reviewed assertions, and accepted conclusions remain distinct;
- multi-account contamination is addressed through separate physical Vaults, immutable policy versions, account-first retrieval, and governed receiving-account review;
- originals remain canonical, parser output remains a derivative observation, and corrections remain lineage rather than destructive mutation;
- SQLite remains the proposed authority while projections, retrieval indexes, graph structures, and Qdrant remain rebuildable derivatives;
- no parser, provider, monitor, connector, Qdrant deployment, graph engine, or automation was admitted by the research.

## Findings

### F-1 — Medium — Source-admission vocabulary divergence

R-M06-01 models automation maturity as lifecycle state. R-M06-03 embeds permitted acquisition methods into state names. The review recommends one admission-stage dimension plus an orthogonal permitted-method set.

### F-2 — Medium — Three load-bearing research spines require explicit owner decisions

The universal ingestion envelope, separate physical Vaults, and static structured context packets are labeled as recommendations but become premises in later reports. Their dependency blast radius must be visible to the owner. D024 favors physical separation but does not by itself finalize the complete M06 Vault architecture.

### F-3 — Medium — Assertion and epistemic vocabularies are not yet mapped

The milestone plan, R-M06-05, R-M06-07, and public doctrine use different vocabularies. Synthesis must define a layered mapping:

```text
internal epistemic type
→ review state
→ LLM output status
→ public/account claim label
```

### F-4 — Low — Object identity terminology drift

Terms such as acquisition event/attempt, page identity/source item, raw response/original artifact, normalized element/source region, and duplicate classes require one canonical map.

### F-5 — Low — Three adversarial rules need to be explicit

- a claim must not survive context truncation without its known contradiction or a bound warning;
- output citations must be post-verified against the exact document version and locator supplied in context;
- imports without an original artifact need minimum-package requirements, explicit labeling, and restricted use.

### F-6 — Low — Editorial-use ruling is not connected to the existing approval state machine

The synthesis must define whether editorial-use assessment/ruling is a precondition, parallel governed record, or input to publication approval. It must not become a second hidden authority system.

### F-7 through F-10 — Observations

- feed monitoring is described slightly more aggressively in R-M06-01 than the final operational sequence supports;
- automatic paid transcript acquisition is the most aggressive rung and requires an independent future admission gate;
- STATUS contained stale research-commit wording;
- duplicate-relationship vocabularies require unification.

## Required corrections before synthesis

1. Unify source-admission stage and permitted-method vocabulary.
2. Adopt one object-identity and duplicate-relationship vocabulary.
3. Produce the layered assertion vocabulary mapping.
4. Correct wording that overstates physical Vault separation as finalized architecture.
5. Add truncation-integrity, citation-verification, and artifact-absent-import rules.
6. Require an explicit relationship between editorial-use rulings and publication approval.
7. Add account isolation for logs and observability.
8. Correct stale STATUS references.
9. Fold the YouTube-specific evidence packet into the general context-run/evidence-packet model.

## Owner decisions required before synthesis

The reviewer identified these load-bearing decisions:

- canonical authority model;
- universal ingestion envelope;
- separate physical Vaults and governed import model;
- generated versus editable Markdown projections;
- separation of research admission, editorial-use ruling, and publication approval;
- cross-account import contract timing;
- immutable correction/supersession model;
- normalized element-package format;
- static structured LLM context-run model;
- chunk identity/invalidation contract.

Additional scope decisions may be made before implementation rather than before synthesis.

## Package options reviewed

### Package 1 — Minimal Manual Vault

Local/manual ingestion, immutable originals, narrow normalization, dossiers/assertions, generated projections, correction lineage, and backup/restore proof. No URL retrieval or lexical search.

### Package 2 — Manual Vault plus normalization and lexical search

Package 1 plus static URL retrieval, HTML/PDF parsing, lexical search, chunk-contract definition, and a context-run schema. This was the reviewer's recommendation.

### Package 3 — Package 2 plus first governed LLM context slice

Adds one admitted provider and one bounded task class. The reviewer identified higher provider and injection-evaluation risk.

## Owner refinement accepted after review

The owner accepted splitting the reviewer's Package 2 into two bounded packages:

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

M06-A remains completely local and contains no network acquisition surface. M06-B introduces bounded single-page retrieval only after M06-A's authority, normalization, lexical retrieval, backup/restore, and hammer-test foundations are proven.

This split is recorded separately in the M06 package-boundary decision record.

## Explicit block

M06 implementation remains blocked until the owner completes the required architecture decisions, accepts the synthesis direction, and explicitly authorizes a bounded implementation package.
