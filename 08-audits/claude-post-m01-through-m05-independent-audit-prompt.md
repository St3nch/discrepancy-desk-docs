# Claude Independent Audit Prompt — Post-M01 Through M05

You are performing an independent, adversarial audit of The Discrepancy Desk after a long implementation sequence.

Do not trust chat summaries, milestone claims, status files, commit messages, or previous reviewer conclusions without verifying them against live repository truth.

Do not edit either repository.

Do not create commits, branches, pull requests, or files.

Do not run destructive commands.

Do not expose secrets or print the contents of local credential files.

## Repositories

Inspect both live repositories:

```text
C:\dev\x\discrepancy-desk
C:\dev\x\discrepancy-desk-docs
```

Use your filesystem/Git tooling to read the repositories directly.

For each repository, first report:

- configured path;
- branch;
- working-tree state;
- HEAD SHA;
- origin/main SHA if available;
- whether local and remote are synchronized;
- latest ten commits.

Stop and report if either working tree is unexpectedly dirty or the repository state prevents a reliable audit.

## Audit Baseline

The last preserved independent audit is:

```text
C:\dev\x\discrepancy-desk-docs\08-audits\claude-audit-through-m01-hammer-review-acceptance.md
```

Read that report first.

This audit covers everything accepted after that report through the current M05 closure, including all application and documentation changes.

## Required Reading — Documentation Repository

Read at minimum:

```text
LLM_MAP.md
STATUS.md
05-implementation-planning/implementation-roadmap.md
05-implementation-planning/editorial-control-room-roadmap-ruling.md
05-implementation-planning/m02-operational-persistence-contract.md
05-implementation-planning/m02-lifecycle-state-model.md
05-implementation-planning/m02-migration-and-hammer-execution-plan.md
05-implementation-planning/m02-adversarial-test-matrix.md
05-implementation-planning/m03-work-package-i-hammer-closure-review-return.md
05-implementation-planning/m04-exact-implementation-work-package.md
05-implementation-planning/m04-adversarial-test-matrix.md
05-implementation-planning/m05-exact-tauri-technical-plan.md
05-implementation-planning/m05-adversarial-test-matrix.md
05-implementation-planning/milestone-03-local-dashboard.md
05-implementation-planning/milestone-04-x-operations-and-metrics.md
05-implementation-planning/milestone-05-tauri-desktop-foundation.md
05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md
05-implementation-planning/m05-to-m06-transition-audit-and-research-plan.md
03-system-design/architecture-overview.md
03-system-design/multi-account-model.md
03-system-design/obsidian-qdrant-sqlite-plan.md
99-decisions/decision-log.md
08-audits/claude-audit-through-m01-hammer-review-acceptance.md
```

Also inspect any other file needed to verify claims or resolve contradictions.

## Required Reading — Application Repository

Read at minimum:

```text
pyproject.toml
uv.lock
alembic.ini
migrations/**
src/discrepancy_desk/**
tests/**
scripts/run_ht_evidence.py
scripts/build_desktop_sidecar.py
docs/m04-service-api-contract.md
docs/m05-desktop-security-contract.md
docs/m05-exit-gate-review.md
docs/ht-coverage-ledger.md
desktop/package.json
desktop/package-lock.json
desktop/src/**
desktop/src-tauri/Cargo.toml
desktop/src-tauri/tauri.conf.json
desktop/src-tauri/capabilities/**
desktop/src-tauri/src/**
.gitignore
```

Inspect Git history and diffs from the previous audit baseline through current HEAD where useful.

## Scope of Audit

### 1. M02 contract integrity

Verify that:

- lifecycle states and transitions match implementation;
- invalid transitions fail closed;
- idempotency and concurrency claims are real;
- migration, evidence, and recovery plans match executable behavior;
- the adversarial matrix maps to actual tests rather than narrative-only coverage.

### 2. M03 persistence and authority

Verify:

- SQLite/Alembic schema matches documented contracts;
- revisions and approvals are bound to exact content;
- evidence registration verifies governed paths and hashes;
- publications, mismatches, replacements, and metrics preserve lineage;
- audit events are append-only and hash-chained as claimed;
- archive, backup, restore, and reconciliation fail closed;
- no generic transition or service route bypasses authority;
- the web harness does not create hidden authority.

### 3. M04 editorial workflow

Verify:

- account scoping is enforced for organization, scheduling, queries, and metrics;
- Archive, Docket, Flash Release, Reserve, schedule lineage, and horizon behavior match doctrine;
- Ready-to-Post and Need-a-Post remain derived, deterministic, and non-authoritative;
- realistic-week and correction-lineage tests prove more than happy paths;
- M04 changes did not regress M03 authority.

### 4. M05 desktop security and parity

Verify:

- React and Rust do not directly mutate SQLite;
- all desktop mutations use the governed local API;
- the backend binds only to dynamic loopback;
- launch tokens are long, per-launch, unavailable without explicit startup configuration, and not exposed in command-line arguments or frontend storage;
- wrong or missing tokens fail before route execution;
- process ownership and child shutdown are correct for normal and proof paths;
- the packaged sidecar design does not leave hidden worker processes;
- app-data, evidence, migration, and sidecar paths are governed and deterministic;
- native evidence import is constrained, account-safe, size-bounded, and does not grant broad filesystem/database authority;
- Tauri capabilities are minimal;
- disabled product areas are honestly disabled;
- Records and Metrics views remain account-scoped;
- full manual workflow parity is actually proven;
- no updater, remote origin, public hosting, signing, or production-release claim was silently introduced.

### 5. Packaging and evidence claims

Verify:

- PyInstaller sidecar build script produces the architecture claimed;
- NSIS configuration is current-user and updater-free;
- migration resources are included correctly;
- generated binaries and secondary lockfiles cannot enter source control;
- installer lifecycle proof is accurately described;
- user data is preserved on uninstall as claimed;
- runtime evidence hashes and implementation commit bindings are internally consistent;
- `scripts/run_ht_evidence.py` binds evidence to the actual commit and does not conceal failures;
- the 29-passed/1-deferred claim is reproducible or appropriately caveated.

### 6. Documentation and roadmap consistency

Verify:

- M03, M04, and M05 status and closure records agree;
- roadmap numbering and duplicate legacy milestone files do not create ambiguity;
- D023 and D024 are represented consistently;
- current `STATUS.md`, milestone files, and application truth agree;
- M06 is research/planning only and implementation remains blocked;
- obsolete Obsidian/Qdrant language is clearly identified as preliminary where appropriate.

### 7. Security and repository hygiene

Search for:

- secrets, API keys, tokens, cookies, credentials, personal paths that should not be committed, and sensitive evidence;
- broad Tauri permissions;
- LAN or wildcard binding;
- arbitrary shell/process execution;
- path traversal;
- unsafe archive extraction;
- SQL injection or direct dynamic SQL authority;
- frontend-accessible secrets;
- generated executables or dependency directories tracked by Git;
- stale test artifacts or runtime evidence treated as source.

Do not print secret values. Report only file paths, secret type, and remediation.

## Validation

Run the safest applicable validation commands supported by the repository and your environment. At minimum attempt:

```text
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
uv run python scripts/run_ht_evidence.py
npm --prefix desktop run build
cargo test --manifest-path desktop/src-tauri/Cargo.toml
```

Do not claim a command passed if it was not actually run.

If installer or GUI execution is unsafe or unavailable, inspect the recorded evidence and clearly distinguish direct reproduction from documentary verification.

## Required Finding Format

For every finding provide:

```text
ID:
Severity: critical | high | medium | low | observation
Title:
Repository:
Files/lines:
Claim or invariant affected:
Evidence:
Reproduction or reasoning:
Required correction:
Suggested validation:
```

Order findings by severity.

Do not inflate stylistic preferences into defects. Focus on correctness, authority, security, recoverability, evidence quality, milestone truth, and material maintainability risks.

## Required Non-Finding Review

Even if no defect is found, explicitly state what you verified for:

- exact approval binding;
- account isolation;
- direct database authority absence in desktop code;
- launch-token boundary;
- loopback-only binding;
- packaged backend shutdown;
- native evidence import boundary;
- updater/signing absence;
- installer data preservation;
- evidence-to-commit binding;
- repository secret hygiene.

## Final Verdict

Return exactly one:

```text
ACCEPT
ACCEPT WITH CORRECTIONS
REJECT PENDING CORRECTION
```

Then include:

- blocking findings;
- non-blocking findings;
- strongest verified properties;
- residual risks;
- whether M06 deep research may proceed while corrections are made;
- whether any M06 architecture work must be blocked until correction.

Remember: you are an independent auditor. Do not redesign M06 in this report, and do not edit the repositories.
