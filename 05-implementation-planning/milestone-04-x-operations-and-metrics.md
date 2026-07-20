# Milestone 04 — Editorial Control Room and X Operating Workflow

## Status

Implementation active. The owner accepted the exact M04 work package and 60-invariant adversarial matrix on 2026-07-20. Work may proceed only within those accepted documents and their stop conditions.

## Goal

Deliver a substantial functional editorial operating system in the existing FastAPI/Jinja web control room. Prove planning, scheduling, review, manual publishing, reconciliation, and metrics contracts before product-grade Tauri implementation.

## Required Reading

- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `05-implementation-planning/editorial-control-room-roadmap-ruling.md`
- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- `00-doctrine/operating-doctrine.md`
- `00-doctrine/human-approval-policy.md`
- `02-product/product-overview.md`
- `02-product/workflow-overview.md`
- `02-product/module-map.md`
- `03-system-design/architecture-overview.md`
- `03-system-design/multi-account-model.md`
- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/automation-boundaries.md`
- `05-implementation-planning/hammer-test-strategy.md`
- `05-implementation-planning/m03-work-package-i-hammer-closure-review-return.md`
- application repo: `docs/m03-exit-gate-review.md`
- accepted D023 and D024 in `99-decisions/decision-log.md`
- `05-implementation-planning/m04-exact-implementation-work-package.md`
- `05-implementation-planning/m04-adversarial-test-matrix.md`

## Entry Gate

- M03 owner-accepted and closed.
- Roadmap/product ruling owner-accepted.
- Application and docs repositories synchronized and understood.
- Exact M04 work package identifies migration/service/UI/test/evidence scope.
- New scheduling and editorial-state adversarial tests are approved.
- Current X policy or monetization data used by the product is reverified immediately before implementation.

## Admitted Scope

### 1. Command Center

A single prioritized operating surface showing:

- Today and upcoming scheduled work;
- Needs Attention;
- Awaiting Review;
- Approved and Unscheduled;
- Manual Ready;
- Posted/Awaiting Reconciliation;
- Publication Mismatch;
- Blocked work;
- Work-in-progress summaries;
- meaningful empty schedule slots.

### 2. Editorial organization

- Archive, Docket, and Flash Release lane property;
- topic and tags;
- priority;
- operator notes;
- dormant/active organization without inventing new lifecycle authority.

### 3. Rolling 90-day scheduling foundation

- exact scheduled time;
- preferred window;
- earliest useful date;
- stale/expiration date;
- optional hard deadline;
- evergreen indicator;
- schedule, reschedule, and unschedule;
- first-class Unscheduled Reserve;
- full schedule-change audit history;
- no active scheduling beyond 90 days;
- schedule changes do not alter approved text;
- content edits supersede approval.

The initial web UI may use clear forms, lists, calendar views, and move controls. Product-grade drag/drop is not required for the M04 exit gate.

### 4. Pipeline and saved operational views

At minimum:

- Needs My Review;
- Approved and Unscheduled;
- Manual Ready;
- Awaiting Reconciliation;
- Publication Mismatches;
- Blocked;
- Recently Published.

### 5. Ready-to-Post

Derived eligibility requires a current live approval, current exact revision, valid required evidence, no unresolved blocker, no expiration, and no prior completed publication for that exact authority action.

### 6. Need-a-Post

A bounded deterministic assistant view showing schedule gaps and suitable existing inventory. It may surface approved/unscheduled, evergreen, expiring, or nearly-ready work with an explanation. It must be allowed to say no strong candidate exists and leave a slot empty.

### 7. Complete manual X operating loop

```text
capture
→ organize/research/source
→ draft
→ exact human review
→ approve or reject
→ schedule/manual-ready
→ human posts through X
→ record external URL/ID
→ reconcile match/mismatch
→ successor/replacement when required
→ manual metrics
```

### 8. Manual metrics and progress

- manual snapshots using the existing observation vocabulary;
- post-level metrics;
- manual follower/impression progress observations where supportable;
- dated/configurable eligibility targets rather than hard-coded permanent policy truth.

### 9. Trustworthy UX

Every blocked action or failure explains:

- what happened;
- what was preserved;
- what was not changed;
- what the operator can safely do next.

## Explicitly Excluded

- autonomous or API publishing;
- Hermes or any live agent runtime;
- agent-facing API/MCP;
- automated Release Watch or source polling;
- Federal Register or other provider integration;
- full dossiers/Anomaly Vault;
- Reply Desk;
- Article Room;
- Asset Library;
- Truth Social;
- Qdrant;
- advanced analytics or causal strategy;
- multi-agent orchestration;
- major visual polish of the disposable web shell.

## Coherent Implementation Packages

### Package A — Editorial and scheduling contract

Deliver together:

- accepted conceptual/physical design;
- migrations;
- lane/topic/tag/schedule/Reserve semantics;
- governed service operations;
- audit/idempotency behavior;
- adversarial tests.

### Package B — Functional operator control room

Deliver together:

- Command Center;
- scheduler/Reserve;
- Pipeline views;
- Ready-to-Post;
- Need-a-Post;
- work-item state-aware actions;
- plain-language errors.

### Package C — Real operating proof and closure

Deliver together:

- complete manual X workflow;
- reconciliation/correction proof;
- manual metrics;
- realistic editorial-week scenario;
- route/end-to-end tests;
- hammer evidence;
- docs and closure package.

Ordinary implementation inside an accepted package does not require repeated owner approval. Stop on new authority/provider/platform, destructive change, unresolved contract, failed validation, or audit finding.

## Required Adversarial Proof

At minimum prove:

- no schedule beyond the rolling horizon;
- invalid date/window combinations rejected;
- schedule replay is idempotent;
- reschedule/unschedule preserves history;
- schedule-only changes preserve exact approval;
- content edits supersede approval;
- stale/consumed approval cannot become Ready-to-Post;
- blocked/expired/published items cannot be falsely shown ready;
- duplicate publication recording does not duplicate authority;
- malformed metrics preserve explicit failure/unknown states;
- fabricated work/revision/approval/schedule IDs fail closed;
- Command Center/derived queues agree with persisted truth;
- failure/retry does not silently mutate schedule or lifecycle.

## Exit Gate

The owner can operate a realistic editorial week entirely through the web control room:

1. create and organize multiple work items;
2. assign lanes/topics/tags;
3. schedule within 90 days;
4. move one item earlier;
5. return one item to Reserve;
6. approve exact content;
7. prove schedule-only changes preserve approval;
8. prove content edits invalidate approval;
9. identify Ready-to-Post and a meaningful empty slot;
10. manually publish and record the external result;
11. reconcile both match and mismatch paths;
12. complete successor/replacement correction;
13. record honest manual metrics;
14. understand every blocked action;
15. repeat/recover safely from failed or duplicate requests;
16. complete all work without SQL, raw database manipulation, or unclear authority.

All tests and admitted hammer evidence pass; docs and status are synchronized; repositories are clean; owner explicitly accepts closure.

## Completion Record

Record date, implementation commit, test commands/results, hammer evidence path/hash, accepted deviations, unresolved later work, and owner acceptance.