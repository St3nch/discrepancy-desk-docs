<!--
  Frozen historical artifact — do not edit.
  Preserved from the v1 documentation archive on 2026-08-09.
  Original path: vedaops-legacy-2026-08-05/discrepancy-desk-docs/08-audits/vela-findings-disposition.md
  Companion runbook: reference/vela-v1-runbook.md
  Purpose: v1 Vela live-pass findings disposition alongside the frozen Source Manifest.
-->

# Vela Findings Disposition

## Status

Owner-accepted through D081. The Vela live pass is closed as incomplete but decisive. The five drafts are retired as failed editorial artifacts and remain preserved.

## Reviewed Evidence

- `09-operations/vela-incident-first-live-pipeline-pass.md`
- application runtime Vela logs and read-only database state
- `08-audits/claude-vela-product-fit-review.md`
- `08-audits/claude-vela-deep-research-editorial-review.md`
- application commit `8f300e1ab462a1f0692aeb5a61b4421c98db0717`

## Verified Live Result

```text
work items: 5
state: human_review_needed for all five
revisions: 5
source records: 9
source notes: 9 null of 9
approvals: 0
publications: 0
metric snapshots: 0
schedule slots: 0
audit events: 42
refusal tests executed: 0 of 10
largest stored revision: 1,195 characters as one flat blob
```

The recorded transitions happened in a scripted burst. Human review, approval, manual-ready, publication, reconciliation, metrics, and refusal paths were not executed.

The Vela pass therefore did not satisfy its original completion criteria. It reached the drafting boundary and stopped.

## Draft Authorship

The five drafts were written by the prior Project Steward GPT from the Vela runbook and inserted through normal application routes.

They were not authored by:

- Claude;
- the owner;
- autonomous application generation.

## Owner Finding

The owner judged the drafts unusable—“absolute crap.” That is accepted as a product finding, not dismissed as taste.

The failure chain is:

```text
shallow research
→ no story discovery
→ weak angle
→ poor prose
→ flattened presentation
```

The review interface defect and thread flattening were real. They were secondary. Repairing only the composer would have produced better-formatted bad writing.

The key missed lesson was that a source manifest is not research and a format name is not an angle.

## Artifact Disposition

The five Vela work items and revisions must:

- remain preserved as evidence;
- remain unapproved and unpublished;
- be retired or withdrawn through a later separately authorized application operation;
- carry the reason “retired as failed editorial artifact — see Vela findings disposition”;
- remain excluded from the publishable corpus;
- never be published merely to complete the old test.

This documentation decision records their disposition. It does not mutate production rows.

## Commit 8f300e1

Application commit `8f300e1ab462a1f0692aeb5a61b4421c98db0717` made review drafts readable across seven narrow application paths during a pass whose written plan required a stop before application change.

The owner retroactively ratifies the narrow correction through D081 because it was necessary to inspect the product result and did not create publication authority. The governance drift remains recorded rather than rewritten out of history.

No broader application change is ratified by this decision.

## Accepted Product Consequences

- freeze Gate 4, Gate 5, Phase 3B, later M06-A, M06-B, and non-text parser admission;
- move the active product boundary to M07;
- build research depth and story discovery before composition;
- keep The Vault as ingestion/document storage;
- build The Record for proof;
- build The Angle Room for story-worthiness;
- represent X content as ordered units;
- preserve exact approval and manual publication;
- make publication reconciliation and metrics usable;
- keep M13 closed.

## Review Recommendation Disposition

The reviews are accepted as independent product-review inputs, not blanket implementation authority.

Adopted:

- research-first phase order;
- Case as the operator-facing research object;
- separate Record and Angle Room;
- quotations as first-class objects;
- ordered content units;
- fast, standard, and case-file lanes;
- human-only evidence, identity, angle, and publication decisions;
- manual publication and metrics loop;
- explicit private-person boundary.

Corrected before adoption:

- mixed evidence labels are replaced by separate evidence dimensions;
- media binding includes immutable byte identity;
- publication lineage is non-circular;
- Vault-link uniqueness is NULL-safe;
- `not_applicable` is added;
- character counting is platform-aware and versioned;
- surface-form observations are repeatable.

Not adopted:

- any autonomous publication;
- any automatic recurrence detector in M07;
- any direct application implementation from proposed DDL;
- any resumption of parser work;
- publication of the diagnostic Vela examples without a new normal editorial process and exact owner approval.

## Closure

Vela is incomplete as an execution campaign and decisive as a product diagnosis. No further work is authorized merely to make its old scorecard look complete.
