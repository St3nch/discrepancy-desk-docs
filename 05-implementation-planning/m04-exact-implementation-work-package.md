# M04 Exact Implementation Work Package — Editorial Control Room and X Operating Workflow

## Status

Accepted by the owner on 2026-07-20 together with `05-implementation-planning/m04-adversarial-test-matrix.md`. This acceptance authorizes M04 application implementation within this exact change surface and stop-condition boundary.

## Governing Baseline

- application repository: `C:\dev\x\discrepancy-desk`
- application baseline commit: `08ce68212923bc903d12bb21ab301fb342c170b6`
- documentation repository baseline commit: `c54706b214249313c0dda9aae18d2d5fb6efebb5`
- M03: owner-accepted and closed
- M04: implementation authorized within this accepted package
- D023 and D024: accepted, subject to their named downstream gates

Required reading is the complete list in `05-implementation-planning/milestone-04-x-operations-and-metrics.md`.

## Objective

Extend the existing governed FastAPI/Jinja control room into a complete, manual, multi-account-capable editorial operating loop without adding autonomous platform action, direct agent access, a second persistence authority, or product-grade desktop polish.

The package must let the owner organize, schedule, review, manually publish, reconcile, and observe a realistic editorial week entirely through governed service operations and the web control room.

## Fixed Scope Boundary

### Included

- editorial lane, topic, tags, priority, operator notes, dormant/active organization;
- rolling 90-day schedule and Unscheduled Reserve;
- schedule, reschedule, and unschedule history;
- deterministic Ready-to-Post and Need-a-Post views;
- Command Center and named operational queues;
- state-aware work-item actions and plain-language failures;
- complete manual X publication/reconciliation/correction loop;
- manual metric snapshots and dated configurable progress targets;
- multi-account isolation for every new durable record and derived queue;
- migrations, governed services, routes/templates, tests, executable hammer evidence, and closure documentation.

### Excluded

- X write API or autonomous posting;
- provider polling, Release Watch feeds, Hermes, agent API/MCP;
- Truth Social;
- dossiers/Anomaly Vault, Qdrant, No Coincidences;
- Reply Desk, Article Room, Asset Library;
- causal analytics, automated strategy, multi-agent orchestration;
- React, Tauri, or major visual polish.

## Exact Application Change Surface

### Existing files to modify

- `migrations/manifest.sha256`
- `src/discrepancy_desk/persistence.py`
- `src/discrepancy_desk/operator_service.py`
- `src/discrepancy_desk/web.py`
- `src/discrepancy_desk/templates/base.html`
- `src/discrepancy_desk/templates/index.html`
- `src/discrepancy_desk/templates/work_item.html`
- `src/discrepancy_desk/templates/error.html`
- `tests/conftest.py`
- `scripts/run_ht_evidence.py`
- `docs/ht-coverage-ledger.md`

### New application files admitted

- `migrations/versions/0004_editorial_schedule_contract.py`
- `src/discrepancy_desk/editorial_queries.py`
- `src/discrepancy_desk/templates/command_center.html`
- `src/discrepancy_desk/templates/schedule.html`
- `src/discrepancy_desk/templates/pipeline.html`
- `tests/test_m04_editorial_schedule_contract.py`
- `tests/test_m04_operator_queries.py`
- `tests/test_m04_web_workflow.py`
- `docs/m04-service-api-contract.md`
- `docs/m04-exit-gate-review.md`

No other application path is admitted without an explicit amendment to this package.

## Package A — Editorial and Scheduling Contract

### Physical persistence additions

Migration `migrations/versions/0004_editorial_schedule_contract.py` will add the following durable structures.

#### `editorial_profiles`

One row per work item.

Required fields:

- `work_item_id` primary key and foreign key to `work_items(id)` with delete restriction;
- `account_id` foreign key to `owned_accounts(id)` with delete restriction;
- `lane` constrained to `archive`, `docket`, or `flash_release`;
- `topic` nullable bounded text;
- `priority` constrained integer with documented range;
- `operator_notes` nullable UTF-8 bytes or bounded text consistent with existing text-storage doctrine;
- `is_dormant` strict boolean representation;
- created and updated timestamps.

A work item may not be organized without an explicit account. Existing M03 work items must receive an explicit migration-safe account assignment or remain visibly blocked until assigned; migration must not fabricate ownership silently.

#### `work_item_tags`

- `work_item_id` foreign key;
- normalized `tag`;
- composite primary key `(work_item_id, tag)`;
- bounded length and non-empty normalized value.

#### `schedule_slots`

Append-preserving schedule authority record.

Required fields:

- `id` primary key;
- `work_item_id` and `account_id` foreign keys;
- `approved_revision_id` nullable foreign key to `revisions(id)`;
- `scheduled_for` nullable UTC timestamp;
- `preferred_window_start` and `preferred_window_end` nullable UTC timestamps;
- `earliest_useful_at` nullable UTC timestamp;
- `stale_after` nullable UTC timestamp;
- `hard_deadline_at` nullable UTC timestamp;
- `is_evergreen` strict boolean representation;
- `status` constrained to `active`, `unscheduled`, or `superseded`;
- `supersedes_schedule_id` nullable self-reference;
- `created_at`, `created_by`, and operation/audit linkage.

Only one active schedule row may exist per work item. Reschedule and unschedule create successor rows and supersede the prior row; they do not overwrite schedule history.

#### `editorial_targets`

Dated configurable target observations, not permanent platform truth.

Required fields:

- `id`, `account_id`, `target_kind`, `window_days`, `target_value`;
- `effective_from`, optional `effective_until`;
- `source_note` or source reference;
- created timestamp and actor.

This table may hold operator-entered progress targets. It does not assert current X eligibility unless supported by dated research and human entry.

### Constraints and indexes

