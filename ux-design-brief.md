# The Discrepancy Desk — UX Design Brief

**Status:** design input, not a ticket. Written 2026-08-07, after tickets 01–14 shipped and
before the Vela run.

> **Dated artifact (note added 2026-08-09).** Preserved as written. Two things have moved since:
> the client has gained a "Reached, not captured" section on the case page (D22, ticket 15), and
> `claimCard()`'s missing `corroboration` dimension is now a ticket 16 ride-along (D24). §2's
> queue and §3's dual-pane surface remain unbuilt and remain the highest-value items.

**Timing:** the first pass belongs *after* the operator has walked the seeded case end to end
in a browser, not before. Design from friction, not from a blank page. VISION's own test is
that the owner opens the application and produces work worth posting — a working backend
behind a screen that fights him fails that test as completely as broken code would.

**Where this sits in the record:** `architecture-decisions.md` lists the web UX model as open
fog, with Notion-style tiles as a reference point and the reason stated — a case holds
heterogeneous contents (captures, claims, open questions, angles, renditions) that a tiled
surface handles better than a form. Nothing here is decided; this is the input for deciding.

---

## 1. The diagnosis

The current client is not ugly so much as **inverted**. The first screenshot showed three
panels of equal weight: lead inbox largest and loudest at the top, create-case in the middle,
cases smallest and empty at the bottom.

That hierarchy reflects **the order tickets were built**, not the order the operator works.
It is the honest cost of tracer-bullet development — nobody ever designed a page; nine tickets
each appended a panel.

The deeper problem: **the app has no idea what is waiting on the operator.** Every screen is a
projection of a case. But the morning question is not "what is the state of case 1," it is
*what needs me right now* — and answering that currently means opening each case and reading
six panels.

---

## 2. The Desk — a home queue ordered by who is blocked

The single highest-value change, and it is not cosmetic. Everything else in the app can stay
as it is and this alone changes the job.

### Blocked on you
A **suspended run** is an executor frozen mid-work waiting for a scope ruling. Nothing else in
the system has another party waiting. Loud, and first.

### Ready for your judgement
- Completed runs with a research agenda to review
- Claims unconfirmed and blocking angle work
- Coverage reading `worked` that could be attested
- Renditions cleared but not recorded as published

### Gone stale
- A clearance that no longer stands (text, order, or membership changed)
- An attestation invalidated by new unexamined material

These are the ones that silently rot. Today they are only findable by looking.

### Ambient
Leads in the inbox.

Each row: the case, the thing, the single action. Nothing else.

---

## 3. Confirmation — the dual-pane verification surface

This is where the operator's time actually goes, and §12 says it is where output pressure will
attack: *when a post is due and fifteen unconfirmed claims stand in the way, the temptation is
to confirm faster and looser.*

**The pattern to copy** comes from AgentSLR, a research prototype for LLM-assisted systematic
review. Dual-pane: source document on the left, structured extraction fields on the right,
AI-predicted entries prefilled with **highlighted evidence excerpts** from the source.
Reviewers accept, revise, or reject individual fields, with a status indicator showing whether
human intervention was required. The document viewer keeps the original visible so context
never requires a screen switch.

That is the claim confirmation screen, exactly. And the Desk already has everything it needs:
byte-exact captures, region locators (`e/{n}/r/{start}-{end}`), and `claim_confirmations`
recording proposed against confirmed.

**Requirements:**

- **Show the quote in place.** The exact quotation highlighted inside the captured element,
  with source URL and capture time beside it. One click from claim to bytes is the entire
  product promise; it should be one click.
- **Accept-as-proposed is one action; correcting expands.** Most claims are fine. The ones
  changed are the signal.
- **Show the correction rate as it accumulates.** "You've corrected certainty on 6 of 14
  claims in this case." That is D9's drift view, more useful during the work than in a report
  afterwards.
- **Never a confirm-all button.** The friction is the feature.

### Two numbers worth stealing from HITL practice

From current human-in-the-loop literature, with thresholds attached:

- **Override rate below ~5%** usually means the gate is unnecessary.
- **Override rate above ~30%** means the *instructions* are wrong and the rubric needs
  revision — not more human review.
- **A rising incident rate against a stable override rate is the clearest signal that
  approvers are rubber-stamping.** §12 predicts exactly this failure and currently gives no
  way to detect it.

These map directly onto ticket 16's rubric tuning.

