# R-M06-08 — Retrieval, Chunking, Graph, and Qdrant Readiness

## Research status

Research report completed on 2026-07-20.

This report is source material for M06 architecture synthesis. It does not authorize Qdrant installation, collection creation, embedding generation, graph extraction, GraphRAG, chunk-schema adoption, provider admission, retrieval implementation, or migration of canonical authority out of SQLite and governed files.

All options remain subject to owner review and later independent Claude review.

## Question and project boundary

How should The Discrepancy Desk prepare canonical Vault records for later lexical, semantic, graph-assisted, and hybrid retrieval while preserving account isolation, exact source lineage, correction propagation, reproducibility, poisoning resistance, and complete rebuildability?

The report covers:

- canonical chunk identity;
- source locators and document versions;
- lexical, dense, sparse, and hybrid retrieval;
- metadata and account filtering;
- Qdrant collection and tenant strategies;
- embedding model/version lineage;
- invalidation and rebuild;
- graph and GraphRAG derivatives;
- retrieval poisoning and prompt injection;
- evaluation and fail-closed behavior;
- backup and restore implications.

It does not design implementation code or admit a retrieval engine.

---

# 1. Executive findings

## 1.1 Retrieval structures are disposable derivatives

**PROJECT RECOMMENDATION**

The canonical Vault should remain:

```text
SQLite authority and review state
+ immutable original artifacts
+ versioned normalized element packages
```

Future retrieval structures should be rebuildable:

```text
lexical index
Qdrant points and vectors
embedding caches
graph nodes/edges
community summaries
reranking features
```

Loss or corruption of retrieval infrastructure must not destroy research truth.

## 1.2 Chunks must derive from canonical elements

**PROJECT RECOMMENDATION**

A chunk should have a deterministic identity derived from:

```text
account_id
document_version_id
ordered element IDs
chunking_strategy_id
chunking_strategy_version
```

It must retain exact source locators and must not become the only copy of the text.

## 1.3 Account filtering occurs before ranking

**PROJECT RECOMMENDATION**

Every retrieval path must enforce the exact account boundary and admitted record state before lexical, semantic, graph, or reranking logic.

A high similarity score cannot override isolation.

## 1.4 Hybrid retrieval is likely stronger than semantic-only retrieval

**VERIFIED FACT**

Qdrant supports dense, sparse, and multiple named vectors, payload filtering, and hybrid/multi-stage queries including reciprocal-rank fusion.

**INFERENCE**

Claim-heavy research needs exact names, dates, quotations, IDs, and uncommon phrases as well as semantic similarity. A future system should compare lexical, sparse, dense, and hybrid retrieval rather than assuming dense vectors are sufficient.

## 1.5 Graph outputs are candidates, not facts

**VERIFIED FACT**

Microsoft GraphRAG extracts entities, relationships, claims, communities, and summaries from text units using LLM- or NLP-based workflows.

**PROJECT RECOMMENDATION**

Any GraphRAG entity, relationship, claim, or community must be represented as a versioned retrieval derivative. It may suggest candidates for review but may not alter canonical entity identity, accepted assertions, or dossier membership automatically.

---

# 2. Qdrant capabilities and boundaries

## 2.1 Core data model

**VERIFIED FACT**

Qdrant stores points consisting of vectors plus optional payload metadata inside collections. It supports dense, sparse, and multivector configurations, payload filters, indexing, quantization, and hybrid queries.

## 2.2 Multitenancy options

**SOURCE/PROVIDER CLAIM**

Qdrant generally recommends payload-based partitioning for many tenants and notes that separate collections can provide stronger isolation when the tenant count is limited, at greater resource cost.

**ACCEPTED PROJECT DIRECTION**

D024 favors separate physical Vaults and account-scoped Qdrant collections.

**PROJECT RECOMMENDATION — OPTION FOR OWNER REVIEW**

For the expected small number of owner-controlled accounts, separate collections per account and embedding configuration remain the safer leading option. Shared payload-partitioned collections should not be adopted merely because they are efficient at SaaS scale.

