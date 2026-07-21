# Independent Verification — M06-A Canonical Rewrite Strategy

## A. Live Repository Truth

### Documentation repository

```text
Path:    C:\dev\x\discrepancy-desk-docs
Branch:  main
HEAD:    531377cb1db50409851251e749cec9ba666efc79
Remote:  synchronized with origin/main
```

Working tree:

```text
## main...origin/main
?? 08-audits/m06a-local-manual-vault-planning-package.md
```

The Claude planning report exists in the live documentation repository as an untracked file.

Its Git object hash was measured before and after this verification:

```text
5a5f260449ba3a19c53652f1f602c6014631ed9f
```

The hash did not change.

### Application repository

```text
Path:    C:\dev\x\discrepancy-desk
Branch:  main
HEAD:    6cbd0366e55bfba0c9687201615b72e70bf485d5
Remote:  synchronized with origin/main
Status:  clean
```

### Existing governing baseline

The live repository confirms:

- `m06-architecture-synthesis.md` is the accepted M06 architecture baseline.
- M06-D01 through M06-D16 are owner-approved.
- AC-02 is closed.
- M06-A exact planning is authorized.
- M06-A implementation remains blocked.
- M06-B planning and implementation remain blocked.
- The planning-package outline explicitly requires the independent-review prompt to be supplied **outside the repository**.
- `00-doctrine/audit-artifact-boundary.md` explicitly prohibits committing live audit prompts.
- Commit `91146e4 — Remove live audit prompt from repository` established that boundary by removing an M06 review prompt and adding the governing doctrine.
- No proposed canonical-plan or disposition path currently exists in Git.

### Read-only result

No file was modified, created, staged, committed, moved, renamed, or deleted.

---

## B. Final Strategy Verdict

```text
CLAUDE STRATEGY VERIFIED WITH CORRECTIONS
```

Claude’s principal strategy is correct:

```text
preserve external planning input
→ preserve completed reviews
→ create a new corrected planning package
→ map findings through a disposition record
→ independently review
→ obtain owner decisions and acceptance
→ authorize implementation separately
```

A new package is safer than rewriting the Claude report or patching accepted architecture documents. Claude’s proposed three-document split is also appropriately bounded.

Corrections are required in four areas:

1. more navigation and governing-reference files require bounded amendment than Claude identified;
2. owner decisions OD-1 through OD-3 must be resolved during the design-review cycle, before the final resolved canonical package is accepted;
3. no review prompt may be committed under `07-prompts/` or anywhere else;
4. the new package does not become canonical merely because it is written or committed—it becomes controlling only after independent review, finding disposition, and explicit owner acceptance.

---

## C. File-by-File Verification

| File | Verified treatment | Reason |
|---|---|---|
| `08-audits/m06a-local-manual-vault-planning-package.md` | **Preserve byte-for-byte** | External planning input. Never patch, rewrite, rename, or silently promote into authority. |
| Completed independent technical review | **Create once as a completed audit report, then preserve** | It is a completed review, not a prompt or temporary instruction. |
| Claude document-strategy report | **Preserve as a completed audit report** | It is relevant audit history, including its incorrect ephemeral-clone conclusion about the live untracked report. |
| This strategy verification | **Preserve as a completed audit report** | It records the live-truth correction and final document strategy. |
| `m06-architecture-synthesis.md` | **Preserve** | Accepted architecture baseline. The new plan implements it; it does not replace it. |
| `m06-owner-architecture-rulings.md` | **Preserve; append only if the owner explicitly adds a ruling** | M06-D01 through D16 remain accepted. |
| `m06a-planning-authorization.md` | **Preserve** | Owner authority record. No amendment is needed. |
| `m06a-m06b-package-boundary.md` | **Preserve** | Accepted package split and M06-B block remain accurate. |
| `m06-pre-synthesis-correction-specification.md` | **Preserve** | Closed prior correction input and still useful terminology authority. |
| `m06a-planning-package-outline.md` | **Bounded amendment after the corrected package exists** | Add a status/fulfillment pointer so future stewards do not mistake the preparation outline for the controlling plan. Do not rewrite its requirements. |
| `milestone-06-records-dossiers-anomaly-vault.md` | **Bounded amendment required** | Its Required Reading and entry-gate lists must identify the corrected plan, parser plan, matrix, review, and disposition. |
| `implementation-roadmap.md` | **Bounded amendment required** | Its Mandatory Milestone Documentation Rule requires the roadmap and milestone file to be updated together when governing M06 documents change. |
| `STATUS.md` | **Bounded amendment required** | Must reflect the actual planning-review/correction gate and remove the stale `pending current commit` line. |
| `LLM_MAP.md` | **Bounded amendment required** | Must add the new governing package, reviews, and disposition to the reading order and current-mode summary. |
| `decision-log.md` | **Append only after owner decisions** | OD-1 through OD-3 must not be recorded as accepted before the owner rules. |
| Prior AC-02 and Claude audit records | **Preserve** | Closed historical evidence. |
| `obsidian-qdrant-sqlite-plan.md` | **Preserve as historical** | It is already correctly marked superseded. |
| `README.md` | **Preserve** | Its entry path through `LLM_MAP`, `STATUS`, and the roadmap remains sufficient. |
| `PROJECT_BRIEF.md` | **Preserve** | No project-purpose change is occurring. |
| Existing `07-prompts/*` files | **Preserve as legacy history; add no new audit prompt** | Their existence predates the explicit audit-artifact boundary and is not precedent for new prompt commits. |

