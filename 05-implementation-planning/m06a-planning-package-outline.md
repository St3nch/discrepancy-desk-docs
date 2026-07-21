# M06-A Exact Planning Package Outline

## Status

Completed and owner-accepted on 2026-07-21 through D028. This is a planning baseline only; implementation remains blocked pending separate explicit authorization.

## Required Planning Artifacts

The owner-accepted M06-A planning package contains:

1. **Physical schema proposal**
   - account-scoped SQLite object families;
   - keys, constraints, uniqueness, lifecycle, and lineage rules;
   - FTS/search and invalidation boundaries;
   - no schema for M06-B, Qdrant, graph, live LLM, or recurring automation.

2. **Migration and rollback plan**
   - exact Alembic revision sequence;
   - upgrade/downgrade boundaries;
   - failure and partial-application behavior;
   - disposable proof database procedure;
   - compatibility with existing M00-M05 data.

3. **Vault filesystem and package layout**
   - one physical Vault root per editorial/public brand identity, with platform-owned accounts bound centrally;
   - immutable originals;
   - normalized JSON element packages;
   - manifests, hashes, projections, temp paths, and backup generations;
   - path traversal, collision, overwrite, and account-leak protections.

4. **Parser admission records**
   - separate records for TXT, Markdown, SRT, VTT, JSON, local RSS/Atom, local basic HTML, and born-digital PDF;
   - exact tool/library candidates;
   - format limits, warning/failure semantics, security boundaries, and no-network-egress proof;
   - no parser is admitted merely by appearing in the architecture.

5. **Backup, restore, and rebuild plan**
   - canonical generation contents;
   - immutable manifests and hashes;
   - disposable restore location;
   - wrong-account restore rejection;
   - missing-original and tamper detection;
   - projection/index rebuild and cross-account contamination checks.

6. **Adversarial matrix**
   - authority bypass;
   - malformed and hostile local files;
   - duplicate/replay and identity confusion;
   - parser partial output and detector failure;
   - merge/split evidence bypass;
   - correction/search/chunk invalidation;
   - projection edits;
   - backup/restore tamper and account leakage;
   - parser network-egress attempts.

7. **Bounded implementation package**
   - exact app-repo files/components proposed for change;
   - exact tests and evidence outputs;
   - validation commands;
   - rollback and stop conditions;
   - independent-review prompt supplied outside the repository;
   - explicit owner implementation-authorization gate.

## Required Review Sequence

```text
prepare exact planning package
→ validate internal consistency
→ independent technical review and owner rulings
→ independent Claude resolved-package review
→ correct and disposition R-01 through R-06
→ focused correction verification
→ owner acceptance through D028
→ close R-06 navigation and planning correction cycle
→ separate explicit implementation authorization still required
```

## Accepted Artifact Set

```text
05-implementation-planning/m06a-local-manual-vault-canonical-plan.md
05-implementation-planning/m06a-parser-admission-plan.md
05-implementation-planning/m06a-adversarial-closure-matrix.md
08-audits/m06a-planning-correction-disposition.md
08-audits/m06a-planning-correction-closure.md
```

## Hard Block

No application code, migration, parser, database, Vault directory, runtime configuration, or operational data may be created or modified under this planning authorization.
