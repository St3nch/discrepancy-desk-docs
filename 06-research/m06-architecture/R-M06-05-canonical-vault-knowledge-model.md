# R-M06-05 — Canonical Vault and Knowledge Model

## Research status

Research report completed on 2026-07-20.

This report is source material for M06 architecture synthesis. It does not authorize implementation, schema adoption, database changes, graph construction, entity resolution, automatic assertion promotion, Qdrant indexing, or any Vault format.

Every object type, status vocabulary, authority rule, relationship model, and storage option in this report remains subject to owner review during architecture synthesis.

## Question and project boundary

What durable concepts must The Discrepancy Desk represent so that heterogeneous evidence can support research, dossiers, chronologies, questions, contradictions, content drafting, and later correlation retrieval without confusing source text, extracted claims, human conclusions, entity guesses, or LLM-generated relationships with accepted truth?

The report covers candidate concepts for:

- sources, source items, occurrences, observations, and artifacts;
- normalized documents and addressable source regions;
- mentions and entity candidates;
- entities, aliases, identity assertions, and external identifiers;
- events and temporal assertions;
- claims, allegations, quotations, facts, inferences, unknowns, and accepted conclusions;
- evidence support, contradiction, qualification, and uncertainty;
- dossiers, chronologies, questions, topics, and connection candidates;
- human review, correction, supersession, and provenance;
- operational authority boundaries between SQLite, files, human-readable projections, and future retrieval indexes.

It does not design physical database tables or implement an ontology.

---

# 1. Research method and evidence labels

This report uses:

- **VERIFIED FACT** — directly supported by a cited authoritative source.
- **SOURCE/PROVIDER CLAIM** — a capability or model described by its publisher.
- **INFERENCE** — a reasoned implication from verified material.
- **PROJECT RECOMMENDATION** — a proposed Discrepancy Desk rule requiring owner acceptance.
- **OPEN QUESTION** — unresolved and unsafe to treat as settled.

Primary research inputs include W3C PROV-O, W3C Web Annotation, SKOS, PREMIS, CIDOC CRM, Wikidata's statement model, Schema.org ClaimReview, ActivityStreams 2.0, JSON Schema, and Microsoft GraphRAG documentation.

---

# 2. Executive findings

## 2.1 The Vault must not have one generic “note” as its canonical object

**PROJECT RECOMMENDATION**

A generic Markdown note cannot safely carry every role. The system needs distinct durable concepts because these statements are materially different:

```text
A webpage contains the sentence “Agency X concealed the file.”
A named speaker alleged that Agency X concealed the file.
A parser extracted a possible allegation from a transcript.
A human reviewer believes the allegation is supported.
The owner accepted the allegation as sufficiently established for a specific editorial use.
```

Collapsing them into one note or graph edge would erase attribution, epistemic status, and authority.

## 2.2 The candidate canonical chain

**PROJECT RECOMMENDATION**

The leading conceptual chain is:

```text
Source
  → Source item
  → Observation/acquisition
  → Original artifact
  → Normalized document version
  → Addressable element or region
  → Mention / extracted assertion candidate
  → Human-reviewed assertion
  → Dossier, chronology, question, or connection candidate
```

Each transition produces a derivative. No derivative silently replaces its parent.

## 2.3 Assertions need attribution, qualifiers, evidence links, and review state

**VERIFIED FACT**

Wikidata represents knowledge as subject–predicate–object statements, while qualifiers can restrict when, where, or under what conditions a statement applies. References and rank/deprecation are material when deciding whether a statement remains applicable.

**PROJECT RECOMMENDATION**

A Discrepancy Desk assertion should not be a bare triple. It needs at least:

```text
assertion identity
assertion type
subject or quoted proposition
predicate/relation when structured
object/value when structured
speaker/source attribution
source-region locators
time/place/jurisdiction qualifiers
supporting and contradicting evidence links
review state
confidence or uncertainty class
correction/supersession lineage
account scope
```

