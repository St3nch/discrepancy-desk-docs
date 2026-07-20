# M04 Adversarial Test Matrix — Editorial Control Room and X Operating Workflow

## Status

Accepted by the owner on 2026-07-20 together with `05-implementation-planning/m04-exact-implementation-work-package.md`. These 60 invariants are the required M04 adversarial baseline and must map to executable evidence before closure.

## Test Doctrine

- Test real SQLite behavior, not mocks alone.
- Fail closed on malformed, fabricated, stale, cross-account, conflicting, or replayed authority.
- Preserve append-only evidence, audit, approval, schedule, publication, and metric history.
- Prove non-detection and detector/query failure cases, not only happy paths.
- Derived views must agree with persisted truth and must never become a second authority store.
- Every admitted invariant must map to named pytest node IDs and executable evidence.

## Matrix

| ID | Invariant | Deliberate attack or failure | Expected result | Required proof |
|---|---|---|---|---|
| M04-HT-01 | Every editorial profile is account-scoped | Organize a work item using another account's ID | Reject; no profile or audit mutation | Real DB transaction and row-count proof |
| M04-HT-02 | Existing M03 rows are not assigned fabricated ownership | Upgrade a database containing unassigned work items | Rows remain visibly blocked or require explicit assignment; no silent account fabrication | Upgrade fixture from `0003` |
| M04-HT-03 | Lane vocabulary is closed | Submit unknown or case-confused lane | Reject with typed operator error; no mutation | Service and direct constraint test |
| M04-HT-04 | Tags normalize and deduplicate | Submit empty, whitespace, duplicate, and normalization-colliding tags | Reject empty; store one normalized value; replay stable | Constraint plus operation-key proof |
| M04-HT-05 | One active schedule exists per work item | Concurrently create two active schedules | At most one commits; loser fails closed | Two-connection real SQLite test |
| M04-HT-06 | Active schedule cannot exceed 90 days | Schedule at horizon plus one second/day | Reject; no schedule or lifecycle mutation | Frozen-time boundary tests |
| M04-HT-07 | Boundary schedule is admitted correctly | Schedule exactly at permitted horizon | Accept once with exact UTC semantics | Boundary test with stored timestamp |
| M04-HT-08 | Invalid date/window combinations fail | End before start; stale-before-earliest; deadline contradictions | Reject atomically | Parameterized real DB tests |
| M04-HT-09 | Reschedule preserves history | Move an active schedule earlier/later | Prior row becomes superseded; successor points back; no overwrite | Row lineage and audit proof |
| M04-HT-10 | Unschedule returns work to Reserve without deleting history | Unschedule an active item | Successor unscheduled row recorded; prior schedule preserved | DB and rendered Reserve proof |
| M04-HT-11 | Schedule replay is idempotent | Repeat same operation key and payload | Return original result; no duplicate row or audit event | Operation-key count proof |
| M04-HT-12 | Conflicting replay fails closed | Reuse operation key with changed date/account | Reject idempotency conflict; original preserved | Request-hash proof |
| M04-HT-13 | Schedule-only change preserves exact approval | Reschedule approved revision without editing text | Approval remains live and same binding hash | Approval/query proof |
| M04-HT-14 | Content successor supersedes approval | Create successor revision after scheduling | Prior approval no longer qualifies; Ready-to-Post false | Revision/approval/schedule query proof |
| M04-HT-15 | Stale approval cannot be Ready-to-Post | Point schedule at superseded approval/revision | Ready false with explicit reason | Derived-query proof |
| M04-HT-16 | Consumed approval cannot be reused | Publish, then attempt second publication from same action | Reject duplicate authority; no second publication | Unique authority and audit proof |
| M04-HT-17 | Blocked evidence prevents readiness | Corrupt or unverify required evidence | Ready false; item remains visibly blocked | Evidence verification and UI reason |
| M04-HT-18 | Expired or dormant work is not ready | Mark stale-after in past or dormant | Ready false; no silent unscheduling unless explicit operation | Query and persistence proof |
| M04-HT-19 | Published exact authority is not shown ready again | Query after successful publication | Excluded from ready queue with consumed/published reason | Query against persisted publication |
| M04-HT-20 | Command Center is account-isolated | Seed two accounts with overlapping states | Each account view contains only its records | Two-account fixture and ID-set comparison |
| M04-HT-21 | Missing account scope fails closed | Call every M04 query without account ID | Typed rejection; no ambient/default cross-account query | Direct query tests |
| M04-HT-22 | Wrong account scope fails closed | Request a known work item through another account | Reject/not found without leaking title, state, or IDs | Service and route tests |
| M04-HT-23 | Derived queues agree with persisted truth | Seed each relevant state and compare SQL truth to queue membership | Exact membership agreement | Independent fixture oracle |
| M04-HT-24 | Derived-query failure does not persist partial results | Inject query exception midway | No durable queue rows or mutations exist | Transaction/row absence proof |
| M04-HT-25 | Need-a-Post may return no candidate | Seed empty/weak inventory for a real gap | Returns explicit empty recommendation; creates nothing | Query result and row-count proof |
| M04-HT-26 | Need-a-Post never promotes authority | Seed unapproved attractive candidate | May explain exclusion but cannot approve/schedule/publish | Before/after authority counts |
| M04-HT-27 | Need-a-Post rationale is traceable | Seed ranked candidates | Every recommendation cites deterministic stored factors | Stable repeatable result proof |
| M04-HT-28 | Topic/lane repetition warning is non-authoritative | Trigger repetition warning | Warning does not block or mutate schedule automatically | UI/service proof |
| M04-HT-29 | Fabricated work item ID fails closed | Submit schedule/organization for nonexistent ID | Reject; no orphan record | FK/service test |
| M04-HT-30 | Fabricated revision or approval ID fails closed | Bind schedule/publication to nonexistent IDs | Reject; no partial mutation | FK and transaction proof |
| M04-HT-31 | Fabricated schedule predecessor fails closed | Reschedule unknown schedule ID | Reject; no successor row | Service test |
| M04-HT-32 | Cross-account revision binding fails closed | Bind account A schedule to account B revision | Reject atomically | Two-account relational test |
| M04-HT-33 | Dormant transition cannot leave hidden active schedule | Mark scheduled work dormant without explicit schedule resolution | Reject or require explicit unschedule according to accepted service contract | Service contract test |
| M04-HT-34 | Duplicate publication record does not duplicate authority | Retry same publication operation | Idempotent original result; one publication | Operation-key and unique-row proof |
| M04-HT-35 | Conflicting publication retry fails closed | Same key, changed external ID/URL | Reject; original publication preserved | Request-hash proof |
| M04-HT-36 | Match reconciliation is explicit | Record external result then verified match | Exact state and verification history recorded | Publication/audit proof |
| M04-HT-37 | Mismatch cannot be hidden as success | Record mismatched external content | `publication_mismatch` path visible; Ready false | State/query/UI proof |
| M04-HT-38 | Replacement preserves lineage | Correct mismatch with replacement publication | New publication references prior; prior remains immutable | Lineage and uniqueness proof |
| M04-HT-39 | Replacement cannot cross accounts | Attempt replacement using another account publication | Reject without leakage or mutation | Two-account real DB test |
| M04-HT-40 | Manual metrics retain observation truth | Record observed value, empty, unavailable, errored | Exact observation state retained; unavailable is not zero | DB/query/UI proof |
| M04-HT-41 | Malformed metric input fails honestly | Non-numeric value where numeric required; unsupported state | Reject or record admitted malformed state exactly; never coerce silently | Parameterized service test |
| M04-HT-42 | Metric correction is append-only | Correct prior manual snapshot | New row references prior; original unchanged | Lineage proof |
| M04-HT-43 | Targets are dated configuration, not eternal truth | Enter overlapping/expired targets | Query resolves by effective dates; historical rows unchanged | Frozen-time tests |
| M04-HT-44 | Route error explains preservation | Trigger validation/integrity/idempotency failure | Response states what happened, preserved, unchanged, and safe next step | HTML assertions; no raw SQL/traceback |
| M04-HT-45 | Route failure does not partially mutate | Inject failure after first intended write | Full rollback; retry is safe | Fault injection and row-count proof |
| M04-HT-46 | Concurrent organization update is deterministic | Two operators update same profile | Defined winner/conflict behavior; no torn data | Two-connection test |
| M04-HT-47 | Schedule history survives backup/restore | Create schedule lineage, backup, restore | Lineage, account scope, audit, and readiness match | Existing backup harness extension |
| M04-HT-48 | Modified backup/evidence still fails closed | Tamper restored artifacts | Existing integrity protections reject; M04 rows not trusted silently | Recovery proof |
| M04-HT-49 | Migration from `0003` is non-destructive | Upgrade representative M03 database | Existing approvals/publications/evidence/audit remain intact | Pre/post hashes and counts |
| M04-HT-50 | Interrupted `0004` migration is detected | Simulate partial/dirty migration | Startup fails closed/read-only per existing doctrine; no false success | Guarded migration evidence |
| M04-HT-51 | Migration manifest covers `0004` | Modify migration after manifest generation | Integrity check rejects | Manifest hammer proof |
| M04-HT-52 | Web workflow requires no SQL/raw IDs from operator | Execute realistic editorial week through routes | Full admitted loop succeeds via forms/views only | End-to-end client test |
| M04-HT-53 | Empty schedule slot is permitted | Leave a meaningful gap intentionally | No forced filler, approval, or automatic scheduling | UI and query proof |
| M04-HT-54 | No autonomous platform action exists | Inspect/call admitted route and service surface | No X write, like, reply, follow, repost, DM, browser automation, or provider call | Static surface check plus mock network prohibition |
| M04-HT-55 | Evidence runner rejects fabricated execution claims | Remove or fabricate node IDs/results in evidence assembly | Runner fails; no successful closure artifact | Executable evidence self-test |
| M04-HT-56 | Empty admitted execution list fails | Run evidence assembly with no M04 tests | Fail closed | Runner self-test |
| M04-HT-57 | Modified evidence output is detectable | Alter emitted evidence after run | Hash/verification mismatch detected | Evidence verification proof |
| M04-HT-58 | Secret review remains clean | Scan staged M04 files/evidence for credential values | No secret values, tokens, private keys, or local env files admitted | Named scan command/output |
| M04-HT-59 | Working-tree/evidence scope is measured | Closure runs with unexpected tracked modifications or untracked governed files | Closure record reports and blocks unexplained scope | Git-status evidence |
| M04-HT-60 | Full realistic week remains reproducible | Re-run the entire fixture from clean DB | Same admitted outcomes and stable deterministic queues | Repeated end-to-end proof |

## Evidence Mapping Requirement

Before M04 closure, every matrix row must contain:

- one or more exact pytest node IDs;
- whether it requires real SQLite, two connections, web client, migration fixture, backup/restore, or evidence-runner execution;
- expected fail-closed result;
- emitted evidence record identifier;
- implementation commit binding.

No row may be marked passed solely by prose review.

## Required Test Files

- `tests/test_m04_editorial_schedule_contract.py`
- `tests/test_m04_operator_queries.py`
- `tests/test_m04_web_workflow.py`
- extensions to existing migration, integrity, backup, recovery, lineage, and evidence-runner tests where the invariant belongs to an existing contract.

## Acceptance Rule

Owner acceptance of this matrix admits the listed invariants as the M04 adversarial baseline. It does not predeclare them passed and does not authorize scope outside the exact M04 work package.