**What not to take from that literature:** it is built for reviewer *teams* — routing, SLAs,
specialist queues, capacity planning. The Desk has one operator. And **confidence-gated
auto-approval is against doctrine**: every claim is confirmed at use by a human, and no score
gets to skip that.

---

## 4. Naming

Some jargon is load-bearing and stays. "Rendition" means *independently composed per platform,
not cut down from a master* — calling it "draft" would lose the distinction D2 exists to
protect. Keep the term, let the subtitle carry the meaning.

| Current | Show instead |
|---|---|
| Cleared | **Approved — exact text** |
| Attest coverage | **Mark this stage complete** |
| `identity_only` | **URL only — login wall, nothing captured** |
| `unsupported_type` | **URL parked — can't read this format** |
| Coverage dimension | **What this run is for** |
| Source basis | **What it rests on** |
| Posture | **How to treat it** |
| Publication risk | **Who it's about** |
| Approval stands | **Still matches what you approved** |
| Approval does not stand | **Text changed since you approved** |

**`unconfirmed` stays exactly as it is.** That word should never soften.

---

## 5. Tooltips as the operator's rubric

Ticket 16 ships the classification vocabulary to the *executor* inside the work packet
(F-56). **The operator gets nothing.**

Every enum value the operator sets or reviews should carry its one-line meaning on hover —
**the same text, from the same source**, so the two cannot drift. That is the vocabulary
reconciliation rule applied to the interface.

**Empty states should teach the way refusals do.** Not "No angles yet," but:

> *Angle work opens when the official foundation is complete. Yours reads "worked" — 2
> captures still unexamined.*

The refusal doctrine applied to the screen.

---

## 6. One visual rule worth committing to

**Things the operator decided must look different from things the system computed.**

- **Derived:** coverage readings, clearance standing, capture status, publication gates
- **Authored:** attestations, confirmations, approvals, dismissals, scope rulings

If those share a visual treatment, the interface quietly claims authorship of judgements it
did not make — the one thing this architecture exists to prevent.

This also answers the colour question. **Colour is already spoken for by four state
vocabularies:**

- Captures: cited / examined / unexamined
- Leads: captured / identity-only / unsupported-type
- Coverage: unworked / worked / complete / unmeasurable
- Clearance: stands / does not stand

If colour means *state* consistently, a board tells the operator where a case stands at a
glance — which is what the coverage gauge currently attempts with paragraphs.

---

## 7. Cards — where they fit and where they don't

The instinct toward cards is right and matches the recorded fog item. The constraint:

**Dragging implies changing state, and most state here is not draggable.** Coverage is
derived. Capture status is reported. Confirmation needs dimensions. A board where some cards
move and others refuse would be worse than no board.

**Arrangement is different from state.** The Angle Room is the natural home: claims,
quotations, and public questions as cards the operator arranges under an angle — grouping
evidence with the point it supports, parking dismissed angles. Nothing draggable changes a
status; it changes layout. That sidesteps the trap entirely.

---

## 8. Command palette

Worth building, **after the queue exists**. A palette over a bad hierarchy just hides the
hierarchy.

The profile fits: palettes suit tools with power users who need to reach many features
efficiently, and are not worth it when there are only a few commands. A single expert operator
with a growing surface is the right case. The real benefit is that it **lets the user skip the
linear information architecture** — the actual fix for "open case, scroll six panels."

Standard shortcut (⌘K), fuzzy search, and it should reach every governed operation the
operator is allowed to perform.

---

## 9. What to cut

**Doctrine text in panel subtitles.** *"Unattached material. Captured on drop (always). No
claims until a case and run work it."* That is documentation wearing chrome. The operator
wrote the doctrine; he does not need telling. Move to a help affordance and give the space
back.

---

## 10. Build order

1. **The Desk queue** — changes the job
2. **Dual-pane confirmation** — changes where the time goes
3. **Naming + tooltips + the derived/authored visual rule** — cheap, high clarity
4. **Angle Room cards** — arrangement only
5. **Command palette** — makes it fast

---

## Open question

The client is where the worst defects have landed: F-44 (form couldn't reach a capability),
F-53 (missing capture budget field), and ticket 14's S-01 (client fabricated publication
identifiers). **All three reached the operator rather than a reviewer**, because a green test
suite says nothing about a file no test imports.

Any redesign should decide how the client gets tested, or the same class recurs at greater
volume.

> **Still true, and now four tickets running** — tickets 15, 15b, and 16 have each changed
> `client/src/` with no automated coverage. Whatever else the redesign decides, this is the
> question it must answer.
