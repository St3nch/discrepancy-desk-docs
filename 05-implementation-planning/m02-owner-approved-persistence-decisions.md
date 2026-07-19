# M02 Owner-Approved Persistence Decisions

## Status

Accepted by the owner on 2026-07-19. Planning authority only.

These decisions resolve UD-01 through UD-12. They constrain later physical design but do not authorize SQL, migrations, SQLite creation, ORM models, CRUD code, application persistence, dashboard implementation, or platform writes.

## UD-01 — Canonical raw-evidence storage

Canonical raw evidence remains immutable external files under a governed evidence root. SQLite will later store a root-relative locator, SHA-256, byte size, capture provenance, content/media type when known, verification state, and verification time. Absolute paths and path traversal are rejected. Missing files, orphan files, or disagreement among database hash, manifest hash, and actual bytes fail closed. Existing M01 evidence remains in place.

## UD-02 — Exact revision identity and approval binding

Each immutable platform-specific revision preserves exact component bytes and receives a versioned aggregate SHA-256 binding. The binding covers platform, owned account, exact authored UTF-8 text bytes, exact line endings, ordered links, ordered media identities and hashes, exact alt text, ordered review-bound labels, revision identity, and binding-procedure version. No Unicode, whitespace, or newline normalization occurs before identity comparison. Any changed component creates a new revision and requires new approval. Old bindings are never silently recalculated under a later procedure version.

## UD-03 — Lifecycle vocabulary and reopening

The accepted lifecycle vocabulary is:

```text
captured
research_needed
research_ready
drafting
human_review_needed
approved
manual_ready
published
rejected
withdrawn
publication_mismatch
evidence_blocked
```

Rejected work may reopen only through a new revision. Withdrawn work may reopen only through explicit human action creating a successor active revision or work cycle. Published history never moves backward. Corrections and replacements create successor revisions and separate publication records. Publication mismatches and evidence blocks require explicit human reconciliation; neither silently restores a former state.

## UD-04 — Audit integrity

Audit history will use append-only events with hash chaining. Normal application operations may not update or delete accepted audit events; database controls must reject those attempts. Required audit events are written atomically with the accepted state change. Chain verification must detect changed, missing, duplicated, or reordered events. Digital signatures and secret-key signing are deferred.

## UD-05 — Idempotency and uniqueness

Idempotency is operation-specific. Manual capture uses a client-generated operation ID. Evidence registration uses evidence SHA-256 plus governed capture scope. Revision creation uses work item plus exact binding hash. Approval uses revision ID, binding hash, and approval action ID. Manual-ready designation uses approval ID plus operation ID. Publication recording uses platform, owned account, and stable external post ID. Metric snapshots use publication, observation method, and capture-session ID. Identical replay returns the original result or no-op; key reuse with conflicting content is rejected. Retries reuse the original operation identity.

## UD-06 — Unknown and exceptional values

Observation state is explicit and separate from the observed value. Accepted states are:

```text
observed_value
observed_empty
not_requested
not_returned
unavailable
withheld
malformed
errored
unsupported
```

`NULL` alone may not collapse these meanings. Error class and bounded safe detail are retained separately. Unsupported future states fail closed rather than being converted to missing or empty.

## UD-07 — Retention and deletion

Owned-account and owned-post evidence is retained indefinitely unless owner-directed deletion or policy requires removal. Incidental third-party evidence is retained for 12 months unless explicitly renewed. Security and defect evidence without third-party content is retained for at least 24 months when operationally useful. The latest three verified backup generations are retained, including at least one copy separate from the working machine. Deletion is owner-only, dependency-aware, audited, and leaves a bounded tombstone with identity, former hash, reason, authority, time, and affected references but not deleted third-party text. Raw evidence does not enter Git or a public repository.

## UD-08 — Manual publication confirmation

An accepted manual publication record requires owner confirmation, platform, owned account, external post ID, canonical URL, observation time, approved revision, and approval binding. Verification states are `owner_confirmed`, `platform_observed`, `verified_match`, `verified_mismatch`, and `deleted_or_inaccessible`. Owner confirmation permits recording but does not claim platform-text verification. Known platform-generated rendering additions such as X-generated links are not automatically mismatches. Human copy errors, changed links, media, text, or account produce `publication_mismatch` without rewriting history.

## UD-09 — Metric snapshot identity

A metric snapshot is identified by publication, observation method, capture-session ID, exact UTC capture timestamp, and metric-set version. Manual and API observations may coexist. Historical snapshots are immutable. Human corrections append a correcting snapshot linked to the mistaken snapshot. Repeated capture-session identity with identical content is a no-op; changed content under the same identity is rejected. Equal counts alone do not make observations duplicates.

## UD-10 — SQLite operating settings

The planned local MVP settings are:

```text
journal_mode = WAL
foreign_keys = ON
busy_timeout = 5000 ms
write transaction start = BEGIN IMMEDIATE
```

Every operational connection must verify the required settings and known database version state. Continued `SQLITE_BUSY` after the bounded timeout rejects cleanly. Logical retries require the original idempotency key and an outcome check. After uncertain interruption, integrity, version, and dirty-state checks run before new writes are accepted.

## UD-11 — Migration authority and dirty state

Alembic will be the sole migration authority, supplemented by a governed migration-integrity manifest and explicit durable dirty-state marker. Preflight must verify database identity, current revision, expected predecessor, migration hashes, clean state, no unexpected temporary objects, SQLite integrity, required connection settings, and an accepted recovery point for destructive or table-copy work. Dirty state is set before migration activity and cleared only after migration, version, constraint, index, trigger, row-count, and equivalence verification. Dirty or modified states refuse normal operation and further migration except governed recovery.

## UD-12 — Backup and disposable restore

One recoverable generation includes a safe SQLite backup, required sidecar handling, canonical raw-evidence files, evidence manifests, non-secret configuration required to interpret paths and versions, migration/version manifest, and audit verification material. The generation is assembled as a directory, archived as ZIP for portability, and encrypted with age before remote or removable storage. Generation identity, timestamps, included roots, per-file hashes, archive hash, encrypted-file hash, and application/schema versions are recorded. The latest three verified generations are retained. A new generation is accepted only after restore into a clean disposable location verifies manifests, SQLite integrity, migration state, audit chain, identifiers, relationships, exact approved text, three-way evidence hashes, record counts, and read-only application validation.

## Consequences

- UD-01 through UD-12 are closed.
- The operational persistence contract and lifecycle model must be reconciled to these decisions.
- The adversarial matrix remains the minimum baseline and may be strengthened.
- Physical persistence implementation remains blocked until the full M02 exit gate and a bounded M03 implementation package are owner-approved.
