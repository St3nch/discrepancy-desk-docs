# Human Evidence Review

D25. Settled 2026-08-09, from live evidence rather than in anticipation: the ML2 rehearsal's
Run A, frozen at exactly the state that produced it. Closes **R-OBS-03** and **R-OBS-05**, and
the doctrine half of **A-06**, deferred since the pre-rehearsal audit.

**Implementation is a separate ticket.** This decides the shape.

---

## What was observed

Run #5: two captures, seven proposed claims, zero confirmed. The case page reported official
foundation `worked`, `2 cited, 0 unexamined`, and stated:

> *"No unexamined captures — attestation does not need a look report."*

No human had read either capture. One of the seven claims asserted something its quotation could
not support, and the claim card displayed no quotation at all.

The chain, each link individually recorded and jointly unruled:

1. `propose_claim` marks a capture `cited` — **while the claim is unconfirmed**.
2. `complete` requires zero unexamined captures; `cited` satisfies that.
3. Coverage reads `worked` on a completed run plus ≥1 claim, confirmed or not.

The executor walks the entire path from raw capture to attestable alone. The operator's only act
is the attestation click.

**Worse than it appears:** `cited` does not establish that even the *executor* read the document.
On capture 6 it skipped to element 210 on the strength of the previous page's layout — its own
words, *"pattern-matching on layout, not knowledge"* (R-OBS-11).

## The root cause

**One status axis answering two different questions.**

D11 defines `cited` correctly and literally: a claim binds to this capture. It has never meant
*human reviewed*. The defect is that **D20's attestation invariant uses the capture work-state
axis (`unexamined` / `examined` / `cited`) as a proxy for a different fact** — that the human has
had the evidence in view. Those diverge the moment an executor proposes anything, which is
always.

Stating it this way points the fix at attestation and review semantics rather than inviting a
future change to `cited`, which is not broken.

## The ordering is not the defect

An earlier draft called this an inversion and floated ungating confirmation as a possible
narrowing of what §7 and D20 "always meant." **That was wrong.**

D20 states plainly: *"Ticket 11 must call the gate on every angle-start **and claim-confirmation**
path."* D4 attaches confirmation at use. Attestation preceding the first use-confirmation is the
direct consequence of locked decisions. D20 also defines attestation as the corpus-sufficiency
judgement — *"I have seen enough"* — not a judgement resting on confirmed claims.

**The order stands:** read evidence → attest sufficiency → enter the Angle Room → confirm the
claims actually used.

And D20 already required the missing piece. Its rejected-alternative section, arguing against a
bare attestation with nothing derived beneath it, says: ***"The attestation should be a judgement
about evidence, with the evidence in view."*** That condition was written into the decision and
never mechanised. D25 mechanises it.

---

## D25 — Human evidence review is a distinct, explicit, per-surface act

> **An append-only review record**, scoped to the evidence surface actually read:
> `capture_id` + `document_version_id` + `actor` + `reviewed_at`, optional note.
>
> **Official-foundation attestation refuses while any case-owned evidence surface lacks a review
> record.** Case-wide, fail-closed.
>
> **Review is produced only by an explicit human action.** Never inferred from opening a view,
> scrolling, following a claim's highlighted span, or any executor activity. **Never batchable.**
>
> **Review and attestation remain separate acts.** Review answers *did I read this evidence
> surface?* Attestation answers *have I seen enough of the official record to begin
> interpretation?* Different questions, different objects.
>
> **New material makes an attestation visibly stale without invalidating prior review records.**
> A review is a durable fact about a surface that was read; it does not expire because other
> material arrived.

### Why document-version scoped

Elements are a versioned normalised package. The human read *that* parse at *those* locators. A
later document version must not silently inherit a review of an earlier surface. Raw capture
identity remains available through the capture and its hash.

`capture_id` + actor + timestamp alone is too coarse for this architecture.

### Why not stage-scoped

Reading an immutable evidence surface is one factual human act. It should not need repeating
because the same source later bears on `deep_context` rather than `official_foundation`.
**Stage-specific judgement belongs on the coverage attestation, which is where it already lives.**

### Why case-wide rather than claim-bound