Migration must enforce at minimum:

- account scope agrees across work item organization, schedule, publication, and derived queries;
- one active schedule per work item;
- active exact schedule cannot exceed 90 days from the operation's recorded current time;
- preferred-window end is not before start;
- stale-after and hard-deadline contradictions reject;
- dormant work cannot silently remain actively scheduled;
- replacement/successor relations cannot self-reference;
- tags cannot be empty or duplicated after normalization;
- no cascade deletes of governed editorial, schedule, approval, publication, or metric history.

Where SQLite cannot enforce a time-relative rule with a static CHECK constraint, the governed service operation must enforce it transactionally and hammer tests must prove bypass attempts fail through every admitted operation.

### Governed service operations

Add exact idempotent operations in `src/discrepancy_desk/operator_service.py` or delegated persistence helpers:

- `organize_work_item`
- `set_work_item_tags`
- `schedule_work_item`
- `reschedule_work_item`
- `unschedule_work_item`
- `set_editorial_target`
- `record_manual_metric_observation`
- `record_manual_publication_result`
- `reconcile_publication_result`

Every mutation requires:

- explicit `account_id`;
- explicit actor identity;
- caller-supplied operation key;
- request hash and conflict detection;
- one transaction;
- append-only audit event;
- typed fail-closed rejection;
- no hidden lifecycle transition unless that transition is an explicit documented part of the operation.

Scheduling must bind the schedule row to the exact currently approved revision when one exists. A schedule-only successor preserves that revision binding. A content successor revision supersedes the approval and causes Ready-to-Post to become false until renewed human approval.

## Package B — Functional Operator Control Room

### Query module

Create `src/discrepancy_desk/editorial_queries.py` as read-only derived-query logic. It must not mutate state or persist derived queues.

Required query functions:

- `get_command_center(account_id, now)`
- `list_schedule(account_id, start, end)`
- `list_unscheduled_reserve(account_id)`
- `list_pipeline_view(account_id, view_name)`
- `evaluate_ready_to_post(work_item_id, account_id, now)`
- `recommend_need_a_post(account_id, slot_start, slot_end, now)`

Every query requires explicit account scope. Missing or mismatched scope fails closed. Results may not include records from another account.

### Deterministic Ready-to-Post

Ready is true only when all conditions hold:

- work item belongs to the requested account;
- current revision has a live human approval bound to its exact bytes;
- approval is neither superseded nor consumed;
- required evidence is verified;
- no unresolved blocker or evidence-blocked state exists;
- item is not expired or dormant;
- active schedule, when present, references the same approved revision;
- no completed publication has already consumed that authority action.

The query must return explicit reasons for every failed condition.

### Deterministic Need-a-Post

Need-a-Post is a bounded ranked view over existing inventory. It may consider:

- approved and unscheduled items;
- evergreen Reserve items;
- items nearing stale-after;
- lane/topic repetition;
- expected operator effort;
- existing schedule gaps.

It must return a rationale and may return an empty candidate list with the explicit result that leaving the slot empty is correct. It may not create, draft, approve, schedule, or publish anything.

### Routes and templates

Admitted route families:

- `GET /` → Command Center;
- `GET /schedule`;
- `GET /pipeline?view=<named-view>`;
- governed POST routes for organization, tagging, schedule, reschedule, unschedule, manual publication record, reconciliation, and metric observation;
- existing work-item detail expanded with editorial and schedule history.

Required named views:

- Needs My Review;
- Approved and Unscheduled;
- Manual Ready;
- Awaiting Reconciliation;
- Publication Mismatches;
- Blocked;
- Recently Published.

Errors must explain:

- what happened;
- what was preserved;
- what was not changed;
- the safe next action.

Raw SQL, tracebacks, or internal database details must not be rendered to the operator.

## Package C — Real Operating Proof and Closure

### Realistic editorial-week fixture

Use at least two synthetic owned accounts and multiple work items covering all three lanes. The scenario must prove:

- account isolation;
- scheduling inside the horizon;
- one reschedule;
- one return to Reserve;
- exact approval preservation after schedule-only change;
- approval invalidation after content change;
- Ready-to-Post true and false cases;
- an empty Need-a-Post result;
- manual publication match;
- publication mismatch and replacement lineage;
- honest manual metric states, including unavailable or errored;
- duplicate/retry recovery.

No real X write action is performed. External identifiers in tests are synthetic.

### Validation commands

At minimum:

```text
uv run ruff check .
uv run pytest
uv run python scripts/run_ht_evidence.py
```

Migration validation must include:

- fresh database upgrade through `0004`;
- upgrade from the M03 `0003` database;
- interrupted/dirty migration behavior;
- manifest verification;
- backup/restore compatibility;
- no destructive downgrade claim.

### Evidence outputs

- executable M04 hammer evidence under `runtime/ht-evidence/` using the existing runtime-data policy;
- tracked coverage ledger update in `docs/ht-coverage-ledger.md`;
- service/API contract at `docs/m04-service-api-contract.md`;
- closure review at `docs/m04-exit-gate-review.md`;
- implementation commit binding and SHA-256 for final evidence.

## Stop Conditions

Stop and return for owner ruling if implementation discovers:

- a new lifecycle state is required;
- schedule semantics would alter approved content bytes;
- account ownership cannot be assigned without fabricating data;
- a provider, platform API, recurring job, or agent runtime is needed;
- existing M02/M03 authority contracts must be weakened;
- destructive migration or evidence deletion is proposed;
- the exact change surface must expand materially;
- validation or an admitted hammer invariant fails.

## Owner Review Gate

Owner acceptance of this document and `05-implementation-planning/m04-adversarial-test-matrix.md` authorizes Packages A through C within the exact scope above. It does not authorize excluded capabilities or any later milestone.