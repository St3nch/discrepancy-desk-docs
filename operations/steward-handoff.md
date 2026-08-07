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

**Done, committed, pushed on `main`:** tickets 01–12a (including 09a, 10a). F-03 closed
in 10a; F-24 closed in 11 (D21); composition object-backed in 12; refusal invariant and
`find_quote` in 12a (F-55, F-60 closed). Docs repo carries D18–D21 and object-backed D20
(composition via renditions; `story_intelligence` still unmeasurable by decision).

**Next:** ticket 13, rendition approval. Issue file is still the thin draft until amended.
Opens with the exact-content binding constraint (below). Do not start until the issue is
amended; brief both review axes with the implementer.

**Remaining:** 13 rendition approval → 14 publication recording →
**15 capture acquisition receipt** → **16 rubric artifacts** → **17 the Vela run**.

---

## Open obligations

**No findings from tickets 01–12a are open** except deferred design (F-57) and recorded
no-action items (F-59). F-24 closed in ticket 11 (D21) after ten tickets — it took three
rounds, because the same laundering wore three faces: a severity ladder with `unknown`
mis-ranked, a check bound to the proposed rather than confirmed value, and an unconstrained
kind boundary at confirmation. The generalisation is in D21 and worth carrying: fixing which
value a check reads does not help unless something constrains what that value may become.

**F-57 needs designing before Vela — deferred, with the design question open.** Two
independent primary documents can assert the same thing, and there is no vocabulary for the
operator to record that they corroborate each other. A live executor hit this on the first
real run: it classified each claim `single_source` — correct under D4, since each claim is
about what one document says — then said plainly it lacked the vocabulary and left the
judgement to the operator. VISION §12 reserves "decide whether sources are genuinely
independent" to the human and gives it nowhere to land.

Two views exist and they compose, but the shape is not settled:

- **The implementer's:** a first-class human-only relation between claims, with actor,
  timestamp, note, append-only when re-ruled — the `claim_confirmations` pattern. Argued
  against a field on the claim (forces asymmetric or dual writes, blurs D4's per-claim basis)
  and against a property of the angle (independence is about sources, not framing).
- **An outside review's:** the system finds candidate pairs and the operator attests, so the
  judgement stays human while the finding is automated.

**Two questions to settle before sizing.** Is independence a property of a *pair*, or of a
pair *for a given proposition*? Two documents can be independent about one fact and share a
source on another — a wire report quoting an agency statement. Per-pair is simple and
sometimes wrong; per-proposition is honest and reintroduces the volume problem the
confirmation surface was designed around. And does an independence ruling feed back into the
claim's `corroboration` dimension, or stay separate? Separate is correct on D4 grounds, but
then the ruling lives somewhere nobody looks — F-44's shape.

Note also that candidate-finding cannot use semantic matching. "Same proposition family"
needs embeddings or a model call in the backend, and backend-as-LLM-client was refused at
ticket 09 and again at ticket 12; semantic retrieval is deferred until a governed corpus and
a measured retrieval need exist. Structural signals only — claims on the same angle citing
captures from distinct domains.

**F-55 closed in ticket 12a.** Quotation-surface conventions were learnable only by failing
once; `find_quote` makes the vault self-describing on exact substring → `e/{n}/r/{start}-{end}`
(or structured miss). `propose_claim` still verifies independently. No fuzzy matching.

**F-56 written into ticket 16's criteria** (not closed until 16 ships). Classification
vocabulary invisible to the executor — enums and a one-line meaning each, plus one canonical
worked example claim, so cold-start does not depend on case history. Origin was the live
run pattern-matching confirmed claims.

**F-60 closed in ticket 12a.** The refusal invariant sat inside the framework on the first
pass (body decorator only; tests called `tool.fn`). Round two intercepts at
`ToolManager.call_tool`: three categories — domain `DeskRefusal`, `TOOL_ARGUMENT_INVALID`
(actionable framework validation), `TOOL_INTERNAL_ERROR` (non-correctable). Discriminates on
`ToolError.__cause__`, not string-matching. Coding standards carry the three-category
contract.

**Live-model wrong-type smoke sits with the operator.** Both 12a axes agree it is not an
acceptance criterion. The automated suite shows `TOOL_ARGUMENT_INVALID` is well-formed; only
a live session shows whether a real executor reads it and corrects. Do before relying on the
invariant in production use.

**F-59 is recorded, no action.** `DEFAULT_CAPTURE_BUDGET` now exists in `runs.py` and
`api.ts`, with a comment in each saying it matches the other. Third instance of the
two-artifacts-one-contract shape after F-51 and F-54; low cost, worth watching.

**Ticket 13 opens with a constraint worth stating before work starts.** Approval binds exact
content — VISION §14 says the human clears the text as it will appear, and may edit before
approving, at which point the edited text is what gets bound. A boolean and a timestamp on a
draft satisfies a careless reading and breaks the first time someone edits after approval.
Do not start until the issue file is amended.

**Brief both axes, not just the implementer.** Through ticket 12 the implementer received
amended issue files and named defect classes while the spec reviewer received summaries. His
strongest findings — the populated migration, the source-basis field binding, the case-wide
claim pool — all came when he had something concrete to push on. Write two prompts per
ticket and send them together.

**F-51 closed in ticket 12** — `tests/test_client_api_paths.py` extracts literal `/api/…`
paths from `client/src/api.ts` and asserts each resolves on the router. Operator JSON.parse
symptom was a port conflict (wrong process on :8000), not a missing path; the guard landed
regardless. Same two-artifacts-one-contract class as F-54 / F-58 / F-59.

**Ticket 12 shelf questions closed on evidence.** Case-scoped shelf retained (cite
eligibility stays angle-scoped at `propose_rendition`). Whole-element locators allowed;
region form remains available. Recorded in CONTEXT and the ticket 12 review.

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
