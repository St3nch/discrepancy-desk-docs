# R-M06-06 — Identity, Versioning, Correction, and Deduplication

## Research status

Research report completed on 2026-07-20.

This report is source material for M06 architecture synthesis. It does not authorize implementation, schema adoption, merge automation, cross-account sharing, destructive deduplication, policy migration, or any new persistence authority.

All identity, correction, merge, split, import, and versioning options remain subject to owner review during architecture synthesis.

## Question and project boundary

How should The Discrepancy Desk preserve stable identity across repeated captures, changing webpages, revised transcripts, parser reruns, duplicate files, entity ambiguity, corrections, merges, splits, account-policy changes, and governed cross-account reuse without erasing history or allowing one account's conclusions to contaminate another?

The report covers:

- stable internal identity;
- external identifiers and locators;
- byte identity versus intellectual identity;
- source-item and observation versioning;
- parser and normalized-document versioning;
- duplicate detection and duplicate claims;
- entity merges and splits;
- assertion correction and supersession;
- account-policy versioning;
- cross-account import/reference lineage;
- deletion, withdrawal, and source disappearance;
- concurrency and stale-review protection;
- rebuild and invalidation consequences.

It does not design database tables or implementation code.

---

# 1. Evidence labels

- **VERIFIED FACT** — supported by a cited standard or authoritative source.
- **INFERENCE** — reasoned consequence of verified material.
- **PROJECT RECOMMENDATION** — candidate direction requiring owner approval.
- **OPEN QUESTION** — unresolved and unsafe to treat as settled.

---

# 2. Executive findings

## 2.1 Identity must be layered

**PROJECT RECOMMENDATION**

The system should distinguish at least:

```text
internal record identity
external platform/source identifier
locator or URL
content-byte identity
normalized-content identity
intellectual/source-item identity
observation identity
account-local imported identity
```

These are not interchangeable.

Example:

```text
same URL, changed bytes                 new observation/version
same video ID, revised title            same source item, new metadata observation
same PDF bytes at two URLs               same artifact bytes, two source occurrences
same article syndicated by two outlets  possible intellectual duplicate, distinct publications
same person name                         not sufficient for entity identity
same research package imported twice    exact replay, not two independent conclusions
```

## 2.2 Deduplication must not delete provenance

**PROJECT RECOMMENDATION**

A duplicate determination should normally create a relationship, not erase a record.

```text
exact_byte_duplicate
same_external_identity
canonical_url_candidate
syndicated_copy_candidate
near_duplicate_text
same_intellectual_work_candidate
import_replay
```

Each duplicate relationship should preserve the evidence and method supporting the determination.

## 2.3 Correction is lineage, not mutation

**PROJECT RECOMMENDATION**

Corrections should append or supersede while preserving the prior record:

```text
recorded version
→ correction proposal
→ human review
→ accepted successor/correction
→ downstream derivative invalidation
```

The system should not rewrite an original transcript, source artifact, observation, assertion, or accepted conclusion to make the historical record appear cleaner.

## 2.4 Account policy is versioned authority

**PROJECT RECOMMENDATION**

An account's evidence and editorial-use policy should have immutable versions.

A policy change must not silently reinterpret every previously reviewed item. Instead, the system should be able to identify affected records and create bounded re-review candidates.

## 2.5 Cross-account reuse should be import lineage, not shared truth

**PROJECT RECOMMENDATION**

The safer candidate remains:

```text
separate physical account Vaults
+ owner-governed import/reference operation
+ preserved source-package identity and hashes
+ independent target-account review
```

A source package may be reused. The originating account's acceptance, framing, or editorial-use decision must not transfer automatically.

---

# 3. Standards and useful models

## 3.1 PROV entities, activities, agents, and derivation

**VERIFIED FACT**

W3C PROV models provenance using entities, activities, and agents, including derivation and responsibility relationships.

**INFERENCE**

This supports representing a parser rerun, human correction, import, merge, split, or policy review as an activity producing a new entity/version rather than mutating an unexplained record.

## 3.2 PREMIS preservation objects and events

**VERIFIED FACT**

PREMIS defines preservation-oriented objects, events, agents, rights, identifiers, fixity, and relationships. PREMIS 3 expanded the ability to represent intellectual entities and structural or derivative relationships.

**INFERENCE**

The Discrepancy Desk does not need to implement PREMIS wholesale, but its distinction between object identity, fixity, events, agents, and derivative relationships is directly relevant.

## 3.3 JSON Patch

**VERIFIED FACT**

