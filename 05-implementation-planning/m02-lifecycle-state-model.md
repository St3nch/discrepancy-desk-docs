# M02 Lifecycle and State Model

## Status

Draft planning artifact for owner review. No physical schema or implementation is authorized.

## Purpose

Define the smallest lifecycle required to move a captured Discrepancy Desk item through drafting, exact human approval, manual publication, and later metric observation without allowing illegal jumps, stale approval, silent mutation, or ambiguous failure.

State names are conceptual. They do not approve database values, enums, tables, columns, constraints, triggers, application code, or UI behavior.

## Governing Rules

1. Lifecycle state represents accepted operational truth, not a model suggestion.
2. Every accepted state transition must have an authorized actor, a valid source state, satisfied prerequisites, and an audit event.
3. Human approval binds to one exact platform-specific revision.
4. Revisions are immutable after creation. A change creates a successor revision.
5. A changed approval-relevant component invalidates the prior approval for publication readiness.
6. Publication is a human-performed external action recorded afterward.
7. A platform response, classifier result, detector result, or model output cannot advance an item through the human gate.
8. Missing evidence, stale approval, fabricated identity, partial failure, or unrecognized state must fail closed.
9. Published history is not rewritten to make later corrections look as though they were always true.
10. X and Truth Social lifecycle records remain platform-specific.

## Lifecycle Objects

The lifecycle applies across three related conceptual objects:

1. **Work item** — the captured lead or content effort.
2. **Content revision** — an immutable platform-specific authored version.
3. **Approval binding** — the human decision authorizing one exact revision for manual publication.

Publication and metric observations attach only after their own prerequisites are satisfied.

## Work-Item States

### `captured`

A lead, source, observed event, or manual idea has been recorded.

Minimum prerequisites:

- durable internal identity;
- origin or capture method;
- owner context;
- creation time;
- platform scope when applicable.

### `research_needed`

The item cannot responsibly proceed to drafting without additional source review or context.

This state is optional for manual ideas or low-risk administrative content when the human determines research is not required.

### `research_ready`

The human has determined that available source notes and evidence are sufficient to begin drafting.

This does not mean every claim is confirmed. Uncertainty and disputed status may remain explicit.

### `drafting`

At least one content revision is being prepared, but no revision is currently awaiting a human decision.

### `human_review_needed`

A specific immutable revision is ready for human review, or a previously approved revision has been invalidated and requires review again.

### `approved`

A current human approval exists for one exact revision and its approval-relevant components.

This state alone does not represent external publication and does not authorize automated posting.

### `manual_ready`

The exact approved revision has passed all current readiness checks and may be copied or manually entered by the human into the official platform UI.

The transition to this state must revalidate approval freshness and evidence requirements.

### `published`

The human has completed the external platform action and an accepted publication record links the external post to the exact approved revision.

### `rejected`

The human rejected the current content direction or exact revision.

Rejection does not delete the item, source evidence, review history, or revision history.

### `withdrawn`

The owner deliberately ended the work item before publication or withdrew it from active operation.

Withdrawal is distinct from rejection. A withdrawn item may have been acceptable but intentionally abandoned.

### `publication_mismatch`

A human-recorded or platform-observed publication does not match the exact approved revision, target account, platform, or expected evidence.

This is a fail-closed exception state. It is not treated as a successful publication.

### `evidence_blocked`

Required evidence is missing, altered, unverified, mismatched, or unavailable in a way that prevents trusted progression.

The underlying historical state remains auditable, but progression is blocked until a governed reconciliation occurs.

## Legal Work-Item Transitions

The minimal legal transition set is:

```text
captured → research_needed
captured → research_ready
captured → drafting

research_needed → research_ready
research_needed → withdrawn

research_ready → drafting
research_ready → research_needed
research_ready → withdrawn

drafting → human_review_needed
drafting → research_needed
drafting → withdrawn

human_review_needed → approved
human_review_needed → rejected
human_review_needed → drafting
human_review_needed → research_needed
human_review_needed → evidence_blocked

approved → manual_ready
approved → human_review_needed
approved → withdrawn
approved → evidence_blocked

manual_ready → published
manual_ready → human_review_needed
manual_ready → withdrawn
manual_ready → publication_mismatch
manual_ready → evidence_blocked

rejected → drafting
rejected → research_needed
rejected → withdrawn

publication_mismatch → human_review_needed
publication_mismatch → evidence_blocked
publication_mismatch → withdrawn

evidence_blocked → prior valid non-published state after explicit reconciliation
evidence_blocked → withdrawn
```

A transition out of `evidence_blocked` must record:

- the blocking condition;
- reconciliation evidence;
- the human or governed process authorizing release;
- the exact restored state;
- a new audit event.

The system must not guess the prior valid state.

## Published-State Rules

`published` is terminal for the exact published revision and publication record.

After publication:

- metric snapshots may append;
- platform observations may append;
- deletion, correction, edit, unavailability, or policy action may append as later publication observations;
- the published revision and original approval history remain immutable;
- a corrected or replacement public post requires a new content revision, new human review, new approval, and a new publication record;
- a platform edit, when later admitted and observed, must not silently mutate the original authored or approved revision.

The work item may conceptually spawn a new successor effort, but the published historical record does not move backward to drafting.

## Revision Lifecycle

A content revision has a simpler conceptual lifecycle:

