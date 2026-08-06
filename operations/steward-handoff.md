# Steward Handoff — Project Review Context

**Written:** 2026-08-05, at the end of the session that reviewed tickets 01–08.
**For:** the next Claude session acting as project steward and seam reviewer.

This holds what lives in conversation rather than on disk. Everything else — doctrine,
decisions, findings, conventions — is already in the repositories and should be read
there, not summarised here.

---

## Who does what

| Role | Who | Scope |
|---|---|---|
| Operator | **Chaz** (VedaOps) | All authority. Relays every message between agents |
| Implementer | **Grok Build** | Writes all code. Never talks to reviewers directly |
| Spec review | **GPT** | Acceptance criteria, ADR fidelity, vocabulary, scope. Findings prefixed `S-` |
| Seam review + steward | **Claude** (you) | Six standing checks plus adversarial read. Findings prefixed `F-` |

Address Chaz by name. Mark anything not meant for relay as **[CHAZ]** or **[GPT]**.
Prompt blocks for the implementer are labelled **PROMPT FOR GROK**.

---

## The routing, and the one rule that matters

1. Grok reports → Chaz sends it to **you and GPT separately**
2. You each review independently
3. Chaz sends GPT's findings to **you**, after you have responded
4. You merge both into **one** prompt for Grok

**Only your prompts reach Grok.** Two voices to one implementer creates reconciliation
work it should not be doing. Do not attribute findings to GPT in Grok-facing text —
attribution is noise in an instruction.

**Step 3 must come after step 2.** It was collapsed twice during tickets 06 and 07, and
both times the second independent pass was lost. If you are handed both at once, say so
and review the code yourself before reading the other findings.

**Never write diff-specific review criteria to disk.** The six standing checks are
public in `codingstandards.md` deliberately — an implementer building against them is a
feature. What you intend to probe on a *particular* change stays in conversation. The
implementing agent has filesystem access; a file is not a boundary.

---

## How to review

Read the code. Not the report. Reports have been accurate but they are summaries, and
at least one arrived truncated. You have filesystem access through the `workbench` MCP
server — use `workbench_search` to find the function, then read it.

**Run things, do not only read them.** `workbench_run_command` executes allowlisted
Git/Python/uv commands — `uv run pytest` works, and so does running a single test file.
The spec reviewer found the ticket 10 migration blocker by building a populated database
and upgrading it; the seam reviewer read the same migration, confirmed its shape was
correct, and missed it. Reading finds contract drift between paths. Running finds the
things that only fail when data exists. Do both, and never report a test count that came
from the implementer.

Run the six checks from `codingstandards.md` and **state each result explicitly**,
including the clean ones. A silent clean pass is indistinguishable from a check that
was never run.

**The failure shape this implementer has, consistently:** boundaries that hold on one
path and not the parallel one. `get_case` unscoped while `list_cases` was scoped.
Vocabulary reconciliation covering one direction. Pragmas set but unverified. `examined`
reported on close but inferred by a `WHERE` clause. It builds each path correctly and
does not check the paths against each other — which is precisely what a fresh reader
catches and an implementer structurally cannot.

**Say what held, not only what failed.** It has repeatedly built things nobody asked
for — the conditional update in `claim_next_run`, `CAPTURE_WRONG_CASE`, rejecting
empty regions in the locator range guard. Naming those keeps the instinct alive.

---

## Where things stand

**Done, committed, pushed on `main`:** tickets 01–11 (including 09a, 10a). F-03 closed
in 10a; F-24 closed in 11 (D21). Docs repo carries D18–D21 and object-backed D20 coverage.

**Next:** ticket 12, rendition composition. Open with the two deferred shelf questions
(angle-scoped vs case-scoped; whole-element locators) — decide at start, not at review.

**Remaining:** 12 rendition composition → 13 rendition approval → 14 publication recording →
**15 capture acquisition receipt** → **16 rubric artifacts** → **17 the Vela run**.

---

## Open obligations

**No findings from tickets 01–11 are open.** F-24 closed in ticket 11 (D21) after ten
tickets — it took three rounds, because the same laundering wore three faces: a severity
ladder with `unknown` mis-ranked, a check bound to the proposed rather than confirmed value,
and an unconstrained kind boundary at confirmation. The generalisation is in D21 and worth
carrying: fixing which value a check reads does not help unless something constrains what
that value may become.

**Ticket 12 opens with two deferred questions**, both raised by the implementer and both
given stated triggers rather than left vague:

- The quotation shelf is case-scoped, not angle-scoped. VISION puts it inside the Angle Room,
  which argues for per-angle; nothing forced the question until renditions consume it.
- Shelf entries may be whole-element locators. A region-form requirement was considered twice
  and rejected as ceremony (see ticket 11's amended criterion). If block-granularity entries
  produce bad renditions, that is evidence and the decision reopens.

Decide both at ticket 12's start rather than discovering them at its review.

**F-03 closed in ticket 10a** — bidirectional API route ↔ `api_operation_names` checks that
name offenders. MCP still fails closed at startup against `mcp_tool_names()`. Keep both
directions when adding routes.

**Cross-operation interaction tests exist (ticket 10a)** and are a standing rule in
`codingstandards.md`. Every real defect in this project has been operation A changing what
operation B reports — extend the suite when new pairs share state.

**Ticket 15 gates the Vela run.** The capture receipt cannot be backfilled. Do not run Vela
with a thin one — that is the run the whole architecture exists to compare against v1, and
its provenance is the comparison.

**D19 is reopenable on one condition.** Soft `200 OK` walls are captured as ordinary
material and an operator "not usable" mark was rejected because `material_status` does not
reach `propose_claim`, `close_run`, or `attach_lead` — enforcement would need four sites,
which is the D17 shape. If a single authoritative enforcement point for capture use ever
exists, revisit. Absent that, do not relitigate; the reasoning and both rejected
alternatives are in `decisions/lead-material-admission.md`.

---

## Standing rules for the steward

**Governance must not outrun execution.** The previous build reached 309 documents
across 131 planning packages and 99 audit records and never published a post from the
system it documented. If you start proposing artifacts faster than the code moves, that
is the failure recurring. Chaz has standing permission to call it.

**A document earns its place by being read.** The review backfill and D18 were written
because commit messages pointed at nothing and a real ambiguity would otherwise have
been resolved silently. Neither was routine.

**Write review files per ticket, going forward.** 01–08 exist. Do not let them
accumulate as a backlog again.

**Decisions get recorded where they bind.** ADRs in the code repo for what constrains
code; `decisions/` in the docs repo for the fuller reasoning with rejected alternatives.
The rejected alternative is the part that stops re-litigation.

---

## Working notes

Chaz runs Claude Code and Grok on a subscription; this chat interface is metered. Prefer
deciding here and building there.

Grok's context fills after roughly two tickets. `/handoff` at session end, then the seat
prompt in `docs/agents/seat-prompt.md` for the next one.

The MCP SDK is `mcp==2.0.0`, pinned. `FastMCP` was renamed `MCPServer` on 2026-07-28;
the old import path is removed, not deprecated.

Machine is `pop-os`, Python 3.12.3 — which matters, because the `ipaddress` IPv4-mapped
fix landed in 3.12.4 and the SSRF guard normalises around it rather than depending on it
(F-18).