### Existing files requiring full rewrite

```text
None
```

The new canonical documents should be created at new paths. No accepted or historical file should be replaced in place.

---

## D. Canonical Package Structure

The proposed three-file structure is sufficient and not over-fragmented.

### 1. Core plan

```text
05-implementation-planning/m06a-local-manual-vault-canonical-plan.md
```

Required scope:

- exact M06-A scope, exclusions, and stop conditions;
- canonical object vocabulary;
- observations and unresolved questions;
- acquisition lifecycle and artifact cardinality;
- deterministic package hashing;
- actor identity and human-only authority;
- referential-integrity strategy;
- same-account provenance;
- Vault account and physical identity;
- SQLite topology and migration isolation;
- audit and idempotency topology;
- multi-Vault desktop/backend routing;
- filesystem layout and dual-write recovery;
- cross-database-reference rules;
- rights, retention, retrieval, export, and public-use boundaries;
- lexical search, chunks, projections, and invalidation;
- assertions, entities, dossiers, policy, and context runs;
- backup, restore, rebuild, and derived-data treatment;
- exact application change surface;
- corrected implementation phases;
- owner-decision boxes;
- explicit implementation block.

This file is the controlling technical-plan candidate, but its status must remain:

```text
candidate — not owner-accepted
```

until the review and owner gates close.

### 2. Parser plan

```text
05-implementation-planning/m06a-parser-admission-plan.md
```

Required scope:

- exact parser implementation choice per format;
- dependency, version, and license;
- supported subset;
- limits;
- warning and partial-failure vocabulary;
- deterministic-output requirements;
- fixture corpus;
- runtime admission mechanism;
- packaging proof;
- enforceable no-network boundary;
- per-parser owner-admission gate.

Parser records deserve a separate file because parser admission is independently gated and can evolve one parser at a time.

### 3. Adversarial closure matrix

```text
05-implementation-planning/m06a-adversarial-closure-matrix.md
```

Required scope for every invariant:

- requirement source;
- valid fixture;
- deliberate violation fixture;
- exact test identifier or planned test node;
- real-engine requirement;
- expected fail-closed result;
- evidence output;
- implementation phase;
- closure requirement.

The matrix should remain separate because it will eventually bind directly to executable hammer evidence.

### Package-size conclusion

Three files are enough.

A single mega-document would become harder to review. Splitting migration, backup, routing, rights, identity, and schema into many additional files would create unnecessary cross-file drift.

---

## E. Audit and Disposition Structure

### Disposition path

```text
08-audits/m06a-planning-correction-disposition.md
```

This path and document role match repository precedent established by:

```text
08-audits/ac02-m06-research-correction-disposition.md
```

### Required disposition structure

```text
Status
Source planning input
Source independent technical review
Strategy reviews
Review verdict
Accepted correction set
Rejected or modified proposals
Finding-to-document mapping
Owner decisions and dates
Independent corrected-package review
Closure conditions
Implementation record
Current authority block
```

Each technical-review finding must be represented individually.

Recommended format:

```text
M06A-PC-01 — <finding title>

Disposition:
  accepted
  accepted with modification
  rejected

Required result:
  <exact corrected result>

Lands in:
  <canonical file and section>

Closure evidence:
  <review or owner record>
```

No finding may disappear merely because the new plan reads better.

### Supersession meaning

The corrected package may eventually supersede the Claude report **as the controlling M06-A implementation plan**.

It does not:

- alter the Claude report;
- invalidate its historical value;
- replace the accepted architecture synthesis;
- replace the planning authorization;
- replace owner rulings;
- create implementation authority.

Supersession becomes effective only after owner acceptance.

---

## F. Navigation Updates

Claude’s recommendation to amend only `STATUS.md` and `LLM_MAP.md` is incomplete.

### Required amendments

#### `STATUS.md`

Update:

- current mode;
- active planning gate;
- preserved external reports;
- independent technical-review verdict;
- strategy-verification verdict;
- remaining owner decisions;
- next bounded action;
- stale documentation checkpoint line.

