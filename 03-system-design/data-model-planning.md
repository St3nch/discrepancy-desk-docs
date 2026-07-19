# Data Model Planning

## Status

Conceptual planning only. No table, column, relationship, storage form, constraint, migration, or implementation in this document is approved.

The M01 payload evidence and the owner-approved M02 persistence and hammer-test contract supersede any earlier model guess that conflicts with observed evidence or fail-closed requirements.

## Governing Rule

Do not finalize schema from intuition, naming convenience, or one provider response.

Before physical design:

- justify every operational record from an admitted MVP workflow;
- distinguish canonical raw evidence from parsed or normalized derivatives;
- preserve provenance, capture context, exact text, approval binding, and hashes;
- define missing, empty, unavailable, withheld, malformed, and errored states deliberately;
- resolve the database-to-filesystem evidence boundary;
- define engine-enforced invariants and deliberate violation tests;
- obtain owner approval through the M02 exit gate.

## Candidate Concepts

The following names are discussion aids only. They are not approved tables:

```text
accounts
raw_evidence
observations
sources
source_notes
claims
entities
entity_mentions
posts
drafts
human_reviews
publication_records
metric_snapshots
model_outputs
api_calls
audit_events
connection_candidates
connection_evidence
```

A concept must be removed, split, combined, or renamed when real workflow and evidence require it.

## Candidate Workflow Relationships

These flows are provisional models, not foreign-key authority:

```text
raw evidence -> observation -> normalized operational record
capture -> draft -> exact-revision review -> manual publication record -> metric snapshots
source -> note/claim -> drafted content -> human review
```

Truth Social remains manual-only under the current platform admission and cannot silently inherit X API assumptions.

## Exact Raw-Evidence Boundary

M01 proved that integrity verification depends on the exact response bytes written by the probe runner.

Therefore:

- canonical raw API evidence must remain exact bytes, or an immutable file referenced by path plus SHA-256 and capture provenance;
- parsed JSON is derivative data and cannot replace canonical raw bytes;
- reparsing, key reordering, whitespace normalization, character re-encoding, or serialization into a JSON column must not be represented as the original response;
- normalized records must retain a verifiable link to canonical raw evidence;
- missing, moved, altered, or orphaned raw evidence must be detectable;
- the final path-plus-hash versus database-BLOB decision belongs to M02 and is not decided here.

## Candidate Post and Publication Attributes

The following are questions for M02, not approved columns:

- external account and post identifiers;
- source and conversation relationships;
- exact authored text;
- platform-returned text;
- rendered or displayed text when separately observed;
- edit or revision identity;
- URL and media relationships;
- claim labels;
- parent or reply relationships;
- publication URL and manual-publication evidence;
- exact approval-bound revision hash;
- metric snapshots with observation method and capture time.

## Approval Binding

Approval must bind to the exact reviewed platform-specific revision, including text, links, media, labels, and other owner-approved components.

Any post-approval change must invalidate or supersede approval. Platform-returned text, including generated links, cannot silently replace the exact authored text that was reviewed.

## Prohibited Inference

This document does not authorize:

- schema or migration creation;
- SQLite database creation;
- ORM or application models;
- CRUD implementation;
- choosing JSON columns as canonical raw evidence;
- recurring collection;
- platform writes;
- treating candidate names as accepted architecture.
