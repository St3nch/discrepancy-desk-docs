# Milestone 06 — Records, Dossiers, and Anomaly Vault

## Status
Not started.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `03-system-design/multi-account-model.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- M05 completion record in `05-implementation-planning/milestone-05-tauri-desktop-foundation.md`
- accepted D024 in `99-decisions/decision-log.md`
- `08-audits/m06-vault-architecture-review.md` (must be created before entry)

## Goal
Create governed research memory without duplicating SQLite operational truth.

## Entry Gate
M05 stable; assertion vocabulary, promotion/correction rules, Markdown/SQLite boundary, multi-account vault-isolation proposal, and vault hammer plan prepared. Before owner approval, an independent Claude AI review must inspect the complete proposal against repo truth, identify cross-account leakage and duplicate-truth risks, and return a written verdict. The review is advisory; the owner retains decision authority.

## Leading Multi-Account Proposal — Review Required
- one physically separate Obsidian-compatible vault per account that needs research memory
- no automatic cross-vault links, search, promotion, or context assembly
- account-specific interpretations, claims, conclusions, voice notes, and editorial memory remain inside that account's vault
- shared canonical evidence may be referenced by more than one vault without sharing account-specific interpretations
- a shared research vault is not admitted by default and would require a later explicit owner decision
- each vault maps explicitly to one `account_id` and later to one account-scoped Qdrant collection

This is the leading proposal, not final architecture. Claude AI must independently review it before the M06 entry gate can close.

## Scope
- topic dossiers, chronology, entities, organizations, questions, related filings
- typed assertions: source fact, extracted claim, allegation, inference, parody framing, accepted conclusion, disputed conclusion, unknown
- evidence/provenance links and human promotion
- Obsidian-compatible records where appropriate
- per-account vault identity, isolation, backup, restore, and broken-link behavior
- correction and broken-link behavior

## Out of Scope
Qdrant, autonomous conclusions, agent memory as truth, general scraping, pattern declarations, and a shared cross-account vault unless separately proposed, reviewed, and owner-approved.

## Exit Gate
Every durable assertion is typed, attributable, evidence-linked, correctable, and unable to promote from candidate to accepted truth without human authority; vault/account isolation is proven with at least two synthetic accounts; backup and restore preserve vault identity; cross-vault retrieval is absent unless explicitly requested; boundary and failure hammer tests pass; the independent Claude review findings are resolved or explicitly ruled on by the owner.