#### `LLM_MAP.md`

Add:

- the corrected core plan;
- parser-admission plan;
- adversarial matrix;
- independent technical review;
- strategy reviews;
- correction disposition.

Update the current-project-mode paragraph.

#### `implementation-roadmap.md`

A bounded amendment is mandatory.

Its own Mandatory Milestone Documentation Rule says the roadmap and milestone file must be updated together whenever governing documents change.

Add the corrected package and disposition under the M06 architecture/package/governing-document lists.

#### `milestone-06-records-dossiers-anomaly-vault.md`

A bounded amendment is mandatory.

Update:

- Required Reading;
- M06-A entry gate;
- exact controlling-plan references;
- review/disposition references;
- current implementation block.

#### `m06a-planning-package-outline.md`

Add a bounded status note after the corrected package exists, such as:

```text
This outline remains the preparation requirements record.
Its requirements are fulfilled by:
- <core plan>
- <parser plan>
- <adversarial matrix>

Those files become controlling only after independent review and owner acceptance.
```

Do not rewrite the outline or erase its original preparation role.

### No amendment currently required

```text
README.md
PROJECT_BRIEF.md
m06-architecture-synthesis.md
m06a-planning-authorization.md
m06a-m06b-package-boundary.md
m06-owner-architecture-rulings.md
```

---

## G. Owner-Decision Timing

Claude’s recommendation to wait until after corrected-plan review needs refinement.

OD-1 and OD-2 materially determine the exact schema and migration topology. A supposedly exact canonical plan cannot remain permanently branched between incompatible alternatives.

### Correct timing

```text
draft decision-bearing technical candidate
→ focused independent review of the options and recommendations
→ owner resolves OD-1 through OD-3
→ incorporate one resolved design
→ independently review the complete resolved canonical package
→ owner accepts or returns corrections
```

Therefore:

```text
OD-1 through OD-3 occur during the design-review cycle,
before final corrected-package acceptance.
```

They do not need to be decided before any drafting starts, but they should not remain unresolved until after the final package has already been independently reviewed as complete.

### OD-1 — What one Vault represents

Recommended ruling:

```text
one Vault represents one editorial/public brand identity
```

Platform-owned accounts such as X and Truth Social are explicit linked records, not separate Vault identities merely because they use different platforms.

### OD-2 — SQLite topology

Recommended ruling:

```text
one SQLite file per physical Vault
```

This best matches the accepted physical-isolation direction and minimizes cross-account leakage and M00–M05 migration blast radius.

### OD-3 — Timed deletion and purge authority

Recommended ruling:

```text
M06-A rejects material requiring timed deletion
and defers governed destructive purge authority
```

Rights and retention metadata may still be recorded, but M06-A should not rush a destructive evidence-deletion system.

---

## H. Commit Sequence

### Commit 1 — Preserve audit inputs

After explicit owner authorization to write:

- preserve the Claude planning report unchanged;
- create the completed independent technical-review record unchanged;
- preserve Claude’s completed document-strategy review unchanged;
- preserve this completed strategy verification unchanged.

Recommended paths:

```text
08-audits/m06a-local-manual-vault-planning-package.md
08-audits/m06a-local-manual-vault-planning-package-independent-review.md
08-audits/claude-m06a-canonical-rewrite-strategy-review.md
08-audits/m06a-canonical-rewrite-strategy-verification.md
```

Before staging:

- verify the Claude package hash remains `5a5f260449ba3a19c53652f1f602c6014631ed9f`;
- review all four files;
- confirm no prompt or temporary instruction is included;
- inspect exact status and diff.

A single coherent audit-preservation commit is acceptable. Artificially splitting the input and reviews into several commits is not required.

### Drafting stage — no authority transition

Prepare:

```text
m06a-local-manual-vault-canonical-plan.md
m06a-parser-admission-plan.md
m06a-adversarial-closure-matrix.md
m06a-planning-correction-disposition.md
```

Use candidate status throughout.

Do not commit a review prompt.

### Decision-review stage

- conduct focused independent review outside the repository;
- resolve OD-1 through OD-3;
- incorporate the owner rulings;
- complete the single resolved canonical candidate.

### Final corrected-package review

Independently review the resolved package.

Preserve only the completed review report, not its prompt.

Disposition all findings.

### Canonical-candidate commit

Commit together:

- three corrected planning files;
- correction disposition;
- completed corrected-package review;
- bounded updates to:
  - `STATUS.md`;
  - `LLM_MAP.md`;
  - `implementation-roadmap.md`;
  - `milestone-06-records-dossiers-anomaly-vault.md`;
  - `m06a-planning-package-outline.md`.

