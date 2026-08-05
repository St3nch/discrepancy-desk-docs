# Discrepancy Desk v2 — Locked Architecture Decisions

Output of a grilling session against the Vision Reference. Seven decisions, breadth-first
across the destination path. Each records the alternative that was rejected and why, so it
converts directly into an ADR.

Companion to `discrepancy-desk-vision-reference.md` (the extracted v1 material). Where the
two conflict, this document controls — it is later and it is decided.

---

## D1 — Destination

**One complete vertical pass.** One real topic, researched by the connected LLM against the
open web, captured into the Vault with verified locators, claims extracted and classified,
one angle developed, one X thread rendered, cleared by the human, posted manually, recorded.

One case, one platform, one format, end to end.

Truth Social, Substack/Medium, video scripts, Release Watch, metrics, No Coincidences, and
multi-account are all extension off a proven spine, not part of the destination.

**Rejected:** "the whole product" as destination — v1's implicit choice, which closed seven
milestones without ever publishing from the system.

**Rejected:** a lower bar stopping short of publication — it fails to test the two things
most likely to be wrong: whether LLM-produced research survives editorial judgment, and
whether claim-to-locator binding holds when the source is a live web page rather than an
uploaded file.

---

## D2 — Case owns renditions

Two entities, one direction of ownership.

- **Case** — the durable investigation. A topic. Owns sources, claims, entities, conflicts,
  timeline, open questions, and angles. Never completes.
- **Rendition** — one publishable artifact belonging to exactly one case. Carries the
  lifecycle states. Ends published or rejected. An X thread is a rendition; a Substack piece
  is another rendition of the same case.

**Angle** sits between them but is not a third top-level entity — it is Angle Room content
inside the case, linking to claims, with no lifecycle of its own.

The operator opens a **case**. Renditions are objects inside it. Pipeline is a flattened
view of renditions across cases; the review queue is renditions awaiting the human.

**Rejected:** v1's unreconciled split, where the Control Room's `work_item` (12-state
pipeline, ends in published) and the Record's `case` (durable, never ends) were different
shapes describing overlapping things, with "dossier" as a third word.

**Accepted cost:** a quick one-off post still requires a case — a thin one, perhaps one
source and one claim. Kept deliberately, to preserve a single path. The lane concept
(Archive / Docket / Flash) already carries the "this is a fast one" distinction.

---

## D3 — Capture-then-cite

The LLM cannot cite anything it has not first captured. Reading a page means fetching it
through the backend, which stores the raw response bytes, hashes them, parses into the
element structure, and only then permits a claim to bind to a region inside it.

**The research agent's web-read tool *is* the ingestion tool.** Everything read is captured,
whether or not it ends up supporting a claim.

**Rejected — cite-then-verify:** the LLM researches freely and a later pass fetches and
checks the cited URLs. Failure mode is "claim exists, verification failed, now what?" — a
queue of orphaned claims to adjudicate. Capture-then-cite fails closed at write time instead
of triaging at review time.

**Rejected — hybrid (free browsing for orientation, capture only what gets cited):** worst
of the three. Orientation browsing is where the model forms its picture of the topic; if
that is not captured, the Vault holds the footnotes but not the reasoning substrate.

**Second reason for the strict rule:** capturing everything read, not just everything cited,
is what produces an honest corpus denominator. Without it, a future No Coincidences can
never truthfully say "6 of 74 eligible documents" — only the 6 exist.

**Accepted cost:** storage grows fast; junk is captured alongside signal; every read is also
a write, so research is slower. Requires a cheap **captured vs. promoted** distinction so
raw capture does not clutter the case view. (See Fog.)

---

## D4 — Authority at use, not at storage

Claims enter the Record as **LLM-proposed and unconfirmed**. No review gate on entry. Each
carries the model's proposed evidence dimensions and binds to captured bytes, honestly
labeled `unconfirmed`.

**Human confirmation attaches when a claim is used to support published text.** An angle
pulls in ~a dozen claims; those get confirmed — dimensions accepted or corrected,
qualification language checked. **A rendition unit may only cite a confirmed claim.**
Unconfirmed claims are invisible to the composer.

