# M03 Work Package H — Successor Revision and Replacement Publication Lineage Return

## Application checkpoint

- Commit: `20b010a — Add successor revision and replacement publication lineage`
- Validation: `54 passed`; Ruff clean.

## Implemented contract

- Migration `0003` adds append-only lineage:
  - `revisions.supersedes_revision_id`
  - `publications.replaces_publication_id`
  - `publications.resolution_kind`
- Creating a successor revision:
  - requires a predecessor revision for the same work item;
  - preserves the predecessor;
  - supersedes any still-active approval atomically;
  - returns the work item to `human_review_needed`;
  - records lineage and audit evidence.
- Recording a replacement publication:
  - requires a preserved `verified_mismatch` publication;
  - requires an approved successor revision for the same work item;
  - requires `manual_ready` state;
  - preserves the mismatched publication;
  - links the replacement directly to it;
  - consumes the new approval and resolves the item to `published`;
  - prevents more than one replacement for the same mismatched publication.

## Interface harness

The thin web harness exposes successor revision and replacement publication forms only to prove the service contract that a later Tauri client must call. This is not acceptance of the current web layout as the product UI.

## Proven path

```text
mismatched publication
→ successor revision
→ exact new approval
→ manual-ready
→ linked replacement publication
→ published
```

## Remaining M03 work

- named per-fixture HT evidence;
- HT-01 through HT-20 closure review;
- disposable owner walkthrough on the corrected workflow;
- M03 exit-gate package;
- Tauri planning only after service and evidence closure.