## 2.3 Snapshots are not canonical backup

**VERIFIED FACT**

Qdrant collection snapshots contain collection configuration, points, and payload for a specific collection/node. Collection aliases are not included, and distributed deployments require node-aware handling.

**PROJECT RECOMMENDATION**

Qdrant snapshots may speed recovery but must not replace canonical Vault backup. A complete rebuild from canonical records must remain possible.

## 2.4 Qdrant is not a graph or ontology engine

**SOURCE/PROVIDER CLAIM**

Qdrant's documentation states that it does not plan to provide built-in ontologies or knowledge graphs.

**INFERENCE**

Canonical relationship authority must remain outside Qdrant. Graph traversal, if admitted later, requires a separate derivative layer or application logic.

---

# 3. Candidate chunk model

## 3.1 Chunk inputs

**PROJECT RECOMMENDATION**

Chunks should be built only from admitted normalized document versions and should identify:

```text
account_id
source_item_id
observation_id
artifact_id
document_version_id
element IDs
source locators
language
content type
parser/transcript provenance
admission state
correction/supersession state
```

## 3.2 Chunk boundaries

Candidate boundaries include:

```text
paragraph or heading group
PDF page/region sequence
transcript time window with speaker boundary
feed entry section
JSON object/array subtree
manual note section
```

Fixed character windows may be used only as a fallback and should still reference the canonical elements they overlap.

## 3.3 Context preservation

A retrieval chunk may include a generated contextual prefix identifying document title, section, source, date, and relationship to the larger document. The prefix is a derivative and must be separately identifiable from source text.

## 3.4 Overlap

Overlap may improve retrieval but creates duplicate evidence. The context builder must deduplicate overlapping hits and cite the canonical source region rather than presenting repeated chunks as separate corroboration.

## 3.5 Stable identity and invalidation

A chunk ID should change when:

- its source document version changes;
- included element membership changes;
- chunking strategy/version changes;
- account changes.

An embedding ID should also include embedding provider/model/version and preprocessing version.

---

# 4. Candidate retrieval modes

## 4.1 Exact and metadata retrieval

Use for:

- stable IDs;
- URLs;
- people/organization identifiers;
- dates;
- source names;
- document types;
- policy/admission states.

This should remain available without Qdrant.

## 4.2 Lexical retrieval

Useful for exact phrases, quotations, rare names, codes, and terminology.

## 4.3 Dense semantic retrieval

Useful for concept similarity and paraphrases, but may retrieve persuasive or topically similar material lacking the needed evidentiary relationship.

## 4.4 Sparse semantic/lexical vectors

Can preserve term-level matching while supporting learned weighting.

## 4.5 Hybrid retrieval

**PROJECT RECOMMENDATION — NOT APPROVED**

A future evaluation should compare:

```text
lexical only
dense only
sparse only
lexical + dense
sparse + dense
hybrid + metadata filters + reranking
```

## 4.6 Graph-assisted retrieval

May expand from accepted entities/events/relationships or from derivative candidates. The retrieval trace must distinguish canonical accepted edges from model-extracted candidate edges.

---

# 5. Candidate Qdrant payload

**PROJECT RECOMMENDATION — OPTION FOR OWNER REVIEW**

A point payload may need fields conceptually equivalent to:

```text
account_id
canonical_chunk_id
document_version_id
source_item_id
observation_id
element_ids
locator_summary
record_type
language
published_at
observed_at
admission_state
truth/support status
editorial-use state
correction/supersession state
chunking strategy/version
embedding model/version
content hash
```

This is not approved schema.

Payload metadata must be sufficient to reject stale, superseded, cross-account, or disallowed results before model context assembly.

---

# 6. Embedding governance

## 6.1 Model lineage

Record:

```text
provider
model name/version
dimension
distance metric
preprocessing
input hash
created_at
```

## 6.2 Model replacement

Embedding model changes require a new collection or named-vector version and a controlled rebuild. Old and new scores should not be treated as directly comparable without evaluation.

## 6.3 Provider-hosted inference