This preserves v1's rule ("only the human may set authoritative evidence dimensions") exactly
where it was aimed — nothing unconfirmed can reach a published post — while keeping the
review surface for one thread at ten or fifteen decisions instead of four figures.

**Rejected — confirm at storage, in bulk, with claims grouped by model confidence:**
bulk-confirming a hundred classifications not individually read is the rubber-stamp behavior
the architecture exists to prevent. Better honestly unconfirmed than dishonestly cleared.

**Accepted cost:** the Record holds a large body of material with model-generated, unaudited
classifications. `unconfirmed` must be visually loud everywhere it appears, and any future
feature reading the Record — No Coincidences especially — must treat confirmed and
unconfirmed as different populations. A pattern candidate built on unconfirmed claims is a
lead about a lead.

**Downstream benefit:** confirmation persists with the claim, so cross-case reuse is cheap.
A claim confirmed for one rendition is already confirmed when a later case pulls it in.

---

## D5 — Question-scoped research runs

Research is a **loop, not a pipeline**. A run has an explicit question or question set and a
bounded scope, and produces (a) captured material and claims, and (b) new open questions.
The human reads the questions, picks which matter, and those shape the next dispatch.

The first run's question is broad ("establish the official record on X"). Later runs get
narrow and are shaped by what came back.

**The five-stage sequence** — official foundation → deep context → story intelligence →
editorial development → composition — **is demoted from a pipeline to a coverage gauge.** It
reports whether the case has an official spine, whether deep context has been worked, whether
story intelligence has been done. A case can be on run seven and still filling the official
foundation, because run four opened a hole in it.

The editorial gate survives unchanged — no angle work before the spine is complete — now
evaluated as a condition against coverage rather than a stage marched through.

**Rejected — one deep autonomous run:** a bad official spine is discovered only after a full
case, three angles, and a draft have been built on top of it. Sunk work is the expensive kind
of wrong. Also puts four differently-shaped jobs (retrieval-heavy, synthesis, generative)
behind one prompt and one tool surface, which is how an agent wanders.

**Rejected — staged but auto-advancing:** a checkpoint that defaults to proceed is not a
checkpoint.

**Requires:**
- **Open questions become a first-class worked object**, with dispositions — promote to a
  run, park, answer from existing material, rule out of scope. Otherwise they pile into an
  unusable list. (Same two-axis disposition shape as No Coincidences.)
- **Run lineage.** Which run introduced which claims, and which question prompted the run.
  Cheap at dispatch time, impossible to reconstruct later.

---

## D6 — Active ↔ dormant cases

Cases do not complete. They go **dormant** — no active runs, out of the working queue, fully
intact and searchable — and wake when a new run is dispatched against them.

**Unanswered is a resting state, not a failure state.** For this subject matter an open
question that stays open for years is frequently the point; "missing records" is already
first-class Angle Room material. Open-question dispositions must therefore distinguish:

- `unresolved — likely permanent` (stable)
- `unresolved — awaiting external development` (stable)
- `not yet worked` (the only genuine to-do)

A case can be publication-ready with many open questions, provided the spine is solid and the
questions are honestly labeled. Otherwise the coverage gauge would block forever on exactly
the topics worth covering.

**New material arriving against a dormant case has three outcomes:**

1. **Answers** an open question → question moves to answered, recording the run that did it.
2. **Contradicts a confirmed claim that was already published** → surfaces loudly. v1's
   machinery applies (correction lineage, replacement publication, preserve the prior public
   record, never silently rewrite) but must be reachable from a research run, not only from a
   publication mismatch.
3. **Relates** — neither answers nor contradicts, but connects. This is a pattern candidate,
   M13 territory. Under the current boundary the system stores it and the human notices.

**Note for later:** outcome 3 is the strongest argument for eventually building No
Coincidences — not to find conspiracies, but to detect that a dormant case has become live
again. Still closed for v2; the destination is now clearer.

---

## D7 — Parallel rendition generation

Each rendition is generated **independently, from the angle plus its confirmed claims**,
written natively for its platform. Not cut down from a longer rendition.

