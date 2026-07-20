# Claude Independent Review Prompt — M06 Vault and Ingestion Research Program

You are conducting an independent architecture-research audit for **The Discrepancy Desk**.

Do not edit any files.
Do not implement code.
Do not design database tables as final authority.
Do not select vendors or providers for the owner.
Do not convert recommendations into accepted doctrine.
Do not infer owner approval from the existence of a report.

Your job is to inspect the complete M06 research program, identify contradictions, unsupported conclusions, missing risks, hidden authority changes, multi-account weaknesses, and options that need explicit owner rulings.

## Live repositories

Inspect the current live repositories:

```text
C:\dev\x\discrepancy-desk
C:\dev\x\discrepancy-desk-docs
```

Reconstruct and report branch, HEAD, origin synchronization, and working-tree state. Distinguish direct command reproduction from documentary verification.

## Required project context

Read at minimum:

```text
PROJECT_BRIEF.md
STATUS.md
LLM_MAP.md
00-doctrine/operating-doctrine.md
00-doctrine/human-approval-policy.md
00-doctrine/account-rules-and-boundaries.md
03-system-design/architecture-overview.md
05-implementation-planning/implementation-roadmap.md
05-implementation-planning/m05-to-m06-transition-audit-and-research-plan.md
05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md
99-decisions/decision-log.md
```

Read the complete M06 research workspace:

```text
06-research/m06-architecture/README.md
06-research/m06-architecture/R-M06-01-source-universe-and-admission-policy.md
06-research/m06-architecture/R-M06-02-youtube-audiovisual-ingestion.md
06-research/m06-architecture/R-M06-03-website-feed-and-change-monitoring.md
06-research/m06-architecture/R-M06-04-document-normalization-and-parser-provenance.md
06-research/m06-architecture/R-M06-05-canonical-vault-knowledge-model.md
06-research/m06-architecture/R-M06-06-identity-versioning-correction-and-deduplication.md
06-research/m06-architecture/R-M06-07-llm-context-assembly-and-governed-reasoning.md
06-research/m06-architecture/R-M06-08-retrieval-chunking-graph-and-qdrant-readiness.md
06-research/m06-architecture/R-M06-09-automation-security-backup-and-rebuild.md
06-research/m06-architecture/multi-account-research-and-editorial-policy-layer.md
```

Also read the accepted post-M05 correction records:

```text
08-audits/post-m01-through-m05-audit-disposition.md
08-audits/ac01-post-m05-correction-closure.md
```

## Governing owner boundary

The owner has explicitly stated:

```text
Research may proceed.
No ingestion solution, provider, parser, monitoring method,
Vault architecture, account policy, retrieval design, or implementation
is approved until the owner reviews and accepts the options.
```

The owner also clarified the multi-account requirement:

```text
The Discrepancy Desk is claim-heavy and may responsibly use unresolved,
disputed, historical, or conspiracy-lore material with attribution and framing.
Other future accounts may require stricter corroboration and accuracy policies.
The shared system must cater to both without allowing one account's policy,
conclusions, approvals, or uncertainty tolerance to contaminate another account.
```

Treat these as hard review constraints.

## Core doctrine to protect

```text
AI drafts. Human clears. Database remembers. Metrics judge.
```

Additional boundaries:

- no autonomous posting or engagement;
- no LLM/agent direct database authority;
- no hidden authority transitions;
- exact account isolation;
- separate physical Vaults per account are the current safer doctrine under D024;
- originals and evidence lineage must survive parser/provider/model replacement;
- Qdrant, embeddings, graph structures, and projections must not silently become canonical truth;
- technical feasibility is not permission;
- source content is evidence, not model instruction;
- unreviewed model output is candidate material only;
- the owner approves architecture options and implementation packages.

## Audit questions

### 1. Internal consistency across the nine reports

Check whether terms such as the following are used consistently:

```text
source
source item
occurrence
observation
artifact
document version
element
chunk
mention
entity candidate
accepted entity
assertion
accepted conclusion
event
dossier
chronology
contradiction candidate
connection candidate
policy profile
editorial-use assessment
context packet
retrieval derivative
```

Identify duplicate concepts, incompatible meanings, missing boundaries, or circular dependencies.

### 2. Hidden architecture decisions

Identify any recommendation that is effectively treated as selected even though it is labeled unapproved.

Pay special attention to:

- hybrid SQLite/filesystem authority;
- universal ingestion envelope;
- separate physical Vaults;
- cross-account export/import packages;
- immutable/versioned records;
- static context packets;
- exact/lexical retrieval in M06;
- Qdrant deferred to M14;
- manual-first connector sequencing;
- encrypted backup and `age`;
- Markdown/HTML projections.

State whether each is merely a research-leading option or has already been accepted elsewhere in doctrine.

### 3. Multi-account policy architecture

Review whether the proposed separation is sufficient among:

```text
research admission
truth/support status
editorial usability
required framing
publication approval
```

Test the design mentally against at least these accounts:

1. Discrepancy Desk — claim-heavy, attributed, unresolved material allowed.
2. Accuracy-focused account — stronger corroboration and conservative framing.
3. Satirical account — parody allowed but real factual assertions still controlled.
4. Sensitive/private account — stricter provider and cross-account restrictions.

Look for policy leakage, conclusion leakage, shared-cache leakage, import contamination, policy-version ambiguity, and hidden inheritance.

### 4. Ingestion and normalization

