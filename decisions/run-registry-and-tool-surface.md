# Run Registry and Tool Surface

D12 through D15. Output of the grilling session that closed the two blocking fog
items identified after D11.

These decisions specify the runtime mechanics that D8 (backend owns runs; the
executor is swappable) left open. Together with `architecture-decisions.md` they
are the complete input to the first specification.

---

## D12 — Runs are claimed, not assigned

Runs sit in the registry with status `approved`. An executor calls
`claim_next_run()` and receives the oldest approved run; the status moves to
`claimed`. The backend never reaches out to an executor.

**Rejected — push, where a run is dispatched to a named executor:** the backend
would need to know what executors exist, how to reach them, and what to do when
one is not listening. Pull keeps the executor anonymous, which is what makes it
swappable — a desktop chat client, an API-driven agent, or anything else is
identical from the registry's side.

**Accepted cost:** a run cannot be targeted at a particular executor. If bulk
extraction should go to a cheaper model and angle work to a stronger one, pull
cannot express that. Fixable later with an optional capability tag on the run and
a filter on the claim; deliberately left out for now.

### Run states

```
draft        question written, not yet approved
approved     claimable
claimed      an executor holds it and is working
suspended    the executor asked a question; waiting on the human
complete     closed normally, findings recorded
abandoned    claimed but never closed; reclaimable
cancelled    the human killed it
```

### Abandonment is lease-based

An executor can simply stop — tab closed, session ended, model gave up — leaving
a run `claimed` forever with nothing to signal it.

A claim therefore carries a lease. The executor's tool calls refresh it. If
nothing touches the run for the lease period it reverts to `approved` and becomes
claimable again.

**Partial work is preserved, not rolled back.** Captures already made stay: they
are bound to the run and they are real material. Proposed claims stay for the same
reason. The next executor to claim the run picks up with work already partly done.

This means a single run may be worked by more than one executor across its life.
That is accepted deliberately rather than tolerated — the alternative is
discarding real captured material because a chat session ended.

### Concurrency

**Serialized by default.** One claimable run per case at a time.

The arguments for allowing concurrency are real but do not apply at this scale:
duplicate capture of the same URL is *correct* under capture-then-cite, since two
fetches at different times are two captures and any difference between them is
itself evidence; duplicate claims are absorbed by the confirmation step, which is
where a human looks at them anyway.

But the destination is one case, one run at a time, and the executor is a chat
client — concurrency would mean running two chat sessions against one case, which
is possible but unlikely. Serialize because it is simpler. Relax it if it chafes.

**Note on scope:** development-time agents (multiple coding agents building the
Desk) and runtime runs (research jobs the finished product dispatches) are
separate layers. Concurrency here concerns runtime only.

---

## D13 — Run close leads with the agenda

When a run closes, the screen presents in this order:

1. **The agenda** — new open questions, which the agent proposes pursuing, why, and
   the scope it would give each. Approve, reject, edit, or write your own.
2. **What the run did** — captures made, claims proposed. Counts, not contents.
   Enough to judge whether the run worked hard or barely moved.
3. **Self-reported low confidence** — where the agent was unsure and where it felt
   the rubric underserved it.
4. **Everything else** — claims and captures, behind a fold.

**Rejected — leading with the claims:** it feels natural and it is wrong. Claims
enter unconfirmed and stay that way until an angle pulls them in (D4), so a screen
opening with forty claims presents work the operator is not supposed to do yet.

**The agenda is the only decision run close actually requires.** Nothing else
happens until the next question is chosen. Leading with it makes the screen match
the decision.

**Second reason:** the agent's rationale is perishable. Reasoning about material it
just read is most useful while that context is fresh; buried under claims, it gets
skimmed.

**Deliberate friction:** claim review must *not* feel available at run close. If it
is one click away it will happen, and claims will be confirmed with no angle in
mind — the same vacuum problem that made extraction-on-drop wrong for leads (D10).
Confirmation needs a purpose to judge against.

