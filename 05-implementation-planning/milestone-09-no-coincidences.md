# Milestone 09 — No Coincidences Prototype

## Status
Not started.

## Goal
Flag explainable pattern candidates for human review without declaring truth.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `02-product/no-coincidences.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- `00-doctrine/operating-doctrine.md`
- M05 and M08 completion records

## Entry Gate
Anomaly Vault contains enough clean, sourced material; owner approves prototype scope.

## Scope
Exact entity/date/organization matches, source overlap, candidate explanations, provenance, human review states, false-positive handling.

## Out of Scope
Truth scoring, causal claims, autonomous publication, opaque ML ranking, unsourced pattern declarations.

## Required Evidence
Labeled known-positive, known-negative, ambiguous, malformed, and adversarial fixtures; false-positive and false-negative tests; non-detection tests; detector-error and partial-execution tests; provenance/explanation checks; duplicate-source and alias-collision tests; human rejection/suppression flow; proof fabricated, modified, deleted, or missing evidence fails closed.

## Exit Gate
Candidates are reproducible, explainable, reviewable, and harmless when the detector fails or finds nothing.

## Completion Record
Record date, evidence, test results, limitations, and next action.