**Rejected — derive by cutting** (write long-form, compress to article, thread, post):

- **Qualification survival.** Every rendition must preserve required qualification from the
  claims it rests on. Cutting is lossy and aimed at length; qualification language is exactly
  what a compression pass sheds first. Under parallel generation, qualification is a
  generation constraint rather than something defended against the compressor.
- **Platform capacity genuinely differs.** A thread carries qualification inline in the unit;
  a single post may not have room — meaning the claim may simply not be usable in that
  format. That is a correct outcome, which cutting would paper over by compressing until it
  fits.
- **Order of work.** Cutting forces long-form first. For the destination that is backwards.

**Accepted cost:** consistency across renditions is not guaranteed; two independently
generated renditions of one angle may emphasize different things. Acceptable — different
platforms, different audiences, shared claim set, so they cannot contradict on facts.
Divergent emphasis is ordinary editorial practice.

**Structural result:** `case → angle → N renditions`, each separately approved, separately
bound, separately recorded. Matches v1's publication model exactly; needs no new machinery.

---

## D8 — Backend owns runs; the executor is swappable

The reasoning runs in an LLM client. The recording runs in the backend. The backend defines
every run — question, scope, rubric version, capture budget — and owns the run record. An
executor claims a run through a tool surface exposed over MCP and works through it:
`claim_next_run`, Record reads, `capture_url`, `propose_claim`, `close_run`.

**The enforcement seam is `propose_claim`**, which rejects any claim whose locator does not
resolve or whose quoted text is absent from the captured bytes. That refusal is the entire
reason an untrusted runtime is safe. Budget enforcement sits with `capture_url` for the same
reason — the executor cannot overspend because it is not the one spending.

**Rejected — backend runs the agent directly via API from day one:** correct eventually, but
it forces per-token cost before there is revenue. The MCP surface makes a flat-subscription
desktop client viable as the first executor with no architectural compromise.

**Rejected — chat client as the research environment with capture done afterwards:** this is
cite-then-verify, already rejected in D3. Without backend-side fetch there is nothing to bind
a claim to and no corpus denominator.

**Rejected — executor holds run state:** it would put rubric binding, lineage, and budget
inside a conversation the backend does not control.

**Consequence to protect:** every artifact is backend-created, so the executor is
interchangeable — a provider change is configuration, not a rebuild. Any temptation to let the
executor hold state or make a judgment the backend should own is that flexibility leaking.

**Deferred by this decision:** the discovery mechanism. The executor brings its own search;
the backend captures what it reads. A search-provider decision is only needed when an
API-driven agent replaces the chat client, and it uses the same tools.

---

## D9 — Per-operation versioned rubrics

Standing question sets attach to **operations** — reading a source, extracting a claim,
working the public question, proposing an angle, closing a run — not to research stages.
Operations are the stable unit: the discipline of reading a source is identical on run one
and run twelve.

Each set is a versioned repository artifact. **Every claim records the rubric version that
produced it. A rubric change never applies retroactively** — correcting a class of error means
amending the rubric and re-running the affected work, producing superseding claims with
lineage.

**Rejected — one document:** simplest, and adequate if tuning were rare. It is not: this is
the part of the system that will be adjusted constantly in the first year, and a single
document means every tweak bumps the whole thing, destroying the ability to say which claims
came from which guidance.

**Rejected — per-stage sets:** stages were already demoted to a coverage gauge (D5), so a run
working the public question still reads sources and extracts claims. Per-stage sets would
either duplicate the source-reading questions six times or leave the agent without them.

**Accepted cost:** more files, more indirection, and a risk of sets drifting apart in tone.
Mitigated by keeping them short and few.

**Companion mechanisms:**

- **Runs suspend and ask.** A run that becomes uncertain mid-flight surfaces the specific
  question, what it is uncertain between, and what it would do by default. This is a state —
  *running / suspended-awaiting-human / complete* — not an error path. A run that builds forty
  minutes of work on a wrong assumption is waste, and the agent usually knows the moment it
  became unsure.
