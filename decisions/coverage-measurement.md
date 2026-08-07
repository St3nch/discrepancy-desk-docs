# Coverage Measurement and the Official-Foundation Gate

D20. Decided at ticket 10 review, when both review axes independently found that the
derived gauge could read `complete` on activity unrelated to the official foundation.
Closes the `architecture-decisions.md` fog item "what coverage actually measures."

---

## The problem

Ticket 10 derived `official_foundation` from case-wide activity: at least one capture, at
least one claim, no unexamined captures. Those signals are real and they are honestly
reported. They measure the wrong thing.

**Nothing binds activity to the dimension it supposedly evidences.** A run working the
public question produces captures and claims on the case, so it satisfies the
official-foundation gate. The ticket's own allowing test used a claim with `source_basis`
of `contemporaneous_report` — ordinary research material, not institutional record.

The gate is the one absolute rule in an otherwise deliberately non-blocking mechanism:
*no angle work begins before the official spine is complete.* As derived, it authorised
angle work precisely when the official foundation might never have been worked.

A lower bar was not the fix. Thresholds — three captures, five claims — are arbitrary
across topics and would be fake precision on a real question.

---

## D20 — The operator sets a run's coverage dimension; `complete` is attested

**A run carries a coverage dimension, set by the operator at dispatch.** One run, one
dimension. Coverage then derives from real activity with correct attribution: captures
and claims inherit the dimension through their run's lineage.

This is not a new authority. Only the human may dispatch a run (D5), and a run already
carries a question and a bounded scope he wrote. Which of the six dimensions it targets is
the same judgement at the same moment. **It is not executor-declared** — nothing the
executor reports at `close_run` touches it, which was the property ticket 10 correctly
protected and must keep.

**Readings:**

| Reading | Meaning |
|---|---|
| `unworked` | No completed run targets this dimension, and no measuring object exists for it on the case |
| `worked` | At least one completed run targets it and produced claims, or a first-class object for that stage exists with a claim link |
| `complete` | The operator has attested it, and the attestation still stands |
| `unmeasurable` | Reserved for a dimension with no measuring object at all |

### Object-backed readings (extended at ticket 11)

The run-scoped contract above is the general one. Two stages gained first-class objects in
ticket 11 and are measured through them instead:

| Stage | Measured by |
|---|---|
| `public_question` | A public question on the case **with at least one claim link** |
| `editorial_development` | An angle on the case **with at least one claim link** |

The claim link is the load-bearing half. VISION §7 requires every Angle Room item to rest on
at least one claim, so an empty angle or an unsupported public question is a draft and does
not move coverage. Without that condition the reading would report that a stage was worked
because somebody typed a title.

**`story_intelligence` remains `unmeasurable`, and that is a decision rather than an
omission.** Story intelligence has no distinct object — central discrepancy, human conflict,
narrative turn, and surprising supported detail are Angle Room content that ticket 11 did not
model separately, and inferring it from angle existence would be the proxy D20 rejects. Do
not silently leave it unmeasurable by forgetting to touch it when other stages gain objects.

**`composition` is object-backed from ticket 12.** A case has worked composition when it has
at least one rendition with at least one unit that cites at least one claim — the same
"object plus claim linkage" shape as public_question and editorial_development. Empty or
uncited drafts do not move the gauge.

**`complete` is a human attestation, recorded with actor and timestamp.** "The spine is
complete" means "I have seen enough," and no count expresses that. It sits naturally
alongside the other §12 human-only decisions — deciding whether sources are genuinely
independent is the same kind of judgement about material sufficiency.

**An attestation goes stale honestly.** Unexamined captures arriving on the case after the
attestation drop the reading back to `worked`, with the reason stated. The operator looks
and re-attests. Nothing silently keeps a stale `complete`, and nothing silently locks the
Angle Room either — the regression is visible and the remedy is one action.

### The invariant that makes staleness cheap

**Attestation refuses while any unexamined capture remains unaccounted for.** Attesting is
a judgement about evidence with the evidence in view; material nobody has looked at is not
in view.

That refusal is what keeps the staleness rule simple. Because the count is zero at the
moment of attestation, *"any unexamined capture exists now"* and *"unexamined material
arrived after the attestation"* are the same statement. No snapshot, no corpus revision, no
capture-attachment timestamp is needed to tell them apart.

This was considered and rejected explicitly: binding an attestation to a corpus revision so
staleness could be computed by comparison. It is machinery for a distinction the invariant
removes, and it would have to be maintained on every path that changes case ownership of a
capture. **Do not reintroduce it to "fix" the staleness rule** — if the rule looks too blunt,
the thing to check is whether the refusal is still in place.

**The refusal must not wedge a case.** Captures from an abandoned or cancelled run stay
`unexamined` indefinitely — `cancel_run` deliberately does not touch capture status. A strict
refusal would make such a case permanently unattestable and permanently blocked from angle
work, which is F-26's failure shape.

So attestation may carry `examined_capture_ids`, the same vocabulary and the same act as
`close_run`: the operator reports that he looked and found nothing worth claiming. F-32
requires `examined` to be reported rather than inferred; a human reporting it is strictly
more authoritative than an executor doing so. Attestation then refuses only when unexamined
captures remain that the operator has neither examined nor reported.

---

## Rejected — thresholds on case-wide activity

The ticket 10 shape with bigger numbers. Rejected because the axis is wrong, not the
magnitude: no count of unattributed captures and claims evidences a *particular*
dimension, and any threshold would be arbitrary across topics that differ enormously in
how much institutional record exists.

## Rejected — executor-declared stage at run close

The cheapest implementation and the one the ticket explicitly warned against. It puts the
system's only absolute gate on the executor's self-report, which is the executor grading
its own work. VISION §10 names the failure directly: a reading that says *"a run labelled
stage 1 finished"* rather than *"the official spine is complete."*

## Rejected — inferring the dimension from `source_basis`

Mapping `contemporaneous_record` and similar onto official foundation. Rejected because
`source_basis` describes what *a claim* is, not what *the case has worked*, and because
those values are executor-proposed until confirmed (D4) — which would route the gate back
to the executor by a longer path. Ticket 10 was right to refuse this proxy for
`deep_context`; the same refusal applies here.

## Rejected — attestation alone, with no run binding

The operator marks the foundation complete, full stop, with no derived signal beneath it.
Simpler, and it preserves human authority. Rejected because it makes the gauge decorative:
coverage would report nothing the operator did not type, and the drift-visibility argument
(§10) depends on aggregate readings that come from recorded activity rather than from
self-report. The attestation should be a judgement *about* evidence, with the evidence in
view.

---

## Consequences

**`unworked` requires a measuring object that exists.** Where the object arrives in a
later ticket, the honest reading is `unmeasurable` — "no first-class record could show
this" is a different statement from "this was not worked." At the time of this decision no
angles, public questions, or renditions tables exist, so four of the six stages were
reading `unworked` on the strength of tables that had not been built.

**Ticket 11 must call the gate on every angle-start and claim-confirmation path**, and its
acceptance record says so. Ticket 10 can prove the refusal is real at the seam; only
ticket 11 can prove an actual attempt to start angle work is refused.

**The fog item closes.** `architecture-decisions.md` lists "what coverage actually
measures" as unresolved. It is resolved here: coverage measures operator-scoped runs and
the activity beneath them, and `complete` is attested rather than computed.