## 2.4 Events should be first-class candidates

**VERIFIED FACT**

CIDOC CRM is an event-centric conceptual model intended to integrate heterogeneous historical information across people, objects, places, activities, and time.

**INFERENCE**

Chronology and discrepancy analysis become unreliable if events exist only as prose inside dossier notes.

**PROJECT RECOMMENDATION**

Events should be explicit candidate objects with participants, places, time assertions, evidence, and uncertainty. A source mentioning an event does not automatically establish that the event occurred as described.

## 2.5 Annotations and corrections must target exact source regions

**VERIFIED FACT**

The W3C Web Annotation Data Model separates an annotation body from its target and supports selectors such as text quotes, text positions, CSS/XPath selectors, data positions, SVG selectors, and temporal state.

**PROJECT RECOMMENDATION**

Claims, corrections, entity mentions, and human notes should point to stable normalized element IDs and source-specific locators instead of relying only on copied excerpts.

## 2.6 Provenance should be explicit but the project need not implement RDF

**VERIFIED FACT**

PROV-O models provenance around entities, activities, and agents. PREMIS similarly records digital objects, events, agents, and rights for preservation purposes.

**PROJECT RECOMMENDATION**

The project should adopt the useful conceptual distinctions—artifact, transformation activity, actor/tool, event, and lineage—without requiring an RDF store or full standards implementation.

## 2.7 LLM and detector outputs must remain candidates

**SOURCE/PROVIDER CLAIM**

Microsoft GraphRAG extracts entities, relationships, and optional claims from text units to build retrieval structures.

**INFERENCE**

This demonstrates a useful derivative model, not an authority model. Extraction can be wrong, entity resolution can merge distinct people, and co-occurrence can create misleading relationships.

**PROJECT RECOMMENDATION**

LLM-produced entities, claims, relations, topics, summaries, and correlations must enter as reviewable candidates with model/version/prompt provenance. They may not create accepted knowledge directly.

---

# 3. Candidate object families

## 3.1 Source identity

A source identity represents the publisher, channel, account, person, organization, feed, API, archive, or collection from which source items originate.

Examples:

```text
YouTube channel
X account
news website
RSS feed
agency records portal
podcast
public dataset
human contributor
```

Candidate fields:

```text
source_id
account_scope
source_type
canonical_name
platform/domain
external identifiers
publisher/owner identity candidates
admission status
acquisition methods allowed
retention constraints
policy review reference
created_at
supersession or merge lineage
```

**PROJECT RECOMMENDATION**

A source identity is not automatically an entity in the research graph. A YouTube channel identity and an organization entity may refer to the same real-world organization, but that relationship requires an identity assertion.

## 3.2 Source item identity

A source item is a stable item within a source system:

```text
YouTube video ID
X Post ID
article canonical identity
podcast episode GUID
government document number
API object ID
uploaded local file identity
```

A source item can have many observations and versions.

Candidate fields:

```text
source_item_id
source_id
external_id
canonical locator
item type
first observed time
current known availability state
identity confidence
```

## 3.3 Source occurrence

A source occurrence represents where or how an item was encountered.

Examples:

```text
same PDF downloaded from two agency URLs
same article syndicated by two domains
same transcript supplied by two services
same video linked from a newsletter and a webpage
```

**PROJECT RECOMMENDATION**

Occurrence must remain separate from item identity so the system does not lose acquisition context or falsely duplicate content.

## 3.4 Observation

An observation records what was seen at a time through a method.

Examples:

```text
video title observed on July 20
webpage HTTP response observed on July 22
X Post metrics observed on July 25
RSS entry observed through a feed poll
human reported that a document was unavailable
```

Candidate fields:

```text
observation_id
source_item_id or occurrence_id
observed_at
observer type and identity
acquisition method
request/provider receipt
raw artifact references
status and warnings
policy/admission basis
```

The observation is the bridge between changing external systems and preserved artifacts.

