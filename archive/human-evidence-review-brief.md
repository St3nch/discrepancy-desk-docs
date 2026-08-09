# Decision brief — Human evidence review vs citation state

> **SUPERSEDED — archived 2026-08-09.** Settled as **D25**,
> `../decisions/human-evidence-review.md`. This is the working brief that produced it, kept for
> the reasoning trail rather than for guidance. **It is not governing. Do not implement from it.**
>
> Worth keeping for two things D25 does not carry:
>
> - **The steward got D20's ordering wrong here**, describing attest-then-confirm as an
>   accidental inversion and floating ungated confirmation as a possible "faithful narrowing" of
>   what §7 and D20 always meant. The spec axis corrected it against D20's own text — *"Ticket 11
>   must call the gate on every angle-start and claim-confirmation path"* — which would otherwise
>   have shipped a doctrine reversal written up as a clarification, inside a decision record.
> - **The six open questions in the shape they had before they were answered**, which is the form
>   the argument took rather than the form the conclusion took.
>
> Where the brief and D25 differ, **D25 governs.** Two answers moved when the operator ruled:
> attestation scope stayed case-wide (the brief called it a recommended starting position), and
> review became an explicit, unbatchable per-document-version act — a question the brief never
> asked.

**Status:** open decision brief, not a decision. Written 2026-08-09 after the ML2 rehearsal's
Run A. Governs **R-OBS-03** and **R-OBS-05**, and closes the doctrine half of **A-06**, deferred
since the pre-rehearsal audit.

**To be settled the way F-65 (D23) and F-57 (D24) were:** design questions argued between axes
*before* any code, recorded with rejected alternatives, then implemented as its own ticket.

**Does not block ticket 20.** The operator needs to see evidence regardless of how this resolves.

---

## The defect, as observed

On Run #5: two captures, seven proposed claims, zero confirmed. The case page reported official
foundation `worked`, `2 cited, 0 unexamined`, and stated:

> *"No unexamined captures — attestation does not need a look report."*

No human had read either capture.

The chain, each link individually recorded and jointly unruled:

1. `propose_claim` marks a capture `cited` — **while the claim is unconfirmed**.
2. `complete` requires zero unexamined captures; `cited` satisfies that.
3. Coverage reads `worked` on a completed run plus ≥1 claim, confirmed or not.
4. Every confirmation-at-use surface is gated behind `official_foundation_complete`.

So the executor walks the entire path from raw capture to attestable alone, and the operator's
only act is the attestation click. **That ordering is correct and designed** (see below) — what is
wrong is that the attestation can be offered with no evidence reviewed by anyone.

And §12 predicts exactly the incentive this creates: the fastest way to open the Angle Room is to
attest without looking.

## The root cause

**One status axis being used to answer two different questions.**

D11 already defines `cited` literally and correctly: a claim binds to this capture. It has never
meant *human reviewed*. The defect is that **D20's attestation invariant uses the capture
work-state axis (`unexamined` / `examined` / `cited`) as a proxy for a different fact** — that the
human has had the evidence in view. Those diverge the moment an executor proposes anything, which
is always.

Stating it this way matters: it points the fix at attestation and review semantics rather than
inviting a future change to `cited`, which is not broken.

---

## What both axes already agree on

- **`cited` keeps its literal meaning** (D11). Do not redefine it to imply human review.
- **Do not reuse `examined`.** In this codebase it means *looked at and found nothing worth
  claiming*. A capture supporting seven claims is not that, and relabelling would corrupt an
  existing vocabulary to patch a new gap — the defect class this project keeps finding.
- **Human evidence review is a distinct durable fact**, whatever object carries it.
- **The existing order is not the defect and should stand** unless later evidence reopens D20:
  read evidence → attest foundation sufficiency → enter Angle Room → confirm the claims actually
  used.

## What the decision must settle

