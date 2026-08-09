# Corroboration Is Proposition-Scoped

D24. Settled 2026-08-09, before ticket 16 was briefed, after the spec axis corrected the
steward's premise. Closes **F-57** as a semantics and authority decision rather than a new
object.

---

## F-57 was stated wrong, and the wrong statement chose the wrong fixes

The handoff recorded F-57 as *"no vocabulary for cross-document corroboration."* The
vocabulary has existed since ticket 05:

```
CORROBORATION = {unassessed, single_source, multi_source_dependent,
                 independently_corroborated, contradicted}
```

In `service/evidence.py`, the `0005_claims` migration CHECK, `CONTEXT.md`, and the client.

The steward then proposed a sharper but still wrong framing: that these values are
*unreachable*, because "under D4 a claim is about what one document says," so every claim can
only be `single_source`.

**Both framings are wrong, and the second one is wrong on the code.** D4 says a claim is a
proposition bound to captured bytes with human authority at use — not that a claim is about one
document. D14 permits multiple locator-and-quote pairs per claim, and `propose_claim` verifies
each binding independently (`claims.py:368`) with **no same-capture restriction**. A claim may
already bind quotations from different captures.

So `independently_corroborated` was never structurally impossible. What was missing was an
agreed answer to *what corroboration is about*.

---

## D24 — Corroboration describes the proposition against the Record

> **`corroboration` is a proposition-scoped, claim-level evidence dimension evaluated against
> the Record as a whole. It is not derived from a claim's number of quote bindings, captures,
> URLs, domains, or publications.**
>
> - `single_source` — the proposition rests on **one underlying evidentiary path**. Multiple
>   passages, captures, or reproductions of the same underlying source do not make it
>   multi-source.
> - `multi_source_dependent` — multiple sources support it, but their support is materially
>   dependent on a shared witness, document, dataset, or reporting chain.
> - `independently_corroborated` — at least two genuinely independent evidentiary paths support
>   the same proposition.
>
> The executor may **propose** any of these. Only human confirmation or re-confirmation makes
> an independence ruling authoritative.

A source-specific claim can therefore be `independently_corroborated` because another
independent source in the Record supports the same proposition — without collapsing the two
sources into one claim.

**Why not merge corroborating documents into one multi-binding claim.** A claim carries one
`source_basis`, one `posture`, one `certainty`. Two documents supporting the same proposition
may differ on all three — a contemporaneous official record and a later technical analysis.
Merging them destroys exactly the dimensional distinctions §11 exists to preserve. Both
extremes are wrong: never *"one document, one duplicate proposition forever,"* and never
*"merge heterogeneous evidence into one dimensionally incoherent claim."*

---

## Independence is proposition-specific, not a property of a source pair

The roadmap's first open question has a clean answer: **per-proposition.**

Two documents are not independent in the abstract. They may hold separate knowledge of
proposition A and share one underlying source for proposition B — a wire report quoting an
agency statement. Independence belongs to *the proposition × its evidentiary paths*, and the
claim is already the proposition object.

This also disposes of a tempting shape: **a global source-pair relation would assert too
much.** "Document A is independent of Document B" is a claim nobody can make. Were a richer
relation ever built, it must be **claim-to-claim**, which is naturally proposition-scoped — the
same two documents produce different claim pairs for different propositions.

## The ruling lands in `corroboration`, not beside it

The roadmap's second question is answered against its own earlier instinct. Keeping the
independence ruling separate from the dimension would produce F-44's shape: one object holding
the meaningful editorial judgement while the dimension actually presented to composition says
something else.

The existing authority mechanism already suffices. The executor proposes; the human confirms or
corrects; `claim_confirmations` records proposed against confirmed with actor and timestamp;
re-confirmation is append-only. **That is where an independence ruling lands today.**

It does not record a prose *basis* — "A interviewed the witness; B relied on the committee
report." Current doctrine has never required written rationale for any evidence-dimension
decision, and building an attestation object solely to acquire one is governance ahead of
demonstrated need. **If Vela makes the missing basis painful, that is excellent evidence for a
post-Vela object.**

---

## Rejected — a minimal corroboration relation before Vela

The steward's position: build the human-only claim-to-claim relation now, skip the candidate
finder, on the grounds that De Geer & Wright versus the official finding is exactly a
two-independent-sources case and is the shape of the story.

**Rejected.** Vela needs the Desk to answer *is this proposition independently corroborated* —
which the claim's authoritative `corroboration` already answers. It does not yet need *show me
the complete historical graph of every independence ruling between every pair of supporting
claims*. That is useful provenance and exactly the kind of structure the reboot says to earn
from execution rather than anticipate.

## Rejected — a candidate finder before Vela

Same-angle plus differing `final_url` domain can flag things worth looking at; it cannot
establish proposition equivalence or independence. Without semantic matching — refused at
ticket 09 and again at ticket 12 — it is intentionally coarse. No reason to build it before one
real case demonstrates operator pain.

## Rejected — marking the two values executor-forbidden in the rubric

All dimensions enter as proposals. An executor can legitimately observe an explicit dependency
— three articles each stating they quote the same report. What it cannot do is turn a proposal
into authority. `unassessed` is the correct fallback when independence is genuinely unresolved.

---

## Consequences

**The retired sentence.** *"Each claim is correctly `single_source` under D4, since each claim
is about what one document says"* appears in the handoff and the post-Vela discussion record.
It is wrong and it is what sent F-57 down the wrong path for five tickets. `single_source`
means **one underlying evidentiary path**, not one quote-bound document.

**The live run's all-`single_source` result is re-read.** The executor was not missing an enum
value or a database object. It was missing the semantic contract — which is precisely what
ticket 16 (F-56) exists to ship. That makes the live-run evidence support ticket 16 rather than
a new relation ticket.

**No automatic feedback, ever.** A future relation may help the operator decide to re-confirm
`corroboration`. It must never silently rewrite an authoritative claim dimension.

**One client fix rides with ticket 16.** `claimCard()` (`ui.ts:1719`) renders posture,
source_basis, certainty, and publication_risk — **five of six dimensions, with `corroboration`
omitted** — while the confirmation forms expose it. If ticket 16 is meant to make
classification legible, the dimension carrying this decision should not be the one the operator
cannot see. Cheap, and it is the difference between this decision landing and being *"recorded
somewhere nobody looks."*
