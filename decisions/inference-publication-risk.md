# Inference Publication Risk (F-24)

D21. Decided at ticket 11 review. Closes F-24, open since ticket 05 and the last finding
outstanding from the first eight tickets.

---

## The problem

An inference claim (`source_basis` = `desk_inference`) cites other claims rather than
captured bytes. Nothing stopped it from carrying a weaker publication risk than the claims
it reasons over, so an inference over a `living_private` claim could be recorded
`not_applicable` — laundering the risk one level up, in the one dimension VISION §13 says
must fail closed.

## Rejected — a severity ladder with an automatic maximum

The first implementation ranked the seven publication-risk values 0–6 and refused any
inference ranked below the maximum of its cited claims.

**The ordering was wrong in a way that reopened the finding.** `unknown` was ranked
second-lowest, so an inference citing an `unknown` claim could be recorded `institution`,
`deceased`, `public_figure`, or `public_official_official_capacity` — all ranked higher, all
accepted. §13 makes `unknown` one of the two *non-publishable* states and says missing
classification fails closed. The ladder treated the fail-closed default as nearly the least
restrictive value.

**The deeper objection is that the ladder should not exist.** Publication risk is not a
degree of one quantity. Is `institution` more restrictive than `deceased`? Less than
`public_figure`? VISION does not say, because these are categorical states describing *what
kind of subject* a claim concerns, not gradations of severity. Inventing a total order reads
as rigour and behaves as a guess, and it invites future argument about the relative rank of
incomparable things. §11's prohibition on automatic sums, minimums, maximums, and ladders is
written about the evidence dimensions; the same reasoning applies here even though
publication risk is a separate control.

## D21 — Categorical inheritance, no ranking

**§13 groups `unknown` and `living_private` as the fail-closed pair: those two are
non-publishable *as cross-case connections*, and everything else is publishable subject to
human judgement.** D21 adopts that same pair for inference inheritance. That binary is the
whole constraint:

> If any cited claim carries `unknown` or `living_private`, the inference must also carry
> `unknown` or `living_private`.

**Scope correction (D23).** An earlier wording of this section read "§13 states the rule
directly: `unknown` and `living_private` are non-publishable," dropping §13's *as cross-case
connections* qualifier. That made a narrow rule about the No Coincidences surface look like a
universal bar on publishing either value, and that misreading produced F-65's framing. The pair
is correct **here**, for inference inheritance, because neither value may be laundered into a
safer one. It is not a universal ordinary-claim rule — see `publication-risk-ordinary-claims.md`
(D23), which settles what each value does at clearance and publication and renames the code
constant so its scope cannot be mistaken again.

No ranking, no comparison between incomparable categories, and `unknown` fails closed by
construction rather than by where it happens to sit in a dictionary.

Outside that rule the operator sets the inference's publication risk as he sets any other
authoritative dimension. An inference over a `deceased` claim recorded `institution` is a
classification question for the human, not a laundering hazard — neither value is
non-publishable, and §13 places that judgement with him.

## D21 — An inference may only be confirmed over confirmed claims

A one-time check at proposal is not enough, and neither is a second one at confirmation.

An inference can be confirmed while its cited claims are still `unconfirmed`, carrying
model-proposed risks. A cited claim later confirmed as `living_private` leaves the
already-confirmed inference behind it, unexamined — the same laundering reached by a
different operation order.

**Confirming an inference requires every claim it cites to be confirmed first.** The
inheritance rule is then evaluated against authoritative values rather than proposals.

**Rejected — invalidating dependent confirmations when a cited claim changes.** It works,
and it means confirmation is no longer durable: a claim confirmed for one rendition could
silently un-confirm because something upstream moved. That breaks D4's promise that
confirmation persists with the claim and makes cross-case reuse unreliable. Requiring
bottom-up confirmation gets the same guarantee with no invalidation machinery and no
re-evaluation pass.

**Accepted cost:** confirming an inference means confirming its citations first, so the
operator cannot confirm an inference in isolation. That is the correct order of work — the
inference is only as good as what it reasons over, and reviewing it without reviewing its
basis is the rubber-stamping D4 exists to prevent.

---

## D21 — Confirmation is correctable; dependencies are protected by refusal

An earlier draft of this decision said bottom-up confirmation meant *no dependency can
change underneath a confirmed inference.* That was true only because confirmation was
one-shot, and one-shot confirmation was itself a defect: a claim confirmed with a wrong
dimension had no correction path short of editing the database, and §12 names confirmation as
the place output pressure produces fast, loose decisions. It also silently discarded
corrected dimensions passed for an already-confirmed claim, and it hollowed out the
correction-rate view §10 requires — if a claim can only be confirmed once, every history row
is a first confirmation and correction is unmeasurable.

**Re-confirmation is allowed and append-only.** Each confirming act writes a
`claim_confirmations` row carrying the prior authoritative values, the newly confirmed
values, actor, and timestamp. The columns on `claims` remain the current projection. History
is never the projection — the fourth application of that pattern in this codebase.

Re-confirming with values identical to the current projection is a no-op rather than a
fabricated correction event, so the correction rate counts real corrections.

**Dependencies are protected by a targeted refusal, not by invalidation.** Re-confirming a
claim to `unknown` or `living_private` while a confirmed inference cites it is refused
(`CONFIRMATION_BLOCKED_BY_INFERENCE`), naming the blocking inference so the operator resolves
it first.

This preserves the durability guarantee above while allowing correction. The rejected
alternative — invalidating or auto-reopening dependent confirmations — is rejected for the
same reason as before: it makes a confirmation something that can be undone by an action
elsewhere, which is precisely what D4 promises does not happen.

## D21 — Confirmation may correct strength, never kind

A claim's support structure is built at proposal: an inference cites claims, everything else
quotes captured bytes (D14). Confirmation must not move a claim across that boundary in
either direction — refused as `SOURCE_BASIS_KIND_MISMATCH`.

Without this, correcting `source_basis` to `desk_inference` at confirmation produced a
confirmed inference with zero cited claims and a capture binding, and the reverse produced a
non-inference claim supported only by citations. Both leave the authoritative kind
disagreeing with the actual support, which makes D14's escape valve meaningless — the reason
inferences may cite claims instead of captures is that they *are* inferences.

**Changing kind requires a new claim with matching support structure**, not a confirmation
correction. Confirmation adjusts how strong a claim is; it does not adjust what the claim is.

**Rejected — a governed operation that rewrites support structure to match a corrected
kind.** It would let confirmation silently rebuild the evidentiary basis of a claim, which is
a proposal-time act performed against captured bytes under a rubric version. A new claim
keeps the lineage honest.

**The generalisation worth keeping:** this defect and the ladder defect were the same shape
— a check bound to the wrong field. Fixing *which value* a check reads does not help unless
something also constrains *what that value may become*.


---

## Consequences

The check runs at `propose_claim` (against proposed values, as an early refusal) and at
confirmation (against authoritative values, as the binding one). `PUBLICATION_RISK_RANK` is
deleted rather than reordered — reordering fixes the instance, deleting fixes the class.
