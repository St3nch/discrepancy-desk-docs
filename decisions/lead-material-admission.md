# Lead Material Admission

D19. Decided at ticket 09 review, after the auth-wall acceptance criterion was found
to be met for hard status-code walls and unmet for soft `200 OK` walls. Argued across
three review rounds by both reviewers and the implementer; recorded here rather than
in a review file because a review file is not where someone looks before dropping a
Medium link.

---

## The tension

**D10 says auth-walled and paywalled URLs are recorded as identity-only leads,
explicitly marked as not captured.** The reason given is that storing a login wall as
though it were an artifact is worse than storing nothing, because it masquerades as
evidence.

**The fetch path can only see status codes.** `safe_http_get` refuses `401`, `402`,
and `403` with `CAPTURE_AUTH_WALLED`, and `add_lead` turns that into an identity-only
lead holding no bytes. A login or subscription wall served as `200 OK` with HTML is
indistinguishable from ordinary material at that layer and is captured normally.

So the criterion as written is met for one class of wall and not the other, and the
question is whether to build detection, build an operator mark, or narrow the
criterion.

---

## D19 — `identity_only` covers status-code walls; soft walls are captured

`identity_only` is triggered by HTTP response status alone — `401`, `402`, `403`.
Those responses carry no material, so there is nothing to capture and recording the
URL without bytes is the honest outcome.

**A soft wall returning `200 OK` is retained as ordinary captured material.** Nothing
in the Record marks it as a wall, and no automatic or operator-driven mechanism
attempts to.

---

## Rejected — automatic wall detection

A content or header heuristic deciding whether a `200` response is a wall.

**The signals are already discarded.** `safe_http_get` returns body bytes and the
Content-Type header and nothing else. Response status, other headers, hop count,
intermediate `Location` values, and the final URL after redirects are all dropped;
`captures.url` records the URL the caller asked for, not where the fetch landed. A
detector would first require changing what capture retains.

**The parsed surface does not separate them either.** The parser
(`desk.html.stdlib-v1`) extracts block-level text only — no attributes, classes, ids,
or meta. A wall page with a teaser and a subscribe call-to-action produces a locator
map of ordinary prose elements. A short genuine article and a short wall teaser can
be the same shape, and the parser does not score article-ness.

**The failure is asymmetric and permanent.** A false positive discards bytes. D10
exists because the material most worth having is the material most likely to
disappear — the deleted post, the pulled video, the quietly edited article. A
heuristic that occasionally throws away real material to avoid storing a wall page
inverts the decision it was built to serve. Storing a wall page costs disk.

---

## Rejected — an operator "not usable" mark

A third `material_status` the operator sets by hand on a lead whose capture turned
out to be a wall. No detector, human authority, which is where this system puts
judgement everywhere else.

**`material_status` does not reach the paths that would have to honour it.**
`propose_claim` keys capture ownership on `case_id`. `close_run` keys on run-or-case
ownership. `attach_lead` sets `captures.case_id`. A lead marked not-usable, whose
capture is still attached to a case, remains citable and remains reportable as
examined. The mark would be decorative — a guard that does not fire, which the
fail-open inventory exists to catch.

Making it bind means enforcing lead material state in `propose_claim`, in
`close_run`, in `attach_lead`, and in every later projection and join that touches a
capture. **That is the shape D17 rejected.** An in-instance boundary that must hold
in every query forever, whose failure is silent, on a codebase whose consistent
defect through eight tickets has been boundaries holding on one path and not the
parallel one.

**Reopenable on terms.** If a future design introduces a single authoritative
enforcement point through which capture use must pass, an operator mark becomes cheap
and this decision should be revisited. Absent that point, it is four enforcement sites
and a silent failure mode.

---

## Why the residual risk is tolerable

Under capture-then-cite, a claim binds to text present in the parsed elements,
byte-exact, and the only quotable text on a wall page is the wall. A quotation of
"Subscribe to continue reading" supports no proposition. Nothing downstream can
launder a wall page into evidence, because the verification seam does not care what
the page was meant to be — only whether the quoted words are there and whether they
say anything.

The exposure D10 names is therefore **presentational**: a wall capture sitting in a
case view looking like a source that was read. The remedy for that is a visible
status, not discarded bytes — and if it becomes a real irritation in use, the cheap
fix is a projection that shows element count and lets the operator look, not a
classifier.

**Stated honestly:** no real soft-wall response has been fetched through this system.
The implementer confirmed at review that ticket 09's tests inject fake HTML or raise
`CAPTURE_AUTH_WALLED` by hand. This decision is made on what the code retains and
discards, which is knowable, rather than on how paywalls behave in the field, which
is not yet. If the Vela run puts real walls through and the picture changes, that is
new information and this decision is cheap to revisit — nothing was built on it.

---

## Consequences

**Ticket 09's auth-wall criterion is narrowed** to status-code walls, and the ticket
records the narrowing rather than being read later as fully met.

**Two adjacent gaps stay open and are not settled here:**

- **Unsupported content types lose the URL.** `assert_content_type_supported` raises
  before any Vault or Record write, so dropping a PDF or an audio URL refuses with no
  lead row at all. A login wall preserves the URL; a podcast does not. That asymmetry
  runs against the inbox's stated purpose. **Scheduled as ticket 09a**, before ticket
  10 — catch the refusal, insert a lead with `capture_id` NULL, the same pattern
  identity-only already proves. Deliberately not folded into ticket 09 after the fact.
- **The capture receipt is thinner than VISION §7 describes.** Final URL, response
  status, and redirect chain are not retained and cannot be backfilled — every capture
  already taken is missing them permanently. **Scheduled as ticket 15**, gating the
  Vela run. Captures made while building tickets 10–14 are test material, so waiting
  costs nothing real; running Vela without it costs the provenance of the first genuine
  corpus.
