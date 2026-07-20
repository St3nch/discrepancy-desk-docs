# M05 Adversarial Test Matrix — Tauri Desktop Foundation and Product Parity

## Status

Owner-accepted on 2026-07-20 with `05-implementation-planning/m05-exact-tauri-technical-plan.md`. This 77-invariant matrix is the accepted M05 adversarial baseline.

## Doctrine

- The desktop must never become a second authority store.
- Test real Tauri capabilities, Rust commands, sidecar lifecycle, loopback API, frontend behavior, installer behavior, and Python contracts.
- Missing account scope, missing launch token, stale state, cross-account access, version mismatch, and lifecycle interruption fail closed.
- Frontend rendering is not persisted truth.
- Preserve the complete M04 regression and hammer baseline.

## Matrix

| ID | Invariant | Deliberate failure | Expected result | Proof |
|---|---|---|---|---|
| M05-HT-01 | No direct SQLite from desktop UI | Attempt JS/Rust UI database access | No dependency, path, or command exists | Dependency and command scan |
| M05-HT-02 | Rust host has no business authority | Invoke approval/publication as Rust command | No such command; backend API remains required | Command manifest test |
| M05-HT-03 | Backend is loopback-only | Request wildcard or LAN bind | Startup refuses | Integration test |
| M05-HT-04 | Launch token is required | Request API without token | Refused; no mutation | HTTP and row-count proof |
| M05-HT-05 | Wrong or prior token fails | Reuse invalid or prior-run token | Refused without data leakage | Restart token test |
| M05-HT-06 | Token is absent from logs and argv | Inspect startup and failure records | Token not present | Captured-output scan |
| M05-HT-07 | One backend owns active DB | Start two supervisors on same data root | One owner only; no corruption | Process/lock test |
| M05-HT-08 | Dev and installed data roots are explicit | Start both modes | Separate roots or clear refusal | Path test |
| M05-HT-09 | Backend exits with desktop | Close shell normally | Child exits and releases DB | Lifecycle test |
| M05-HT-10 | Interrupted desktop exit is recoverable | End shell unexpectedly | Backend reclaimed safely; DB valid | Lifecycle/integrity test |
| M05-HT-11 | Startup timeout is bounded | Sidecar never becomes healthy | Visible unhealthy state; no endless wait | Rust/UI test |
| M05-HT-12 | Runtime backend failure is visible | Stop sidecar after startup | Health changes; mutations disabled | End-to-end test |
| M05-HT-13 | API versions must match | Pair incompatible fixtures | Workflow refused with version guidance | Compatibility test |
| M05-HT-14 | Sidecar is commit-bound | Build from unrecorded source | Verification fails | Build-manifest test |
| M05-HT-15 | Sidecar includes required resources | Remove migration/template/manifest | Packaging check fails | Bundle inspection |
| M05-HT-16 | Sidecar contains no credentials | Build with seeded environment values | Values absent from artifact/config | Artifact scan |
| M05-HT-17 | Arbitrary process execution is unavailable | Request unapproved executable | Capability denial | Tauri permission test |
| M05-HT-18 | Sidecar arguments are fixed | Add unapproved flags or paths | Command rejected | Rust unit test |
| M05-HT-19 | Main capability is minimal | Add broad shell/filesystem permission | Policy test fails | Config analysis |
| M05-HT-20 | Capability overlap cannot widen access | Add overlapping broad capability | Policy test detects merged exposure | Config analysis |
| M05-HT-21 | Remote web content has no command access | Load remote content and invoke | Denied | Webview security test |
| M05-HT-22 | Arbitrary window creation is unavailable | Request privileged extra window | Denied | Invoke test |
| M05-HT-23 | Arbitrary filesystem read is unavailable | Request profile/repo files | Denied outside picker grant | Permission test |
| M05-HT-24 | File picker requires user action | Attempt background selection | No file is selected/read | Native UI test |
| M05-HT-25 | Drag/drop is bounded | Drop directory, executable, oversized, or unsupported file | Refused; no import | Native event test |
| M05-HT-26 | Evidence path remains governed | Alter dropped path or use traversal | Backend verification refuses | End-to-end test |
| M05-HT-27 | UI preferences are non-authoritative | Tamper account/readiness/layout store | Backend truth wins | Store-tamper test |
| M05-HT-28 | Preference store contains no secrets | Inspect persisted UI state | No token or credential material | Store scan |
| M05-HT-29 | Every query requires account scope | Remove account ID | Typed refusal | API/client test |
| M05-HT-30 | Cross-account query fails | Request another account's record | Refused or excluded | Two-account test |
| M05-HT-31 | Cross-account mutation fails | Change account between display and submit | Refused; no mutation | Tamper test |
| M05-HT-32 | Frontend cannot override readiness | Alter component state to ready | Backend still controls action | Component/API test |
| M05-HT-33 | Stale view cannot submit silently | Change backend after UI load | Conflict and refresh guidance | Concurrency test |
| M05-HT-34 | Retry preserves operation key | Simulate uncertain response after commit | Original result returned once | Fault test |
| M05-HT-35 | Automatic retry does not mint authority | Trigger client retry logic | Same key or no automatic retry | Client unit test |
| M05-HT-36 | Command Center matches backend truth | Seed realistic M04 state | IDs and states agree | Cross-surface test |
| M05-HT-37 | Calendar preserves schedule lineage | Schedule/reschedule/unschedule in desktop | Same M04 history and audit | End-to-end proof |
| M05-HT-38 | Work parity preserves approval rules | Reschedule then edit approved item | Schedule preserves; edit supersedes | End-to-end proof |
| M05-HT-39 | Publication remains manual | Inspect UI and command surface | No platform write action exists | Static/runtime negative test |
| M05-HT-40 | Reconciliation and replacement work | Run match and mismatch correction | M04 lineage reproduced | Desktop scenario |
| M05-HT-41 | Metrics preserve unknown states | Record unavailable/errored/empty | Exact states retained | API/UI test |
| M05-HT-42 | Need-a-Post may be empty | No qualified inventory | Empty result without filler | UI test |
| M05-HT-43 | Later modules do not fabricate capability | Open Release Watch/Library | Disabled explanatory placeholders | UI/network test |
| M05-HT-44 | Records is bounded to M04 data | Attempt dossier assertion creation | No operation exists | Negative capability test |
| M05-HT-45 | Offline state is honest | Backend unavailable | Authority views unavailable, not stale-as-current | UI state test |
| M05-HT-46 | Refusals explain preservation and next action | Trigger token/scope/state/busy errors | Four-part error explanation | Snapshot/end-to-end test |
| M05-HT-47 | Raw traceback and SQL are hidden | Force backend exception | Sanitized operator error | Error-path test |
| M05-HT-48 | Notifications require permission | Deny OS permission | No notification; app remains usable | Installed/plugin test |
| M05-HT-49 | Notifications contain bounded content | Create reminder from sensitive draft | No draft text or secret content | Payload test |
| M05-HT-50 | Notification actions create no authority | Activate reminder | Opens item only | Action test |
| M05-HT-51 | Window restoration remains visible | Persist invalid monitor coordinates | Safe visible defaults | Rust/window test |
| M05-HT-52 | Pane restoration keeps authority context visible | Persist invalid pane sizes | Safe defaults | Frontend test |
| M05-HT-53 | Command palette obeys current state | Invoke disabled action by ID | Refused | Component/API test |
| M05-HT-54 | Keyboard shortcuts do not bypass confirmation | Trigger authority-related shortcut | Opens governed UI only | UI test |
| M05-HT-55 | Packaged build disables development tools | Inspect release config | Devtools unavailable | Build test |
| M05-HT-56 | Production CSP blocks unsafe script/remote code | Inject inline/eval/remote script | Blocked | CSP/build test |
| M05-HT-57 | NSIS install is current-user | Inspect/install package | No elevation required | Installer proof |
| M05-HT-58 | Reinstall preserves governed data | Reinstall with seeded data | Data remains intact | Disposable install test |
| M05-HT-59 | Uninstall does not silently delete data | Uninstall with seeded data | Data preserved or explicit documented choice | Disposable uninstall test |
| M05-HT-60 | WebView2 failure is actionable | Simulate missing runtime/bootstrap failure | Clear installer/app guidance | Packaging proof |
| M05-HT-61 | Unsigned build is labeled development-only | Build without certificate | No signed/production claim | Release assertion test |
| M05-HT-62 | Updater is absent | Inspect dependencies/capabilities/network | No update check/install path | Static/runtime test |
| M05-HT-63 | Store distribution is absent | Inspect config and artifacts | No Store config or claim | Packaging test |
| M05-HT-64 | No public release endpoint is contacted | Observe packaged startup | No update/telemetry request | Network observation |
| M05-HT-65 | Build is reproducibly versioned | Build same commit twice | Version/source manifests agree | Build comparison |
| M05-HT-66 | Fresh install runs M04 loop | Disposable install and synthetic workflow | Full parity without developer tools | Installed scenario |
| M05-HT-67 | Restart rotates token and preserves data/layout | Restart after work | Data intact; layout restored; token changed | Installed restart proof |
| M05-HT-68 | Lifecycle interruption preserves integrity | Interrupt during read and controlled failure point | DB and audit remain valid | Fault/integrity test |
| M05-HT-69 | Existing web harness remains green | Run full Python suite | All M04 tests pass | Full pytest proof |
| M05-HT-70 | Rust tests are mandatory | Omit/fail Rust test group | Closure runner fails | Runner self-test |
| M05-HT-71 | Frontend tests are mandatory | Omit/fail frontend test group | Closure runner fails | Runner self-test |
| M05-HT-72 | Packaged proof is commit-bound | Change code without rebuilding evidence | Binding mismatch blocks closure | Evidence test |
| M05-HT-73 | Secret review covers source and artifacts | Seed identifiable test value | Value absent from tracked files/logs/bundle/store | Automated scan |
| M05-HT-74 | Evidence is truthful and append-preserving | Modify result or fabricate IDs | Verification fails | Evidence integrity test |
| M05-HT-75 | Runner error cannot count as pass | Force runner error or empty collection | Nonzero result and explicit failure | Runner fault test |
| M05-HT-76 | Dirty trees block clean closure claim | Modify tracked/untracked paths | Dirty state measured and reported | Git-state test |
| M05-HT-77 | Complete workflow requires no SQL/raw IDs | Run installed operator scenario | UI completes admitted tasks clearly | Exit scenario |

## Coverage Rule

Every row must map to named automated test IDs, static verification checks, or explicitly identified installed/manual proof. Manual proof must record machine, OS, installer hash, application commit, steps, expected result, actual result, and artifact references.

No row may pass solely because an adjacent row passed.

## Acceptance Gate

The matrix becomes the M05 implementation baseline only after explicit owner acceptance. Implementation remains blocked until then.