```text
created
→ submitted_for_review
→ approved | rejected | changes_requested
→ superseded when a successor revision exists
→ published when linked through an accepted publication record
```

Rules:

- `created` revisions may not be edited in place once submitted for review;
- any change after submission creates a new revision;
- an approved revision may become `superseded`, but its historical approval remains recorded as no longer current;
- only the exact current approved revision may satisfy manual-ready checks;
- a rejected or changes-requested revision cannot be published;
- a published revision remains historically immutable.

## Approval Lifecycle

An approval binding may be:

```text
current
invalidated
withdrawn
superseded
consumed-by-publication
```

These terms describe approval usability and history, not necessarily physical database states.

### Current

The approval remains valid for the exact revision, platform, account, text, links, media, alt text, and review-bound labels.

### Invalidated

A prerequisite changed or failed. Examples:

- successor revision created;
- text, whitespace, newline, punctuation, Unicode form, URL, media, alt text, label, platform, or account changed;
- required evidence disappeared or failed verification;
- approval identity or review relationship proved invalid;
- an integrity detector failed in a way that makes approval trust ambiguous.

### Withdrawn

The approving human explicitly revokes permission before publication.

### Superseded

A later valid approval exists for a successor revision.

### Consumed by publication

The approval was used to support one accepted manual publication record.

Consumption does not erase approval. Whether the same approval may support a deliberate repost or second publication is unresolved and must default to **no** until separately approved.

## Approval Invalidation Procedure

When any approval-relevant mutation or integrity failure occurs before publication:

1. preserve the original revision and approval record;
2. create or identify the successor revision or blocking event;
3. mark the prior approval unusable for readiness;
4. transition the work item from `approved` or `manual_ready` to `human_review_needed` or `evidence_blocked`;
5. emit the required audit event atomically with the accepted state change;
6. require a new human review before returning to `approved`.

The system must not compare only visually rendered text. Exact-byte or explicitly defined exact-component comparison must detect:

- appended or removed characters;
- whitespace and line-ending changes;
- Unicode normalization changes;
- zero-width characters;
- punctuation substitutions;
- URL changes or shortening;
- platform-generated links substituted for authored text;
- media or alt-text changes;
- cross-platform variant reuse.

## Invalid Transitions

The following are always illegal under the current contract:

```text
captured → approved
captured → manual_ready
captured → published
research_needed → approved
research_ready → published
drafting → approved without a recorded human review decision
drafting → published
human_review_needed → manual_ready without current approval
rejected → approved without a new reviewable revision and human decision
rejected → published
withdrawn → published
approved → published without manual-ready validation and publication recording
manual_ready → published when approval is stale or evidence is blocked
published → drafting
published → approved
published → deleted-from-history
any X state or approval satisfying a Truth Social requirement
any Truth Social manual observation satisfying an X API-evidence requirement
```

Additional illegal behavior includes:

- accepting an unknown lifecycle state;
- silently coercing a misspelled or deprecated state;
- jumping over required audit emission;
- changing state after a transaction partially fails;
- using a fabricated or cross-record approval identifier;
- accepting a metric snapshot before publication;
- treating classifier failure as “no problem” and advancing the item;
- treating missing evidence as equivalent to no evidence required.

## Fail-Closed Transition Evaluation

A requested transition must be rejected unless all of the following are established:

- current state is known and supported;
- requested target state is legal from the current state;
- actor is authorized for the transition;
- target platform and account match;
- referenced records exist and are not fabricated;
- required evidence exists and verifies;
- exact revision identity is current;
- human approval exists when required;
- approval remains fresh and usable;
- idempotency or replay behavior is recognized;
- required audit emission can complete atomically;
- no unclassified persistence or detector failure occurred.

An unrecognized exception must not produce a successful transition.

## Duplicate and Replay Behavior

State-changing operations must use a named operation identity or a proven uniqueness boundary.

A replay may result only in:

```text
return the already accepted result without new side effects
```

or:

```text
reject with a specific duplicate/replay outcome
```

A timeout or `SQLITE_BUSY` condition does not authorize blind retry. The caller must be able to determine whether the prior operation committed before attempting a governed retry.

## Partial-Failure Behavior

For operations that combine lifecycle change, approval handling, provenance, and audit emission:

- all accepted effects commit together;
- otherwise all effects roll back;
- if rollback certainty cannot be established, enter an explicit blocked recovery condition;
- never report success based only on the UI request or application intent.

Examples that must not exist:

- `manual_ready` without the approval event;
- publication record without exact approved revision linkage;
- approval invalidated without the work item leaving readiness;
- accepted metric snapshot without source and capture time;
- normalized observation accepted without its canonical evidence reference.

## Human Authority

Only the human owner or a later explicitly admitted human role may:

- approve content;
- reject content;
- withdraw approval;
- authorize manual-ready status when judgment is required;
- confirm manual publication when no independently admitted observation exists;
- reconcile ambiguous publication mismatch;
- approve release from a fail-closed blocked state.

AI may draft, summarize, compare, classify, and flag. It may not convert its own output into approval or publication authority.

## State-Model Acceptance Criteria

This state model is ready for owner approval only when:

- the owner accepts the admitted states and legal transitions;
- the owner resolves any disputed terminal or reopen behavior;
- every transition maps to an adversarial matrix case;
- exact approval invalidation behavior is accepted;
- no implementation mechanism is treated as decided merely because the conceptual rule is clear.
