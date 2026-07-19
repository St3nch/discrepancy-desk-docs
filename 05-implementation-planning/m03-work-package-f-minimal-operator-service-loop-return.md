# M03 Work Package F — Minimal Operator Service Loop Return

## Status

Implemented, validated, committed, and pushed. M03 remains active.

## Application Commit

```text
346a17596fc6a958477f59071223ed7bc8cb55bf
```

Subject:

```text
Add minimal governed operator service loop
```

## Implemented Scope

- migration `0002` adding bounded `source_records`;
- migration-manifest coverage for the new migration;
- governed owned-account creation;
- governed work-item capture;
- bounded URL, manual-note, owned-post, and API-evidence source record types;
- exact note-byte preservation;
- source-note hash in audit payload instead of duplicate note text;
- human rejection operation with required reason;
- control-room item read model;
- complete matched operator orchestration path:
  - owned account;
  - captured work item;
  - source record;
  - canonical evidence registration;
  - drafting;
  - immutable revision;
  - human-review state;
  - exact revision approval;
  - manual-ready designation;
  - manual publication record;
  - metric snapshot;
  - control-room readback;
- public shared idempotency helper contract instead of cross-module private-helper imports.

## Migration Proof

The suite proves upgrade from `0001` to `0002` preserves existing owned-account and work-item data and creates `source_records` with Alembic head at `0002`.

The migration remains under the existing manifest-integrity and dirty-state migration authority.

## Hammer Coverage

Tests cover:

- complete capture-to-publication persistence loop;
- exact evidence hash registration;
- source-record idempotent replay;
- conflicting source-operation key rejection;
- owned-account idempotent replay and conflict rejection;
- required source locator or note;
- rejection before human review failing closed;
- human rejection with preserved audit reason;
- unknown control-room item rejection;
- migration data preservation;
- publication, metrics, and audit-chain readback.

## Validation

```text
uv run ruff check .
All checks passed

uv run pytest
42 passed
```

Post-commit runtime evidence:

```text
runtime/test-evidence/latest-pytest-session.json
```

Evidence commit binding:

```text
346a17596fc6a958477f59071223ed7bc8cb55bf
```

Environment recorded by the evidence emitter:

- Python `3.11.15`;
- SQLite `3.50.4`;
- 42 passed;
- zero failed, errors, skips, xfails, or xpasses.

## Boundaries Preserved

- no autonomous platform action;
- no platform write API;
- no production dashboard;
- no Truth Social implementation;
- no mention workflow expansion;
- no raw evidence in Git;
- no credentials or secret material;
- no generic lifecycle bypass of approval, readiness, publication, or mismatch operations.

## Next Bounded Package

The persistence service boundary is now sufficient for the first thin local control-room interface package.

That package should include only:

1. local FastAPI application startup;
2. read-only health and work-item views;
3. capture form;
4. source-note/URL form;
5. revision drafting form;
6. explicit human review and approve/reject controls;
7. manual-ready control;
8. manual publication record form;
9. metric observation form;
10. tests proving every UI action calls the governed service operations and cannot bypass lifecycle authority.

No visual polish, API polling, automated posting, account engagement, or broader analytics belongs in that package.
