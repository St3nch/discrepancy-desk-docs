# M06-A Canonical Rewrite Strategy — Document-Strategy Review

## A. Live Repository Truth

Both repositories were inspected read-only via ephemeral clones. Nothing was staged, edited, or committed.

**Documentation — `discrepancy-desk-docs`**
- Branch: `main`
- HEAD: `531377cb1db50409851251e749cec9ba666efc79`
- Latest commit: `531377c — Reconcile M06 milestone planning gate`
- Origin: synchronized with `origin/main`
- Working tree: clean; no modified, staged, or untracked files

**Application — `discrepancy-desk`**
- Branch: `main`
- HEAD: `6cbd0366e55bfba0c9687201615b72e70bf485d5`
- Latest commit: `6cbd036 — Bind AC-01 correction evidence`
- Origin: synchronized with `origin/main`
- Working tree: clean

Both match the expected checkpoints exactly.

**One live-truth correction to the assignment's stated condition.** The assignment says the docs repo "may contain this untracked external report" at `08-audits/m06a-local-manual-vault-planning-package.md`. Live truth: it does **not**. The working tree is clean, and that path does not exist in the repository. The Claude planning report exists only as an external artifact outside the repository. This does not change the strategy — the report must still be treated as an external input to be preserved on first commit — but the report is **not yet tracked**, so "preserve byte-for-byte" here means "commit once, unchanged" rather than "leave an existing tracked file alone." I flag it because a strategy that assumes the file is already committed would misstate the first commit step.

**Two other findings already resolved in-repo** (both were open in the earlier synthesis-review cycle):
- `03-system-design/obsidian-qdrant-sqlite-plan.md` already carries a "Historical Research and Superseded Architecture" header pointing to the accepted synthesis/rulings/milestone. F-4 is closed; no action.
- STATUS's `191fc8a`/`c90f1af` history is present; the only stale line is the docs-truth footer `AC-02 closure and M06-A planning authorization: pending current commit`, which `531377c` now satisfies.

## B. Final Recommendation

```text
CREATE A NEW CANONICAL PACKAGE AND SUPERSEDE PRIOR PLANNING
```

Concretely, this is a **hybrid executed as supersession-by-addition**:

- **Preserve** the Claude planning report and the independent Steward review as immutable external audit inputs (commit once, never edit).
- **Author a new, compact canonical planning set** (three files) that incorporates the Steward corrections and is internally consistent on its own.
- **Bind them** with one correction-disposition record that maps every finding, in the exact house format already used for AC-02.
- **Amend only the two mutable navigation records** (`STATUS.md`, `LLM_MAP.md`) so they point at the new canonical plan.
- **Rewrite nothing that already exists** in `05-implementation-planning/`, `99-decisions/`, or the historical `08-audits/` records.

This is not "surgical amendments to the Claude report" (doctrine forbids editing a completed external report, and the corrections are too cross-cutting to patch), and it is not "rewrite existing canonical files" (no existing file is the M06-A plan — the plan is the external report). The correct move is a *new* canonical artifact, exactly as the project already did when the M06 research reports needed correction: the reports were preserved and a new `m06-architecture-synthesis.md` was written to incorporate the corrections.

## C. Why Rewrite (New Canonical) Rather Than Patch

Three independent reasons converge:

**1. Doctrine forbids patching the input.** Section 5 and the project's own history require the sequence *external input → independent review → disposition → accepted canonical planning → authorization*. The Claude report is the external input. Editing it to satisfy the Steward's corrections would collapse two audit stages into one and erase the record of what was corrected and why. The report must stay frozen.

**2. The corrections are cross-cutting, not local.** The Steward's findings do not sit in one section — they thread through the whole plan. Vault-identity enforcement touches schema columns, migration isolation, routing, per-Vault audit, *and* backup. Actor identity touches schema, audit, corrections, and merge/split acceptance. Deterministic package hashing touches schema, parser output, and backup treatment of derived rows. Same-account provenance touches schema, routing, and the adversarial matrix. Patching a ~1,000-line report with this many interlocking edits is precisely the "accumulating contradictory patches" failure the doctrine warns against; a clean canonical statement is safer.