## 3.5 Original artifact

The original artifact is the exact acquired or supplied representation:

```text
HTTP response body
API JSON
uploaded PDF
pasted transcript text
SRT file
screenshot
image
feed XML
human-authored note text
```

R-M06-04 recommends preserving exact bytes or exact supplied text with hashes and media-type evidence. This report treats that artifact as evidence, not knowledge interpretation.

## 3.6 Normalized document version

A normalized document version is a parser-produced derivative tied to:

```text
one or more original artifacts
parser/tool identity and version
configuration
schema version
warnings
output hash
creation time
```

It contains addressable elements and source locators but does not overwrite the original.

## 3.7 Addressable element or region

An addressable region is a specific target inside an artifact or normalized document:

```text
PDF page and bounding box
HTML heading or paragraph
text quote and character range
transcript time range
image region
JSON Pointer
feed entry
spreadsheet cell range
```

This is the preferred target for:

- assertions;
- quotations;
- entity mentions;
- corrections;
- annotations;
- visual observations;
- retrieval chunks later.

---

# 4. Mentions, entities, and identity

## 4.1 Mention

A mention records that a particular string, image region, speaker label, or source element may refer to an entity.

Candidate fields:

```text
mention_id
source region
surface form
mention type
candidate entity IDs
extractor identity
confidence/warnings
human disposition
```

A mention is not an entity.

## 4.2 Entity candidate

Entity candidates may represent:

```text
person
organization
location
government body
publication
website/channel/account
object or artifact
event series
product/project
other named thing
```

**PROJECT RECOMMENDATION**

Entity candidates should begin unresolved unless an external stable identifier or human decision supports identity.

## 4.3 Entity identity assertion

An identity assertion states that:

```text
mention M refers to entity E
external ID X identifies entity E
source account A is operated by organization E
entity E1 is the same as / different from E2
```

Identity assertions need provenance and review state. Automatic `same_as` merges are dangerous because they can contaminate every later dossier, chronology, and correlation.

## 4.4 Aliases and labels

Aliases should not be stored as unqualified strings only. Candidate metadata includes:

```text
alias text
language
script
alias type
source attribution
valid time
confidence/review state
```

SKOS provides useful conceptual patterns for preferred labels, alternative labels, broader/narrower topics, and related concepts. The project may borrow these distinctions without adopting SKOS/RDF wholesale.

## 4.5 External identifiers

Possible identifiers include:

```text
Wikidata QID
ORCID
ROR
DOI
YouTube channel/video ID
X user/post ID
government document number
domain
ISBN/ISSN
local stable ID
```

External identifiers are evidence for identity, not infallible truth. Provider corrections and identifier reassignment must remain possible.

---

# 5. Assertions and epistemic types

## 5.1 Why a closed assertion vocabulary is needed

The project already identifies candidate types such as source fact, extracted claim, allegation, inference, parody framing, accepted conclusion, disputed conclusion, and unknown. R-M06-05 finds that this direction is sound but requires sharper boundaries.

## 5.2 Candidate assertion classes

### `source_content_fact`

A directly verifiable fact about what an artifact contains.

Examples:

```text
The PDF contains the phrase “Project Lantern.”
The video title was X at observation time T.
The transcript segment from 12:10–12:22 contains statement Y.
```

This does not establish that the content's underlying claim is true.

### `attributed_claim`

A proposition attributed to a named or described speaker/source.

```text
Person P stated proposition X.
Document D alleges proposition X.
Organization O reported proposition X.
```

### `quotation`

An exact or bounded excerpt linked to a source region. It must record whether punctuation, ellipses, normalization, translation, or correction occurred.

### `allegation`

An attributed claim involving alleged wrongdoing, concealment, misconduct, disputed conduct, or other high-risk accusation. It requires especially strong attribution and should never be promoted by a model.

### `observation_assertion`

A human or instrument reports an observed condition:

