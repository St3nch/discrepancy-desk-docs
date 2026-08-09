# The Discrepancy Desk — A Worked Example

**Status:** editorial illustration. It explains the product in terms of the objects the system
actually has, and it is the intended onboarding document for anyone joining the project.

**Why it exists:** VISION describes the architecture. It does not show what a finished
investigation looks like passing through it. This does — using MKUltra, because its shape is
the shape of most Discrepancy Desk cases: an admitted official record, a persistent public
belief that the record is incomplete, and a hearing where the gap between them becomes visible
in the transcript.

> ## ⚠ ILLUSTRATIVE CLAIMS — NOT RESEARCH FINDINGS
>
> **Nothing in this document has been researched.** Every claim below is a placeholder showing
> the *form* a finished case takes. Real ones would be captured, quoted byte-exact, classified,
> and confirmed by the operator. Do not cite anything here.

---

## The work is loops, not stages

Before the example: **coverage is a gauge, not a state machine.** Research stages can be worked
in any order and revisited, and several runs can target the same dimension. A case is not a
pipeline that advances.

That matters for reading what follows. The hearing below is worked by a *second*
official-foundation run, not by a later stage — because a hearing is more institutional record,
arriving after the earlier record was already worked. Coverage does not march forward; it gets
deeper in places.

The one absolute ordering rule: **angle work is hard-blocked until the operator attests
official foundation complete.** No interpretation before the official record is worked.

---

## The case: "MKUltra — the admitted record and the persistent question"

### Run A — Establish the official foundation

*Dispatched by the operator with a question, a scope, and `coverage_dimension =
official_foundation`.*

The executor captures the primary documents: the Church Committee report, the 1977 Senate
Select Committee hearing, released agency documents, the destruction order and what survived
it.

| Claim | Source basis | Certainty | Posture | Publication risk |
|---|---|---|---|---|
| The program existed under that designation | `contemporaneous_record` | `established` | `factual_assertion` | `institution` |
| Records were destroyed on a director's order in 1973 | `contemporaneous_record` | `established` | `factual_assertion` | `public_official_official_capacity` |
| Surviving financial records were located separately from destroyed operational files | `contemporaneous_record` | `established` | `factual_assertion` | `institution` |
| The official account states the program was terminated | `contemporaneous_record` | `established` | `factual_assertion` | `institution` |

Each binds to an exact quotation with a locator (`e/{n}/r/{start}-{end}`). Each is confirmed at
use by the operator, who accepts or corrects every dimension — and both the proposal and the
correction are kept.

### Run B — Work the public question

*A different coverage dimension. Not a claim about the world — an observation about the
discourse.*

> **Question circulating:** that the program continued after its stated termination, under other
> designations.
>
> **Where asked:** [captured discourse sources]
>
> **Origin:** [traced to the earliest captured articulation]

This is where a Discrepancy Desk case differs from ordinary reporting. The belief is treated as
a **documented phenomenon** — something that demonstrably circulates — rather than as either
true or false. It rests on claims about the discourse itself, each captured and quoted, so
"people believe X" cannot float free of evidence that they do.

### Run C — Work the later hearing

*`official_foundation` again. Same dimension, new material, and this is the sharp part.*

A hearing produces claims of a kind the earlier documents cannot:

| Claim | Source basis | Posture |
|---|---|---|
| Senator X asked whether Y | `contemporaneous_record` | `factual_assertion` |
| The witness declined to answer Z, citing W | `contemporaneous_record` | `factual_assertion` |

**A recorded refusal to answer is a fact.** "I decline to answer that question" is in the
transcript, quotable byte-exact. Not inference, not accusation — what the record says happened.

**Whether an ostensibly responsive answer actually answers the question is usually an
inference.** If a witness gives three paragraphs that sound responsive and never resolve Y,
"the response did not address Y" is the Desk's judgement about the transcript, not the
transcript itself. It belongs in a `desk_inference` claim citing the quoted exchange — not in a
`contemporaneous_record` claim.

That distinction is the system in miniature: **the transcript and what the Desk concludes about
the transcript are different objects with different evidentiary standing.**

### The Record — propose the discrepancy

*An inference claim: `source_basis = desk_inference`.*

> Among the official materials worked for this case, the Desk found the question asked directly
> and did not find an official answer to it.

Note how tightly that is bounded. **Absence claims are bounded by the corpus actually worked.**
"The record does not say" is a far stronger claim than "the sources we examined did not say,"
and only the second is supportable. One hearing failing to answer something establishes nothing
about whether an answer exists elsewhere. Coverage readings are what make the bound honest —
they say which dimensions were worked and how deeply.

This is the claim the whole architecture exists to make safely:

- It cites claims rather than captured bytes — the only kind of claim permitted to do so.
- **Every claim it cites must be confirmed first.** No inference over unconfirmed material.
- **It inherits publication risk categorically.** If any cited claim is `unknown` or
  `living_private`, the inference must be too. No laundering.
- Its kind is fixed at proposal. Confirmation corrects how strong a claim is, never what it is.

So the interpretive claim — carrying the most weight and the most risk — is the most constrained
object in the system.

### The Angle Room — choose the editorial angle

*Human-only. Opens only once official foundation reads complete.*