- **Instance versus class.** Answering a suspended question resolves this instance; amending a
  rubric resolves the class. The interface must distinguish them, because the cheap habit is
  answering the instance forty times instead of fixing the rubric once.
- **Drift visibility.** Instance errors are caught reliably at confirmation time. Class drift
  is structurally invisible from inside any single review. The remedy is aggregate views —
  classification distributions per rubric version, operator correction rate, cross-version
  comparison, confirmation rate — all counts over data already recorded.

---

## D10 — Lead inbox holds material, never claims

A **lead** is a URL dropped into an inbox unattached to any case, for material encountered
outside a dispatched run.

**Captured immediately on drop, always.** The material most worth having is the material most
likely to disappear. Fetching is cheap; decay is not recoverable.

**No claims until attached to a case.** The same source yields different propositions
depending on what is being investigated, so extraction in a vacuum produces claims that serve
no case. Claims come from runs, which carry questions, rubric versions, and lineage.

An optional **summary** — description, not extraction — keeps the inbox browsable. It is the
only part that costs money, so it is deferrable and skippable.

**Rejected — extraction on drop:** fills the Record with generic claims from material that
mostly never becomes anything, costs money per stray link, and creates a claim provenance path
that skips the run model entirely.

**Rejected — fetch as an operator-toggleable option:** conflates two different costs. Fetch is
bandwidth and CPU on your own machine; the summary is the model call. Splitting them gets the
cost control without trading away the thing that cannot be recovered.

**Exception:** auth-walled and paywalled URLs are recorded as identity-only leads, explicitly
marked as not captured. Storing a login wall as though it were an artifact is worse than
storing nothing, because it masquerades as evidence.

---

## D11 — Promotion by use

A capture is **cited** once a claim binds to it. Citation is promotion; there is no separate
operator action and no relevance-scoring step.

Captures with no claims are distinguished as **examined** (a run looked and found nothing
worth claiming) or **unexamined** (nobody has looked). "Read and yielded nothing" is an
editorial finding; "nobody looked" is a gap. Runs record what they examined at close time,
which is free.

The case view shows cited sources. The full capture set remains available with its denominator
intact, less prominently.

**Rejected — human marks relevance:** adds an operator action to answer a question the claim
graph already answers.

**Rejected — agent-proposed relevance scores:** adds a review queue for the same
already-answered question.

---

## Glossary Seed (for `CONTEXT.md`)

Terms whose ambiguity caused real drift in v1. Opinionated on purpose.

**Case**
The durable investigation into one topic. Owns sources, claims, entities, conflicts,
timeline, open questions, and angles. Never completes; goes dormant.
_Avoid_: dossier, topic, work item, story, investigation

**Rendition**
One publishable artifact belonging to exactly one case, targeting one platform and format.
Carries the publication lifecycle.
_Avoid_: post, draft, content item, work item, piece

**Unit**
One ordered component of a rendition — a single post within a thread, a section within an
article. The thing approval binds to.
_Avoid_: tweet, segment, part

**Angle**
The developed answer to "what makes this story worth reading," living inside a case and
linking to claims. Produces renditions; has no lifecycle of its own.
_Avoid_: hook, take, framing, pitch

**Claim**
A proposition recorded in the Record, bound to captured bytes, carrying six independent
evidence dimensions. Either `unconfirmed` (model-proposed) or confirmed (human-set).
_Avoid_: fact, assertion, finding, statement

**Source**
An artifact and its intrinsic provenance, stored once and reusable across cases.
Case-specific relevance and notes live on the case-source relationship, not the source.
_Avoid_: reference, citation, link, document

**Capture**
The stored, hashed, byte-exact result of one read of external material, whether or not it
ends up supporting a claim.
_Avoid_: fetch, scrape, download, snapshot

**Run**
One dispatched research job with an explicit question and bounded scope, producing claims
and new open questions, recording its lineage.
_Avoid_: session, task, job, pass

**Open question**
A first-class worked object recording something the case does not know, carrying a
disposition that distinguishes permanently unresolved from not yet worked.
_Avoid_: todo, gap, unknown, issue