```text
URL returned 404 at T.
Object appeared in frame at time range T1–T2.
Post metrics were N at observation time.
```

### `derived_inference`

A conclusion reasoned from one or more assertions or observations. It must identify derivation inputs and actor/tool.

### `relationship_candidate`

A possible relation between entities/events/assertions generated from co-occurrence, shared evidence, metadata, semantic similarity, or human hypothesis.

### `accepted_conclusion`

A human-authorized conclusion for a specified scope and use. Acceptance must not erase disagreement, uncertainty, or supporting/contradicting evidence.

### `disputed_conclusion`

A conclusion explicitly preserved as contested, with disagreement attribution.

### `unknown`

A durable representation that a material question remains unresolved or evidence is insufficient.

### `parody_or_editorial_framing`

A proposed rhetorical or fictional framing for content. It is not a research assertion and must never be retrieved as factual evidence without its type.

## 5.3 Assertions need scope

Assertions may be limited by:

```text
time interval
place
jurisdiction
source version
speaker
account/editorial context
hypothetical or quoted context
translation
uncertainty
```

Wikidata's qualifier model demonstrates why these restrictions materially affect statement meaning.

## 5.4 Support and contradiction

Evidence links should express roles rather than a generic list:

```text
supports
contradicts
qualifies
mentions
quotes
contextualizes
supersedes
corrects
is_source_for_attribution_only
```

**PROJECT RECOMMENDATION**

“Supports” should not be assigned automatically merely because a chunk is semantically similar to an assertion.

## 5.5 Assertion review state

Candidate states for later owner approval:

```text
extracted_candidate
human_review_needed
accepted_for_research
accepted_for_editorial_use
rejected
superseded
corrected
withdrawn
unknown/unresolved
```

An assertion's epistemic type and workflow state are different dimensions. An `allegation` can be accepted as an accurately attributed allegation while remaining unproven as an underlying event.

---

# 6. Events and chronology

## 6.1 Event candidate

An event candidate represents a bounded happening or state change described by evidence.

Candidate fields:

```text
event_id
event type/title
start/end time assertions
place assertions
participant-role assertions
source-region evidence
status/review state
uncertainty
parent/part-of relations
correction/supersession lineage
```

## 6.2 Time assertions

A time assertion may be:

```text
exact instant
exact interval
calendar date
month/year
approximate
before/after another event
source-stated but unverified
unknown
```

The system should preserve source wording separately from normalized date interpretation.

## 6.3 Participant roles

Participants need explicit roles:

```text
speaker
subject
publisher
author
witness
alleged actor
recipient
location owner
source custodian
```

A person mentioned near an event must not automatically become a participant.

## 6.4 Chronology

A chronology is a governed ordered view over event and observation candidates, not an independent duplicate copy of their facts.

Candidate chronology objects may define:

```text
scope/dossier
included event IDs
ordering policy
uncertain-date display
conflict display
human notes
projection version
```

## 6.5 Contradictory timelines

If sources disagree, the system should preserve multiple time assertions and identify the conflict rather than selecting one silently.

---

# 7. Dossiers, topics, questions, and contradictions

## 7.1 Dossier

A dossier is a curated research workspace that groups references without becoming a second source of truth.

Candidate contents:

```text
dossier identity and account scope
purpose/scope
linked source items and observations
linked entities/events/assertions
chronologies
open questions
connection candidates
human summaries/projections
editorial-use notes
correction history
```

The dossier should link to canonical records. Human-readable Markdown may project the dossier but should not independently own assertion state.

## 7.2 Topic/concept

A topic is a controlled research concept, not merely a free-form tag.

Candidate distinctions:

```text
preferred label
alternative labels
broader/narrower concepts
related concepts
account-local versus shared concept
status
source/reviewer
```

SKOS is useful as a conceptual reference for labels and semantic relationships.

## 7.3 Research question

A question is first-class because unknowns drive acquisition and editorial caution.

Candidate fields:

```text
question_id
question text
scope/dossier
related assertions/entities/events
priority
status
answer candidate links
closure decision
created/reviewed by
```

Statuses might include:

```text
open
partially_answered
answered
unanswerable_with_current_evidence
superseded
withdrawn
```

## 7.4 Contradiction candidate

A contradiction candidate links assertions that may conflict.

It should record:

```text
left assertion
right assertion
conflict dimension
source/qualifier comparison
machine or human proposer
explanation
review state
```

Different time, place, definition, or speaker scope can make apparently conflicting assertions compatible. Therefore contradiction detection remains advisory.

---

# 8. Connection candidates and correlations

## 8.1 Connection candidate

A connection candidate represents a proposed relationship worthy of review.

Possible signals:

```text
shared entities
shared identifiers
shared sources
shared phrases
semantic similarity
time proximity
place proximity
common event participation
citation/reference path
contradictory claims
human hypothesis
```

## 8.2 Required explanation

Every generated candidate should carry machine-readable signals and a human-readable explanation:

```text
why these records were connected
which evidence regions produced the signals
which model/rule/version generated it
what uncertainty remains
whether any signal depends on unresolved entity identity
```

## 8.3 Candidate is not accepted relation

GraphRAG documentation demonstrates that relationships may be derived from text-unit co-occurrence. This is useful for retrieval but insufficient as accepted factual relation.

**PROJECT RECOMMENDATION**

The project should preserve separate concepts for:

```text
text co-occurrence edge
retrieval similarity edge
extracted relationship candidate
human-accepted relationship assertion
```

## 8.4 No Coincidences boundary

M13 may later generate pattern candidates, but M06 should define the objects and review boundary only. It should not implement scoring or pattern declaration.

---

# 9. Provenance and lineage

## 9.1 Provenance roles

Drawing from PROV-O and PREMIS, each derived record should identify:

```text
input entity/artifact
activity/transformation
agent: human, parser, provider, model, or application
started/completed time
configuration/model/prompt/schema version
output artifact/record
warnings and disposition
```

## 9.2 Agent classes

Candidate agent types:

```text
human owner/reviewer
human external contributor
Discrepancy Desk application
parser/OCR/ASR tool
third-party provider
LLM/model
scheduled worker
platform/API
```

ActivityStreams also distinguishes generalized actors such as Person, Organization, Application, Group, and Service. These may inform external-source actor records without becoming the internal authority model.

## 9.3 Revision and supersession

Records should not be silently edited when the change matters to provenance.

Candidate lineage relations:

```text
revision_of
supersedes
corrects
retracts
reprocesses
translated_from
derived_from
replaces_projection
```

## 9.4 Deletion and unavailability

External disappearance must append an observation:

```text
unavailable at T
removed/deleted according to platform
access denied
content changed
unknown retrieval failure
```

It should not delete preserved evidence automatically.

---

# 10. Candidate authority matrix

This matrix is research material only.

| Concept | Candidate canonical authority | Human-readable projection | Future Qdrant role |
|---|---|---|---|
| source identity/admission | SQLite | Markdown/HTML view | filter pointer only |
| source item and observation | SQLite + original artifact reference | view | metadata filter |
| original bytes/files | governed filesystem | preview | not canonical |
| normalized document package | versioned JSON artifact + SQLite manifest | Markdown/HTML | chunk source |
| source region/element | normalized package + manifest | citations/highlights | pointer payload |
| mention/entity candidate | SQLite | dossier/entity view | retrieval metadata |
| assertion and review state | SQLite | dossier/claim view | indexed derivative only |
| event candidate | SQLite | chronology view | indexed derivative only |
| dossier membership | SQLite | Markdown dossier projection | scope filter |
| human summary/editorial note | versioned governed record | Markdown | optional indexed derivative |
| vector/chunk | none; rebuildable | none | Qdrant only |

**PROJECT RECOMMENDATION**

