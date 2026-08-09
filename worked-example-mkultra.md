# The Discrepancy Desk — A Worked Example

**Status:** editorial illustration. Explains the product in terms of the objects the system
actually has.

**Why this exists:** VISION describes the architecture. It does not show what a finished
investigation looks like passing through it. This does — using MKUltra, because its shape is
the shape of most Discrepancy Desk cases: an admitted official record, a persistent public
belief that the record is incomplete, and a hearing where the gap between them becomes visible
in the transcript.

> ## ILLUSTRATIVE — NOT RESEARCH FINDINGS
>
> **MKUltra is a real subject and nothing below is researched.** Every claim in this document
> is a placeholder showing the *form* a finished case takes. Real claims would be captured,
> quoted byte-exact, classified, and confirmed by the operator.

---

## How work actually moves

The most common misreading of this system is that research proceeds through numbered stages.
It does not. **Coverage is a gauge, not a state machine** (D5, D20). Multiple runs can work the
same dimension, dimensions are worked out of order, and a case can be deep into editorial work
while its official foundation reopens because a new document surfaced.

What follows is one plausible path through a case, not a sequence the software enforces.

---

## Run A — establish the official foundation

*Operator dispatches a run with `coverage_dimension = official_foundation`, a bounded question,
and a scope.*

The executor captures the primary documents: the Church Committee report, the 1977 Senate
Select Committee hearing, released agency documents, the destruction order and what survived it.

Claims proposed look like:

| Claim | Source basis | Certainty | Posture | Publication risk |
|---|---|---|---|---|
| The program existed under that designation | `contemporaneous_record` | `established` | `factual_assertion` | `institution` |
| Records were destroyed on a director's order in 1973 | `contemporaneous_record` | `established` | `factual_assertion` | `public_official_official_capacity` |
| Surviving financial records were located separately from destroyed operational files | `contemporaneous_record` | `established` | `factual_assertion` | `institution` |
| The official account states the program was terminated | `contemporaneous_record` | `established` | `factual_assertion` | `institution` |

Each binds to an exact quotation with a locator (`e/{n}/r/{start}-{end}`). Each is confirmed at
use by the operator, who accepts or corrects every dimension.

**The gate:** angle work is hard-blocked until the operator attests official-foundation coverage
complete. That is the one absolute rule in the system — no interpretation before the official
record is worked. Attestation is a human act, not a computed threshold.

## Run B — work the public question

*A separate run, `coverage_dimension = public_question`.*

The public question is **not a claim about the world**. It is an editorial observation about the
discourse: what is circulating, where it is asked, in what version, traceable to what origin.

> **Question circulating:** that the program continued after its stated termination, under other
> designations.

It rests on claims about the discourse itself — each captured and quoted, so "people believe X"
cannot float free of evidence that they do.

This is where a Discrepancy Desk case differs from ordinary reporting. The belief is treated as
a **documented phenomenon** rather than as either true or false.

## Run C — work a later hearing

*`coverage_dimension = official_foundation` again. Same dimension, different run, later material.*

This is the point about coverage being a gauge: nothing "advanced" past official foundation. A
new hearing is new institutional record, so it is foundation work, and the gauge may return to
`worked` if it produces unexamined captures.

A hearing transcript produces claims the earlier documents cannot — and it forces a distinction
the rest of this system exists to protect:

| Claim | Source basis | Why |
|---|---|---|
| Senator X asked whether Y | `contemporaneous_record` | The question is in the transcript |
| The witness declined to answer Z, citing W | `contemporaneous_record` | The refusal is in the transcript |
| The witness's three-paragraph answer did not resolve Y | `desk_inference` | Whether an ostensibly responsive answer actually answers is a judgement |

**A recorded refusal to answer is a fact. Whether an ostensibly responsive answer actually
answers the question may be an inference.**

That line is the whole discipline in miniature. The transcript is one thing; what the Desk
concludes about the transcript is another, and they carry different source bases, different
confirmation burdens, and different publication constraints.

## The Record — proposing the discrepancy

*An inference claim: `source_basis = desk_inference`.*

> Among the official materials worked for this case, the Desk found the question asked directly
> and did not find an official answer to it.

Note what that sentence does **not** say. It does not say the official record contains no
answer. One hearing, or one corpus, establishes nothing about everything that exists.

**Absence is always bounded by the corpus actually worked.** "The record does not say" is a
materially stronger claim than "the sources we examined did not say," and only the second is
supportable from a case's own captures. Bounding it is not hedging — it is the difference
between a claim the Desk can stand behind and one it cannot.

This is the claim the architecture exists to make safely:

- It cites claims rather than captured bytes — the only kind of claim permitted to do so (D14).
- **Every claim it cites must be confirmed first.** No inference over unconfirmed material.
- **It inherits publication risk categorically.** If any cited claim is `unknown` or
  `living_private`, the inference must be too. No laundering (D21).
- Its `source_basis` cannot change at confirmation. Kind is fixed at proposal; confirmation
  corrects strength, not kind.

So the interpretive claim — the one carrying the most weight and the most risk — is the most
constrained object in the system.

## Angle Room — the editorial decision

*Human-only. Opens once official-foundation coverage is attested complete.*

> **Angle:** The question was asked and not answered. That is different from the question being
> settled.

