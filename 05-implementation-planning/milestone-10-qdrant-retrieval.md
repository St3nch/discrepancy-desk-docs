# Milestone 10 — Qdrant Semantic Retrieval

## Status
Not started.

## Goal
Add semantic retrieval only after the Anomaly Vault has enough high-quality material to evaluate it honestly.

## Required Reading
- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `03-system-design/obsidian-qdrant-sqlite-plan.md`
- M05 completion record
- current embedding/model retention and cost research

## Entry Gate
Sufficient curated notes exist; baseline keyword/link retrieval has been measured; owner approves Qdrant admission.

## Scope
Chunking experiment, embeddings, Qdrant collection, provenance-preserving retrieval, context builder, deletion/reindex behavior, quality evaluation, fallback path.

## Out of Scope
Replacing canonical Markdown, letting vectors become evidence, autonomous conclusions, storing secrets or unapproved private data.

## Required Evidence
Labeled retrieval corpus with expected relevant, irrelevant, ambiguous, and no-answer outcomes; keyword/link baseline comparison; precision, recall, top-k hit rate, irrelevant-result rate, and no-answer correctness; stale/orphan/duplicate-vector tests; metadata-filter failure tests; interrupted-ingestion and model/chunking-version tests; full rebuild proof; unavailable-Qdrant fallback; provenance retained in every result.

## Exit Gate
Semantic retrieval demonstrably improves useful recall without becoming canonical truth or breaking safe operation when unavailable.

## Completion Record
Record date, model/version, evidence, evaluation results, limitations, and next action.