If Qdrant Cloud or another provider generates embeddings, provider retention, training, regional, credential, and deletion boundaries require explicit admission.

## 6.4 Local embeddings

Local models may reduce provider disclosure but create hardware, model provenance, patching, and reproducibility obligations.

No embedding model is selected.

---

# 7. Graph and GraphRAG readiness

## 7.1 GraphRAG pipeline

**VERIFIED FACT**

Microsoft GraphRAG slices documents into text units, extracts entities, relationships, and optional claims, performs community detection, generates community reports, and embeds outputs.

## 7.2 Cost and versioning

**SOURCE/PROVIDER CLAIM**

GraphRAG warns that indexing can consume significant LLM resources and documents breaking/version migration considerations.

## 7.3 Candidate graph derivative contract

**PROJECT RECOMMENDATION**

Every graph node/edge/report should bind to:

```text
account_id
source text-unit IDs
extraction pipeline/version
model/provider
prompt/config version
output hash
review state
canonical identity mapping when reviewed
```

## 7.4 No automatic canonical merge

Graph-extracted `John Smith` nodes must not merge canonical entities. Relationship edges must remain candidate assertions unless accepted independently.

## 7.5 Graph option timing

GraphRAG should not be required for initial M06. Canonical entities, assertions, events, dossiers, and exact retrieval should be proven first. Graph experiments belong after stable chunk and identity contracts exist.

---

# 8. Poisoning and adversarial retrieval

## 8.1 Vector stores create a poisoning surface

**VERIFIED FACT**

OWASP LLM08:2025 identifies vector/embedding weaknesses, data poisoning, access-control failures, and cross-context leakage as material RAG risks.

## 8.2 Retrieval is not trust

A high-ranked result may be:

- malicious;
- duplicated;
- biased;
- superseded;
- irrelevant but semantically similar;
- from a low-quality source;
- prompt-injection content.

## 8.3 Candidate controls

```text
account and admission filters before ranking
source and observation diversity
correction/supersession exclusion
per-source caps
near-duplicate collapse
contradiction expansion
prompt-injection labeling
human-pinned evidence
retrieval trace and score disclosure
no tool authority from retrieved content
```

No sanitization mechanism proves safety.

---

# 9. Invalidation and rebuild

## 9.1 Trigger events

Retrieval derivatives should invalidate on:

- document correction or supersession;
- account-policy changes affecting eligibility;
- entity merge/split when payload mappings change;
- parser/normalizer replacement;
- chunk strategy change;
- embedding model change;
- deletion or retention disposition;
- compromised provider/model discovery.

## 9.2 Rebuild register

A rebuild should record:

```text
campaign ID
account ID
canonical source snapshot/hash
chunk strategy/version
embedding strategy/version
expected counts
actual counts
errors
excluded records
completion state
```

## 9.3 Fail closed

If the derivative index is stale, partially rebuilt, or cannot prove account filtering, semantic retrieval should be disabled. Exact canonical retrieval should remain available.

---

# 10. Evaluation requirements

A future benchmark should include:

- exact quotation retrieval;
- paraphrase retrieval;
- rare entity/name retrieval;
- contradictory evidence;
- superseded document rejection;
- duplicate collapse;
- cross-account leakage attempts;
- poisoned chunks;
- hostile instructions;
- missing locator rejection;
- stale embedding rejection;
- entity ambiguity;
- multi-document chronology;
- no-answer cases.

Measure:

```text
recall
precision
citation/locator accuracy
source diversity
contradiction coverage
account-isolation failures
stale-result failures
cost and latency
```

---

# 11. Candidate architecture options

## Q1 — No vector database in M06

Use relational metadata, exact IDs, and lexical/full-text search only.

**Advantages:** simplest, auditable, sufficient for early workflows.

**Risks:** weaker paraphrase discovery.

## Q2 — Per-account Qdrant collections with canonical chunks

**Advantages:** strong account isolation, clear rebuild boundary.

**Risks:** collection overhead and embedding governance.

## Q3 — Shared Qdrant collections with payload partitioning