Angle Room items link to the claims they rest on and inherit their dimensions. *The Angle Room
may make a story vivid; it may not launder a weak claim.* An angle with no linked confirmed
claim cannot be chosen.

The quotation shelf holds quotations the operator **selected** — with speaker and attribution
frame — not an automatic dump of every binding.

## Rendition — composed for one platform

Composed natively for X by the executor under a composition rubric. The backend never generates
text.

Every unit cites only **confirmed claims linked to that angle**. Confirmation belongs to the
claim; eligibility is angle-scoped. Required qualification language must appear in the unit that
cites the claim requiring it.

Renditions are independent artifacts, never cut down from a master draft (D2). A Substack piece
or a video script on the same angle is composed separately.

## Clearance — the human binds exact content

The operator reviews the rendition, edits if he wants, and clears. **What is cleared is the text
as it will appear**, and the edited text is what gets bound.

Clearance is an append-only record carrying the actor, the timestamp, and the ordered unit
bodies as approved. Re-clearing appends; it never overwrites.

## Publication — recording what actually went out

Posted manually. The operator then records, per unit: external post identity, canonical URL,
published time, verification state. The publication set binds the specific approval that
authorized it.

---

## The lineage, end to end

```
Run
  ↓
Capture ──→ exact locator  e/{n}/r/{start}-{end}
  ↓
Claim
  ↓
Human confirmation  (proposed vs confirmed, both recorded)
  ↓
Angle
  ↓
Rendition ──→ ordered Units
  ↓
Approval #2 ──→ exact ordered bodies, actor, timestamp
  ↓
Publication ──→ approval_id #2
  ↓
External post IDs / canonical URLs
```

That last edge — publication binding the approval that authorized it — is why clearance is a
durable human act rather than a status flag. On a rendition cleared three times, the Record can
still say which clearance authorized what went out.

## What happens when something changes

| Change | Effect |
|---|---|
| Edit unit text after clearance | Clearance standing becomes false, stating what diverged. Nothing silently reverts to draft |
| Reorder or add/remove units | Same — order and membership are bound, not just bodies |
| Re-clear after editing | A new approval row. The first is never mutated |
| Re-confirm a cited claim with stricter qualification | Bodies may still match the approval, but publication refuses until the text satisfies the new qualification |

That last row is the one worth understanding. **Material underneath a decision can change
without anyone touching the decision.** A clearance can go stale because a claim moved, exactly
as a coverage attestation goes stale because new captures arrived. Every gate revalidates
against current state rather than trusting that an earlier check is still fresh.

---

## What this shape gives you that ordinary reporting does not

**The belief is documented, not dismissed or endorsed.** Most coverage of a topic like this
either treats the public question as a fringe curiosity or adopts it. The Desk records it as a
phenomenon with evidence, alongside the official record, and lets the discrepancy between them
be the story.

**The gap is a citable object.** "The materials worked for this case do not answer this" is a
claim with a source basis, a certainty, and quotations behind it — not a rhetorical gesture.

**Unconfirmed claims remain visible as research substrate.** They are loudly marked as
model-proposed and unconfirmed in the operator surface, and they cannot support a rendition
until confirmed at use.

**Every factual unit has a receipt trail.** A published unit traces to confirmed claims, to
byte-exact quotations, to a capture with a hash and a timestamp. The reader does not have to
trust the narrator.

---

## Where the boundaries are

**Within one case, this all works.** Official record, discourse, hearing, inference, angle,
rendition, clearance, publication — every object exists.

**Across cases, it does not, and that is deliberate.** A name appearing in two investigations, a
document referenced by both, a pattern spanning topics — that is No Coincidences, explicitly
closed. The stated reasoning: *the system stores; the human notices.* Cross-case pattern
detection was a failure mode the reboot was built against, and a "bigger picture" that means
across cases is standing on that line.

**Corroboration across documents within a case is a known gap (F-57).** Two independent primary
sources asserting the same thing are each correctly `single_source`, because each claim is about
what one document says — and there is nowhere for the operator to record that they corroborate
each other. A live executor hit this on the first real run and said so rather than inflating the
classification.

**Publication risk is a separate control, and its enforcement is currently partial (F-65).**
VISION makes `unknown` and `living_private` non-publishable. D21 enforces that categorically for
inference claims. Nothing enforces it for ordinary claims — one about a `living_private` person
can be confirmed, linked to an angle, cleared, and published. Any public surface must enforce
the non-publishability rules rather than route around them, and the gate itself needs deciding.

---

## Why Vela is the right first run

Vela has this exact structure: an official finding, a scientific panel that examined the same
evidence and disagreed, and forty-seven years of unresolved argument. The discrepancy is not
alleged — it is on the record, between two official bodies.

So the rehearsal tests two things at once. Whether the machinery works, and **whether the
machinery holds the shape of story the Desk is for.**

Worth watching specifically during the run: does the Angle Room let the operator set an official
finding against a documented dissent cleanly, or does it fight him? That is a product answer no
test can give.

---

*Implementation provenance: the objects described here were built across tickets 01–14. The
official-foundation gate and coverage measurement are D20; inference publication risk and the
kind boundary are D21; the lead inbox and its transport are D18 and D19. F-57 and F-65 are open
findings recorded in the steward handoff.*
