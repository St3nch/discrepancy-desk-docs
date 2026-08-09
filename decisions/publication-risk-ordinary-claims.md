# Publication Risk on Ordinary Claims

D23. Settled 2026-08-09, before ticket 16 was briefed, after the spec axis corrected the
steward's framing on both of its load-bearing points. Recorded as its own decision rather than
folded into D21, because extending D21 silently is exactly the error this decision exists to
stop.

**Governs:** F-65. **Implementation is a separate ticket** — this decides the shape, not the
schedule.

---

## The gap as it actually stood

`NON_PUBLISHABLE_PUBLICATION_RISKS = frozenset({"unknown", "living_private"})` lives in
`service/evidence.py` and is referenced in exactly one file — `service/confirmation.py` — in
three places, all about inference claims: the anti-laundering helper, its call from
`propose_claim`, and the re-confirmation check that stops a claim being confirmed to a
non-publishable risk when confirmed inferences already cite it.

Nothing else reads it. An ordinary `factual_assertion` claim classified `living_private` could
be proposed, confirmed, linked to an angle, composed, cleared as exact text, and recorded as
published, with nothing refusing at any step. The dimension was recorded, surfaced, and never
gated.

---

## D23 — The two values do different jobs and must stop sharing one rule

> **`unknown` is an unresolved classification. Hard refusal at clearance. No override.**
> The human has not supplied an answer yet; the remedy is re-confirming the claim with an
> authoritative category, not asserting "publish anyway." An override for `unknown` would make
> *"I have not classified this"* equivalent to a classification, which is the fail-closed rule
> inverted.
>
> **`living_private` is a known sensitive classification. Publishable only through an explicit,
> reasoned, human override bound to one exact-content clearance.**
>
> **Neither blocks drafting.** Both may be proposed, confirmed, linked, and composed.

The settled matrix:

| Context | `unknown` | `living_private` |
|---|---|---|
| Inference inheritance (D21) | cannot be laundered | cannot be laundered |
| Cross-case connection (VISION §13) | hard non-publishable | hard non-publishable |
| Ordinary draft / composition | allowed | allowed |
| Exact-content clearance | refuse until classified | explicit human override + reason |
| Publication recording | recheck; refuse | recheck; standing approval must contain a matching override |

The categories do different jobs without a severity ladder.

---

## The override binds to the approval, not to the claim

An append-only child of `rendition_approvals`, roughly:

```
rendition_approval_publication_risk_overrides
    approval_id
    claim_id
    publication_risk_at_approval    -- living_private
    reason                          -- required, human-authored, non-empty
    actor
    created_at
```

`approve_rendition` requires an override for every cited `living_private` claim.
`record_publication` **re-runs** current publication-risk eligibility and **accepts no new
overrides** — it honours only those belonging to the standing authorizing approval.

**Confirmation and override answer different questions.** Confirmation answers *what is this
claim, authoritatively.* An override answers *given this exact proposed public use, am I
willing to publish it anyway.* Different scopes, different objects.

**The stale-state property falls out for free.** If claim 42 is `public_figure` at clearance and
is re-confirmed to `living_private` before publication, the standing approval contains no
override for claim 42 — publication refuses, and the operator must re-clear the exact content
while confronting the new risk. A currently-`unknown` claim refuses regardless of what any
older approval says. This is ticket 13's standing model doing the work, not a new mechanism.

---

## Rejected — a universal bar on `living_private`

Make ordinary `living_private` claims categorically unpublishable, matching the inference rule.

**Rejected because it adopts a major editorial doctrine by omission.** It would mean the Desk
cannot publish sourced claims about a living private individual under any circumstances — ruling
out legitimate accountability work by architecture rather than by editorial judgement. Nothing
in VISION commits the project to that. If it is ever adopted it should be adopted deliberately,
in prose, as a statement about what this publication is for.

## Rejected — letting ordinary clearance count as the override

Allow `living_private` through the normal Clear button on the reasoning that clearance is
already a human decision.