Markdown should be a projection/editing surface only where explicitly admitted. It should not create an invisible second authority for assertion state, entity identity, or dossier membership.

---

# 11. Candidate architecture options

No option is approved.

## Option K1 — Markdown-centric Vault

Canonical research records are Markdown files with frontmatter and links.

### Benefits

- human-readable;
- easy backup and manual inspection;
- compatible with many editors;
- low initial complexity.

### Risks

- difficult transactional integrity;
- ambiguous concurrent edits;
- fragile IDs and link repair;
- duplicate operational truth;
- weak enforcement of assertion types and review state;
- difficult account isolation and exact mutation audit.

## Option K2 — Relational canonical knowledge model

SQLite owns sources, observations, entities, assertions, events, dossiers, and lineage. Files hold originals and generated projections.

### Benefits

- strong IDs, constraints, transactions, and audit;
- consistent account scope;
- explicit authority;
- easier correction and review gates.

### Risks

- complex schema;
- less directly editable by humans;
- migrations become significant;
- risk of over-modeling before real use.

## Option K3 — Property graph canonical model

A graph database owns entities, relationships, events, and claims.

### Benefits

- natural traversal;
- flexible relation model;
- potentially useful for correlations.

### Risks

- premature new persistence authority;
- weak fit for immutable artifact/version workflow;
- easy promotion of candidate edges into apparent truth;
- more operations, backup, and isolation complexity;
- conflicts with current SQLite-first boundary.

## Option K4 — Event-sourced knowledge ledger

Every research mutation is an append-only event, and views are reconstructed.

### Benefits

- complete lineage;
- corrections and historical state are natural;
- strong audit semantics.

### Risks

- substantial complexity;
- projection/version management;
- migration and operator burden;
- likely excessive for M06.

## Option K5 — Hybrid relational authority plus file artifacts and projections

SQLite owns identities, workflow state, memberships, provenance manifests, assertions, events, and review authority. The filesystem owns immutable originals and versioned normalized packages. Markdown/HTML are generated or governed projections. Future Qdrant and graph structures remain rebuildable derivatives.

### Benefits

- aligns with the accepted application architecture;
- preserves strong authority and human-readable outputs;
- supports heterogeneous artifacts;
- avoids a new canonical graph database;
- compatible with later retrieval.

### Risks

- requires careful boundary design;
- projections can drift if manually editable without reconciliation;
- schema may still become too broad;
- needs disciplined import/rebuild semantics.

**PROJECT RECOMMENDATION**

K5 is the leading research option, but it must not be adopted until R-M06-06 through R-M06-09 clarify identity/versioning, LLM context, retrieval, automation, security, backup, and rebuild requirements and the owner approves the synthesis.

---

# 12. Rejected or unsafe shortcuts

The following should be rejected in synthesis unless materially re-argued:

- one Markdown file equals one canonical source, entity, claim, and dossier;
- titles or filenames used as durable identity;
- every named string automatically creates or merges an entity;
- source wording treated as accepted fact;
- bare graph edges without attribution, qualifiers, or evidence;
- LLM extraction directly creates accepted assertions;
- semantic similarity treated as support or causality;
- an allegation accepted as attribution being treated as proof of the alleged event;
- one “confidence score” replacing review state and evidence role;
- corrections overwriting parser output or original evidence;
- dossiers copying authoritative records into independent editable prose;
- Qdrant or a graph engine becoming the only store for relationships;
- automatic cross-account entity or dossier sharing;
- deleting evidence because the external source disappeared;
- treating generated summaries as source documents.

---

# 13. Questions requiring owner approval during synthesis

