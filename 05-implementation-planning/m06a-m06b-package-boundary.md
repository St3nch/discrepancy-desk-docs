# M06-A / M06-B Package Boundary

## Status

Owner-approved package split recorded 2026-07-20.

This document defines planning boundaries only. It does not authorize implementation.

## Owner ruling

The independently reviewed Package 2 is divided into two separately governed packages:

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

The purpose of the split is to keep the first Vault implementation local and authority-focused while isolating the first new network and SSRF boundary in its own later package.

## M06-A — Local Manual Vault

### Candidate purpose

Establish the governed per-account Vault foundation and prove that records can be admitted, normalized, searched locally, corrected, backed up, restored, and reconstructed without any network acquisition surface.

### Candidate included capabilities

Subject to owner architecture decisions and later implementation authorization:

- separate account-scoped Vault root;
- SQLite authority for identity, workflow, review state, provenance manifests, and memberships;
- governed filesystem originals and versioned normalized packages;
- manual pasted text;
- manual file upload/import from an owner-selected narrow format set;
- YouTube URL recorded as a locator plus pasted/uploaded transcript, without automatic YouTube calls;
- exact original preservation and hashing;
- parser execution provenance;
- deterministic document-version and element identity;
- local exact/metadata and lexical retrieval;
- chunk identity/invalidation contract definition;
- generated read-only human projections by default;
- assertion, dossier, question, and correction lineage according to the accepted synthesis;
- static context-run/evidence-packet schema definition without live provider integration;
- per-account backup generation and disposable restore proof;
- account-scoped logs, caches, traces, and temporary files;
- adversarial tests for identity, correction, policy binding, citation locators, truncation integrity, backup tampering, and account isolation.

### Explicit exclusions

- HTTP retrieval;
- RSS/Atom polling;
- WebSub;
- browser rendering or automation;
- transcript SaaS;
- official YouTube metadata calls;
- media downloading;
- OCR unless later explicitly admitted into M06-A scope;
- recurring monitoring;
- cross-account import execution;
- live LLM provider integration;
- Qdrant;
- graph/GraphRAG;
- autonomous publication or engagement.

## M06-B — Bounded Static Webpage Retrieval

### Candidate purpose

Add one human-triggered, bounded, single-page network retrieval path after M06-A's canonical authority and recovery behavior are proven.

### Preconditions

M06-B planning may not be treated as implementation authority. Entry requires evidence that M06-A has proven:

- canonical identity and account scope;
- original-artifact preservation;
- normalized-version lineage;
- local retrieval and locator behavior;
- correction and supersession handling;
- backup/restore and rebuild;
- no hidden second authority;
- account-scoped observability;
- fail-closed admission and validation behavior.

### Candidate included capabilities

Subject to a separate owner-approved plan:

- one human-entered public HTTP or HTTPS URL;
- one bounded static request chain;
- strict redirect limits;
- DNS/IP revalidation;
- private, loopback, link-local, metadata, and disallowed-address blocking;
- scheme and port allowlists;
- response-size and timeout limits;
- media-type and content-disposition validation;
- exact response receipts and original artifact hashes;
- HTML normalization through an independently admitted parser;
- no JavaScript execution;
- no login or authenticated retrieval;
- no crawling or link following beyond the explicitly bounded redirect policy;
- no recurring monitoring;
- human review before Vault admission.

### Separate gate required

M06-B requires:

- its own architecture plan;
- source-admission and policy review;
- SSRF threat model;
- hammer-test matrix;
- cost and retry limits;
- evidence and no-overwrite contract;
- explicit owner approval;
- explicit implementation authorization.

## Dependency rule

M06-B may consume the accepted M06-A ingestion envelope and normalized package contract. It must not redefine canonical identity, evidence authority, assertion authority, or account policy.

## Current block

Neither M06-A nor M06-B is authorized for implementation.