The narrower alternative — review only captures bound by a claim on this stage — is cheaper and
was rejected. **An unbound capture is exactly the material that should worry the operator:**
something nobody claimed anything from is either irrelevant or the thing that contradicts the
story. Attesting *"I have seen enough"* while unreviewed material sits in the case is the
honest-denominator problem (D3) in a new place.

Case-wide also gives staleness for free: a later capture or a later document version leaves an
unreviewed surface, and the attestation goes stale with no second mechanism. Clearance standing
(ticket 13) is the analogous pattern if one is ever wanted.

### Why explicit and unbatchable

The act must cost something or it means nothing. Inferring review from opening a view rebuilds
R-OBS-03 one layer down: a click that satisfies a gate without a judgement behind it. A
"review all" control does the same thing faster.

**Following a claim's highlighted span is not review of the document.** Reviewing a binding and
reviewing an evidence surface are different acts, and ticket 20's "view in capture" action must
not mark anything reviewed.

### How the cost is paid

§12 warns that when the honest path is expensive and the dishonest one is cheap, the mechanism
manufactures rubber-stamping. Case-wide review is a real cost on a seven-source case.

**Pay it in the interface, not by coarsening the record.** Storage granularity and click count are
separate design questions. The pattern is a **"Reviewed — next evidence"** action that advances
through unreviewed surfaces in one motion: the act stays explicit and per-surface, the navigation
stops being a hunt. Ticket 20's evidence surfaces ship first precisely so that reading is
possible before it is required.

---

## Rejected alternatives

**A fourth capture status.** Rejected: `unexamined` / `examined` / `cited` already answer three
different questions and the axis is being asked a fourth. Adding to it deepens the conflation
this decision exists to end.

**Reusing `examined`.** Rejected: in this codebase it means *looked at and found nothing worth
claiming*. A capture supporting seven claims is not that. Relabelling would corrupt an existing
vocabulary to patch a new gap — the defect class this project keeps finding.

**Redefining `cited`.** Rejected: D11's definition is correct. The status is not broken; the
invariant that reads it is.

**A property of the attestation rather than of the evidence.** Rejected: it would record that a
click happened, not that a surface was read, and would carry no per-surface provenance to go
stale against.

**Review inferred from opening the capture view or following a binding.** Rejected: rebuilds
R-OBS-03 at a lower level. A gate satisfied by navigation is not a gate.

**Batch review.** Rejected for the same reason, faster.

**Claim-bound scope instead of case-wide.** Rejected: see above — unbound captures are the ones
that matter most.

**Ungating confirmation from attestation.** Rejected, and it must never be written up as a
clarification: it reverses D20 explicitly and, depending on implementation, D4's authority-at-use
shape. A standalone confirm operation would additionally contradict D4's confirmation-at-use
principle. Both remain available only as **honest doctrine reversals** naming the decisions they
overturn.

**Coarsening the review record to reduce clicks.** Rejected: the audit fact stays precise per
evidence surface. Efficiency is an interface problem.

---

## Constraints on implementation

- **No automated review.** Nothing may mark a surface reviewed except a human act. No confidence
  score, claim count, or executor report satisfies it.
- **Fail closed.** Absent review evidence, attestation refuses (VISION §13's standing rule).
- **Legacy captures are not silently reclassified.** Runs 1–4 and their captures predate this
  entirely. How they read must be a stated decision at implementation, not an accident — same
  discipline as ticket 15 refusing to backfill `final_url` and ticket 16 preserving legacy rubric
  strings.
- **The existing three statuses keep their meanings**, including historically.
- **Ticket 20 ships first.** Reading must be possible before it is required.

## Consequences

**Attestation becomes what D20 described.** A judgement about evidence, with the evidence in
view — the sentence that has been in the decision since ticket 10 and unenforced since.

**The ML2 rehearsal resumes against this.** Case #2 is frozen with two captures and seven
unconfirmed claims; whatever legacy rule is chosen determines whether those two surfaces need
review before attestation. **They should** — they are the material the rehearsal exists to have
the operator read.

**R-OBS-11 remains open and is not solved here.** `cited` still cannot establish that the
executor read a whole document, and nothing records which elements were read. That bears on
absence claims — *"this document does not address X"* has a reading requirement positive claims
do not — and belongs to rubric v2, which must prevent ordinary quote-bound claims from
masquerading as whole-document absence findings.
