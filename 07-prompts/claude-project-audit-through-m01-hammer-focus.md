# Claude Audit Prompt — Discrepancy Desk Through M01, Hammer-Test Focus

You are performing an independent audit of **The Discrepancy Desk** project.

This is not a rewrite request and not permission to implement anything.

Use your filesystem/repository tools to inspect the live repositories. Do not trust this prompt, chat summaries, commit messages, or status claims without verifying them against the files and Git history.

## Repositories

Planning/docs repo:

```text
C:\dev\x\discrepancy-desk-docs
```

Main app/assets/evidence repo:

```text
C:\dev\x\discrepancy-desk
```

The docs repo is the planning and governance authority. The main repo currently contains approved public assets and M01 API-probe evidence; it may not yet be a Git repository. Do not initialize it, move files, edit files, generate credentials, call APIs, or create databases.

## Project Identity

```text
Public brand: The Discrepancy Desk
Persona: Quinton Clearance
Lore: Basement 1, Level 7 / B1-L7
Handle: @DiscrepancyDesk
Doctrine: AI drafts. Human clears. Database remembers. Metrics judge.
```

The project is a fictional/parody public brand plus a local human-approved research, drafting, review, publishing-support, and metrics system.

It is not an autonomous bot, scraper farm, fake government insider operation, engagement pod, or conspiracy-certainty engine.

## Required Sync Procedure

Before auditing:

1. Inspect Git status, branch, remotes, recent commits, and local-versus-remote state for the docs repo.
2. Read, in order:
   - `LLM_MAP.md`
   - `PROJECT_BRIEF.md`
   - `STATUS.md`
   - `05-implementation-planning/implementation-roadmap.md`
   - `05-implementation-planning/milestone-01-api-probe.md`
   - `05-implementation-planning/milestone-02-persistence-contract.md`
   - every document listed under M02 Required Reading
   - `05-implementation-planning/hammer-test-strategy.md`
   - `05-implementation-planning/m01-work-package-d-probe-analysis-and-m02-handoff.md`
   - `99-decisions/decision-log.md`
   - `99-decisions/research-log.md`
3. Inspect the M01 evidence tree in the main repo, including both attempts, manifests, request records, raw responses, headers, hashes, summaries, and the preserved controlled procedure.
4. Verify hashes independently rather than trusting the recorded `HASHES_OK` claim.
5. Inspect approved asset paths and hashes only as needed to verify M01 closeout claims.

## Current Claimed State — Verify, Do Not Assume

- M00 is complete.
- M01 is complete.
- M02 is active and planning-only.
- Docs HEAD should include `ec4eaf5 Close M01 and activate M02 persistence planning` plus a later hammer-doctrine/audit-preparation commit if this prompt has already been committed.
- X account identity, first post, pinned post, blue check, and approved assets are documented.
- A read-only OAuth 1.0a developer app was configured.
- Four probe requests were approved under a `$0.10` hard ceiling.
- Attempt 001 stopped fail-closed after HTTP 401.
- Attempt 002 completed four GET requests with HTTP 200, zero retries, estimated cost `$0.023`, and console-observed rounded cost `$0.02`.
- No API write occurred.
- No database schema, migration, SQLite file, persistence implementation, or dashboard implementation is admitted.

## Primary Audit Objective

Determine whether the project is genuinely ready to begin **M02 persistence-contract planning** without drifting into premature schema or implementation.

Give special attention to whether the owner-approved hammer-test doctrine is strong, complete, internally consistent, testable, evidence-based, and correctly positioned as a hard gate before database implementation.

## Owner-Approved Hammer Doctrine

Audit this doctrine as binding project authority:

> Every persistence rule must be attacked with deliberate bad data, invalid state transitions, fabricated identifiers, modified evidence, stale approvals, duplicate operations, partial failures, detector failures, migration failures, and recovery failures. Tests must run against the real SQLite engine where database behavior matters. Any ambiguity, missing evidence, integrity mismatch, or unrecognized failure must fail closed. Happy-path tests alone prove nothing.

The project also requires, at minimum:

- exact approved text cannot be replaced by platform-returned text containing generated links or other platform-added material;
- edits after approval invalidate prior approval;
- fabricated account, post, media, approval, publication, metric, and evidence IDs are rejected;
- modified raw evidence is detected through hash mismatch;
- missing raw evidence blocks normalized promotion or acceptance;
- invalid lifecycle jumps are rejected at the persistence boundary;
- duplicate publication records, double submits, and replayed operations are either safely idempotent or explicitly rejected;
- metric snapshots require source, observation method, and capture time;
- empty, unavailable, withheld, missing, and errored values remain distinct;
- spam or relevance classification never deletes or rewrites original mention evidence;
- classifier, detector, or review-tool failure never auto-promotes anything;
- migration interruption, rollback, and recovery are tested against the real engine;
- backup and restore preserve identifiers, relationships, exact approved text, provenance, and evidence hashes;
- deliberate non-detection, malformed evidence, partial writes, modified evidence, fabricated IDs, empty results, and recovery failures must be caught.

