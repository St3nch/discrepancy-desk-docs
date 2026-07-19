# M02 Smallest Operational Persistence Contract

## Status

Draft planning artifact for owner review. No physical schema or implementation is authorized.

## Purpose

Define the smallest set of operational records and integrity rules required to support the real Discrepancy Desk MVP workflow proven by project doctrine, the M01 X probe, and the owner-approved adversarial matrix.

This contract is conceptual. Record-class names are descriptive only. They do not approve tables, columns, field types, relationships, constraints, migrations, ORM models, repositories, CRUD operations, or a SQLite file.

## Governing Workflow

The minimum admitted operational loop is:

```text
capture a lead or manual idea
→ preserve source or raw evidence when applicable
→ create one or more platform-specific content revisions
→ perform human review
→ bind approval to one exact revision
→ mark that exact approved revision manual-ready
→ human publishes through the official platform UI
→ record the resulting external publication
→ append later metric observations
```

The persistence boundary must support that loop without autonomous platform action.

## Core Contract Principles

1. Human approval is mandatory for every public content item.
2. Approval binds to one exact platform-specific revision and all approval-relevant components.
3. Any approval-relevant mutation invalidates or supersedes the prior approval.
4. Canonical raw API evidence remains exact bytes or an immutable external file with verified SHA-256 and capture provenance.
5. Parsed, normalized, summarized, or classified information is derivative and cannot replace canonical evidence.
6. Stable external platform identifiers remain separate from mutable profile, text, verification, and metric observations.
7. Historical observations append; they are not silently rewritten into a timeless current value.
8. Missing, empty, unavailable, withheld, malformed, and errored values remain distinguishable.
9. X and Truth Social records remain platform-isolated. Truth Social remains manual-only.
10. Invalid transitions, unresolved evidence, stale approval, fabricated identity, partial failure, or unrecognized persistence failure must fail closed.
11. Every accepted state-changing operation must leave sufficient provenance and audit evidence to explain who or what acted, when, on which record, and with what result.
12. No detector, classifier, model output, or platform response may substitute for human approval.

## Smallest Required Operational Record Classes

### 1. Platform scope and owned-account identity

**Workflow need:** Publications, approvals, evidence, and metrics must be bound to the correct platform and owned account.

The contract must preserve:

- platform identity;
- stable external account identifier when one exists;
- current and historical mutable account observations when captured;
- whether the account is owned and admitted for the workflow;
- observation provenance and capture time.

A username, display name, profile label, or verification observation must not act as the stable identity anchor when a platform-issued stable identifier exists.

Truth Social must not be represented as API-observed under the current admission boundary.

### 2. Capture or work item

**Workflow need:** Every content effort begins from a lead, source, observed platform event, or manual idea.

The contract must preserve:

- a durable internal identity;
- capture kind and platform scope when applicable;
- human-entered summary or intake note;
- origin and capture method;
- creation time and ownership;
- current lifecycle state;
- links to source references or canonical raw evidence when they exist.

A manual idea may exist without external evidence. An evidence-derived capture may not claim verified provenance when its evidence is missing or unverified.

### 3. Source reference and source note

**Workflow need:** Source-backed drafts must retain where information came from and distinguish source material from human or AI interpretation.

The contract must preserve, when applicable:

- source locator or owner-provided reference;
- source type;
- source observation time;
- human notes, quotations, summaries, and AI interpretations as distinguishable forms;
- withdrawal, unavailability, or later correction without erasing historical use;
- relationship to the capture or content revision that used it.

A source reference is not automatically proof of a claim. The current MVP does not require a full claim graph.

### 4. Canonical evidence reference

**Workflow need:** M01 proved that trusted normalized observations require a verifiable relationship to exact raw evidence.

The contract must preserve:

- evidence identity;
- capture or probe context;
- exact-byte storage boundary selected later by owner decision;
- SHA-256;
- capture time;
- endpoint or manual method;
- redaction state;
- verification state and verification evidence;
- relationships to derivative observations.

The persistence boundary must detect missing, moved, altered, truncated, re-encoded, substituted, or orphaned evidence.

A parsed JSON object, normalized record, screenshot description, or human summary cannot impersonate the original bytes.

### 5. Platform-specific content revision

**Workflow need:** Drafting, review, approval, and publication must operate on immutable identifiable revisions rather than mutable draft text.

Each revision must conceptually bind:

- its parent work item;
- target platform;
- exact authored text;
- links included by the author;
- media selections and media identity when present;
- alt text when present;
- claim or risk labels included in the review boundary;
- creator type and identity context;
- creation time;
- predecessor revision when applicable;
- a deterministic exact-revision integrity value or equivalent binding mechanism, pending owner decision.

Once reviewed, a revision is not edited in place. A change creates a new revision.

Platform-returned text, including generated links, must remain distinct from the approved authored text.

### 6. Human review decision

**Workflow need:** The system must prove that a human reviewed a specific revision and what the human decided.

The contract must preserve:

- reviewer identity or owner authority context;
- exact revision reviewed;
- decision such as approve, reject, or request changes;
- review time;
- review notes when supplied;
- any required risk, source, or claim checks;
- supersession or invalidation relationship when later events change the result.

A model assessment or detector result is not a human review decision.

### 7. Exact-revision approval binding

**Workflow need:** Manual-ready status and publication eligibility require a current approval for the exact revision.