**3. The load-bearing owner questions were re-opened by the review.** OD-1 (what one Vault represents) and OD-2 (per-Vault vs shared SQLite) determine the schema grain and migration topology. A corrected plan has to be written *around* their resolution, not bolted on. That is authorship, not amendment.

Patching would minimize edited lines — which section 5 explicitly says is **not** the goal. The goal is one internally consistent accepted package with preserved history.

## D. File-by-File Classification

Legend: **PRESERVE** = byte-for-byte / commit-once-unchanged · **CLOSURE-NOTE** = preserve with a single appended pointer · **BOUNDED** = bounded surgical amendment · **REWRITE** = full canonical rewrite · **SUPERSEDE** = replaced by a new document at a new path · **HISTORICAL** = retain as historical only.

| File | Current role | Kind | Treatment | Why / rewrite risk / patch risk | Ref new plan? | In commit? |
|---|---|---|---|---|---|---|
| `08-audits/m06a-local-manual-vault-planning-package.md` (Claude report) | External planning input | Historical evidence | **PRESERVE** (commit once, unedited) | It is the input in the audit chain. Rewriting erases what was reviewed; no patch permitted by doctrine. | No (it predates the plan) | Yes — first |
| *Steward independent review* (not yet in repo) | Independent review of the report | Historical evidence | **SUPERSEDE→new path** `08-audits/m06a-local-manual-vault-independent-review.md` (author the record, then freeze) | Must be preserved like `claude-m06-architecture-synthesis-review.md`. No existing file; create and freeze. | No | Yes — first |
| `05-implementation-planning/m06a-planning-authorization.md` | Owner authority | Authority | **PRESERVE** | Owner-approved authorization; rewriting mutates authority. Patch risk: none needed. | No | No |
| `05-implementation-planning/m06a-m06b-package-boundary.md` | Owner package split | Authority | **PRESERVE** | Owner-approved boundary. Immutable. | No | No |
| `05-implementation-planning/m06a-planning-package-outline.md` | Preparation guidance my report was built to | Planning (still accurate) | **CLOSURE-NOTE** (optional single pointer to canonical plan) | Requirements remain correct; corrections are about content, not the outline. Rewriting is needless; a pointer aids navigation. | Yes (one line) | Optional |
| `05-implementation-planning/m06-architecture-synthesis.md` | Accepted architecture baseline (D026) | Authority | **PRESERVE** | Planning may not reopen architecture. Corrections are plan-level, not architecture-level. | No | No |
| `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md` | Accepted milestone definition | Authority | **PRESERVE** | Part of the accepted synthesis baseline. | No | No |
| `05-implementation-planning/m06-pre-synthesis-correction-specification.md` | Research→synthesis correction spec | Historical | **PRESERVE** | Closed historical artifact of the prior cycle. | No | No |
| `05-implementation-planning/implementation-roadmap.md` | Cross-milestone roadmap | Authority-adjacent planning | **PRESERVE** (optional one-line pointer) | Already states M06-A needs its own exact plan/matrix/gate. Rewriting a 30 KB cross-milestone doc is high-risk and unnecessary. | Optional pointer | Optional |
| `05-implementation-planning/hammer-test-strategy.md` | Cross-milestone hammer doctrine | Authority-adjacent | **PRESERVE** | Only generic Vault mentions. M06-A matrix is a separate milestone artifact per precedent. | No | No |
| `08-audits/ac02-m06-research-correction-disposition.md` | Prior disposition | Historical/authority | **PRESERVE** | Closed record; also the format template for the new disposition. | No | No |
| `08-audits/ac02-m06-research-correction-closure.md` | Prior closure | Historical/authority | **PRESERVE** | Closed record. | No | No |
| `08-audits/claude-m06-architecture-synthesis-review.md` | Prior review | Historical | **PRESERVE** | Closed audit. | No | No |
| `08-audits/claude-m06-research-program-independent-review.md` | Prior review | Historical | **PRESERVE** | Closed audit. | No | No |
| `99-decisions/m06-owner-architecture-rulings.md` | D01–D16 rulings | Authority | **PRESERVE** (append-only if owner adds a ruling) | Accepted rulings; never rewrite. New owner decisions append. | No | Only if owner rules |
| `99-decisions/decision-log.md` | Decision ledger (…D026) | Authority ledger | **PRESERVE now; BOUNDED append later** | Owner decisions OD-1/OD-2/OD-3 land here as a new `D0xx` only when the owner rules. | No | Later, with owner decisions |
| `STATUS.md` | Current-state navigation | Mutable current-state | **BOUNDED** | Update Current Mode / Next Action to the plan-returned-and-reviewed gate; fix the stale commit-line footer. Safe to edit — it is explicitly current-state. | Yes | Yes — with canonical |
| `LLM_MAP.md` | Navigation index | Navigation | **BOUNDED** | Add the canonical plan, review, and disposition to the reading list; refresh the M06 paragraph. | Yes | Yes — with canonical |