## Audit Questions

### A. Repository and milestone truth

- Are Git status, commit history, remote state, milestone status, required-reading lists, and next actions consistent?
- Does M01 actually satisfy every exit-gate requirement?
- Are any status statements stale, contradictory, unproven, or stronger than the evidence?
- Are files in the correct repository and directory?

### B. M01 probe integrity

- Does attempt 001 genuinely preserve the failure without overwrite or accidental retry?
- Does attempt 002 contain exactly the admitted P01–P04 request sequence?
- Were requests read-only, bounded, no-retry, and correctly authenticated?
- Are authorization material and credentials absent from files, raw evidence, headers, manifests, logs, and docs?
- Are hashes complete and independently reproducible?
- Are response counts, HTTP statuses, costs, and claims accurately represented?
- Did the procedure accidentally omit, normalize, pretty-print, or otherwise alter bytes that were claimed to be raw?
- Are observed rate-limit headers, request parameters, field lists, and provenance sufficient?
- Did the custom OAuth implementation or evidence runner introduce defects relevant to later design?

### C. Hammer-test doctrine strength

Identify missing or weak coverage for:

- persistence-layer enforcement versus UI-only enforcement;
- exact-text approval and revision binding;
- raw evidence immutability and hash verification;
- fabricated and mismatched identifiers;
- stale approvals and stale references;
- duplicate requests, retries, double clicks, replay, and idempotency;
- concurrency and transaction isolation where SQLite behavior matters;
- partial writes, crashes, interruption, rollback, and restart behavior;
- migration upgrade, downgrade/rollback posture, rerun, dirty state, invalid legacy rows, and version mismatch;
- backup, corruption, partial backup, disposable restore, and post-restore verification;
- missing, null, empty, withheld, unavailable, malformed, and errored values;
- metrics provenance, time ordering, monotonicity assumptions, and impossible values;
- platform separation and manual-only Truth Social boundary;
- classification/detection non-detection, exceptions, empty execution, and false promotion;
- negative tests that prove validators themselves fail closed when they crash or return ambiguous results;
- evidence that tests actually ran against the real engine rather than mocks;
- secret exposure, evidence contamination, and unsafe diagnostic output;
- tests that deliberately prove a broken implementation would be caught.

For every missing category, state:

1. the precise invariant or failure mode;
2. the deliberate violation fixture;
3. the expected fail-closed result;
4. whether it requires real SQLite behavior;
5. the evidence required to prove the test ran and passed.

### D. M02 readiness and drift risk

- Is the current M02 scope planning-only in every governing document?
- Are any docs prematurely implying tables, columns, schema, or implementation decisions unsupported by M01 evidence?
- Are the probe-derived candidate entities clearly labeled as candidates rather than approved schema?
- Is the distinction between exact approved authored text and platform-returned text strong enough?
- Are raw evidence, normalized records, operational workflow state, metrics snapshots, and human decisions kept conceptually separate?
- What must be decided before even drafting a physical schema?

### E. Security and operations

- Are local-secret handling and Bitwarden boundaries adequate?
- Is the local env-file workaround safely outside both repos, and is any documentation accidentally encouraging insecure persistence of secrets?
- Does the app/evidence repo need a `.gitignore`, Git initialization decision, or explicit non-Git evidence handling before future work?
- Are there files or caches that should not exist in evidence?
- Is the current `$5` cap and no-auto-recharge state accurately documented?

## Required Output

Produce a strict audit report with these sections:

1. **Verdict** — `PASS`, `PASS WITH CORRECTIONS`, or `FAIL`.
2. **Verified live state** — repo, branch, HEAD, remote relationship, working-tree status, active milestone.
3. **Critical findings** — defects that block M02 planning or undermine evidence.
4. **Major findings** — required corrections before persistence-contract approval.
5. **Minor findings** — cleanup or clarity issues that do not block planning.
6. **Hammer-test gap matrix** — precise missing invariants, violation fixtures, expected failures, real-engine requirement, and evidence requirement.
7. **M01 evidence audit** — independent verification of request boundary, raw evidence, hashes, secrets, costs, and failure preservation.
8. **M02 admission decision** — exactly what work may begin now and what remains prohibited.
9. **Recommended correction package** — one bounded, ordered package with exact affected files.
10. **Explicit non-authorization** — state that the audit does not authorize schema, migrations, SQLite creation, implementation, API calls, publishing, or other external actions.

## Audit Standards

- Be adversarial, specific, and evidence-based.
- Do not praise style or summarize documents instead of auditing them.
- Do not treat a test name as proof that the test detects the defect.
- Do not accept happy-path theater.
- Do not assume a validator is safe merely because it exists.
- Check whether deliberate breakage would actually be detected.
- Distinguish confirmed defects from recommendations.
- Cite exact file paths, line ranges when practical, commands, hashes, and observed evidence.
- If evidence is missing, say `not proven` rather than filling the gap with confidence.
- Do not edit any file. Return the audit only.