1. Which candidate object families are required in M06 versus deferred?
2. Is K5, the hybrid relational/file/projection model, accepted as the architecture direction?
3. Which assertion vocabulary is sufficiently small for the first implementation?
4. Should `accepted_for_research` and `accepted_for_editorial_use` be distinct states?
5. Can humans edit Markdown projections, or are they generated read-only views?
6. Are topic concepts account-local by default, shared globally, or mixed under explicit promotion?
7. May an entity be shared between account vaults while interpretations remain isolated?
8. What evidence threshold permits entity merges?
9. What relationship types are admitted initially?
10. Are event objects required in first M06 implementation or deferred after dossiers/assertions?
11. How should allegations and high-risk claims be surfaced in drafting context?
12. Should external identifiers be verified manually before identity resolution?
13. How should human-created research notes relate to source evidence?
14. What does “accepted conclusion” authorize: research use, drafting use, or public factual claim?
15. Which correction operations require exact human approval and audit binding?

---

# 14. Inputs required by later reports

## R-M06-06 — Identity, Versioning, Correction, and Deduplication

Must settle:

- stable ID generation;
- source item versus occurrence versus artifact identity;
- content hashes and semantic duplicates;
- entity merges/splits;
- assertion revision and correction;
- deleted/moved/changed sources;
- projection and normalized-package versioning.

## R-M06-07 — LLM Context Assembly and Governed Reasoning

Must define:

- which object types enter context;
- how attribution, assertion type, review state, and contradictions are rendered;
- token budgets and source diversity;
- prompt-injection boundaries;
- how generated candidates are returned.

## R-M06-08 — Retrieval, Chunking, Graph, and Qdrant Readiness

Must determine:

- which canonical objects generate chunks;
- pointer and invalidation model;
- entity/event/relationship retrieval;
- graph candidate storage versus retrieval-only edges;
- evaluation questions and cross-account isolation.

## R-M06-09 — Automation, Security, Backup, and Rebuild

Must cover:

- safe automated creation of observations and candidates;
- backup of artifacts/manifests/authority records;
- complete reconstruction of projections and retrieval indexes;
- hostile content and LLM prompt injection;
- account isolation and deletion obligations.

---

# 15. Provisional research recommendation

No architecture is approved by this report.

The leading direction for owner review is:

```text
SQLite
  owns stable identity, account scope, workflow/review state,
  assertions, entity/event candidates, dossier membership,
  provenance manifests, and correction lineage

Governed filesystem
  owns immutable original artifacts and versioned normalized packages

Markdown/HTML
  provide human-readable dossiers, timelines, entity pages,
  claim views, and review projections under explicit edit rules

Future Qdrant / graph derivatives
  index or connect canonical records but remain disposable and rebuildable
```

The minimum conceptual boundary is:

```text
Source contains X
Speaker/source claims Y
Parser/model proposes Z
Human accepts or rejects Z for a defined use
```

No stage may silently collapse into the next.

---

# 16. Primary sources and access date

Accessed 2026-07-20.

- W3C PROV-O: The PROV Ontology
  https://www.w3.org/TR/prov-o/

- W3C Web Annotation Data Model
  https://www.w3.org/TR/annotation-model/

- W3C Web Annotation Vocabulary
  https://www.w3.org/TR/annotation-vocab/

- W3C SKOS Simple Knowledge Organization System Reference
  https://www.w3.org/TR/skos-reference

- W3C ActivityStreams 2.0
  https://www.w3.org/TR/activitystreams-core/

- Library of Congress PREMIS Data Dictionary Version 3.0
  https://www.loc.gov/standards/premis/v3/

- CIDOC Conceptual Reference Model
  https://cidoc-crm.org/Resources/definition-of-the-cidoc-conceptual-reference-model-v7.1.1

- Wikidata Data Model
  https://www.wikidata.org/wiki/Wikidata:Data_model

- Schema.org ClaimReview
  https://schema.org/ClaimReview

- Microsoft GraphRAG architecture, methods, and outputs
  https://microsoft.github.io/graphrag/index/architecture/
  https://microsoft.github.io/graphrag/index/methods/
  https://microsoft.github.io/graphrag/index/outputs/

- JSON Schema documentation and specification
  https://json-schema.org/docs
  https://json-schema.org/specification
