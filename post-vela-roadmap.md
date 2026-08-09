# The Discrepancy Desk — Post-Vela Roadmap

**Status:** discussion record, not tickets. Written 2026-08-07, after tickets 01–14 shipped.

> **Dated artifact — read the corrections first (added 2026-08-09).** This is preserved as the
> reasoning record it was, not as current state. Four things in it have since changed:
>
> - **§7 (F-57) is settled** — see `decisions/corroboration-proposition-scoped.md` (D24).
>   Corroboration is proposition-scoped and judged against the Record; no relation object or
>   candidate finder before Vela. The premise stated here — that each claim is correctly
>   `single_source` because a claim is about what one document says — **is wrong** and is what
>   kept F-57 open for five tickets. D14 permits bindings across captures; `single_source`
>   means one underlying evidentiary *path*.
> - **"Nine sources" is wrong.** The v1 audit's nine `source_records` were per-work-item rows
>   with repeated locators. The benchmark is seven external sources plus owner notes — F-68,
>   frozen in ticket 17 and `reference/vela-v1-runbook.md`.
> - **Ticket ordering has moved.** 15 shipped and grew to include governed non-capture outcomes
>   (D22); 15a (F-67, charset) and 15b (F-73, lease) were added; 18 will enforce D23.
> - **§1's open question is answered.** D22 settles that captured content enters the Vault as an
>   ordinary capture; the extension is a second door, as `add_lead` was in D18.
>
> Everything else below stands as written.

**Standing constraint on everything here:** tickets 15 (capture acquisition receipt) and 16
(rubric artifacts) come first, then 17 (the Vela run). Nothing in this document is on the path
to the first published post. The v1 failure was 309 documents and nothing shipped; every item
below is deliberately parked behind the run.

---

## 1. Chrome extension — capture side

**Decision: build after Vela. Small ticket.**

A browser extension that drops the current tab into the lead inbox is `add_lead` with a
keyboard shortcut — same door, same SSRF guard, same identity-only handling for walls.

**It closes a real gap rather than being a convenience.** D19 records that soft `200 OK`
paywalls are captured as ordinary material because the server cannot detect them, and that
automatic detection was rejected (a false positive destroys unrecoverable material). An
extension captures what *the operator's own session* sees — past the wall, legitimately.

**Design constraint:** the extension and the eventual X API must deliver the same shape to the
same Desk endpoint. Two ingestion paths will drift the way every two-artifact-one-contract
seam in this project has (F-51, F-54, F-59).

**Open question before code:** does captured X content enter the Vault as an ordinary capture
— byte-exact, hashed, locator-addressable — or is it a new kind of object? Instinct: it is a
capture and the extension is a second door, exactly as `add_lead` was in D18. If so, a decision
record is cheap; if not, it needs one before any code. *(Answered by D22 — see header.)*

## 2. Chrome extension — reply drafting

**Decision: not until after the first post has been replied to.**

A reply is content going out under the brand. Everything tickets 12–14 built exists so that
never happens without exact-content clearance: composed from confirmed claims, cleared as the
text that will appear, recorded with what actually posted.

An extension that drafts replies is either **inside** that pipeline or **outside** it. Outside,
it is a second publishing path with none of the guarantees — and "it's just a reply" is exactly
how an exception becomes the norm. VISION is explicit that a rendition is the only publishable
artifact.

Inside, it is coherent: a reply is a rendition with a short unit count and a parent post as
context. Same clearance, same recording.

**Why wait:** you do not yet know what a reply needs to be. Building response machinery before
the first post is the v1 shape.

## 3. Replies as material for public questions

**The genuinely valuable near-term use of captured X content**, and it needs nothing new.

§7's public question is an observation about the discourse — what is circulating, where it is
asked, what version, from what origin. Ticket 11 requires every public question to link at
least one claim. Right now supporting one means hand-capturing forum posts.

An extension that captures a reply thread gives public questions the evidence they need, and
it enters through the existing lead-and-claim path with nothing new to govern.

## 4. Metrics

**Decision: after Vela plus two or three more posts. Measure to learn; keep selection human.**

§16 lists performance data as recorded but not a governed loop. §18 reserves metrics ingestion,
engagement-informed angle selection, and performance-aware composition — reserved, not
rejected.

**The join key already exists.** Ticket 14 records external post identity and canonical URL per
unit. A metrics ticket pulls engagement against those identities and shows it beside the
rendition — which claims were in the thread, what source basis, which angle. That is the read
worth having: *threads resting on contemporaneous records outperformed inference-heavy ones*,
or the reverse.

**The trap, named explicitly.** Once engagement feeds back into angle selection, the system
optimises for what performs. What performs on X is not what is true, and the entire
architecture — capture-then-cite, confirmation at use, publication risk, qualification
surviving to publication — exists to stop that drift. The engagement-optimised version of this
product is much easier to build and already exists a thousand times over.

**So: the system shows; the operator chooses.** No automated angle selection from performance
data.

**On revenue.** Engagement is not revenue. Substack subscriptions are, and Substack is already
the second platform in the plan. The metric that matters is probably conversion from thread to
subscriber, not likes — worth knowing before building a likes dashboard.

## 5. Topic pipeline and calendar

**Decision: revisit after Vela. Build the small piece, resist the large one.**

Two different things wearing one word:

**The pipeline** is mostly derivable today. Cases have coverage readings; renditions have
clearance standing. "What could go out next" is a view, not new state.