RFC 6902 defines JSON Patch as a sequence of operations such as add, remove, replace, move, copy, and test against a JSON document.

**INFERENCE**

Patch records could be useful as human-readable correction details or review evidence, but a patch alone is not sufficient canonical history. The project should retain full successor outputs and hashes so reconstruction does not depend on replaying an unbounded patch chain.

## 3.4 External identifiers do not replace internal identity

**PROJECT RECOMMENDATION**

External identifiers should be typed, scoped, and version-aware:

```text
youtube_video_id
x_post_id
doi
orcid
ror
wikidata_qid
canonical_url
publisher_document_id
```

An internal stable identifier should remain authoritative for project records because external identifiers can be absent, reused incorrectly, corrected, merged, or mapped ambiguously.

---

# 4. Candidate identity hierarchy

## 4.1 Source identity

Represents a publisher, channel, account, website, feed, archive, person acting as a source, or provider instrument.

## 4.2 Source-item identity

Represents the durable external/intellectual item:

```text
YouTube video
X Post
article
PDF report
podcast episode
webpage/document
API dataset item
```

## 4.3 Source occurrence

Represents where/how the item appeared:

```text
specific URL
feed entry
API endpoint response
uploaded file
human-pasted text
provider result
```

## 4.4 Observation

Represents what was observed at a specific time and under specific acquisition conditions.

## 4.5 Artifact version

Represents exact bytes or exact supplied text captured during an observation.

## 4.6 Normalized-document version

Represents one parser/tool/configuration result derived from an artifact.

## 4.7 Knowledge-record version

Represents assertions, events, dossiers, questions, and relationships created or corrected over time.

## 4.8 Policy version

Represents the exact account policy used for an editorial-use or review decision.

---

# 5. Content identity and duplicate classes

## 5.1 Exact byte identity

**PROJECT RECOMMENDATION**

Use a cryptographic hash to identify identical artifact bytes.

Exact byte identity proves only that the bytes match. It does not prove:

- the source occurrence is the same;
- the publisher is the same;
- the document is authentic;
- the intellectual work is the same in all contexts;
- two references may be collapsed operationally.

## 5.2 Exact supplied-text identity

Pasted text should be hashed exactly as supplied before normalization. Newline, Unicode, and whitespace handling should be explicit.

## 5.3 Normalized-text similarity

Near-duplicate detection may use normalized text hashes, shingles, edit distance, MinHash, or embeddings later.

**PROJECT RECOMMENDATION**

Similarity produces a candidate relationship only. It must not automatically collapse records.

## 5.4 Canonical URL identity

A page's declared canonical URL is evidence from the page/publisher, not absolute project truth.

Different URLs may legitimately represent:

- language variants;
- print views;
- syndicated copies;
- tracking variants;
- archived versions;
- republished or corrected editions.

## 5.5 External-platform identity

Stable platform IDs should normally anchor source-item identity more strongly than mutable titles or URLs.

The system must still record platform, account/channel, and observation context so an ID from one namespace cannot collide with another.

## 5.6 Intellectual duplicate candidate

Two artifacts may express the same intellectual work while differing in bytes, formatting, branding, corrections, or publication context.

Determining this generally requires human review or a strong publisher-provided relationship.

---

# 6. Version model options

## V1 — Mutable current record plus audit log

A current row is updated, while an audit event records changes.

### Advantages

- simple current-state queries;
- lower storage overhead.

### Risks

- historical reconstruction depends on audit completeness;
- source locators and downstream derivatives may point to vanished content;
- corrections can appear as silent replacement.

## V2 — Immutable full-version records

Every changed artifact, normalized package, assertion, policy, or dossier projection receives a new immutable version.

### Advantages

- clear history;
- easy hash binding;
- simple rollback and comparison.

### Risks

- more records and storage;
- requires explicit current/successor relationships.

## V3 — Patch/event-sourced history

Changes are stored as patches or events and reconstructed.

### Advantages

- compact change representation;
- rich activity history.

### Risks

- complicated reconstruction;
- corruption or missing events can break later state;
- difficult evidence review.

## V4 — Hybrid immutable versions plus event/audit records

Store complete immutable versions and separately record the governed activity that produced each successor.

### Research assessment

**PROJECT RECOMMENDATION**

V4 is the leading candidate because it supports exact evidence binding, human review, easy reconstruction, and explicit lineage without making replayed events the only way to recover state.

It is not approved.

---

# 7. Correction model

## 7.1 Original artifacts

