# Capture Acquisition Receipt

D22. Decided before ticket 15 opened, after both review axes argued the shape
independently. Recorded here rather than in a review file because the question —
*why does a receipt hang off two different owners* — is one someone asks in six
months and answers wrongly from the schema alone.

D18 is the precedent: a tension between two doctrine statements that would
otherwise be resolved silently by whoever coded first.

---

## The tension

**VISION §7 describes the Vault as holding immutable originals and acquisition
receipts.** What exists is the original without the receipt. `safe_http_get`
returns body bytes and a Content-Type header; response status, final URL after
redirects, hop count, and intermediate `Location` values are discarded, and
`captures.url` records the URL the caller asked for rather than where the fetch
landed.

**Ticket 15's acceptance criteria are written entirely about captures.** Three
outcomes produce no capture row:

- `identity_only` — `safe_http_get` refuses 401/402/403; `add_lead` parks the URL
  with no bytes (D19).
- `unsupported_type` — the fetch *succeeded*; `assert_content_type_supported`
  raises before any Vault or Record write; `add_lead` parks the URL (ticket 09a).
- Hard refusals — SSRF, DNS, timeout, HTTP error, redirect limit. No lead row.

A URL that redirects through a shortener to an archive path to a login wall is
precisely where the chain is the whole content of the record — and as the criteria
were written, nothing about that acquisition would be retained.

---

## D22 — A receipt exists exactly when a durable research outcome exists

> A capture owns its receipt. An `identity_only` or `unsupported_type` lead owns
> its receipt. **An in-scope run whose acquisition reached a final response but
> produced no capture owns its receipt**, recording the source as reached and
> unusable. Hard transport and security failures — SSRF, DNS, timeout, redirect
> limit, generic HTTP error — produce no durable outcome and therefore no receipt.
> No response body is retained for any non-captured outcome.

The rule reads off the ownership constraint directly, which is what makes it
enforceable rather than aspirational.

**One receipt shape, one persisted representation, whatever the owner.** An
`acquisition_receipts` table with an ordered `acquisition_redirect_hops` child.
Ownership is expressed as nullable `capture_id`, `lead_id`, and `run_id` on the
receipt with

```sql
CHECK (
  (capture_id IS NOT NULL) + (lead_id IS NOT NULL) + (run_id IS NOT NULL) = 1
)
```

so a receipt has exactly one owner by constraint. `run_id` ownership covers only
the non-captured run outcome — a run's successful capture owns its receipt through
the capture, which already carries `run_id`.

The receipt carries an `outcome` of `captured`, `identity_only`, or
`unsupported_type`, with a CHECK binding `outcome = 'captured'` to `capture_id IS
NOT NULL` so the outcome and the owner cannot disagree. The lead projection may
hide the storage distinction; the schema may not.

**Vocabulary reconciliation, stated because this is the obvious place to get it
wrong.** `leads.material_status` already spells these states `captured |
identity_only | unsupported_type`. The receipt uses **the same three tokens**, not
the refusal code's `auth_walled` spelling. Two vocabularies for one product state
is the defect class the seam checks put first, and inventing a second spelling here
would be that defect authored deliberately.

### The recorded fields

```
requested_url
final_url
final_status
response_content_type_header    nullable — the exact value received, or NULL if absent
acquired_at

redirect_hops[]:
    ordinal
    from_url
    status
    to_url
```

`redirected` is **derived from a non-empty hop list**, never from comparing
`requested_url` to `final_url` — a chain can land back where it started.

`from_url` / `status` / `to_url` rather than a bare hop URL and status: it states
what happened and preserves the effective destination that was subsequently
SSRF-validated. The raw `Location` header is not retained; the effective target is
the provenance that matters.

A missing Content-Type header is preserved as NULL. It is **not** recorded as
`application/octet-stream`, which is the fetch layer's fallback and not something
the server said.

---

## Why the receipt is a separate table rather than columns on `captures`

The question was argued the other way first and the answer turned out to have a
consequence neither axis anticipated.

**`captures` needs no new columns.** No `ALTER TABLE`, and no create-copy-drop-
rename rebuild. That matters because `captures` carries four inbound foreign key
references — `document_versions`, `leads`, `quotation_shelf_entries`, and
`claim_quote_bindings` — and a rebuild against a populated database is the exact
shape of the ticket 10 migration blocker that `codingstandards.md` now carries a
standing rule about.

The shape argued for on shape grounds also removed the migration risk. Worth
noting, because the reasoning ran the other way: the receipt was made a separate
owned object because two fidelities were wrong, and the migration benefit was
discovered afterwards rather than used as the argument.