**Confirmed**
State of a claim whose evidence dimensions have been set by the human. Prerequisite for use
in any rendition.
_Avoid_: verified, approved, accepted, cleared

**Cleared**
State of a rendition whose exact content the human has approved for publication.
_Avoid_: approved, signed off, confirmed, ready

**Coverage**
The gauge reporting which of the six research stages a case has genuinely worked. Not a
state machine; a readiness reading.
_Avoid_: progress, completeness, status

**Lead**
A URL dropped into the inbox unattached to any case, captured on drop, holding material and
an optional summary but never claims.
_Avoid_: bookmark, saved link, tip, idea

**Rubric**
A versioned set of standing questions attached to one operation — reading a source,
extracting a claim, proposing an angle. Every claim records the rubric version that produced
it.
_Avoid_: prompt, template, guidelines, instructions

**Executor**
The LLM client that claims a run and works it through the tool surface. Interchangeable by
design; holds no run state and creates no artifacts directly.
_Avoid_: agent, model, worker, assistant

**Tool surface**
The MCP-exposed set of backend operations through which every executor acts. The only path by
which anything enters the Vault or the Record; its refusals are the enforcement.
_Avoid_: API, endpoints, integration, interface

**Cited / examined / unexamined**
The three states of a capture. Cited — a claim binds to it. Examined — a run looked and found
nothing worth claiming. Unexamined — nobody has looked.
_Avoid_: promoted, relevant, used, processed

**Public question**
A first-class Angle Room object recording what people are actually asking about a topic, what
version of the belief circulates, and where it came from. An observation about the discourse,
not a claim about the world.
_Avoid_: the theory, popular belief, the narrative

---

## Fog — In Scope, Not Yet Sharp

Honest fog. None of these is dodged; none is sharp enough to decide without touching real
material. Most resolve after one case runs end to end.

**Resolved since first draft:** the LLM research tool surface (D8), captured vs. promoted
(D11), and the job queue — which largely dissolved, since a backend serving tool calls to an
external executor needs a run registry rather than a worker pool.

Remaining:

- **Run registry mechanics.** Statuses, how a run is claimed, what happens when an executor
  abandons one mid-flight, whether concurrent runs on one case are permitted.
- **Rubric contents.** The indicative questions are sketched; the real text is tuned against
  output rather than written in advance. First draft after the first run, not before.
- **What coverage actually measures.** Signals that honestly indicate "the official spine is
  complete" rather than "a run labelled stage 1 finished."
- **Entity resolution ergonomics.** Authority-at-use covers claims; entity identity is a
  separate human-only decision with its own volume problem.
- **Web UX model.** Notion-style tiles are the current reference point, and the reason they
  may fit is that a case holds heterogeneous contents — captures, claims, open questions,
  angles, renditions — which a tiled surface handles better than a form. Undecided; likely
  `/prototype`. The drift-visibility views (D9) land in the same question.
- **Whether Release Watch is in v2.** Its sharpest job is now "does new material bear on a
  dormant case's open questions," which is better defined than v1's general intake.
- **Long-form rendition shape.** Threads are ordered units; articles and scripts need
  sections/beats, and approval binding for those is not yet designed.
- **Executor model selection.** A recurring research question, not a one-time choice.
  Criteria that will still matter in a year: MCP error handling good enough that a rejected
  `propose_claim` produces self-correction rather than a stalled run; context discipline
  across dozens of tool calls; reliable treatment of captured page content as quoted material
  rather than instruction; honest signalling of uncertainty, since suspend-and-ask depends on
  it; cost per completed run rather than per token.

## Out of Scope for v2

- **No Coincidences / automatic pattern detection.** The system stores; the human notices.
- **Autonomous publication** of any kind, on any platform.
- **Multi-account UI.** Carry `account_id` in the schema; keep the interface single-account.
- **Semantic retrieval / Qdrant / graph.** Deferred until a governed corpus and a measured
  retrieval need exist.
- **Video production.** Scripts are a rendition type reserved in shape only.
- **Desktop shell.** Web, `localhost`, Linux-first. Backend/client boundary kept strict
  enough that a desktop wrapper stays possible without being planned for.