> **Angle:** The question was asked and not answered. That is different from the question being
> settled.

Angle Room items link to the claims they rest on and inherit their dimensions. *The Angle Room
may make a story vivid; it may not launder a weak claim.* An angle with no confirmed claims
cannot be chosen.

The quotation shelf holds the strongest quotations the operator selected — with speaker and
attribution frame — not every binding automatically.

### Rendition → Clearance → Publication

Composed natively for X, by the executor, under a composition rubric. Every unit cites only
**confirmed claims linked to that angle**. Required qualification language must appear in the
unit that cites the claim requiring it.

The operator clears **exact content** — the text as it will appear. He may edit first, and the
edited text is what gets bound. Clearance is a durable human act with the ordered bodies
recorded, not a status flag.

Posted manually. The operator records what actually went out: external post identity, canonical
URL, published time — pasted from the platform, never generated.

---

## The lineage, end to end

```
Run (operator-dispatched, coverage dimension set)
  ↓
Capture → byte-exact bytes, SHA-256, addressable elements
  ↓
Claim → bound to an exact quotation locator
  ↓
Human confirmation → proposed and confirmed both kept
  ↓
Angle → chosen, resting on confirmed claims
  ↓
Rendition → ordered units, each citing angle-linked confirmed claims
  ↓
Approval #2 → exact ordered bodies, actor, timestamp
  ↓
Publication #1 → binds approval_id #2
  ↓
External post IDs / canonical URLs
```

That last edge is the one worth staring at. **A publication binds the specific approval that
authorized it** — not the rendition's current state. On a rendition cleared three times, the
Record can still say which human act authorized what went out.

---

## What happens when something changes

| Change | Effect |
|---|---|
| Edit unit text after clearance | Clearance no longer stands — derived by comparison, not by a flag |
| Reorder units | Same: the published artifact would differ even though every unit reads unchanged |
| Add or remove a unit | Same |
| Re-clear after editing | A **new** approval record. The first is never overwritten |
| Re-confirm a cited claim with stricter qualification | Bodies still match the snapshot, so the clearance appears to stand — but publication refuses until the text carries the new qualification |

That last row is the interesting one. The material underneath a clearance can change without
anyone touching the rendition. So publishability is revalidated against current claim state at
both clearance and publication, rather than trusted from an earlier check.

---

## What this shape gives you that ordinary reporting does not

**The belief is documented, not dismissed or endorsed.** Most coverage of a topic like this
either treats the public question as a fringe curiosity or adopts it. The Desk records it as a
phenomenon with evidence, alongside the official record, and lets the discrepancy between them
be the story.

**The gap is a citable object.** "The materials we worked do not answer this" is a claim with a
source basis, a certainty, and quotations behind it — not a rhetorical gesture.

**Unconfirmed claims remain visible as research substrate.** They are loudly marked as
model-proposed and unconfirmed, and they cannot support a rendition until confirmed at use.
That visibility is what makes the confirmed ones mean anything.

**Nothing rests on the narrator.** The reader does not have to trust the Desk. Every factual
unit has a receipt trail.

---

## Where the boundaries are

**Within one case, this all works today.**

**Across cases, it does not, and that is deliberate.** A name appearing in two investigations, a
document referenced by both, a pattern spanning topics — that is explicitly closed. The stated
reasoning: *the system stores; the human notices.* Cross-case pattern detection was a failure
mode the design was built against, and a "bigger picture" that means across cases is standing on
that line.

**Corroboration across documents within a case is a known gap (F-57).** Two independent primary
sources asserting the same thing are each correctly `single_source`, because each claim is about
what one document says — and there is nowhere for the operator to record that they corroborate
each other. A live executor hit this on the first real run and said so rather than inflating the
classification.

**Publication risk is not yet a universal gate (F-65).** It is enforced categorically for
inference claims. Ordinary claims are ungated — an ordinary claim classified `living_private`
can today be confirmed, linked, cleared, and published with nothing refusing it. Any public
surface must enforce the applicable non-publishability rules rather than route around them, and
where that gate belongs is an open decision.

---

## Why Vela is the right first run

Vela has this exact structure: an official finding, a scientific panel that examined the same
evidence and disagreed, and decades of unresolved argument. The discrepancy is not alleged — it
is on the record, between two official bodies.

So the rehearsal tests two things at once. Whether the machinery works, and **whether the
machinery holds the shape of story the Desk is for.**

Worth watching specifically: does the Angle Room let the operator set an official finding
against a documented dissent cleanly, or does it fight him? That is a product answer no test can
give.

---

## Implementation provenance

Recorded here rather than in the body, so the narrative describes enduring system rules.

- Coverage as a gauge, operator-set run dimensions, attested completion — D20
- Inference publication-risk inheritance, bottom-up confirmation, kind fixed at proposal — D21
- Inference claims may cite claims rather than captures — D14
- Public questions must link at least one claim; angles must rest on confirmed claims — ticket 11
- Angle-scoped rendition eligibility, qualification present in the citing unit — tickets 11–12
- Clearance as append-only ordered-body snapshot, standing derived by comparison — ticket 13
- Publication binds its authorizing approval; revalidation at both gates — ticket 14
- Written 2026-08-07, revised after spec-axis review. Objects current as of ticket 14.