---

## Rejected — full receipt on captures, thin receipt on non-captured leads

The seam axis's first proposal: final URL and status only for `identity_only` and
`unsupported_type`, on the grounds that a parked wall does not need five hops of
provenance.

**Rejected because it creates two receipt schemas with different fidelity, and
attaches the thinner one to the cases where the receipt is the only material there
is.** A captured lead has bytes, elements, a hash, and a locator map; the receipt
is one more fact about it. A non-captured lead has the receipt and nothing else.
Giving the richer record to the case that needs it least inverts the decision.

It also creates a second loader, a second vocabulary, and two places for the field
set to evolve — the two-artifacts-one-contract shape that has recurred as F-51,
F-54, and F-59.

## D22 — `capture_url` returns a non-capture outcome rather than refusing

When an acquisition reaches a final response that cannot become a capture — an
unsupported content type, or an auth wall — `capture_url` returns a structured
outcome instead of raising:

```
requested_url / final_url / final_status / redirected
outcome     = unsupported_type | identity_only
capture_id  = null
receipt_id  = <id>
```

**This is still fail-closed, and the enforcement is unchanged.** There is no
`capture_id`, so `propose_claim` has nothing to cite. The executor cannot turn an
unreadable PDF into evidence however it responds to this.

It also fits the standing refusal rule better than a refusal does.
`codingstandards.md` requires a refusal `code` specific enough that an executor can
self-correct. `CAPTURE_UNSUPPORTED_TYPE` is not correctable — no retry makes a PDF
parseable — so it is terminal information rather than an instruction, which is what
the outcome shape expresses honestly.

**Auth walls on the run path get the same treatment, stated explicitly rather than
carried in on the coat-tails of unsupported types.** "We reached this source and
access was blocked" is the same research-history fact, and the lead path already
records it. The `CaptureAuthWalled` receipt is what makes it available.

**Two constraints on the implementation.** The result type must not let a caller
read `capture_id` without confronting `outcome` — an optional field silently read as
absent is how this becomes fail-open later. And a non-capture outcome does not
consume capture budget (F-15 counts retained captures), which means a run can write
receipt rows without charging anything; that is the same unbounded-write shape as
`add_lead`'s outstanding per-run cap TODO, and it is recorded here rather than
solved.

## Rejected — recording the run outcome by writing a row and then refusing

The seam reviewer's first proposal for F-69: `capture_url` inserts a durable row for
an unsupported type, then raises `CAPTURE_UNSUPPORTED_TYPE` as it does today.

**It does not work, and the reason is worth keeping.** `connection_scope` is
`engine.begin()` — it commits on success and rolls back when an exception escapes.
The insert would be rolled back by the refusal it was written alongside, leaving
exactly the trace it existed to create. The `add_lead` path only works because it
*catches* the refusal and does not re-raise.

The generalisation: **in this codebase a durable record and an escaping refusal are
mutually exclusive within one service call.** Any future design that wants to record
something and still refuse must either catch the refusal or return an outcome.

## Rejected — telling the executor to call `add_lead` instead

Amending the `CAPTURE_UNSUPPORTED_TYPE` refusal to point at `add_lead`, which parks
the URL and does not charge budget.

**Rejected because it confuses two domain objects to avoid adding the one state
actually needed.** D10 gives `lead` a specific meaning: ambient material, unattached
to a case, encountered *outside* a run's question. An executor calling `capture_url`
on the 1980 NRL memo while working Vela's official foundation has not found a lead.
Routing it through the inbox replaces the lineage that matters —

```
run → tried this source → could not obtain a quotable capture
```

— with

```
unattached lead → somebody happened to find this URL
```

Those are different statements, and only the first explains a research gap.

## Rejected — leaving the run path silent (the status quo)

The run path refuses and records nothing, while the lead path parks the URL
(ticket 09a).

**Rejected because it is the parallel-path defect in its standard form**, and because
two doctrine commitments depend on the Record knowing what a run tried to read. D3's
second argument for capturing everything read is an honest corpus denominator — "6 of
74 eligible documents" is unsayable if the 68 left no trace. And absence claims are
bounded by the corpus actually worked; a run cannot honestly report what it worked if
unreadable sources vanish.

**The second-order failure is the one that would bite.** An executor refused on a
source does not stop — it routes around, citing a secondary source that discusses the
document instead. The resulting claim carries a weaker source basis, and the operator
confirming it days later cannot see that the downgrade came from a parser limit rather
than an editorial judgement. The refusal is honest at the seam and invisible
downstream.

## Rejected — a receipt for every fetch attempt