**The calendar** — specific content on specific dates — is genuinely new state and is the
smaller piece.

**The reactive case is the important one.** A government UAP release drops and you want to be
out fast. Everything the Desk does is friction, and that friction is the product — speed is how
a desk starts publishing things it cannot stand behind.

**So the answer is not a fast path. Standing depth is what makes you fast.** If official
foundation is already complete on a topic — institutional record captured, claims confirmed,
quotations shelved — a new release is one run, one angle, one thread. Hours. Starting cold is
starting cold, and no scheduling feature changes that.

**Build:** a topic backlog (title, why it matters, whether a case exists), scheduled slots on
cleared renditions, and a readiness view — which cases could produce a thread this week and
what blocks the ones that cannot.

**Do not build:** a full editorial calendar with drafts assigned to dates. Renditions do not
exist until an angle is chosen, and angles do not exist until foundations are complete. A
calendar of dated placeholders that cannot be filled is planning as a substitute for shipping.

## 6. Source monitoring

**Decision: after Vela. Shape is already settled; the new parts are small.**

A source watcher is the lead inbox with a timer — something checks a page, notices a change,
drops a lead. Same door, same capture-on-drop, same triage.

**Change detection is free.** Every capture is SHA-256 hashed (D3). Fetch, hash, compare to the
last capture of that URL. No heuristics, no semantics. An unplanned consequence of byte-exact
storage.

**Candidate sources for the beat:** National Archives and CIA reading room, NARA's JFK
collection, AARO and DoD releases, GAO reports, the Federal Register, agency FOIA reading
rooms. Most publish index pages that change when something lands.

**Two cautions:**

- **Ticket 15 matters more here than anywhere.** A capture records the requested URL and not
  the final one after redirects. A document release page redirecting to a dated archive path is
  exactly where that gap bites. *(Shipped — D22.)*
- **A watcher spends budget without an operator asking.** `add_lead` deliberately does not
  charge a run's budget, with a `TODO` for a per-run drop cap that was never set. Forty sources
  checked daily makes that number urgent rather than theoretical.

**It pairs with the topic backlog.** A watched source list *is* a beat, and monitored topics
are the ones whose foundations get built during quiet weeks.

## 7. F-57 — cross-document corroboration

> **Settled 2026-08-09 as D24** — `decisions/corroboration-proposition-scoped.md`. The section
> below is preserved as the reasoning that preceded it. Its central premise is wrong; see the
> header.

**Deferred, design question open. Needs deciding before Vela.**

Two independent primary documents can assert the same thing, and there is no vocabulary for
the operator to record that they corroborate each other. A live executor hit this on the first
real run: it classified each claim `single_source` — correct under D4, since each claim is
about what one document says — then said plainly it lacked the vocabulary and left the
judgement to the operator. §12 reserves "decide whether sources are genuinely independent" to
the human and gives it nowhere to land.

**Two views on the record, and they compose:**

- **Implementer:** a first-class human-only relation between claims, with actor, timestamp,
  note, append-only when re-ruled — the `claim_confirmations` pattern. Argued against a field
  on the claim (asymmetric writes, blurs D4's per-claim basis) and against a property of the
  angle (independence is about sources, not framing).
- **Outside review:** the system finds candidate pairs; the operator attests. Judgement stays
  human, finding is automated.

**Two questions to settle before sizing:**

1. Is independence a property of a **pair**, or of a pair **for a given proposition**? Two
   documents can be independent about one fact and share a source on another — a wire report
   quoting an agency statement. Per-pair is simple and sometimes wrong; per-proposition is
   honest and reintroduces the volume problem the confirmation surface was designed around.
2. Does an independence ruling feed back into the claim's `corroboration` dimension, or stay
   separate? Separate is correct on D4 grounds — but then the ruling lives somewhere nobody
   looks, which is F-44's shape.

**Constraint on candidate-finding:** it cannot use semantic matching. "Same proposition family"
needs embeddings or a backend model call, and backend-as-LLM-client was refused at ticket 09
and again at ticket 12. Structural signals only — claims on the same angle citing captures from
distinct domains.

## 8. Qdrant / semantic retrieval

**Still deferred, and the conditions are unmet.**

`architecture-decisions.md` lists it as deferred until a governed corpus and a measured
retrieval need exist. Neither holds: after Vela the corpus is one case and a few dozen claims —
readable by hand. And nothing needs to find things by meaning; `read_case_context` serves the
executor by structure.

**The honest trigger to watch during Vela:** if the executor visibly struggles to find prior
material within a case, that is a measured retrieval need and the door opens. If it does not,
the deferral was right.

**The retrofit stays cheap.** Captures are byte-exact and hashed, elements are
locator-addressable, claims carry rubric version and lineage, confirmation history records
proposed and confirmed separately. An embedding layer would index what already exists.

---

## Ordering, if it all happened

*(Superseded — see header. Preserved as written.)*

1. **Ticket 15** — capture acquisition receipt (gates Vela; provenance cannot be backfilled)
2. **Ticket 16** — rubric artifacts, including F-56's classification vocabulary
3. **F-57 decision** — before Vela, since nine sources on a contested topic will hit it
4. **Ticket 17** — the Vela run
5. **UX pass** — see the separate design brief; first input is operator friction from the run
6. Capture-side extension, source monitoring, topic backlog
7. Metrics, after several posts exist to compare
8. Reply drafting, after the first reply exists to respond to
