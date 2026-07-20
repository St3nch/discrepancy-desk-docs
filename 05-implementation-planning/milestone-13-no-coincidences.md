# Milestone 13 — No Coincidences Pattern Candidates

## Status
Not started.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `02-product/no-coincidences.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `05-implementation-planning/hammer-test-strategy.md`
- M06 completion record in `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`
- `03-system-design/multi-account-model.md`
- D010 in `99-decisions/decision-log.md`
- `08-audits/m13-no-coincidences-architecture-review.md` (must be created before entry)

## Goal
Flag explainable overlaps in governed records without declaring truth.

## Entry Gate
Sufficient governed dossier/source material; assertion types stable; candidate-review lifecycle and detector hammer plan approved.

## Scope
- exact name/entity/date/phrase/source/organization overlap
- chronology proximity and conflicting-statement candidates
- explanation, provenance, confidence/ambiguity, human review and rejection
- deliberate non-detection, detector-error, fabricated-ID, and false-positive tests

## Hard Rule
The system produces pattern candidates only. It never declares conspiracy, causation, intent, or accepted truth.

## Exit Gate
Every candidate explains why it was flagged and traces to canonical records; detector failures fail closed; false positives can be rejected; accepted interpretations remain human-controlled; no candidate auto-promotes into publication truth.