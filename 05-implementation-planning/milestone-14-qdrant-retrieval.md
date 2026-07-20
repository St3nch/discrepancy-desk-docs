# Milestone 14 — Qdrant Semantic Retrieval

## Status
Not started.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- M06 completion record in `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`
- accepted D024 in `99-decisions/decision-log.md`
- `06-research/qdrant-capability-and-boundary-review.md` (must be current before entry)
- `05-implementation-planning/m14-retrieval-evaluation-plan.md` and `05-implementation-planning/m14-retrieval-evaluation-dataset.md` (both must be created before M14 implementation)
- `08-audits/m14-qdrant-architecture-review.md` (must be created before entry)

## Goal
Add provenance-preserving semantic/hybrid retrieval only after the governed corpus and measured need justify it.

## Entry Gate
Substantial high-quality corpus; deterministic retrieval limits measured; fixed evaluation set, embedding/provider/cost boundary, deletion/reindex contract, account-isolation design, and Qdrant hammer plan prepared. Before owner approval, Claude AI must independently review the complete Qdrant proposal against the implemented M06 vault model and current Qdrant capabilities. The review must challenge collection topology, filters, deletion/reindex behavior, backup/rebuild assumptions, cross-account leakage, and failure recovery. The review is advisory; the owner retains decision authority.

## Leading Multi-Account Proposal — Review Required
- one physical Markdown vault per account that needs research memory
- one Qdrant collection per account vault as the initial isolation model
- one shared Qdrant service/instance is acceptable initially; separate Qdrant instances are not required unless a later legal, customer, security, or operational boundary justifies them
- every point carries `account_id`, `vault_id`, canonical record ID, source or note revision ID, and embedding-profile/version metadata
- ordinary retrieval defaults to exactly one active account and one allowed collection
- missing account scope fails closed
- cross-account retrieval requires an explicit owner-admitted operation, selected account list, and separate permission
- canonical evidence may be referenced by multiple account vaults, but account-specific interpretation and accepted conclusions remain isolated
- a future neutral shared-source collection may be considered only through a separate reviewed decision; it is not admitted by this milestone definition
- Qdrant is disposable derived infrastructure and must be rebuildable from governed vault and canonical records

This is the leading proposal, not final architecture. Claude AI must independently review it before the M14 entry gate can close.

## Scope
- embeddings for admitted, account-scoped records
- semantic and hybrid retrieval
- source-linked context assembly
- account and vault scope enforcement
- retrieval diagnostics
- correction/deletion propagation
- rebuild/reindex and deterministic fallback
- collection backup for convenience plus authoritative rebuild proof from canonical records

## Out of Scope
Qdrant as canonical truth, source-less context, unreviewed LLM facts, automatic conclusion promotion, ambient cross-account search, a default shared collection, or adoption merely because embeddings are available.

## Exit Gate
Quality is measured against the fixed set; every result traces to canonical records; at least two synthetic account vaults prove collection isolation; a missing or incorrect account scope fails closed; cross-account retrieval is impossible without the explicit admitted operation; stale/deleted/corrected content behaves correctly; collection loss can be rebuilt deterministically; failed Qdrant falls back safely; the operator can inspect why each item was retrieved; the independent Claude review findings are resolved or explicitly ruled on by the owner.