Review whether the manual-first workflows are coherent across pasted text, files, webpages, YouTube/transcripts, feeds, PDFs, images, JSON, and future providers.

Check:

- exact original preservation;
- source identity versus observation;
- parser provenance;
- partial and failed normalization;
- timestamps and locators;
- hostile files;
- prompt injection;
- copyright/retention boundaries;
- provider/native/generated transcript distinctions;
- no invented timestamps or speaker identities.

### 5. Canonical Vault authority

Evaluate the candidate object model and whether it risks becoming either:

- too weak and note-centric;
- too complex and ontology-heavy;
- a hidden strategy/truth engine;
- a second editorial database;
- a generic knowledge graph with ambiguous authority.

Check whether accepted conclusions, attributed claims, source observations, model inferences, editorial framing, and satire/parody are adequately separated.

### 6. Identity, correction, and deduplication

Review stable IDs, external IDs, aliases, merge/split behavior, duplicate relationships, immutable versions, corrections, supersessions, deletion, and cross-account reuse.

Identify failure modes involving:

- two people with the same name;
- one person with multiple identities;
- URL/canonicalization collisions;
- copied articles;
- transcript variants;
- corrected source documents;
- removed or edited videos;
- policy changes after an editorial-use ruling;
- receiving-account re-review of imported material.

### 7. LLM context and prompt injection

Review the proposed trust hierarchy and static structured context-packet option.

Check:

- source content cannot become instruction;
- account policy is exact-version bound;
- unreviewed candidates are labeled;
- contradiction and negative evidence are represented;
- omissions and context limits are disclosed;
- model output cannot accept itself;
- citations map to canonical locators;
- read-only retrieval tools do not create hidden authority;
- provider/model portability and retention are covered.

### 8. Retrieval, chunks, Qdrant, and graph readiness

Review whether chunk identity and invalidation are sufficiently tied to canonical document versions and elements.

Check:

- lexical versus semantic retrieval;
- dense/sparse/hybrid evaluation;
- account filtering before ranking;
- cross-account leakage;
- poisoning and duplicate crowd-out;
- stale/superseded result exclusion;
- Qdrant collection strategy under D024;
- snapshots versus canonical backup;
- graph/GraphRAG derivatives versus accepted relationships;
- whether Qdrant should remain M14.

### 9. Automation, security, backup, and rebuild

Review connector admission, credentials, hostile-input processing, limits, cost, retries, stop conditions, provider changes, retention/deletion, backup generations, encryption, restore proof, account isolation, and derivative rebuild.

Check whether the plan can recover with no SaaS provider, no Qdrant, no embedding model, and no original parser available.

### 10. Scope and sequencing

Determine the smallest coherent initial M06 that would deliver useful manual Vault functionality without prematurely implementing:

- recurring monitors;
- broad crawling;
- transcript SaaS;
- Qdrant;
- GraphRAG;
- autonomous LLM retrieval;
- cross-account sharing;
- full onboarding policy UI.

Do not select the scope for the owner. Present bounded options and tradeoffs.

## Required report format

### A. Repository verification

Report live Git truth and execution limitations.

### B. Executive verdict

Choose one:

```text
RESEARCH PACKAGE COHERENT — READY FOR OWNER OPTION REVIEW
RESEARCH PACKAGE READY WITH CORRECTIONS
RESEARCH PACKAGE NOT READY FOR SYNTHESIS
```

### C. Findings

For every finding provide:

```text
ID
severity: critical | high | medium | low | observation
report/file and section
claim or boundary affected
evidence/reasoning
required correction
suggested validation or owner ruling
```

Order by severity.

### D. Cross-report terminology map

Provide one recommended glossary, but clearly mark it as a review recommendation rather than accepted doctrine.

### E. Owner decision matrix

Create a concise matrix of decisions the owner must make. For each decision include:

```text
decision ID
question
option A
option B
option C when material
tradeoffs
Claude recommendation
what remains blocked until decided
```

At minimum include decisions for:

- initial M06 scope;
- hybrid authority model;
- universal ingestion package;
- initial source workflows;
- account policy layering;
- cross-account import/reference;
- normalized JSON element package;
- editable versus generated projections;
- initial Vault object families;
- event/dossier/chronology timing;
- static LLM context packets;
- LLM provider boundary;
- lexical retrieval;
- Qdrant timing and collection isolation;
- graph/GraphRAG timing;
- connector timing;
- monitoring timing;
- backup/encryption scope;
- deletion/retention policy.

### F. Minimum coherent M06 packages

Provide 2–4 bounded package options, for example:

```text
minimal manual Vault
manual Vault plus local normalization
manual Vault plus one human-triggered metadata connector
broader architecture package
```

For each list inclusions, exclusions, risks, and evidence required before closure.

### G. Required adversarial matrix themes

List the hammer-test themes needed after owner decisions, without writing implementation tests.

### H. Final gate statement

State explicitly:

- whether owner option review may begin;
- whether architecture synthesis may begin;
- whether any implementation is authorized (it is not);
- what must be corrected or decided first.

## Important review behavior

- Verify before claiming.
- Prefer primary sources when checking external technical claims.
- Do not mistake a provider's documentation for project admission.
- Do not dismiss claim-heavy editorial use as inherently inaccurate; evaluate whether attribution, uncertainty, and account-specific policy safely govern it.
- Do not weaken accuracy-focused accounts to match Discrepancy Desk.
- Do not strengthen Discrepancy Desk into a generic academic publishing system.
- Preserve owner authority throughout.