**Additional files identified (not in the assignment list):**
- `03-system-design/obsidian-qdrant-sqlite-plan.md` — **HISTORICAL** (already correctly marked; no action).
- `03-system-design/data-model-planning.md` — **PRESERVE/HISTORICAL** (explicitly "conceptual, not approved"; the canonical plan supersedes it for M06-A scope by addition, no edit).
- `03-system-design/multi-account-model.md` — **PRESERVE** (early direction; not contradicted).
- *The review prompt for the canonical plan* — when the corrected plan goes to independent review, its prompt should be preserved under `07-prompts/`, matching the house pattern (e.g. `07-prompts/claude-project-audit-...`). Not part of this commit.

## E. Files That Must Remain Immutable

Byte-for-byte, no edits ever (append-only where noted):

- `08-audits/m06a-local-manual-vault-planning-package.md` (the Claude report)
- The preserved Steward review (once written and committed)
- `m06a-planning-authorization.md`, `m06a-m06b-package-boundary.md`
- `m06-architecture-synthesis.md`, `milestone-06-records-dossiers-anomaly-vault.md`, `m06-pre-synthesis-correction-specification.md`
- All four prior `08-audits` records (`ac02-*`, `claude-m06-*`)
- `99-decisions/m06-owner-architecture-rulings.md` (append-only), `99-decisions/decision-log.md` (append-only)
- `03-system-design/obsidian-qdrant-sqlite-plan.md`, `data-model-planning.md`

## F. Files Safe for Full Rewrite

**None of the existing files.** No currently-existing document is a candidate for full in-place rewrite. Every "rewrite" in this strategy is the authoring of a **new** canonical file at a **new** path (Section H). This is deliberate: nothing that is authority or historical evidence is replaced at its own path, so no history is erased.

## G. Files Requiring Only Bounded Amendment

- `STATUS.md` — Current Mode / Active gate / Next Bounded Action updated to "planning package returned; independent review = READY WITH CORRECTIONS; canonical corrected plan prepared; owner decisions OD-1/OD-2 pending"; refresh the docs-truth footer commit line. No prior decision text removed.
- `LLM_MAP.md` — add the three canonical files + the review + the disposition to the numbered reading list; update the one M06 status paragraph.
- `decision-log.md` / `m06-owner-architecture-rulings.md` — **append-only, and only when the owner actually rules** OD-1/OD-2/OD-3. Not part of the canonical-plan commit.
- `m06a-planning-package-outline.md` — optional single appended pointer line ("Superseded as the controlling plan by `…canonical-plan.md`; retained as the preparation outline").

## H. Proposed Canonical Planning Package

**Recommended structure: a coordinated set of three planning files** (a trimmed Option B). Not one consolidated file; not the seven-file example.

**Why three and not one.** The project's own house style for a heavy milestone is a small coordinated set, not a mega-file — M02 ships `m02-operational-persistence-contract`, `m02-lifecycle-state-model`, `m02-adversarial-test-matrix`, and `m02-migration-and-hammer-execution-plan` as separate documents, and M04/M05 each keep their adversarial matrix as its own file. A single consolidated plan that carried schema + nine parser records + a 75-row matrix + phases would exceed the Claude report in length and be reviewed worse, not better — the exact readability failure the doctrine cares about.