Original captured bytes or exact supplied inputs are not corrected in place.

A later corrected publication becomes another source observation/artifact.

## 7.2 Parser corrections

A human correction should target an exact normalized-document version and source locator.

Candidate fields:

```text
correction_id
account_id
target_document_version_id
target_locator
original_value
proposed_value
reason
evidence_refs
actor_id
review_state
created_at
accepted_at
successor_projection_id
```

## 7.3 Assertion corrections

Possible operations:

```text
qualify
retract
supersede
dispute
merge
split
replace attribution
correct locator
correct entity reference
```

The old assertion remains historical and points to the correction/successor.

## 7.4 Entity corrections

Entity identity requires special operations:

```text
merge candidate
accepted merge
split candidate
accepted split
alias addition
alias withdrawal
external-ID correction
```

A merge should preserve both prior IDs as aliases or tombstoned predecessors. A split must identify which mentions/assertions/events move to which successor candidates.

## 7.5 Dossier and chronology corrections

Dossiers and chronologies should generally be projections or membership/order records over canonical objects.

Correcting a dossier should not duplicate or rewrite the source assertion/event itself.

---

# 8. Policy versioning and onboarding consequences

## 8.1 Candidate account-policy binding

Every account-specific research or editorial-use decision may need to bind to:

```text
account_id
policy_version_id
record/version IDs reviewed
review decision
required framing
review actor
review time
```

## 8.2 Policy changes

A policy change may affect:

- source-class admission;
- corroboration thresholds;
- uncertainty framing;
- prohibited categories;
- entity confirmation;
- cross-account imports;
- allowed LLM operations;
- publication review requirements.

**PROJECT RECOMMENDATION**

A new policy version should not bulk-rewrite old decisions. It should generate an impact set such as:

```text
unaffected
still compliant
requires review
now blocked
eligible under new policy
```

Only a human-governed review should change the account-specific decision.

## 8.3 Draft invalidation

If a policy change affects the evidence or framing requirements used by an active draft, the draft should return to review rather than remaining silently approved.

## 8.4 Onboarding templates

Future onboarding may offer policy templates, but a template should be copied into an account-owned immutable policy version and explicitly reviewed.

A later template update must not silently alter existing accounts.

---

# 9. Cross-account import/reference options

## C1 — Shared central record IDs across all accounts

### Advantages

- simple reuse;
- one copy of source material.

### Risks

- weak physical isolation;
- correction and deletion propagation becomes complex;
- account-local conclusions may leak.

## C2 — Full package copy into target Vault

### Advantages

- strong isolation;
- target account can preserve independent history.

### Risks

- storage duplication;
- correction synchronization is not automatic;
- duplicate detection required.

## C3 — Separate Vaults with signed/hash-bound import package and lineage

The source Vault exports a bounded package containing selected originals/derivatives, identifiers, hashes, provenance, and correction lineage. The target Vault imports it as a new account-local package and independently reviews it.

### Advantages

- physical isolation;
- exact transfer evidence;
- no transfer of account acceptance;
- later correction notices can refer to the imported lineage.

### Risks

- more complex import/export contract;
- needs explicit update/revocation handling.

### Research assessment

**PROJECT RECOMMENDATION**

C3 remains the leading candidate. It is not approved.

## 9.1 What must not transfer automatically

```text
accepted conclusion
editorial usability
publication framing
entity merge acceptance
relationship acceptance
LLM-generated summary
account policy
approval state
```

## 9.2 What may be transferable as evidence

Subject to rights/retention policy:

```text
original artifact or exact supplied input
source metadata
observation metadata
normalized package
parser provenance
correction lineage
external identifiers
hashes
source-region locators
```

## 9.3 Correction propagation

A source Vault correction should not mutate the imported target package.

Possible future mechanism:

```text
source package emits correction/supersession notice
→ target Vault records pending update candidate
→ human reviews impact
→ target imports successor or records independent disposition
```

---

# 10. Stale review and concurrency

## 10.1 Exact version binding

A review, merge, split, correction, import, or policy decision should bind to exact record/version hashes or immutable IDs.

If the target changes before acceptance, the operation fails stale and requires re-review.

## 10.2 Idempotency

Exact replay of an import or correction operation should return the original result.

Reuse of the same operation key with different content must fail.

## 10.3 Atomicity

Merge, split, and correction operations must either complete with all lineage/audit updates or leave no partial accepted state.

---

# 11. Deletion, disappearance, and rights changes

## 11.1 Source disappearance