**Rejected because it defeats the point of recording the dimension.** If the operator can clear
a sensitive claim with the same generic action as any other, the classification never forces
anyone to confront the exceptional state. The friction is the feature.

## Rejected — a claim-level publishability attestation

An override on the `claim_confirmations` pattern, attached to the claim.

**Rejected because it is silently reusable.** Approve a `living_private` claim once and it could
authorize a completely different rendition six months later. Approval binds the rendition, not
the angle and not the claim (D2, ticket 13). A claim-scoped override would be the one place
that rule did not hold.

## Rejected — adding the check to `assert_units_eligible_for_clearance_or_publication`

The steward's proposal, and the one that looked cheapest.

**Rejected because that helper is not clearance-only.** It has four callers:
`propose_rendition`, `update_rendition`, `approve_rendition`, `record_publication`. Adding a
publication-risk refusal there would make classification a **composition** gate — an executor
refused at draft time because of what a claim is *about*. That contradicts VISION §5
(classification is not an exclusion gate), §12 (the LLM may draft; authoritative publication
decisions are the human's), and §14 (exact-content approval is the publishability gate). A
draft containing sensitive material is useful editorial substrate; the invariant is that it
cannot become an *authorized publication* without the human confronting the risk.

**Split the concern instead:**

- **Rendition eligibility** — confirmed, linked to angle, qualification present. Runs during
  composition and update, unchanged.
- **Publication-risk clearance** — one shared check invoked by `approve_rendition` and
  `record_publication` only.

That preserves the ticket 13/14 anti-drift pattern — one function, two gates, no copy — without
teaching the executor that a classification is a prohibition on drafting.

**How the wrong answer nearly shipped, recorded because the mechanism matters more than the
miss:** the helper's docstring says *"Used by `approve_rendition` (clearance) and
`record_publication` (ticket 14)"* — naming two of its four callers. The steward read the
docstring, not the call sites, and recommended the gate on that basis. **F-78:** correct that
docstring, and treat a stale caller list in a deliberately-shared function as a defect rather
than untidiness.

---

## D21's paraphrase of VISION §13 is corrected

D21 says: *"§13 states the rule directly: `unknown` and `living_private` are non-publishable."*

VISION §13 is narrower. It says those two are non-publishable **as cross-case connections** — a
rule about the No Coincidences surface, in a section about entity resolution and connection
publishability.

D21 legitimately adopted the pair for inference inheritance; its wording made a narrow rule look
like a universal one, and that misreading is what produced F-65's framing in the first place.
D21 is amended in place to quote §13 with its qualifier.

## The constant is renamed

`NON_PUBLISHABLE_PUBLICATION_RISKS` is a universal-sounding name for an inference-inheritance
rule, and it is precisely what tempted the "just reuse the existing set" implementation.
Rename to `INFERENCE_RISK_INHERITANCE_BLOCKERS` (or equivalent) when this is implemented, so
the next reader cannot mistake its scope. D21's grouping stays correct in D21's context: an
inference may launder neither value into `institution`, `public_figure`, or anything else.

---

## Consequences

**Ticket 16's rubric must describe the mechanism that will exist**, including that `unknown`
fails closed at clearance and `living_private` requires a human override, plus the
anti-laundering sentence for `desk_inference` and an explicit statement that no
publication-risk value is authorization to publish. Wording is in the ticket 16 issue file.

**Implementation is a separate ticket**, not folded into 16. It touches clearance, publication,
a new child table, and the client — a different blast radius from rubric artifacts, and ticket
15 already demonstrated what happens when a ticket absorbs an adjacent decision mid-cycle.

**Vela is the reason this was settled now.** A 1979–1980 case with named officials, panel
scientists, and witnesses is the first run producing ordinary claims about identifiable people.
D21's own reasoning holds that an unconfirmed claim about a `living_private` person is *more*
dangerous published than a confirmed one; until this ships, the architecture agrees in prose and
not in code.
