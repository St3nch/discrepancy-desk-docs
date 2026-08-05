# Using the Pocock Skills for v2 Planning

How to run `mattpocock/skills` against the Discrepancy Desk reboot without recreating the
failure that ended v1.

---

## The Governing Risk

v1 died of governance outrunning execution: 200+ documents, a decision log 162 entries deep,
four correction cycles on one milestone, and no post ever published from the system.

A skill pack full of planning tools is the obvious way to do that again, faster. The entire
adoption plan below is therefore shaped by one rule:

> **No planning artifact is created until the work it plans is the next thing being built.**

The skills are pull-based. Reach for one when a specific question is blocking, not to
populate a documentation structure in advance.

Two of Pocock's own rules do most of the enforcement work, and both should be treated as
hard:

- **ADRs require all three** — hard to reverse, surprising without context, the result of a
  real trade-off. Miss one, no ADR. Under this bar, maybe fifteen of v1's 162 decisions
  qualify.
- **`CONTEXT.md` is a glossary and nothing else.** Not a spec, not a scratch pad, no
  implementation detail. v1's failure was documents that grew into governance; this rule
  makes that structurally impossible for the one document guaranteed to be read.

---

## Reality Check

These are **agent skills** — Claude Code or Codex, running in a repo, on the filesystem.
Several assume an issue tracker. They cannot run in a chat window. The sequence below happens
on the Linux box, against the new repo.

`setup-matt-pocock-skills` is run once per repo and configures the issue tracker, triage
labels, and domain doc layout. It is step zero of everything below.

---

## Phase 0 — Repo and Seed

**Do:**

1. Create the fresh Linux-first repo.
2. Run `/setup-matt-pocock-skills`. Choose the local-markdown tracker unless GitHub Issues is
   genuinely wanted — a solo operator does not need a hosted tracker, and local means the
   agent reads it without network.
3. Create `CONTEXT.md` from the **Glossary Seed** in
   `decisions/architecture-decisions.md`. Nineteen
   terms, already opinionated, already carrying `_Avoid_` lists.
4. Write **nine ADRs** — the decisions that pass all three bars. One paragraph each; the
   rejected alternative and its reason is the whole point.
   - `0001-capture-then-cite.md` (D3)
   - `0002-claim-authority-at-use.md` (D4)
   - `0003-question-scoped-research-runs.md` (D5)
   - `0004-parallel-rendition-generation.md` (D7)
   - `0005-backend-owns-runs-swappable-executor.md` (D8)
   - `0006-per-operation-versioned-rubrics.md` (D9)
   - `0007-leads-hold-material-not-claims.md` (D10)
   - `0008-runs-are-claimed-not-assigned.md` (D12)
   - `0009-byte-exact-quotation-required.md` (D14)

**Do not:** write ADRs for D1, D2, D6, D11, D13, or D15. The destination is a scope
statement; case-owns-renditions is structural rather than surprising; active/dormant follows
obviously once cases are durable; promotion-by-use is the absence of a mechanism rather than
a choice between two; run-close ordering is interface design; and returning the locator map
is forced by D14 rather than chosen against a live alternative. Recording them as ADRs would
already be the drift.

**0005 and 0009 are the two that matter most.** 0005 keeps the project independent of any
model provider. 0009 is its enforcement seam — byte-exact quotation is the single mechanism
that makes an untrusted executor safe, and without it 0005 is a liability rather than a
design.

**Do not:** port anything else from the v1 docs repo. The Vision Reference is the extract;
that is sufficient.

---

## Phase 1 — Fog Is Closed for the Destination

The fog items are listed in `decisions/architecture-decisions.md`. They are not all equal,
and most resolve for free once a case runs end to end. Only resolve what blocks the next
build.

**Nothing blocks the destination any more.** The five originally-blocking items are closed:
tool surface (D8, D14, D15), job queue and run registry (D12, D13), and captured vs. promoted
(D11). Phase 2 can begin.

**Left in fog deliberately:**

Rubric contents, coverage measurement, entity resolution ergonomics, Release Watch, long-form
rendition shape, web UX. Each gets dramatically easier after one real case exists, and
answering now means guessing. Coverage in particular is unanswerable in the abstract — it
needs a real spine looked at and judged complete. Rubric contents should be drafted *after*
the first run, tuned against actual output rather than written in advance.

**Executor model selection** is a standing `/research` ticket, re-run periodically rather than
decided once.

**Read before building the research agent**, per `reference/repository-inventory.md`:
R-M06-07 (governed LLM context assembly — retrieved material is data, never instruction) and
R-M06-03 (website and feed capture). Both answer questions the tool surface will raise, and
both were paid for already.

**Use `/prototype` for one thing only:** the composer. "How should a rendition be produced
and reviewed" is a how-should-it-behave question, which is what prototype is for — a
throwaway runnable artifact to react to, not a design document.

---

## Phase 2 — Spec and Slice

Once fog is down to non-blocking:

1. **`/to-spec`** — turns the accumulated conversation into a spec and publishes it to the
   tracker. Run this once, for the destination, not per-feature.
2. **`/to-tickets`** — breaks the spec into tracer-bullet tickets with declared blocking
   edges. "Tracer bullet" is the operative phrase: each ticket should cut through every layer
   thinly rather than completing one layer fully. That is the direct antidote to v1's
   horizontal seam failures — a tracer-bullet ticket crosses the seam by construction, so
   drift between the UI's contract and the service's contract cannot accumulate silently
   across a phase closure.