Navigation must not point at files that are absent from the same commit.

### Owner acceptance commit

After explicit owner acceptance:

- append owner decisions to `99-decisions/decision-log.md`;
- update disposition status;
- create a closure record if the correction cycle is closed;
- mark the new package owner-accepted and controlling.

### Implementation authorization

Implementation authorization remains a later, separate owner action and record.

---

## I. Claude Recommendations Accepted

The following recommendations are verified:

1. **Create a new corrected planning package rather than patch the Claude report.**
2. **Preserve the Claude report unchanged.**
3. **Preserve the independent technical review unchanged.**
4. **Use three coordinated planning documents rather than one mega-file or a seven-file sprawl.**
5. **Create a correction-disposition record under `08-audits/`.**
6. **Preserve accepted architecture, authorization, milestone history, owner rulings, and prior audits.**
7. **Use bounded navigation amendments rather than rewriting historical authority.**
8. **Keep M06-A implementation blocked.**
9. **Keep M06-B blocked.**
10. **Place low-level implementation admissions outside owner decision burden.**
11. **Commit preserved reports before beginning canonical authoring.**
12. **Use a later closure record rather than pretending the correction cycle is already closed.**

---

## J. Claude Recommendations Rejected or Corrected

### J-1 — Ephemeral-clone live truth

Claude reported that the live docs repository was clean and the planning package was absent.

That conclusion is incorrect.

The actual live repository contains:

```text
?? 08-audits/m06a-local-manual-vault-planning-package.md
```

An ephemeral clone cannot contain untracked local files and therefore cannot establish the live working-tree state.

### J-2 — Only `STATUS.md` and `LLM_MAP.md` need amendment

Rejected.

The following also require bounded updates:

```text
05-implementation-planning/implementation-roadmap.md
05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md
05-implementation-planning/m06a-planning-package-outline.md
```

The roadmap update is mandatory under its own governing-document rule.

### J-3 — Preserve a future review prompt under `07-prompts/`

Rejected.

Live doctrine says:

```text
Live audit prompts must not be committed.
```

The required workflow is:

```text
prepare prompt outside repository
→ send directly to reviewer
→ receive completed report
→ store completed report
```

Commit `91146e4` specifically removed a live M06 review prompt from the repository.

Existing older files in `07-prompts/` do not override the newer explicit doctrine.

No M06-A review prompt, sync prompt, correction prompt, or temporary reviewer instruction should be committed.

### J-4 — Resolve owner decisions after corrected-plan review

Corrected.

OD-1 and OD-2 shape the schema, migration, routing, audit, and backup design. They must be resolved during the design-review cycle before the final resolved package is independently reviewed and accepted.

### J-5 — New files immediately become canonical

Corrected.

The files may be named as the intended canonical package, but they remain candidates until:

```text
independent review
+ finding disposition
+ owner decisions
+ explicit owner acceptance
```

### J-6 — “Supersede prior planning” without qualification

Corrected.

The new package supersedes the Claude report only as the controlling M06-A implementation plan after acceptance.

It does not supersede:

- accepted architecture;
- owner rulings;
- planning authorization;
- package boundary;
- historical audit records.

---

## K. Exact Next Authorized Documentation Package

No documentation changes are authorized by this read-only verification itself.

The next package to prepare after explicit owner direction should be the **audit-preservation package**, not the canonical plan.

### Exact package

```text
08-audits/m06a-local-manual-vault-planning-package.md
08-audits/m06a-local-manual-vault-planning-package-independent-review.md
08-audits/claude-m06a-canonical-rewrite-strategy-review.md
08-audits/m06a-canonical-rewrite-strategy-verification.md
```

### Rules

- preserve the existing Claude planning package byte-for-byte;
- copy each completed report faithfully;
- do not include prompts;
- do not add canonical-plan content;
- do not amend navigation yet;
- inspect hashes, integrity, diff, and status before staging;
- stage only these exact audit files;
- obtain owner approval before commit or push.

After that audit-preservation package is committed, the next separately bounded task is authoring the three planning candidates and disposition record.

---

## L. Explicit Authority Block

```text
No M06-A implementation authority exists.

This verification confirms a documentation strategy only.

It does not authorize repository edits, file creation, staging, commits,
pushes, application code, migrations, databases, Vault creation, parser
work, dependency installation, runtime changes, M06-B work, providers,
network retrieval, monitoring, live LLM integration, Qdrant, graph work,
or implementation.

The untracked Claude planning report remains external planning input and
must remain byte-for-byte unchanged.

No audit, review, synchronization, or temporary operator prompt may be
committed.

Any corrected M06-A planning package remains a candidate until it has
received independent review, complete finding disposition, owner decisions,
explicit owner acceptance, and a separate implementation authorization.
```
