# M06 Owner Architecture Rulings

## Status

Active owner-approved architecture rulings for M06 synthesis.

These rulings authorize architecture synthesis only. They do not authorize M06-A implementation, M06-B planning, provider admission, parser admission, monitoring, live LLM integration, Qdrant, graph work, or cross-account transfer execution.

## Decision Set 1 — Foundational Authority and Isolation

Owner approval recorded on 2026-07-20.

### M06-D01 — Canonical authority model

**Decision: APPROVED**

Adopt the hybrid authority model:

```text
SQLite
  owns identity, workflow, review state, account-policy binding,
  assertions, dossiers, memberships, and provenance manifests

Governed filesystem
  owns immutable original artifacts and versioned normalized packages

Generated projections
  provide read-only Markdown and HTML views

Rebuildable derivatives
  include lexical indexes, retrieval chunks, Qdrant data,
  graph derivatives, caches, and rendered projections
```

No projection or derivative may become a second canonical truth system.

### M06-D02 — Universal ingestion envelope

**Decision: APPROVED**

Every admitted source class must produce one common governed ingestion envelope with source-specific extensions where required.

Manual and future automated acquisition must converge on the same downstream authority and provenance model. Source-specific paths may not create independent truth systems.

### M06-D03 — Physical account isolation

**Decision: APPROVED**

Adopt separate physical Vaults per account as the final M06 multi-account direction.

Cross-account reuse, if later authorized, must use:

```text
governed hash-bound export/import packages
+ independent receiving-account review
+ no transfer of accepted conclusions
+ no transfer of editorial-use rulings
+ no transfer of approvals or account policy
```

Cross-account import implementation remains deferred and separately gated.

### M06-D04 — Markdown and HTML projection behavior

**Decision: APPROVED**

M06-A Markdown and HTML projections are generated and read-only.

Editable Vault notes are not part of M06-A. Any later editable projection requires an explicit reconciliation contract, separate authority analysis, owner approval, and adversarial testing.

## Consequences for synthesis

The M06 synthesis must treat these four decisions as settled inputs:

1. hybrid SQLite/filesystem authority;
2. one universal ingestion envelope;
3. separate physical Vaults per account;
4. generated read-only Markdown/HTML projections.

All remaining architecture and scope decisions remain open until separately approved by the owner.

## Decision Set 2 — Research Authority, Versioning, Normalization, and LLM Context

Owner approval recorded on 2026-07-20.

### M06-D05 — Research admission and editorial-use approval

**Decision: APPROVED**

Adopt separate governed records and workflows for:

```text
research admission
truth or support status
editorial-use ruling
required publication framing
exact-content publication approval
```

Research admission determines whether material may enter the Vault. It does not establish truth, editorial usability, or publication authority.

An editorial-use ruling is account-scoped and policy-version-bound. It may permit, restrict, require framing for, or block use of admitted research. It does not approve any exact draft for publication.

Exact-content publication approval remains a separate human-controlled workflow bound to the reviewed content hash.

### M06-D06 — Correction and supersession model

**Decision: APPROVED**

Adopt immutable full versions with explicit correction, supersession, merge, split, and withdrawal lineage.

Original artifacts, normalized document versions, assertions, accepted conclusions, and governed policy records may not be silently overwritten.

Corrections create new governed records linked to the records they correct. Superseded records remain preserved and visibly non-current.

### M06-D07 — Normalized document package

**Decision: APPROVED**

Adopt a versioned JSON element package containing:

```text
exact source locators
parser identity and version
parser configuration identity
input and output hashes
warnings and partial-failure state
confidence or uncertainty where applicable
raw and normalized text distinctions
structural, page, region, timestamp, or JSON locators
```

Generated Markdown and HTML remain read-only projections of this governed package.

Parser-native outputs may be preserved as evidence or diagnostics, but they do not replace the common normalized package.

### M06-D08 — LLM context model

**Decision: APPROVED**

Adopt static, structured, provider-neutral context runs as the M06 architecture direction.

Each context run must bind:

```text
exact account identity
exact account-policy version and hash
task instructions
reviewed conclusions, distinctly labeled
untrusted evidence packets
known contradictions and corrections
canonical source locators
selection and retrieval trace
omissions and truncation record
provider/model/configuration identity
```

Retrieved content, parser output, prior model output, and tool-result content remain untrusted evidence and never become instructions or mutation authority.

Dynamic live Vault tools and broader agent interfaces remain deferred to M08 and require separate owner approval.

## Consequences of Decision Set 2

The M06 synthesis must preserve these boundaries:

1. admission is not truth, editorial usability, or publication approval;
2. versions and corrections are immutable lineage, not replacement;
3. normalized JSON element packages are the common structured derivative;
4. LLM reasoning begins with static, reconstructable context runs and no mutation authority.

## Decision Set 3 — Identity, Scope, Parsing, and Retrieval

Owner approval recorded on 2026-07-20.

### M06-D09 — Entity merge and split authority

**Decision: APPROVED**

The system may propose entity merges or splits, but only a human may accept them.

Accepted merges and splits must preserve:

```text
proposal identity
supporting evidence
actor
review timestamp
previous identity state
resulting identity state
reversal or split lineage
```

No parser, extractor, graph system, retrieval engine, or LLM may merge canonical entities automatically.

### M06-D10 — Events and chronologies in the first M06-A implementation

**Decision: APPROVED — DEFER**

First-class events and chronologies are deferred from the first M06-A implementation package.

The first implementation should focus on:

```text
sources
source items
occurrences
observations
acquisitions
original artifacts
document versions
elements
assertions
dossiers
```

Event candidates and chronology views may be added only after the core Vault contract is proven stable and separately authorized.

### M06-D11 — Initial parser scope

**Decision: APPROVED**

The initial M06-A parser scope is:

```text
plain text
Markdown
SRT
VTT
JSON
RSS/Atom
basic HTML files
born-digital PDFs
```

The following remain excluded from the first M06-A implementation:

```text
OCR
scanned PDFs
office-document parsing
rendered-browser capture
```

Each parser still requires an implementation-time admission record, fixture corpus, failure behavior, provenance contract, and owner authorization before use.

### M06-D12 — Lexical search and chunk contract

**Decision: APPROVED**

M06-A will include local SQLite lexical/full-text search.

M06-A must also define a deterministic chunk identity and invalidation contract covering at minimum:

```text
account identity
document-version identity
included element identities
chunking strategy and version
content hash
supersession and correction invalidation
```

Embeddings, semantic retrieval, and Qdrant remain deferred to M14.

## Consequences for synthesis

The synthesis must treat these four decisions as settled inputs:

1. entity merge/split proposals are human-approved only;
2. first-class events and chronologies are deferred from initial M06-A;
3. the first parser scope is narrow and excludes OCR, scanned PDFs, office documents, and rendered capture;
4. local lexical search and the deterministic chunk contract belong in M06-A, while embeddings and Qdrant remain deferred.