**Do not run `/wayfinder`.** It is the right tool for work too big for one session wrapped in
fog — which described this reboot before the grilling session, and no longer does. The
destination is named, seven decisions are locked, and the remaining fog is explicitly
non-blocking. Charting a decision-ticket map now would be building the planning apparatus
that killed v1, in a lighter wrapper. If a later effort genuinely arrives foggy — the
long-form rendition model, or Release Watch — chart that one then.

---

## Phase 3 — Build

- **`/implement`** drives the work described by the spec or tickets, invoking `/tdd` at
  pre-agreed seams and closing with `/code-review` before committing. It is deliberately thin
  — around five lines — and leans on the agent's priors and on `AGENTS.md`. **The skill is
  not doing the work; `AGENTS.md` is.** Keep the commands block there accurate, or
  `/implement` will guess at how to typecheck, test, and run.
- **`/tdd`** builds one vertical slice at a time. **As of skills v1.1 the loop is red →
  green only** — refactor moved out of TDD and into `/code-review`. `/tdd` is now reference
  material rather than an interactive step-confirm skill, specifically so an unattended agent
  can be handed it and work. An implementer refactoring mid-implement is doing it wrong.
- **`/code-review`** runs two axes as parallel subagents: **Standards** (repo conventions
  plus a Fowler smell baseline) and **Spec** (does it faithfully implement the originating
  issue). The Spec axis is the one that matters for this project — v1's defects were
  implementations that drifted from their contract, not code that smelled. Refactoring now
  lands here too.
- **`/handoff`** writes up a long session so another agent can continue it. With Grok
  implementing and other models reviewing, this is the rotation mechanism — use it at the end
  of any session that produced state a successor needs.
- **`/codebase-design`** supplies the deep-module vocabulary: small interfaces, clean seams,
  testable through the interface. Worth reading once before the first module boundary is
  drawn, not applied ceremonially.

**Where the v1 audit discipline goes.** Do not rebuild the milestone/correction-package
machinery. The genuinely valuable part was the six standing checks in
`audit-baseline-checks.md` — vocabulary reconciliation, fail-open inventory, destructive-
write inventory, dead-capability inventory, write-once inventory, projection completeness.
These now live in **`codingstandards.md` in the code repository**, which is what the
`/code-review` Standards axis reads — a standard, not an audit ceremony. Check one,
vocabulary reconciliation, is largely obviated by maintaining `CONTEXT.md`, which is the
point.

---

## Ongoing — The Two Habits

**`/domain-modeling` runs continuously, not as a phase.** Any session where a term is
challenged, sharpened, or resolved updates `CONTEXT.md` inline, immediately, not batched. The
skill explicitly cross-references code against stated behavior and surfaces contradictions —
that is the vocabulary drift check running as a habit instead of an audit.

**`/grilling` is the default for any real decision.** One question at a time, recommended
answer supplied, no action until shared understanding is confirmed. It looks up facts itself
and reserves decisions for the human — the same division this project is built on.

---

## What Success Looks Like at Phase 3 Entry

```
/
├── AGENTS.md                      project, doc pointers, hard constraints, commands
├── CONTEXT.md                     ~21 terms, opinionated
├── codingstandards.md             fixed conventions, six seam checks, smells
├── docs/adr/                      10 files, one page each
├── docs/agents/                   tracker and domain conventions
├── .scratch/first-destination/    one spec, 14 tracer-bullet tickets
└── src/
```

If that tree has grown a `governance/`, a `milestones/`, or a numbered decision log before
the first thread is published, the failure has recurred and the correct response is to stop
and delete, not to reconcile.

---

## Independent Review

Preserve the v1 pattern that worked: one agent implements with the repo as memory; other
models do high-stakes independent adversarial review. **Grok Build implements. Claude and GPT
review through filesystem MCP access, from outside the implementing session.**

Adapt the trigger. v1 reviewed at milestone boundaries, which is why reviews arrived after
seams had already drifted across a phase closure. v2 has no milestones. Trigger review on
**the first end-to-end pass through a newly crossed seam** — the first time a claim travels
capture → Record → angle → rendition → clearance, and again the first time a correction
travels backward from published material to a confirmed claim. **Ticket 01 is also a review
point**, not because the code is risky but because whatever shape it takes becomes the house
style, and changing it costs more with every subsequent ticket.

### The asymmetry problem, and how it is resolved

`/implement` now calls `/code-review` itself before committing. Left alone, that means the
implementing agent is briefed on both review axes, and the information asymmetry that made v1
audits valuable disappears.

**Split the axes:**

- **Standards runs in-loop.** It is mechanical — repo conventions, seam checks, smells — and
  an implementer building against `codingstandards.md` is a feature. Asymmetry buys nothing
  here.
- **Spec runs out-of-loop and unbriefed.** Does this faithfully implement the ticket's
  contract? That pass is run by a reviewing model that was not in the implementing session,
  against criteria the implementer has not seen.

**What may be public:** the six standing seam checks. They are in `codingstandards.md`
deliberately.

**What must not be written down anywhere:** diff-specific review criteria — what a reviewer
intends to probe on a particular change. Not in the repo, and not in a file on the machine if
an implementing agent runs with filesystem access. If the boundary is not enforced by
separate Unix users, the only reliable boundary is that the file does not exist.
