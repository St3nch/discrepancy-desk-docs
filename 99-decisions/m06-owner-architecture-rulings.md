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