**Why three and not seven.** Backup/restore, runtime/routing, migration topology, and the bounded implementation package are so tightly coupled to the core data contract (Vault identity, actor identity, cross-database references, and provenance thread through all of them) that splitting them into separate files re-introduces cross-file drift — the very problem we are eliminating. Only the two genuinely standalone, differently-reviewed artifacts are peeled off: parser admission (per-parser sign-off authority) and the adversarial matrix (reviewed test-by-test, and separate by precedent).

**File 1 — `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`**
- Purpose: the single load-bearing corrected plan.
- Controlling authority: `m06a-planning-authorization.md`, `m06-owner-architecture-rulings.md` (D01–D16), `m06-architecture-synthesis.md`, the Steward review.
- Required sections: scope/exclusions/stop conditions; the physical data contract (schema families, acquisition/artifact cardinality, acquisition lifecycle, deterministic package hashing, actor identity, polymorphic-reference discipline, same-account provenance); Vault-identity enforcement; migration topology and isolation; per-Vault audit and idempotency; desktop/backend multi-Vault routing; cross-database reference rules; Vault filesystem/package layout; search + deterministic chunk contract; assertions/dossiers/corrections; static context-run contract; backup/restore/recovery including treatment of derived SQLite rows and rights/retention separation; the bounded implementation package (exact app-repo change surface, phases, validation, stop conditions).
- Sources: the Claude report (content), the Steward findings (corrections), D01–D16, the synthesis.
- Relationships: parent of Files 2 and 3; those two are referenced, not duplicated.
- Created from scratch: yes. Supersedes the Claude report **as the controlling plan** (report retained as input).
- Acceptance: independent review + explicit owner acceptance before it is canonical.

**File 2 — `05-implementation-planning/m06a-parser-admission-plan.md`**
- Purpose: the nine parser admission records with admission authority and mandatory network-egress denial.
- Controlling authority: D11 (narrow parser scope), the authorization's parser exclusions, the Steward's parser-admission-authority and egress findings.
- Required sections: per-format record (TXT, Markdown, SRT, VTT, JSON, local RSS/Atom, local basic HTML, born-digital PDF, YouTube locator+transcript); library candidate; limits; warn/fail-closed semantics; security boundary; no-network proof obligation; explicit admission-authority statement (no parser admitted merely by appearing in architecture).
- Relationship: consumes the canonical plan's normalized-package hashing contract.
- Created from scratch: yes. Acceptance: independent review + per-parser owner sign-off.

**File 3 — `05-implementation-planning/m06a-adversarial-closure-matrix.md`**
- Purpose: the full hammer matrix with **executable** hammer mapping (each row bound to a concrete HT identifier / test path), addressing the Steward's "executable hammer mapping" finding.
- Controlling authority: `hammer-test-strategy.md` doctrine + the authorization's adversarial requirements.
- Required sections: the matrix rows (authority bypass, hostile local files, duplicate/replay, parser partial output and detector failure, merge/split evidence bypass, correction/search/chunk invalidation, projection edits, backup/restore tamper and account leakage, parser network-egress, fabricated-evidence fail-closed), each mapped to an executable test id and expected fail-closed result.
- Created from scratch: yes. Acceptance: independent review test-by-test.

## I. Proposed Disposition Record

**Path:** `08-audits/m06a-planning-correction-disposition.md` (mirrors `ac02-m06-research-correction-disposition.md`).

- Purpose: the single binder connecting the Claude report, the Steward review, the corrected canonical plan, the owner decisions, and future implementation authorization.
- Required sections: Status; Source review (the Steward review path); Independent-review verdict (`M06-A PLAN READY WITH CORRECTIONS`); **Accepted correction set** (one entry per finding); Owner decisions recorded; Closure conditions; Implementation record (which canonical files each correction lands in); Current block.
- Finding-disposition format: per finding, `MC-nn — <title>` → `Disposition: accepted | accepted-with-modification | rejected` → `Required result:` → `Lands in: <canonical file + section>`. Every independent-review finding **must** be mapped — no silent drops (this is what the AC-02 disposition did for AC02-01…09).
- Rejected Claude proposals stay visible: where the corrected plan overrides a Claude-report choice, the disposition records the original proposal, the reason for rejection, and the replacement — so the report is never edited but the divergence is auditable.
- Owner decisions: OD-1/OD-2/OD-3 recorded here with the ruling and date, then mirrored into `decision-log.md` as a `D0xx` entry.
- Authority block: ends with the explicit "no implementation authority" statement.