A 404, deletion, private-state change, or channel removal is a new observation. It does not prove why the content disappeared.

## 11.2 Local retention removal

If policy or rights require local content removal, the system may need to retain a tombstone containing:

```text
internal identity
source identity/locator
prior hashes
removal reason class
removal authority
removal time
lineage references
```

The exact amount retained requires legal/policy review.

## 11.3 Derived invalidation

When an original or normalized version is removed or invalidated, downstream retrieval chunks, summaries, assertions, and context packets must become invalid or unavailable according to lineage.

---

# 12. Adversarial cases

The eventual architecture should test at least:

1. Same bytes acquired from two URLs are not collapsed without preserving both occurrences.
2. Same URL with changed bytes creates a new observation.
3. Canonical-link disagreement does not silently merge items.
4. Similar titles do not merge two videos.
5. Same person name does not merge entities.
6. An accepted entity merge can be reversed through a governed split without losing history.
7. A correction cannot target a stale document version.
8. A parser rerun cannot overwrite the prior normalized package.
9. A policy update does not silently change prior editorial decisions.
10. A permissive account's accepted conclusion does not transfer to a strict account.
11. Reimporting the same package is idempotent.
12. Reusing an import key with altered bytes fails.
13. A correction notice cannot mutate the target account without review.
14. Removal of an original invalidates dependent retrieval derivatives.
15. Fabricated external IDs do not create accepted identity.
16. A near-duplicate detector error does not delete or merge records.
17. Concurrent merge decisions fail safely.
18. A split cannot leave assertions pointing ambiguously to both successors.
19. A current projection can be regenerated from canonical versions and membership records.
20. A broken lineage link fails closed during backup/restore verification.

---

# 13. Candidate decision set for owner synthesis

The owner will later need to decide:

1. Whether V4—immutable full versions plus governed event/audit records—is accepted.
2. Which object types require immutable versions in M06.
3. Whether exact byte hashes and exact supplied-text hashes are mandatory.
4. Which duplicate classes exist initially.
5. Whether similarity detection is included in M06 or deferred.
6. What merge/split operations are admitted for entities.
7. Whether account policies are immutable/versioned from initial onboarding.
8. Whether policy changes create automatic impact candidates.
9. Whether C3 governed package import is the approved cross-account model.
10. Whether cross-account import itself belongs in M06 or a later milestone.
11. What correction notices may pass between Vaults.
12. What tombstone metadata may remain after required content removal.
13. Which operations require exact human approval binding.
14. What rebuild/invalidation checks are required at M06 exit.

---

# 14. Recommended direction for synthesis

**PROJECT RECOMMENDATION — NOT APPROVED**

The synthesis should evaluate a model with:

```text
stable internal IDs
+ typed external identifiers
+ immutable observation/artifact/document versions
+ complete successor versions
+ governed provenance events
+ duplicate relationships instead of destructive collapse
+ reviewed entity merge/split lineage
+ immutable account-policy versions
+ exact policy binding on account decisions
+ separate physical Vaults
+ hash-bound cross-account import packages
+ independent target-account review
+ downstream derivative invalidation
```

The architecture should favor explainable, reversible, append-oriented operations over clever automatic merging.

---

# 15. Inputs required by later reports

## R-M06-07

Needs policy-version binding, assertion versions, target locators, account-specific usability, and stale-context invalidation.

## R-M06-08

Needs immutable document/assertion IDs, merge/split lineage, duplicate classes, account scope, and derivative invalidation rules.

## R-M06-09

Needs import/export authority, correction propagation, deletion/tombstone policy, concurrency, backup verification, and rebuild contracts.

## Claude review

Claude should review this report together with `multi-account-research-and-editorial-policy-layer.md`, focusing on whether policy versioning or cross-account imports create hidden authority transitions.

---

# 16. Primary sources and access date

Accessed 2026-07-20.

- W3C PROV-O
  https://www.w3.org/TR/prov-o/
- W3C Provenance Working Group publications
  https://www.w3.org/groups/wg/prov/publications/
- RFC 6902 — JSON Patch
  https://www.rfc-editor.org/info/rfc6902/
- PREMIS Data Dictionary Version 3.0
  https://www.loc.gov/standards/premis/v3/premis-hierarchical-3-0.html
- PREMIS 3.0 announcement and data-model changes
  https://www.loc.gov/standards/premis/version-3-0-dd-announcement.html
- PREMIS OWL Ontology Version 3
  https://www.loc.gov/standards/premis/ontology/owl-version3.html
