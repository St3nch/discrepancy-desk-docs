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

**Done, committed, pushed on `main`:** tickets 01–09a. Foundation `4037bd6` (01–06),
`65bcc27` (07), `c53e240` (08), handoff `d1190af`, ticket 09 `ed1f134`, review backfill
`8bf7f3a`, ticket 09 narrowings and docs `f72e671`, ticket 09a `638fe1b`. Docs repo:
D18 and D19 at `41f4220`.

**Ticket 09a was pushed before review** — it reviewed clean, but the convention is one
commit after review passes. Push implementation commits after the review, not with the
preceding document batch.

**Next:** ticket 10, coverage gauge. It owes the official-foundation gate — angle work
hard-blocked until coverage reads complete.

**Remaining:** 10 coverage gauge → 10a interaction tests and F-03 → 11 Angle Room and claim
confirmation → 12 rendition composition → 13 rendition approval → 14 publication recording →
**15 capture acquisition receipt** → **16 rubric artifacts** → **17 the Vela run**.

---

## Open obligations

**F-24 is the only finding from tickets 01–08 still open.** Inference claims do not inherit
publication risk from the claims they cite, so an inference over a `living_private` claim can
be recorded `not_applicable`, laundering the risk one level up. Noted in the `claims.py`
docstring. **Must close in ticket 11**, before confirmation and use paths exist.

**Ticket 11 also owes the quotation shelf** the region locators built in ticket 05.

**Ticket 10 owes the official-foundation gate** — angle work hard-blocked until
coverage reads complete.

**F-03 is open and has been since ticket 01.** `api_operation_names()` has no call site.
MCP registration is verified at startup against `mcp_tool_names()`, so the safety-critical
direction holds — a human-authority operation on MCP fails the app. The API side has no
equivalent: nothing detects a route added for an operation absent from `wiring.py`, or an
`API_ONLY` entry with no route. `API_ONLY` is now sixteen entries of decorative registry.
Close it with a test or delete the function; an unused registry that looks enforced is
worse than none.

**Cross-operation interaction tests do not exist.** Every real defect in this project has
been operation A changing what operation B reports — F-07, F-25b, F-34, F-38 — and every one
was found by a reviewer reading two files side by side. Worth a deliberate handful (attach
a lead then close a run reporting it; attest then attach then read the gauge; cancel a run
then read its captures' status) rather than a sweep. Now a standing rule in
`codingstandards.md`.

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