**Do not edit the Claude report.** All divergence is captured in the disposition, never by touching the input.

A matching **closure record** — `08-audits/m06a-planning-correction-closure.md` — is authored **later**, only after owner acceptance, mirroring `ac02-...closure.md`.

## J. Reference and Navigation Update Map

- `LLM_MAP.md`: add to the numbered reading list — the three canonical files, the preserved Steward review, the disposition record; update the M06 status paragraph to name the canonical plan as the controlling M06-A plan.
- `STATUS.md`: Current Mode → "M06-A corrected canonical plan prepared; independent review returned CORRECTIONS; disposition recorded; owner decisions OD-1/OD-2 pending"; Next Bounded Action → owner review + OD-1/OD-2; fix the stale docs-truth footer line.
- `m06a-planning-package-outline.md`: optional one-line pointer to the canonical plan.
- `implementation-roadmap.md`: optional one-line pointer under the M06-A section.
- Inbound-reference sweep: before commit, grep both repos for any link to the canonical filenames to confirm no dangling references; confirm nothing links to a path that does not yet exist.

## K. Rewrite (New-Authoring) Validation Controls

Because every "rewrite" is new-file authoring plus two bounded nav amendments, the controls are:

1. Read the Claude report and the Steward review in full before authoring.
2. Build a **coverage matrix**: every Claude-report section → its destination canonical section; every Steward finding → its destination canonical section + disposition entry. No unmapped source, no unmapped finding.
3. Verify no D01–D16 ruling and no accepted authorization requirement disappears (checklist against `m06-owner-architecture-rulings.md` and the authorization's 10-item required-return list).
4. Check inbound references from other documents; update navigation (Section J).
5. Full diff review of the new files and the two amended nav files.
6. Secret scan (`git diff --check`; grep for keys/tokens/credentials) — planning docs must carry none.
7. Consistency validation: terminology matches the synthesis; no architecture ruling reopened; exclusions intact.
8. Independent review of the corrected canonical set (separate prompt, preserved under `07-prompts/`).
9. Owner acceptance.
10. Commit boundaries per Section M.

**Path discipline:** new canonical files take **new paths**; no document is replaced at its own path. The Claude report and all authority/historical records keep their paths untouched. Same-path edits are used **only** for the two clearly-mutable current-state records (`STATUS.md`, `LLM_MAP.md`).

## L. Owner-Decision Timing

**Does the rewrite strategy itself need an owner decision?** No new one to *begin*. The authorization permits planning to "prepare, reconcile, and independently review," and the outline's review sequence already names "disposition findings → owner review." Preparing a corrected canonical plan + disposition is inside existing authority. Owner **acceptance** is required before the plan becomes canonical and before commit-as-accepted — but no fresh authorization is needed to start.

**OD-1 — what one Vault represents.** Genuinely unresolved and **load-bearing**: my report surfaced that `owned_accounts` is a platform handle, not the persona/Vault owner, so the Vault grain determines `vault_accounts` and every downstream FK. Resolve it **at owner review of the corrected plan**, which should present bounded options (per-persona recommended) with a recommendation — not ask the owner to decide cold. Must be resolved before the plan is *accepted*, not before it is *drafted* (draft parameterized on the recommended default).

**OD-2 — per-Vault SQLite vs shared.** Genuinely unresolved and **the most consequential** for migration topology, isolation, per-Vault audit/idempotency, and cross-database references — all of which the Steward flagged. Same timing: bounded options + recommendation (separate per-Vault DB) presented in the corrected plan; resolved at owner review before acceptance.

**OD-3 — reject timed-deletion vs add purge authority.** Lower stakes and resolvable by recommendation: **reject timed deletion in M06-A**, record rights/retention metadata only, defer purge authority — this aligns with D06 immutable correction/supersession. Carry as a recommended default in the plan; confirm at owner review.

**Defer-until-after-design test:** all three should be decided **after** the owner sees the corrected technical design, not before. Presenting a corrected plan that is explicitly parameterized around OD-1/OD-2 (with recommendations) is safer than extracting the decisions in isolation. **No new owner decisions are introduced.**

## M. Recommended Commit Sequence

Derived from live truth (docs clean at `531377c`; report and review both external/untracked):

1. **Commit the external inputs first, unedited** — the Claude report and the preserved Steward review, together, as the *input + independent-review* pair (this mirrors AC-02's `b675014`, which landed the research review + disposition together). Committing these first preserves the audit order *input → review* in history; committing them after the rewrite would invert it.
2. **Prepare** the disposition record and the three canonical files as one bounded documentation package.
3. **Independently review** that corrected package.
4. **Commit** the canonical set + disposition + the two bounded nav amendments (`STATUS.md`, `LLM_MAP.md`) together, so navigation never points at a nonexistent file.
5. **Owner review** → record OD-1/OD-2 (and OD-3 default) — append `decision-log.md` (`D0xx`) and, if it rises to a ruling, `m06-owner-architecture-rulings.md`.
6. **Commit the closure record** after owner acceptance.
7. **Explicit implementation authorization** — a separate later gate, not part of any commit above.

Answering the sub-questions directly: the untracked Claude report should be committed **at step 1**, paired with the Steward review; the independent review should be committed **before** the canonical rewrite (it is an input to it), never after.

## N. Risks of the Recommended Strategy

- **New-file proliferation risk** — mitigated by capping the canonical set at three and forbidding further splits.
- **Duplication drift between canonical plan and its two child files** — mitigated by the rule that Files 2–3 are referenced, not restated, and that shared contracts (hashing, Vault identity) live only in File 1.
- **Disposition-coverage gaps** — a finding could be silently dropped; mitigated by the mandatory finding→section coverage matrix and independent review.
- **Navigation lag** — nav could point at unwritten files; mitigated by committing nav in the *same* commit as the canonical files (step 4).
- **Premature owner decisions** — resolving OD-1/OD-2 before the corrected design is seen; mitigated by binding those decisions to owner review of the corrected plan.
- **Report-editing temptation** — pressure to "just fix" the report; mitigated by the absolute rule that all divergence lives in the disposition.

## O. Alternative Strategy Considered

**One consolidated canonical plan (Option A) + disposition.** Rejected: it maximizes single-file consistency but produces a document longer than the Claude report, which reviews worse and breaks the house precedent of separate adversarial-matrix and admission artifacts. **Seven-file set (Option B example) also rejected:** splitting backup, routing, migration, and the implementation package from the core data contract re-introduces the cross-file drift the correction cycle exists to remove. **Pure surgical amendment of the Claude report** was rejected at the outset — doctrine forbids editing a completed external report, and the corrections are too cross-cutting to patch coherently. The three-file hybrid is the smallest set that stays internally consistent and reviewable.

## P. Exact Next Action

Commit the Claude report and the preserved Steward independent review, unedited, as the input + independent-review pair (Section M step 1). Then prepare — as a separate bounded documentation package, not in this review — the disposition record and the three canonical files, parameterized on the recommended OD-1/OD-2 defaults, for independent review and owner acceptance. Do not edit the Claude report, do not touch any authority or historical file, and do not begin implementation.

## Q. Explicit Authority Block

```text
No M06-A implementation authority exists.

This review may recommend which planning documents should be preserved,
amended, rewritten, or superseded.

It does not authorize repository edits, application code, migrations,
database creation, Vault creation, parser work, dependency installation,
runtime changes, M06-B work, or implementation.

Any canonical rewrite must be prepared as a separate bounded documentation
package, independently reviewed, owner-accepted, committed, and explicitly
authorized before implementation work begins.
```

---

**Verdict: `CREATE A NEW CANONICAL PACKAGE AND SUPERSEDE PRIOR PLANNING`** — preserve the report and review as immutable inputs, author a compact three-file canonical plan plus one disposition binder, amend only STATUS and LLM_MAP, and resolve OD-1/OD-2 at owner review of the corrected design. Both repositories remain untouched; implementation remains blocked.