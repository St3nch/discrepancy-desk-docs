# M06 Pre-Synthesis Correction Specification

## Status

Required correction specification created from AC-02 on 2026-07-20.

This is not the M06 architecture synthesis and does not authorize implementation. It defines vocabulary and safety requirements that the later synthesis must use unless the owner explicitly rules otherwise.

## 1. Canonical object vocabulary

### Source

A publisher, channel, account, feed, archive, person, organization, or other origin from which source items are obtained.

### Source item

A durable external or intellectual item such as a post ID, video ID, article identity, episode GUID, report, or document.

`page identity` is a website-specific alias and must not replace the general term.

### Occurrence

The place or representation through which a source item was encountered, such as a URL, feed entry, upload, provider result, or imported package.

### Observation

What was seen at a specific time through a specific instrument or acquisition method.

### Acquisition

One attempted or successful act of obtaining a representation. Success and failure are outcomes of the acquisition record. `ingestion` refers to the broader governed intake workflow.

### Original artifact

Exact bytes or exact supplied text preserved with a hash. `raw response artifact` is a source-specific alias only.

### Document version

One immutable normalized derivative produced by an identified parser execution from one or more original artifacts.

### Element and region

An element is an addressable structural unit within a document version. A region is a more precise geometric, temporal, character, or sub-element target.

### Mention, entity candidate, and entity

These remain separate:

```text
mention
→ entity candidate
→ human-reviewed entity
```

### Assertion

The umbrella governed record for attributed claims, quotations, allegations, observations, inferences, relationship candidates, accepted conclusions, disputed conclusions, unknowns, and editorial framing.

### Context run and evidence packet

A context run is one exact provider-neutral reasoning request bound to account policy, task instructions, evidence selection, omissions, and hashes. Evidence packets are typed source units inside that run.

### Chunk

A deterministic, disposable retrieval derivative built from admitted document-version elements. A chunk never becomes canonical evidence or factual authority.

## 2. Admission model

Admission maturity and permitted acquisition methods are orthogonal.

### Candidate admission stages

The final state machine requires owner approval, but synthesis must preserve distinct meanings for at least:

```text
proposed
manual_only
manual_admitted
automation_research
pilot_authorized
automated_admitted
suspended
retired
revoked
prohibited
```

Semantic requirements:

- `retired` means previously admitted but intentionally ended without misconduct or emergency invalidation;
- `revoked` means previously granted authority has been withdrawn;
- `prohibited` means the method/source is not eligible for admission under current policy;
- `suspended` means temporarily disabled pending review.

### Permitted methods

Admission records carry an explicit set such as:

```text
manual_paste
manual_file
manual_url_locator
human_triggered_static_fetch
feed_polling
websub
fixed_url_monitoring
provider_transcript
local_asr
rendered_browser_capture
```

A lifecycle state must never be inferred from the presence of a method, and a method must never be authorized solely by an admission-stage name.

## 3. Layered assertion and claim vocabulary

The synthesis must define four separate layers.

### Layer A — Internal epistemic type

Candidate base set:

```text
source_content_fact
attributed_claim
quotation
allegation
observation_assertion
derived_inference
relationship_candidate
accepted_conclusion
disputed_conclusion
unknown
parody_or_editorial_framing
```

The owner may rename or narrow this set during option review.

### Layer B — Review/workflow state

Examples include candidate, human-review-needed, accepted for a defined use, disputed, superseded, withdrawn, and rejected. Workflow state does not change the epistemic type.

### Layer C — LLM-output presentation status

Examples include source says, observed, corroborated proposition, uncorroborated claim, disputed proposition, model inference, open question, and editorial framing.

These are presentation labels on a candidate output, not authority.

### Layer D — Account/public claim label

Account policy maps governed internal records to public-facing labels such as Documented, Plausible, Disputed, Folklore, Speculative, or account-specific equivalents.

A public label must never be reverse-mapped into evidence authority.

## 4. Duplicate relationships

Duplicate detection creates relationships rather than destructive merges. Candidate classes include:

```text
exact_byte_duplicate
same_external_identity
canonical_url_candidate
syndicated_copy_candidate
near_duplicate_text
same_intellectual_work_candidate
import_replay
```

Merges and splits remain explicit human-governed operations with reversible lineage.

## 5. Context truncation integrity

A context builder must fail closed when a selected claim has a known correction, contradiction, or materially limiting qualifier that cannot fit in the context budget.

Permitted outcomes:

- include the claim and its contradiction/qualifier together;
- include the claim with a machine-bound warning identifying the omitted contradiction and prevent a clean-support presentation;
- omit the claim.

It is prohibited to include the claim as ordinary evidence while silently dropping its known contradiction.

## 6. Citation locator verification

Before an LLM output citation may be displayed as valid, deterministic validation must prove:

- the cited document version exists;
- the cited locator exists in that version;
- the cited evidence packet was included in the exact context run;
- the locator resolves within the account and policy scope;
- the cited text/region is available for human review.

This proves citation existence and context membership. It does not prove that the citation semantically supports the generated sentence; that remains a review and evaluation concern.

## 7. Artifact-absent imports

A receiving account must know whether an import contains the original artifact.

Minimum admissible import metadata requires:

- exporting account/package identity without transferring its policy authority;
- source and source-item identifiers;
- original-artifact presence state;
- hashes for every transferred artifact and normalized package;
- acquisition and normalization provenance;
- rights/retention restrictions;
- correction and supersession lineage included in the package;
- explicit list of excluded classes, including conclusions and approvals;
- receiving-account review state.

When the original artifact is absent:

```text
artifact_availability: reference_only
```

The receiving account must not treat a normalized derivative as independently verified original evidence. Account policy may block editorial use or require reacquisition.

## 8. Editorial-use ruling and publication approval

The later synthesis must choose and document the exact relationship. Regardless of the selected workflow:

- LLM editorial-use assessments are candidates only;
- a human editorial-use ruling is account- and policy-version-bound;
- an editorial-use ruling is not exact-text publication approval;
- exact-content approval remains separately bound to the final content hash/revision;
- policy changes can invalidate editorial-use rulings and publication approvals according to explicit impact rules;
- no component may infer publication authority from `editorial_use: allowed`.

## 9. Account-scoped observability

Account isolation applies to:

- logs;
- traces;
- retrieval receipts;
- context-run manifests;
- caches;
- temporary files;
- parser work directories;
- provider request/response receipts;
- error reports;
- backup and restore diagnostics.

Shared operational logs may contain only the minimum redacted identifiers needed for support and must not become a route for one account's source universe, conclusions, or content to enter another account's context.

## 10. Unified evidence packet model

Video and transcript fields from R-M06-02 become source-specific extensions inside the R-M06-07 general evidence-packet contract.

There must not be a separate video packet authority.

A video evidence packet may extend the general packet with:

- stable video/channel identity;
- metadata-observation identity;
- transcript provenance class;
- caption/ASR origin;
- timing precision;
- timestamp and speaker-cluster locators;
- visual-observation records;
- availability and correction lineage.

## 11. Package split

The owner-approved boundary is:

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

M06-A defines and proves the local canonical foundation. M06-B may consume that foundation but cannot redefine it.

## 12. Synthesis block

The architecture synthesis may begin only after the owner resolves the before-synthesis decision group. Implementation remains blocked after synthesis until independent review and explicit owner acceptance.