1. **What object records human evidence review.** Leading candidate: a durable **append-only
   review record** on the `claim_confirmations` pattern, scoped to the **evidence surface actually
   reviewed** — `capture_id` + `document_version_id` + actor + `reviewed_at`, with an optional
   note.

   **Why the document version and not the capture alone:** elements are a versioned normalised
   package. A later document version must not silently inherit a review of an earlier surface —
   the human read *that* parse, at *those* locators. Raw capture identity remains available
   through the capture and its hash. `capture_id` + actor + timestamp alone is too coarse for this
   architecture.

   Alternatives to record and reject explicitly: a fourth capture status; a property of the
   attestation rather than of the evidence.
2. **Actor and timestamp requirements**, and append-only behaviour when a surface is re-reviewed.
3. **Exactly what official-foundation attestation requires.** Recommended starting position:
   preserve D20's existing **case-wide fail-closed scope** — at attestation, no current
   case-owned evidence surface may lack human review. Cheap invariant, and it avoids introducing
   the corpus-revision mechanism D20 explicitly rejected.
4. **Staleness falls out for free** under that scope: a later capture, or a later document
   version, leaves an unreviewed surface and the attestation goes stale naturally. No second
   staleness model needed. (Clearance standing, ticket 13, is the analogous pattern if one is
   wanted.)
5. **Capture/document-version scoped, not stage scoped.** Reading an immutable evidence surface is
   one factual human act; it should not need repeating because the same source later bears on
   `deep_context` rather than `official_foundation`. **Stage-specific judgement belongs on the
   coverage attestation, not on the review record.**
6. **How the existing `unexamined / examined / cited` statuses relate to the new fact** without
   silently changing their historical meaning — including for the four legacy runs and their
   captures already in the database.

## The ordering question — and why it is probably not open

An earlier draft of this brief called the observed order an accidental inversion and suggested
that ungating confirmation might be a faithful narrowing of what §7 and D20 always meant.
**That was wrong, and the spec axis corrected it.**

D20 states plainly: *"Ticket 11 must call the gate on every angle-start **and
claim-confirmation** path."* D4 attaches confirmation at use, with an angle pulling claims in to
be confirmed. Attestation preceding the first use-confirmation is therefore **the direct
consequence of locked decisions**, not an accident.

D20 also defines what attestation is: the human's corpus-sufficiency judgement — *"I have seen
enough"*. It is not a judgement resting on confirmed claims.

**So the recommended position is: keep the order.**

> read evidence → attest official-foundation sufficiency → enter Angle Room → confirm only the
> claims actually used

That is coherent. Attestation answers *have I seen enough of the official record to begin
interpretation?* Confirmation answers *do I authorise this proposition, with these dimensions,
for this use?* Different judgements; the second need not precede the first.

**Ungating confirmation from attestation remains available as an alternative, but it must be
recorded honestly as a doctrine reversal** naming D20 and, depending on implementation, D4's
authority-at-use shape. It must not be written up as a clarification. A standalone confirm
operation would additionally contradict **D4's** confirmation-at-use principle.

**What failed in Run A was not the order. It was that the first judgement could be offered with
no evidence reviewed at all** — and D20's own rejected-alternative section already required
otherwise: *"The attestation should be a judgement about evidence, with the evidence in view."*
That sentence was written into the decision and never mechanised.

---

## Constraints on any answer

- **No automated review.** Nothing may mark a capture reviewed except a human act. A confidence
  score, a claim count, or an executor report must never satisfy it.
- **Fail closed.** Absent review evidence, attestation refuses. Missing classification failing
  closed is the standing rule (VISION §13).
- **Do not make the honest path more expensive than the dishonest one.** If reviewing forty
  captures requires forty clicks and attesting requires one, the mechanism manufactures the
  behaviour §12 warns about. **But solve that in the interface, not by coarsening the audit
  record** — storage granularity and click count are separate design questions. Keep the fact
  precise per evidence surface; make review efficient with next-unreviewed navigation or
  equivalent. This is why ticket 20 ships first.
- **Legacy captures must not be silently reclassified.** Runs 1–4 and their captures predate this
  entirely; they read as they read. Same rule as ticket 15's refusal to backfill `final_url` and
  ticket 16's legacy rubric strings.

---

## Evidence to decide from

`ml2-rehearsal-run-a-findings.md` — R-OBS-03 and R-OBS-05, with the live screen text. The
rehearsal is frozen at exactly this point and can be resumed against whatever is decided.