The binding must cover all approval-relevant components, including at minimum:

- platform;
- exact authored text;
- links;
- selected media;
- alt text;
- claim or risk labels included in review;
- revision identity;
- approving human decision.

The approval must become unusable when:

- any bound component changes;
- the revision is superseded;
- the review is withdrawn or invalidated;
- the platform or target account changes;
- required evidence becomes missing, modified, or unverified;
- the approval identity is fabricated, stale, missing, or belongs to another record.

Approval is evidence of permission for manual publishing. It is not permission for an automated platform write.

### 8. Manual publication record

**Workflow need:** After the owner posts through the official platform UI, the system must record what was published without pretending it performed the publication.

The contract must preserve:

- approved revision identity;
- platform and owned account;
- human-recorded publication time or observed time, with the distinction preserved;
- stable external post identifier when available;
- publication URL;
- manual publication method;
- platform-returned text or later platform observation separately from authored text;
- evidence or human confirmation supporting the record;
- correction, deletion, unavailability, or mismatch status without rewriting history.

A publication record must not exist as accepted when the exact revision lacked current approval.

Duplicate recording of the same platform post must be idempotent or explicitly rejected under a named operation identity.

### 9. Metric observation snapshot

**Workflow need:** The doctrine says metrics judge, but metrics are observations rather than mutable attributes of a publication.

Every snapshot must preserve:

- publication identity;
- platform;
- capture time;
- observation method;
- source context or evidence;
- metric names and observed values;
- explicit unavailable, withheld, missing, malformed, or errored states;
- operation identity sufficient to define duplicate behavior.

Later snapshots append. Historical snapshots are not overwritten.

Cross-platform metric names may be stored without claiming semantic equivalence.

### 10. Audit event

**Workflow need:** Accepted state changes and integrity decisions must remain explainable and tamper-evident.

The contract must record events for at least:

- capture creation;
- revision creation;
- review decision;
- approval creation, invalidation, withdrawal, or supersession;
- manual-ready designation;
- publication recording or correction;
- metric snapshot acceptance or rejection;
- evidence verification failure;
- lifecycle transition rejection;
- duplicate or replay handling;
- migration and recovery activity when implementation begins later.

Normal application behavior must not silently update or delete accepted audit history. The exact immutability or tamper-detection mechanism remains an owner decision.

## Required Cross-Cutting Metadata

The final physical design must justify how each accepted record or operation preserves, where applicable:

- durable internal identity;
- platform scope;
- owning account or owner authority;
- creation and observation time;
- actor or producer class: human, application, import, API observation, or model;
- source and evidence provenance;
- lifecycle state and state-change reason;
- operation or idempotency identity;
- integrity value;
- supersession or invalidation relationship;
- explicit error or uncertainty state.

These are contract requirements, not approved columns.

## Records Not Required for the Smallest MVP

The following may become justified later but are not admitted as required standalone operational records by this package:

- generalized entity graph;
- full claim graph;
- semantic chunks or embeddings;
- Qdrant documents;
- No Coincidences candidates;
- automated schedules;
- autonomous engagement actions;
- generic model-output warehouse;
- Truth Social API captures;
- cross-account campaign management;
- content calendar or scheduler;
- complex analytics aggregates;
- separate media library beyond what exact-revision approval and publication evidence require.

Deferral does not prohibit preserving necessary provenance inside the admitted record classes. It prohibits inventing a subsystem before the workflow demonstrates a need.

## Persistence Operations Requiring Explicit Idempotency Behavior

The future physical contract must name the operation identity or uniqueness boundary for at least:

- capture/import acceptance;
- evidence registration;
- human approval creation;
- manual-ready designation;
- manual publication recording;
- external platform post identity;
- metric snapshot acceptance;
- audit-event emission when tied to a state-changing transaction.

For each operation, replay must be either:

```text
proven no-op with the original accepted result
```

or:

```text
specific fail-closed rejection
```

Accidental duplication is not a contract.

## Atomicity Boundary

Any operation that changes accepted lifecycle state and emits required provenance or audit evidence must be atomic at the operational persistence boundary.

Examples include:

- approving an exact revision and recording the approval event;
- invalidating approval when a successor revision is created;
- designating an approved revision manual-ready;
- recording a publication and linking it to the approved revision;
- accepting a metric snapshot and its provenance;
- registering derivative observations against verified canonical evidence.

A partial state must roll back or become an explicit recoverable failure state. It must never be reported as success.

## Evidence and Filesystem Reconciliation Boundary

The final implementation must support a reconciliation operation that can identify:

- database reference with no canonical evidence;
- canonical evidence with no operational reference;
- path escape or unsafe locator;
- hash mismatch;
- sidecar-manifest disagreement;
- evidence attached to the wrong capture or endpoint;
- stale database copy after restore;
- exact approved text or relationship drift after restore.

The exact file-versus-BLOB boundary is unresolved and recorded separately.

## Contract Acceptance Criteria

This conceptual contract is ready for owner approval only when:

- every required record class maps to a real MVP workflow need;
- every lifecycle state and transition is defined in the companion state model;
- every admitted invariant maps to the owner-approved adversarial matrix;
- unresolved choices are explicitly recorded rather than smuggled in as defaults;
- no physical schema, migration, implementation, or platform-write authority is implied.
