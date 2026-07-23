# D039 Project-Steward Adversarial Self-Review

## Status

```text
Review type: Project-Steward adversarial self-review
Independent review claim: none
Application reviewed: 7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9
Current cumulative application HEAD: 529165c19da30185dd9833fab608d6dc28dfed88
Verdict: D039 PROJECT-STEWARD SELF-REVIEW VERIFIED
Material findings: none
Owner closure: pending
```

## Limitation

Claude AI repeatedly crashed and exhausted usage while attempting the independent review. At the owner's direction, the Project Steward performed a fresh adversarial review directly against live repository truth. Because the same steward implemented D039, this record is not described as independent.

## Live Truth

Both repositories were clean and synchronized. The exact D039 diff from `1337b5ac450ae82664aa1ad9667a85af41c4351e` through `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9` changed exactly the 13 recorded allowed paths and no forbidden path.

## Admission Authority

Verified:

- actor identity is derived by the loopback backend and cannot be supplied by the request;
- the active-human and `vault_admin` requirements are re-read from the selected Vault database;
- the V0003 database trigger independently guards every `owner_admitted` insert;
- exact confirmation text, tuple, evidence hashes, dependency lock, predecessor, and material hash are required;
- exactly one current `under_test` predecessor is required;
- admission inserts one append-only successor and preserves the predecessor;
- operation replay is exact and conflicting reuse fails;
- no startup, migration, provisioning, packaging, or SRT path creates plain-text `owner_admitted` state;
- admission creates no execution, package, document, element, or region.

## Canonical Execution

Verified that all required Vault, acquisition, retention, admission, artifact-path, reparse, size, and SHA-256 checks precede worker launch. The worker command, module, filenames, config, security profile, and output location are not request-selectable.

Started executions are persisted before launch. Worker and parent failures finalize safe content-free receipts and create no package or document authority. Two isolated successful runs must match byte-for-byte. Package reuse validates the exact stored record and appends an execution-package link. Initial document creation is limited to ordinal `1`; identical reruns create no version noise and changed results fail because supersession authority is absent.

## API, UI, and Recovery

The supported mutation surface remains the exact admission and one-artifact plain-text parse routes. Document responses contain safe IDs, hashes, states, timestamps, and counts, but no raw/normalized text or absolute paths.

Backup and restore reconcile SQLite package authority with the filesystem by exact set equality and exact size/hash checks. Missing, extra, modified, and cross-Vault package evidence fails closed. Package-before-database interruption leaves a started execution and orphan package requiring reconciliation.

## Evidence

Verified exact immutable hashes and embedded clean commit state:

```text
D039 full suite: 6f565f7f7a149a82624cfa4100db5b6796610ad302bd1354855c88806ff082a4
D039 profile:    cc961f43d3c65605164f46059957382e08fc027875efd5f7cf02953fa311aa47
D039 at D040:    b3053ccd43476b3fb85271658a0eff5c22f0329cfabe5e61e4c2f95f23b62086
```

The original and cumulative profiles contain the exact 28 unique invariant IDs, 28/28 passes, 30/30 mapped tests, zero skipped/xfailed/xpassed/errored tests, exact tuple and admission material, per-Vault admission proof, human-triggered execution proof, packaged execution, reuse, no-version-noise, backup/restore, UI-surface, and later-capability absence claims.

## Current-HEAD Preservation

D040 did not modify `parser_service.py`, `vault_filesystem.py`, the plain-text parser/worker/contract/resources, migrations, or dependencies. Shared changes add SRT status, SRT under-test provisioning, packaged SRT dispatch, and SRT evidence registration without weakening D039 authority.

## Findings

No material D039 findings.

## Verdict

```text
D039 PROJECT-STEWARD SELF-REVIEW VERIFIED
```

This verdict is not labeled independent. It grants no parser admission, SRT authority, later-parser authority, or owner closure by itself.
