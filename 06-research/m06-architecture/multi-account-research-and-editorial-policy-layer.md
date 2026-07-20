# Multi-Account Research Foundation and Editorial Policy Layer

## Status

Research note recorded for later M06 architecture synthesis and independent Claude review.

This document is not accepted doctrine, onboarding implementation authority, schema approval, shared-Vault approval, or permission to alter account-isolation rules.

The owner must explicitly approve any resulting architecture or onboarding policy.

## Owner Intent

The system must support accounts with materially different editorial standards without weakening the integrity of the underlying research records.

The Discrepancy Desk will frequently work with:

- allegations;
- disputed accounts;
- extraordinary claims;
- conspiracy lore;
- contradictions;
- unresolved historical questions;
- folklore and parody framing.

Other future accounts may require stricter factual confidence, stronger corroboration, narrower source admission, or more conservative publication language.

The system therefore must not assume that every account has the same relationship between uncertainty and editorial usefulness.

## Core Candidate Principle

```text
One governed research foundation
plus
account-specific evidence, framing, and publication policy
```

The research layer should preserve what was observed, where it came from, how it was acquired, what was claimed, what supports or disputes it, and what remains unknown.

An account policy should determine how that account may use the material. It must not rewrite the underlying evidence or silently upgrade a claim into accepted fact.

## Required Distinctions

The architecture synthesis should evaluate separate fields or governed concepts for:

```text
research status
truth/support status
editorial usability
required publication framing
account-specific approval state
```

Example:

```text
truth/support status: unresolved
editorial usability for Discrepancy Desk: allowed
required framing: attributed and disputed
```

The same research item could be blocked for a stricter account pending independent corroboration.

## Candidate Account Policy Profile

Future account onboarding may need an owner-approved policy profile containing candidates such as:

- admitted source classes;
- prohibited source classes;
- minimum corroboration requirements;
- whether one-source claims may be used;
- whether unresolved claims are editorially usable;
- required attribution and uncertainty labels;
- citation and evidence-link requirements;
- restricted claim categories;
- entity-identity confirmation requirements;
- whether speculative connections may be surfaced;
- tone, persona, and parody rules;
- required review roles or second-source review;
- publication-risk thresholds;
- allowed LLM output classes;
- account-specific stop conditions.

These are research candidates only. The onboarding model is not selected.

## Candidate LLM Context Contract

A bounded LLM context packet may eventually need to communicate:

```text
source material and exact locators
what is directly observed
what is claimed and by whom
what is disputed or unknown
account evidence policy
account editorial-use policy
required framing
voice and persona
allowed outputs
prohibited conclusions or actions
```

The LLM must not infer that material admitted to research memory is automatically publishable for every account.

## Account Isolation Question

Current project doctrine favors separate physical Vaults per account and fail-closed account isolation.

The synthesis should compare, without presuming approval:

### Option A — Fully separate research records

Each account independently stores and reviews its own sources and derivatives.

### Option B — Shared canonical research with account policy overlays

A central research corpus is referenced by multiple account policy layers.

### Option C — Separate physical Vaults with governed import/reference

Accounts remain physically isolated, but an owner-approved operation may import or reference selected research packages with preserved provenance and independent account review.

The currently safer candidate appears to be Option C because it preserves account isolation while allowing intentional reuse. This is not an approved decision.

## Safety and Authority Boundaries

Any accepted design should preserve these rules:

- account policy never mutates original evidence;
- one account's accepted conclusion is not automatically accepted by another account;
- cross-account transfer is explicit, auditable, and owner-governed;
- an LLM cannot change an account policy;
- an LLM cannot promote a claim to accepted truth;
- editorial usability does not equal factual confirmation;
- uncertainty may restrict framing without making material universally useless;
- parody and claim-heavy accounts still require attribution and provenance;
- stricter accounts may impose additional corroboration and review gates.

## Questions for Claude Review

Claude should independently assess:

1. Whether the proposed separation between research status, truth/support status, editorial usability, and publication framing is coherent and enforceable.
2. Whether account policy profiles risk becoming a second uncontrolled authority system.
3. Whether separate physical Vaults with governed import/reference is the safest multi-account model.
4. Which policy fields belong in onboarding versus later per-item review.
5. How policy version changes should affect previously reviewed or drafted material.
6. Whether account-specific conclusions need their own immutable approval binding.
7. How cross-account imports preserve correction, deletion, and supersession lineage.
8. How to prevent a permissive account policy from contaminating a stricter account.
9. Whether research-memory admission and editorial-use approval should be separate workflows.
10. What adversarial tests are required before multi-account onboarding can be implemented.

## Research Dependency

This note should inform:

- R-M06-06 identity, versioning, correction, and deduplication;
- R-M06-07 LLM context assembly and governed reasoning;
- R-M06-08 retrieval and account-scoped derivative design;
- R-M06-09 automation, security, backup, and rebuild;
- the final M06 architecture synthesis;
- the independent Claude architecture review.

## Governing Reminder

```text
Research admission says the system may preserve and examine material.
Account policy says how one account may use that material.
Human approval says whether a specific output may proceed.
None of these automatically establishes that the underlying claim is true.
```