---

## D14 — `propose_claim` requires byte-exact quotation

```
propose_claim(
  run_id,
  proposition,      the claim in the agent's words
  capture_id,       which capture supports it
  locator,          where in that capture
  quoted_text,      exact bytes from the source
  dimensions,       the six, as proposals
  qualification,    required language for any use
)
```

Verification runs in order and every step fails closed:

1. `capture_id` exists and belongs to this run's case
2. `locator` resolves inside that capture's element structure
3. `quoted_text` appears **byte-exact** at that locator
4. `dimensions` are all present and are valid enum values
5. `qualification` is non-empty when posture is `allegation` or
   `participant_account`

**Step 3 is the load-bearing one.** Without required quotation, verification reduces
to "does this capture exist," which a model satisfies by citing anything it
fetched. With it, verification is "do these exact words appear at this position,"
which a model cannot satisfy by confabulating. That is the difference between a
check and a formality, and it is the single mechanism that makes an untrusted
executor safe.

This is stricter than the previous project, where a claim linked to a source and
the link was the whole verification.

**Two escape valves, both allowed:**

- **Multiple locator and quote pairs per claim**, for propositions resting on more
  than one passage.
- **Inference claims cite claims, not captures.** A claim with posture
  `desk_inference` — "the report contradicts itself across sections four and
  nine" — is not a quotation and cannot be one. It cites the claims it reasons
  over, each of which is itself quote-bound.

Without these, summarising and reasoning claims would be unexpressible and the
rule would be routed around rather than followed.

---

## D15 — `capture_url` returns the locator map

The response carries the capture ID, the parsed elements with their locators, and
the text of each, up to a size cap. **The agent quotes from that response**, never
from its own reading of the page.

**Rejected — returning the capture ID alone:** the agent would then read the page
through its own fetch, which means reading *different bytes* than the backend
stored — different moment, possibly different content, certainly different
parsing. It would cite what it saw against what the backend holds, and the
mismatch would stay invisible until verification failed.

**Rejected — returning extracted text without locators:** the agent would be
guessing at both the locator and the exact quotation demanded by D14. Locators
would not resolve, whitespace-normalised quotes would not match, every claim would
fail, and the run would stall while the agent retried blind.

Returning the locator map is therefore not a convenience. It is what makes D14's
enforcement workable rather than merely strict: quotes match by construction
because the agent is quoting the stored bytes.

**Size cap and deep reading.** A large capture would flood the executor's context,
so the response is capped and a separate `read_capture(capture_id, range)` tool
exists for going further into a capture already made.

The separate tool is preferred over automatic pagination because it keeps
`capture_url` cheap and makes "the agent chose to read further into this document"
a visible, recorded act rather than something that happens silently.

---

## The Tool Surface

Eight calls. This is the complete interface between any executor and the backend,
and the only path by which anything enters the Vault or the Record.

| Call | Purpose |
|---|---|
| `claim_next_run()` | Returns question, scope, rubric version and text |
| `read_case_context(case_id)` | Prior claims, open questions, existing sources |
| `capture_url(url)` | Fetches, hashes, parses, stores; returns capture ID and locator map |
| `read_capture(capture_id, range)` | Deeper into a capture already made |
| `propose_claim(...)` | Five-step verification, fail-closed (D14) |
| `suspend_run(run_id, question, ...)` | Ask the human mid-flight; run state becomes `suspended` |
| `close_run(run_id, questions, ...)` | Agenda, self-reported low-confidence areas |
| `add_lead(url, note)` | Inbox drop; captures immediately, no claims (D10) |

**Budget enforcement lives with `capture_url`**, which counts against the run's cap
and begins refusing once exhausted. The executor cannot overspend because it is not
the one spending.

**The refusals are the product's real boundary.** Every one of them must hold
against an executor assumed to be untrusted, because under D8 that is exactly what
an executor is.