**Advantages:** efficient at large tenant counts.

**Risks:** filtering mistakes can leak accounts; conflicts with the current safer doctrine.

## Q4 — GraphRAG-first retrieval

**Advantages:** rich corpus-level exploration.

**Risks:** high cost, model-derived graph appears authoritative, premature complexity.

## Q5 — Staged path

```text
M06 canonical elements + exact/lexical retrieval
→ stable chunks and evaluation fixtures
→ later per-account Qdrant experiment
→ hybrid retrieval evaluation
→ optional graph derivatives after identity contracts are proven
```

**Research-leading option — NOT APPROVED:** Q5.

---

# 12. Recommended direction for synthesis

The synthesis should consider approving:

```text
canonical records independent of retrieval
stable element-derived chunk IDs
exact/lexical retrieval in initial M06
Qdrant deferred or admitted only as a bounded later experiment
separate account collections if Qdrant is admitted
hybrid retrieval evaluated against fixed fixtures
all vectors and graphs rebuildable
no model-derived edge or summary accepted automatically
```

---

# 13. Open questions for owner approval and Claude review

1. Should M06 ship without Qdrant and reserve it for M14 as currently planned?
2. Should M06 nevertheless define the chunk contract needed by M14?
3. Is one Qdrant collection per account acceptable despite provider efficiency guidance?
4. Which lexical search capability belongs in M06?
5. Which chunk types should exist initially?
6. Is overlap allowed, and how is duplicate evidence prevented?
7. Must every chunk map to exact element IDs?
8. Which embedding providers/models may be evaluated later?
9. Are local embeddings preferred for sensitive accounts?
10. Should graph extraction wait until M13/M14?
11. May derivative graph edges be displayed before human review?
12. What benchmark thresholds are required?
13. What state disables retrieval during partial rebuild?
14. How are deletions propagated to provider-hosted vectors?
15. Are Qdrant snapshots retained if the index is fully rebuildable?
16. How are account-policy changes reflected in existing payloads?
17. Should retrieval include unreviewed candidates by default?
18. How is source diversity enforced?
19. Which retrieval traces must be visible to humans?
20. What poisoning tests are mandatory?

---

# 14. Primary sources and access dates

Accessed 2026-07-20.

- Qdrant — Documentation
  https://qdrant.tech/documentation/

- Qdrant — Manage Data
  https://qdrant.tech/documentation/manage-data/

- Qdrant — Hybrid and Multi-Stage Queries
  https://qdrant.tech/documentation/search/hybrid-queries/

- Qdrant — Multitenancy
  https://qdrant.tech/documentation/tutorials/multiple-partitions/

- Qdrant — Snapshots
  https://qdrant.tech/documentation/operations/snapshots/

- Qdrant — Fundamentals
  https://qdrant.tech/documentation/faq/qdrant-fundamentals/

- Microsoft GraphRAG — Welcome and process
  https://microsoft.github.io/graphrag/

- Microsoft GraphRAG — Indexing overview
  https://microsoft.github.io/graphrag/index/overview/

- Microsoft GraphRAG — Indexing methods
  https://microsoft.github.io/graphrag/index/methods/

- Microsoft GraphRAG — GitHub documentation
  https://github.com/microsoft/graphrag

- OWASP GenAI — LLM08:2025 Vector and Embedding Weaknesses
  https://genai.owasp.org/llmrisk/llm082025-vector-and-embedding-weaknesses/

- OWASP GenAI — LLM01:2025 Prompt Injection
  https://genai.owasp.org/llmrisk/llm01-prompt-injection/

---

# 15. Research conclusion

M06 should prepare clean, stable, source-located chunks and retrieval eligibility rules without making Qdrant or GraphRAG a prerequisite.

The leading staged direction is:

```text
canonical Vault
→ exact and lexical retrieval
→ stable chunk contract
→ fixed adversarial evaluation
→ later per-account Qdrant experiment
→ optional hybrid and graph derivatives
```

No retrieval option is approved until owner synthesis and Claude review are complete.