Persisting receipts for SSRF blocks, DNS failures, timeouts, redirect-limit
aborts, and HTTP errors. **Rejected:** those produce no durable material state, and
ticket 09a's boundary is explicit that they produce no lead row. Retaining them
turns ticket 15 into an acquisition-attempt log, which is a different feature with
its own retention and privacy questions.

## Rejected — a structured payload field on `DeskRefusal`

`safe_http_get` refuses 401/402/403 before `add_lead` can see the response, and
`DeskRefusal` carries five strings and nothing else. Adding a general payload slot
would widen a contract fourteen tickets depend on being narrow, and would invite
every future caller to smuggle data through refusals.

## Rejected — returning 401/402/403 as an ordinary fetch result

Let `safe_http_get` return the receipt and have callers decide. **Rejected
firmly:** it converts a fail-closed path to a fail-open one. `capture_url` would
then have to *remember* to reject the status, and a forgotten check retains a
login-wall body as ordinary captured material — the outcome D10 exists to prevent,
reached by a longer path.

---

## D22 — `CaptureAuthWalled` carries the receipt across the exception boundary

A `CaptureAuthWalled(DeskRefusal)` subclass holding an immutable `AcquisitionReceipt`
attribute. `code` stays `CAPTURE_AUTH_WALLED`; inherited `as_dict()` emits exactly
the five governed strings; both transports render what they render today; the run
capture path still fails closed.

**This is not a sixth refusal field and must not become one.** The subclass is
tested for `as_dict()` key equality with an ordinary `DeskRefusal`, and the MCP run
capture path is tested to render `CAPTURE_AUTH_WALLED` identically. Private
structured exception state that is never rendered is the whole of the permission
being granted here.

**The narrow justification, stated precisely:** `CAPTURE_AUTH_WALLED` is the only
*fetch-layer* refusal whose acquisition receipt must cross an exception boundary
into a durable product state. `CAPTURE_UNSUPPORTED_TYPE` is also a durable lead
state, but its fetch already succeeded and the receipt is still in local scope when
parsing refuses — it needs no mechanism at all.

---

## D22 — The executor sees final source identity, not the chain

`capture_url` returns `final_url`, `final_status`, and `redirected` alongside the
existing fields, with `url` keeping its meaning as the requested URL.
`read_capture` returns `requested_url` as well as those three, because an executor
reading further into a capture may be many tool calls from the response that made
it and should not have to carry source identity in conversation state.

**The reason is evidentiary, not ergonomic.** Quotation verification proves the
words occur in capture N. It does not prove the executor's proposition correctly
identifies the document whose words they are. A silent redirect is the one failure
mode producing a claim that passes every check while being wrong about its own
source, and the backend already knows.

**This does not touch D8.** The receipt is backend-recorded either way; showing it
to the executor moves no state into the conversation and confers no authority.

**The redirect chain is not returned over MCP.** It is durable provenance for the
operator and the Record; repeating it in model context on every capture spends the
budget D15 protected when it split `read_capture` out. A boolean plus final
identity is cheap and sufficient.

---

## Forward constraint — F-57

Any later structural source-origin signal must use the capture's `final_url`, not
the requested URL. Two captures redirecting to the same syndication host are not
independent and look independent against the requested URL.

D22 records the fact. **F-57 remains responsible for defining domain
normalisation, and for keeping domain difference a candidate signal rather than an
independence judgement.** Distinct final domains are not proof of independent
sourcing — syndication, common witnesses, common datasets, and copied reporting all
survive it — and two genuinely independent documents can share a host. The human
still decides independence (VISION §12).

D22 defines no domain semantics and builds no candidate finder.

---

## Consequences

**`captures.content_type` is a parser verdict, not an acquisition fact (F-66).**
`assert_content_type_supported` normalises the type, strips parameters, and sniffs
bytes — returning the literal `text/html` when the server sent something else and
the first 200 bytes look like HTML. The column keeps that meaning and the glossary
says so. The exact received header lives on the receipt, where it can be compared
against the verdict.

**The stripped charset parameter is a quotation-surface defect, not only missing
provenance (F-67).** `parse_bytes` decodes with `errors="replace"` unconditionally,
so a non-UTF-8 page yields U+FFFD in `elements.text` — which is the surface
`propose_claim` verifies against. Both sides of the comparison are corrupt, so
byte-exact verification passes and the corruption survives to publication.
**Scheduled as ticket 15a**, after 15 and before 16. D22 does not fix it; recording
the exact header is what makes it diagnosable.

**`CONTEXT.md`'s Capture entry is amended in the same commit** — receipt fields,
and the parser-verdict meaning of `content_type`.
