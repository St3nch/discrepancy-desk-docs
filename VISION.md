# The Discrepancy Desk

**Project vision. Read this first.**

This document describes what The Discrepancy Desk is, why it exists, what it is not, and how
it works. It is self-contained — nothing outside it is required to understand the project.

> **Prior research exists.** An earlier build of this system produced roughly 8,000 lines of
> domain research that remains valid — source admission policy, audiovisual ingestion,
> website and feed capture, document normalization and parser provenance, the canonical
> knowledge model, identity and correction lineage, governed LLM context assembly, retrieval
> readiness, and backup and rebuild. It is indexed in `v1-repository-inventory.md`, which
> records what each report answers and whether it should be carried, consulted, refreshed, or
> ignored. Consult the index before re-deriving an answer. Two reports in particular —
> **governed LLM context assembly** (retrieved material is data, never instruction) and
> **website and feed capture** — should be read before the research agent is built.

---

## 1. What It Is

The Discrepancy Desk is a **local, web-based editorial system that turns open-web research
into publishable content about anomalies, conspiracy lore, disputed claims, weird history,
and hidden-timeline material.**

A connected LLM does the production work: research, source capture, claim extraction,
classification, angle development, and drafting for every publishing platform. A single human
holds all authority: he directs what gets researched, confirms what the research found, and
clears the exact text before anything is published.

Public output goes out under a fictional persona, **Quinton Clearance**, a records custodian
at **Basement 1, Level 7** who processes reality's anomalies as bureaucratic paperwork.

The system runs on Linux, in a browser, on localhost. It is single-operator by design.

---

## 2. Why This Exists

Two reasons, both real, neither subordinate to the other.

**Because it's fun.** Conspiracy material is dark, weird, and occasionally frightening — and
also genuinely entertaining. The old man on the 2am radio show who worked at Area 51 and
swears the craft ran on green lime Jell-O is absurd. He is also excellent. The Desk records
him, labels him accurately, and is delighted to have him on file. Absurd material is
*desirable*, not merely tolerated. It does not earn a research run, and it may or may not
earn a post — but it always earns a record.

**Because it has to make money.** This project fails if it does not eventually sustain
itself, and profit is preferred to break-even. Monetization is a design goal, not an
afterthought bolted on once the archive is pretty.

### The tension, stated honestly

Monetization wants volume and consistency. The editorial standard says that when nothing is
ready, nothing gets published.

Those pull against each other, and pretending otherwise is how the quality bar quietly dies
six months in. The resolution is not to weaken either one:

> **The pipeline should rarely be empty. When it is, that is a research-capacity problem, not
> a licence to publish weak work.** The fix for an empty slot is dispatching more runs, not
> lowering the bar.

The bar stays absolute. The system's job is to keep you out of the position where it hurts.

---

## 3. The Editorial Thesis

**The Desk does not chase absolute truth. It asks why the discrepancy exists.**

Most of this material has no resolvable answer, and a project that stakes its output on
resolving it will fail on nearly every case. But a different set of questions is answerable
almost every time:

- Why is this a conspiracy theory at all?
- What, specifically, are people asking?
- Where do they feel the record doesn't add up — and are they right?
- Where did this version of the belief come from?
- Why won't it settle?

That reframe changes what the Desk delivers. It is honest about the limits of the material.
It supplies a default angle when none is obvious. It makes the counter-case central rather
than dutiful — **putting an absurd question to rest is content, not a failure to find a
conspiracy.** And it fits the persona exactly: Quinton notices what is missing from the file.
He does not solve cases.

Sometimes the question dissolves under inspection. Sometimes — better — it survives, and now
there is a real discrepancy with a documented spine.

**Truth is transparency.** People gravitate to the account that shows its work, including the
work that came back empty. That instinct governs everything below.

---

## 4. The Newsroom, and Where the Metaphor Stops

The Discrepancy Desk works like an investigative newsroom with exactly two roles.

The **connected LLM is the entire reporting staff** — researcher, investigator, and writer in
one. It does all production: open-web research, source capture, claim extraction,
classification proposals, entity matching, angle development, and drafting every platform
rendition. It also **proposes the research agenda** — which open questions look worth
pursuing, and why.

The **human is the editor-in-chief and the sole authority.** He approves, rejects, or rewrites
the research agenda and dispatches every run. He accepts or rejects each proposal, sets every
authoritative value, and approves exact content before publication.

**The metaphor stops at trust.** A newsroom editor trusts his reporters. This system assumes
the reporter can fabricate a source and a quotation without knowing it. Every claim therefore
binds to captured, byte-verified material, and anything that cannot bind fails closed. That
assumption is the reason the system has structure at all.

Two further limits: there is one worker with several modes, not a staff of agents with an org
chart. And there is no cadence obligation — a newsroom prints whether or not the story is
ready; the Desk publishes when a story is ready and not otherwise.

---

## 5. Governing Doctrine

```
AI drafts. Human clears. Database remembers. Metrics judge.
```

```
Record the crazy. Label what it is. Connect what emerges. Never fabricate the file.
```

```
Crumbs are not conclusions.  Patterns are not proof.  Speculation is not confirmation.
The red string is not evidence. It is a filing request.
Crumbs open drawers. They do not close cases.
```

**These rules prohibit *presenting* speculation as confirmation. They do not prohibit
*storing or publishing* speculation** with truthful attribution and framing.

This distinction is load-bearing. Classification is not an exclusion gate — it determines how
much weight a claim can bear and how it must be worded, never whether it is allowed to exist.
`speculative`, `allegation`, `contradicted`, and `unassessed` are honest labels the Desk is
equipped to carry, not rejection states.

The Desk's job is to give people the record — solid claims and absurd ones alike — labeled
accurately, so the reader decides what to believe. It is not the Desk's job to pre-filter
what reaches the reader by how plausible the Desk finds it.

---

## 6. What It Is Not

- not an autonomous bot of any kind — no autonomous posting, replying, liking, following, or
  reposting
- not a fake insider, government, or classified-leak account
- not a formal fact-checking service, truth-rating system, or court of final factual authority
- not a system that must resolve every claim before editorial use
- not a scraper farm
- not a generic social-media dashboard
- not a conspiracy-certainty machine
- not a second truth store layered on the database

---

## 7. Architecture — Three Layers

Three boundaries, answering three different questions. They must not contaminate each other.

### The Vault — *what exactly do we have?*

Byte-exact capture of every piece of external material the system reads. Immutable originals,
acquisition receipts, versioned normalized element packages with exact addressable locators
(`document_versions → elements → regions`). Generated Markdown/HTML projections are read-only
and never authoritative.

### The Record — *what can we prove?*

Cases and case-specific source use. Claims with independent evidence dimensions. Claim-to-
source support links. Quotations with attribution frames and locators. Human-confirmed
conflicts. People, organizations, programs, facilities, publications, locations. Normalized
surface forms and per-source observations. Human-resolved entity identity. Time-scoped roles
and human-authored relationships. Dates with explicit precision. Timeline entries. Open
questions. Publication-risk classifications. Audit and correction lineage.

A source is stored once. Case-specific relevance and notes live on the case-source
relationship, not on the source.

### The Angle Room — *what makes this story worth reading?*

The central discrepancy. **The public question** — what people are actually asking, where
they are asking it, what version of the belief circulates, and where it came from. Human
conflict. Quotation shelf. Public-versus-private contradictions. Missing records.
Institutional rivalry. Surprising supported details. Narrative turns. Visual opportunities.
Competing angle candidates and immutable reasoned dismissals. Hook alternatives, progression,
payoff.

The public question is a first-class Angle Room object. It is not a claim about the world; it
is an editorial observation about the discourse, and it is frequently where the story lives.

**Every Angle Room item links to at least one claim and inherits that claim's source basis,
corroboration, certainty, posture, required qualification, and publication-risk state.** The
Angle Room may make a story vivid; it may not launder a weak claim.

---

## 8. The Two Core Objects

**Case** — the durable investigation into one topic. Owns sources, claims, entities,
conflicts, timeline, open questions, and angles. Never completes. Goes dormant and wakes.

**Rendition** — one publishable artifact belonging to exactly one case, targeting one
platform and format. Carries the publication lifecycle. Ends published or rejected.

**Angle** sits between them, inside the case, linking to claims. It produces renditions and
has no lifecycle of its own.

```
Case → Angle → N Renditions
```

The operator opens a **case**. Renditions are objects inside it. A quick one-off post still
gets a case — a thin one — to preserve a single path through the system.

---

## 9. How Research Works

### Capture-then-cite

**The LLM cannot cite anything it has not first captured.** Reading a page means fetching it
through the backend, which stores the raw bytes, hashes them, parses them into the element
structure, and only then permits a claim to bind to a region inside it. The research agent's
web-read tool *is* the ingestion tool.

Everything read is captured, whether or not it ends up supporting a claim. This is what makes
fabrication structurally impossible rather than something the human has to catch — and it is
what produces an honest corpus denominator for any later analysis.

**Captures have three states, and no separate promotion step.** A capture is *cited* once a
claim binds to it — citation is promotion, requiring no operator action. It is *examined* if
a run looked at it and found nothing worth claiming. It is *unexamined* if nobody has looked
yet.

The distinction matters because "read and yielded nothing" is an editorial finding, while
"nobody looked" is a gap. A case view shows cited sources; the full capture set stays
available with its denominator intact, less prominently.

### The lead inbox

Not all material arrives through a dispatched run. A podcast encountered by chance, a video
surfaced by a recommendation engine, a link from a conversation — ambient discovery is how a
great deal of real journalism starts, and it needs somewhere to land that is not "open a
whole case."

A **lead** is a URL dropped into an inbox, unattached to any case. It is **captured
immediately on drop** — always, without exception — because the material most worth having
is the material most likely to disappear: the deleted post, the pulled video, the article
quietly edited after publication. Fetching is cheap; decay is not recoverable.

A lead holds **material only, never claims.** The same source yields different propositions
depending on what is being investigated, so extraction in a vacuum produces claims that serve
no case. Claims come from runs, which carry questions, rubric versions, and lineage; a claim
born outside a run would have none.

An optional **summary** — what this is, who is in it, what it appears to be about — may be
generated so the inbox stays browsable. It is deferrable and skippable, being the only part
that costs money. It is a description, not an extraction.

A lead is later attached to an existing case, promoted to a new one, or disposed of.

**Auth-walled and paywalled URLs** are recorded as identity-only leads, explicitly marked as
not captured. Storing a login wall as though it were an artifact is worse than storing
nothing, because it masquerades as evidence.

### Question-scoped runs, dispatched by the human

Research is a **loop, not a pipeline.** A run carries an explicit question and a bounded
scope, and produces captured material, claims, and **new open questions**.

The loop:

```
run → findings + new open questions
    → LLM proposes which questions are worth pursuing, with rationale
    → human approves, rejects, edits scope, or writes his own
    → approved questions become the next run
```

**Nothing dispatches without the human.** The LLM proposes the research agenda; it never sets
it. Findings opening new questions is the normal case, not a failure.

### The backend owns the run; the model is a swappable executor

The reasoning happens in an LLM client. The recording happens in the backend. That split is
the load-bearing decision, and it is what keeps the system independent of any provider.

The backend defines the run — question, scope, rubric version, capture budget — and opens the
run record. An executor claims the run through a tool surface, receives the question and the
rubric text, and works:

```
claim_next_run()      → question, scope, rubric version and text
read Record context   → prior claims, open questions, existing sources
capture_url(url)      → backend fetches, hashes, parses, stores against this run
propose_claim(...)    → bound to a captured locator, tagged run + rubric version
close_run(...)        → new open questions, self-reported low-confidence areas
```

**The enforcement seam is `propose_claim`: it rejects any claim whose locator does not
resolve, or whose quoted text is not present in the captured bytes.** That single refusal is
what makes an untrusted runtime safe. Without it, the architecture is cite-then-verify wearing
a disguise.

Budget enforcement sits with the backend for the same reason — `capture_url` counts against
the run's cap and begins refusing. The executor cannot overspend because it is not the one
spending.

Because every artifact is backend-created, **the executor is interchangeable.** A desktop chat
client under a flat subscription is a viable starting point; an API-driven agent is the same
tool surface with a different caller. Switching providers is a configuration change, not a
rebuild. Every temptation to let the executor hold state or make a judgment the backend should
own is that flexibility leaking away.

Suspend-and-ask becomes conversational under a chat executor — the model asks, the operator
answers, the run continues — at the cost of the run's state living in a conversation rather
than the backend. Acceptable for a single operator present at the machine; a reason to prefer
a backend-driven executor once runs get long.

### The coverage gauge

Six research stages describe what a case has genuinely worked:

1. **Official foundation** — the institutional record, dates, findings, contradictions.
2. **The public question** — where the theory lives, what version people believe, what they
   are asking, and where the belief came from. A different research target with different
   sources: podcasts, forums, viral clips, the one 1997 article everyone cites and nobody
   reads. Tracing a belief to its origin is frequently the entire story.
3. **Deep context** — testimony, interviews, oral histories, memoirs, scholarship, technical
   disputes, later developments.
4. **Story intelligence** — the central discrepancy, human conflict, strongest quotation,
   surprising supported detail, narrative turn, visual opportunity, counter-case.
5. **Editorial development** — angles, audience promise, hook, progression, format,
   qualification plan, payoff.
6. **Composition** — platform-native renditions from the selected angle.

This is a **gauge, not a state machine.** It reports what a case has worked, in any order. A
case can be on run seven and still filling its official foundation because run four opened a
hole in it.

One gate is absolute: **no angle work begins before the official spine is complete.** The
counter-case receives real effort. A Desk that hides the best contrary evidence is advocacy
wearing a file-folder costume.

### Cases sleep, and wake

A case with unanswered questions is not incomplete. For this subject matter, the permanently
unanswered question is frequently the story — missing records are first-class material. Open
questions therefore distinguish *unresolved and likely permanent*, *unresolved and awaiting
external development*, and *not yet worked*. Only the last is a to-do.

A case goes **dormant** — intact, searchable, out of the working queue — and wakes when new
material arrives. New material against a dormant case does one of three things:

1. **Answers** an open question.
2. **Contradicts a confirmed claim that was already published.** This surfaces loudly and
   triggers correction lineage — preserve the prior public record, never silently rewrite.
3. **Relates.** Connects without answering or contradicting. Stored; the human notices.

---

## 10. How the Agent Is Directed

A topic is not a question. Handed a topic, a model produces a summary — plausible, organized,
and answering nothing anyone asked. Everything below exists to prevent that.

### Two levels of question

**The run question** scopes a dispatch and is what the human approves. It must be bounded and
answerable. *"What did the official investigation conclude, and who dissented?"* is a run
question. *"Tell me about Vela"* is not.

**Standing question sets** are the agent's discipline, and they do not change per case. They
attach to **operations**, not stages — reading a source, extracting a claim, working the
public question, proposing an angle — because the same act carries the same discipline
whether it happens on run one or run twelve.

Indicative content, not final:

- *Reading a source* — What kind of support is this? Who is speaking, in what role, at what
  moment? What does this prove, versus what does it merely show was said? What would have to
  be true for this to be wrong?
- *Extracting a claim* — What exact proposition is asserted? What language must accompany it
  to be honest? Does anything already in the Record contradict it?
- *Working the public question* — What version do people actually believe? Where did that
  version come from? What are they asking that the record does not answer?
- *Finding the angle* — What is the sharpest contradiction? Who is in conflict, over what?
  What is surprising and supported? What is the best case against the story I am inclined to
  tell?
- *Closing a run* — What does this open? What do I now know to ask that I did not before?

### Rubrics are versioned artifacts

Each question set is a versioned document in the repository, not text buried in code. Every
claim records which rubric version produced it.

**A rubric change never applies retroactively.** Claims already produced stay bound to the
version that made them; the new version applies from the next run. Correcting a class of
error therefore means amending the rubric and re-running the affected work — producing
superseding claims with lineage, never silent rewriting.

This is what makes "which claims came from the bad rubric" an answerable question.

### Runs can suspend and ask

A run that becomes uncertain mid-flight — a claim it cannot classify confidently, two sources
that appear to contradict, an entity it cannot resolve — **suspends and surfaces the
question**, stating what it is uncertain between and what it would do by default. The human
answers; the run resumes.

This is a state, not an error path: *running / suspended-awaiting-human / complete*. A run
that builds forty minutes of work on a wrong assumption is waste, and the agent usually knows
the moment it became unsure.

**Two different remedies, and the interface must distinguish them:** answering a suspended
question resolves *this instance*. Amending a rubric resolves *the class*. The cheap habit is
answering the instance forty times instead of fixing the rubric once.

### Making drift visible

The operator is the only quality control. The system's job is to make his attention go
further, not to assume he has more of it.

Instance-level errors — this claim is misclassified — are caught reliably at confirmation
time, with the source in hand. **Class-level drift is not.** The agent calling participant
recollections `established` for three weeks is invisible from inside any single review:
each call looks defensible, and the pattern exists only across dozens seen at once.

The remedy is not vigilance. It is showing the aggregate:

- **Classification distributions per rubric version.** Sixty percent of claims marked
  `established` is either obviously wrong or obviously fine at a glance, and invisible one
  claim at a time.
- **Correction rate.** Overriding proposed certainty on 40% of claims means the rubric is
  wrong, and the system knows before the operator consciously registers it.
- **Cross-version comparison.** Did amending the rubric do what was intended, or something
  else?
- **Confirmation rate.** Forty claims in eleven minutes is not forty confirmations.

All of this is counts over data already recorded. None of it is a feature; it is the
counterweight to placing all authority in one person.

**The agent also flags its own drift.** At the close of a run it reports where it was least
confident and where the rubric underserved it. Not authority, not complete — but free, and a
second look at precisely the problem the operator is worst placed to see.

---

## 11. Evidence — Six Dimensions, No Score

Evidence is multidimensional. Provenance, corroboration, certainty, posture, qualification,
and publication risk are never compressed into one ranked label.

| Dimension | Values |
|---|---|
| **Source basis** | `contemporaneous_record`, `contemporaneous_report`, `direct_participant_recollection`, `later_retrospective_claim`, `scholarly_interpretation`, `technical_inference`, `desk_inference`, `other` |
| **Corroboration** | `unassessed`, `single_source`, `multi_source_dependent`, `independently_corroborated`, `contradicted` |
| **Certainty** | `unassessed`, `established`, `probable`, `contested`, `speculative`, `unknown` |
| **Posture** | `factual_assertion`, `interpretation`, `participant_account`, `allegation`, `disputed_assertion`, `research_lead`, `pattern_candidate` |
| **Required qualification** | the exact language that must accompany any use of the claim |
| **Publication risk** | a separate control — not evidence strength (see §13) |

**Hard rules.** No automatic sum, minimum, maximum, ladder, chain score, or overall evidence
score. No relationship strength computed by traversing labeled edges. Two publications
deriving from the same witness, dataset, or authors are not independent. The UI may render
dimensions as readable chips but may never hide a contradictory combination — it must surface
it for correction.

**Dimensions constrain sentence form.** Participant recollection requires attribution.
Allegations require explicit attribution and qualification. Technical inference stays an
inference. Contradicted claims surface the conflict. Research leads and pattern candidates
cannot be rendered as findings.

**Source is not claim.** A source describes an artifact and its provenance. A claim records a
proposition. A claim-source relationship states how the source supports or contradicts it.
Quotations are first-class: exact text, speaker, role at event, role at utterance, date,
context, attribution frame, locator.

---

## 12. Where Authority Attaches

Claims enter the Record **unconfirmed** — model-proposed, carrying the LLM's suggested
dimensions, bound to captured bytes, honestly labeled. No gate on entry. The Record fills
freely with research substrate.

**Human confirmation attaches when a claim is used to support published text.** An angle
pulls in a dozen claims; those get confirmed — dimensions accepted or corrected,
qualification checked. **A rendition may only cite a confirmed claim.** Unconfirmed claims
are invisible to the composer.

This keeps the review surface for one piece at ten or fifteen real decisions instead of
hundreds of rubber stamps, while guaranteeing nothing unconfirmed can reach a published post.
Confirmation persists with the claim, so reuse across cases is cheap.

`unconfirmed` must be visually loud everywhere it appears. Any feature reading the Record
treats confirmed and unconfirmed as different populations.

### The one place output pressure will attack

Not the publishability gate — that is human, visible, and defensible. It will attack **claim
confirmation.** When a post is due and fifteen unconfirmed claims stand in the way, the
temptation is to confirm faster and looser. That degrades silently and does not show up in
the output for months.

**Cheap insurance: record confirmation timestamps and surface the rate.** Forty claims
confirmed in eleven minutes is not forty claims confirmed. Nobody enforces this but the
operator, and he can only see it if the system measures it.

### The LLM may

Research, capture, extract claims, propose classifications, match entities, surface tensions,
propose research agendas, propose angles, generate hook alternatives, split units, tighten
text, draft alt text, summarize performance patterns.

### Only the human may

Dispatch a research run. Set authoritative evidence dimensions. Decide whether sources are
genuinely independent. Resolve entity identity, merge, or split. Confirm a conflict as
editorially live. Author a relationship as accepted research structure. Choose or dismiss an
angle. Classify publication risk. Rule a sensitive connection publishable. Approve exact
ordered text and media. Publish. Dispose of a research lead.

**No LLM or agent gets direct database access.** All access is through governed business
operations, never SQL.

---

## 13. Connections Between People, Organizations, and Stories

```
A recurring name or institutional overlap is a research lead, not a finding.
Preservation does not authorize public presentation.
```

Canonical entity records and source observations are distinct. A normalized surface form may
be observed repeatedly; each observation carries its source record, optional locator, exact
observed text, and human resolution basis. **The existence of the same name in two cases is a
record, not a finding.**

**Publication-risk classes:** `unknown`, `living_private`, `public_official_official_capacity`,
`public_figure`, `deceased`, `institution`, `not_applicable`.

**Fail-closed rules.** Person entities default to `unknown`; moving off it requires a human
actor, basis, and timestamp. `unknown` and `living_private` people are non-publishable as
cross-case connections. Public status alone does not make a connection publishable. A name
match, shared employer, temporal overlap, affiliation, organizational succession, or
multi-hop path does not make a connection publishable. No LLM, detector, query, or
relationship path may set publishability. Missing classification fails closed.

When a connection is approved, the published text must state what is documented, what the
connection consists of, what it does not prove, why it is relevant, the source and required
qualification, and whether it is disputed, incomplete, or inferential.

**The system stores; the human notices.** Automatic pattern detection across cases —
recurrence flags, hub detection, path traversal, chain scoring, automatic lead generation —
is deliberately not built. That capability has a designed future ("No Coincidences") and
remains closed.

---

## 14. Producing Content

An angle produces renditions. **Each rendition is generated independently, from the angle
plus its confirmed claims, written natively for its platform.** Never cut down from a longer
rendition — compression sheds qualification language first, and qualification survival is
non-negotiable.

Platforms differ in what they can carry. A thread can hold qualification inline; a single
post may not have room, which means the claim may simply not be usable in that format. That
is a correct outcome, not a problem to compress around.

**Rendition types:** X post, X thread, X article, Truth Social formats, Substack or Medium
article, and later, video scripts. Each carries its own constraints as data — counting rule,
ordinality, media rules, whether qualification can live in a footer or must be inline.

**Approval binds the rendition, never the angle.** One approval authorizes one publication
set. One approved unit maps to one external post. Each unit records ordinal, platform, owned
account, external post identity, canonical URL, published time, and verification state.

**Exact-content binding.** Approval binds the exact reviewed content — text, links, media,
labels. Any post-approval change invalidates approval. Media binding covers SHA-256, byte
size, MIME type, reference, alt text, and rights state; replacing bytes invalidates approval.
Changing publication time does not alter approved text.

### Recurring formats

The editorial thesis produces at least one repeatable, high-yield format, worth naming
because it drives what gets researched:

**Putting a question to rest.** Take one specific, widely-believed claim. Show exactly what
the record does and does not support. Land it. Sometimes the claim dissolves; sometimes it
survives and becomes a documented discrepancy. Either outcome is publishable, and the format
has a turn built in — the reader arrives thinking they know the answer.

It is also cheap: the scope is one claim rather than one topic, so output per research run is
high. This is the natural home of the **Drawer 17** recurring series.

### The publishability gate — human-only, no score can satisfy it

A rendition advances only when the owner judges that it:

- contains a concrete reason to read
- names people, institutions, places, dates, records, events where the source permits
- has an actual angle rather than a format label
- uses chronology or causation rather than category headers as structure
- includes a turn or payoff when the format needs one
- preserves every required qualification
- links factual assertions to confirmed claims and sources
- fits the platform format under the current counting rule
- has a deliberate visual decision
- sounds like Quinton Clearance without fake-agency roleplay
- **is something the owner would voluntarily publish**

> Vagueness is not caution. It is the absence of research wearing caution's clothes.
> A recurrence is a research assignment, not a finding.
> No draft advances merely because it is accurate and grammatical.

---

## 15. Voice

**Quinton Clearance** is a records custodian who has read the whole file, is unsurprised by
institutional behavior, notices what is missing, and refuses to overclaim because overclaiming
is sloppy. Tired eyes, dry humor, calm under absurdity, treats anomalies as administrative
problems. Bakes bread — which gives "crumbs" a second meaning.

**Use:** specific facts and concrete contradictions; mostly short sentences; causal or
temporal transitions; inline uncertainty ("later recalled," "by his account," "the public
record does not establish"); dry understatement earned by the material; observations about
the record rather than declarations of ultimate truth.

**Avoid:** opening with a category label; abstract institutional nouns when a named person is
available; stock lore as a substitute for a conclusion; fake classification markings or claims
of privileged access; "what they don't want you to know" framing; joke density around
disputed claims; disclaimer blocks arriving after an unqualified assertion; certainty
manufactured from typography.

> B1-L7 lore is seasoning, not dinner.

Quinton is clearly fictional, parody, commentary. He may have lore. He must not claim real
classified access. Parody shapes presentation; it never fabricates the record.

---

## 16. Metrics

Metrics evaluate formats, hooks, lanes, and editorial hypotheses **after** manual publication.
They do not rewrite history, promote claims, choose angles, or authorize publication.

Every observation carries an explicit state — observed, empty, not requested, not returned,
unavailable, withheld, malformed, errored, unsupported — and a method. Unknown is first-class
and never silently zero.

Every surfaced pattern is labeled observation, correlation, hypothesis, experiment, or
accepted conclusion. Correlation is never presented as cause. A metrics target never justifies
publishing weak content.

Monetization progress is tracked as a first-class measurement, against the actual published
thresholds of each platform's program, with pace stated honestly.

---

## 17. Technical Shape

**Web application, Linux-first, localhost, single operator.**

A backend owns all governed operations, the Vault, the run registry, and the tool surface. A
browser client holds no privileged access.

**The tool surface is the product's real boundary.** It is exposed over MCP so that any
MCP-capable client can act as a research executor, and it is the only path by which anything
enters the Vault or the Record. Its refusals — unresolvable locators, quoted text absent from
captured bytes, exhausted capture budgets — are the enforcement, and they must hold against a
runtime that is assumed untrusted.

Run state lives in the backend, not the executor. A run is a record with a question, a scope,
a rubric version, a budget, a status, and lineage back to the question that prompted it.

The backend/client boundary stays strict enough that a desktop wrapper remains possible
without being planned for.

**Model selection is deliberately not decided.** The criteria that will still matter in a
year: MCP error handling good enough that a rejected `propose_claim` produces self-correction
rather than a stalled run; context discipline across dozens of tool calls; reliable treatment
of captured page content as quoted material rather than instruction; honest signalling of
uncertainty, since suspend-and-ask depends on it; and cost per completed run rather than per
token. Aggregators exist chiefly to make comparison cheap. This is a recurring research
question, not a one-time choice.

**Data roles:**

```
Database           identity, workflow, claims, entities, approvals, audit — source of truth
Governed filesystem immutable captured originals + normalized element packages
Projections        generated, read-only, never authoritative
```

Canonical captured evidence is exact bytes, or an immutable file referenced by path plus
SHA-256 and capture provenance. Parsed output is derivative and never presented as the
original. Missing, moved, altered, or orphaned evidence must be detectable.

**Multi-account:** carry account scope in the schema from the start; keep the interface
single-account until there is a reason otherwise. Accounts never coordinate for artificial
engagement.

---

## 18. Reserved Shapes

Not built for the first destination, but recorded here because they are expensive to retrofit
and cheap to leave room for.

**The dismissal ledger.** A public record of claims the Desk investigated and found nothing
in. Nobody in this space publishes their misses; everyone publishes hits. An account that
says "we ran this down, here is exactly why it is nothing" is doing something structurally
different — and it buys real credibility for the times it says "this one is genuinely
unresolved." It is also free content, generated as a byproduct of research that already
happened, and it is very Quinton: the clerk files the closed cases too.

Requirement now: record dispositions in a form that could later be published. Nothing more.

**No Coincidences.** Governed cross-case pattern detection producing explainable candidates
for human review, never findings. Its sharpest use is not finding conspiracies — it is
detecting that a dormant case has become live again.

**Video.** Scripts are a rendition type. Shape reserved; nothing designed. Note that the
locator model must be able to address a **time range** from the start — a claim bound to a
moment in a video is structurally identical to one bound to a region in a document, and
retrofitting the locator model later would touch every claim binding in the system.

**A second-model auditor.** A different model, from a different lab, pointed at a rubric
version and a body of claims, asked whether the rubric is being applied consistently and
whether the distribution looks right. Also spot-audits: re-derive the classification for a
sample of confirmed claims from their bound sources and report disagreements as a rate.

This is aggregate pattern work over material the operator will never read in one sitting —
precisely the structural blind spot. **It produces findings, never decisions.** The moment a
second model's assessment carries weight, there are two authorities, and two models agreeing
feels like verification when it may only be correlated error.

Not built for the destination — with few enough claims to read by hand, it is overkill.
Reserved because the requirement is only that claim-level records already carry rubric
version, original proposal, human correction, and timestamp. They do, so an auditor can be
pointed at the corpus later with no retrofit.

---

## 19. First Destination

**One complete vertical pass:** one real topic, researched by the LLM against the open web,
captured into the Vault with verified locators, claims extracted and classified, one angle
developed, one X thread rendered, cleared by the human, posted manually, recorded.

One case, one platform, one format, end to end.

### The candidate first case

**The 1979 Vela Incident.** A double-flash detection over the South Atlantic, an official
finding disputed by the scientific panel that examined it, decades of unresolved argument —
exactly the Desk's material.

It is also the only topic the previous build ever ran live, and it failed: five drafts, all
retired as failed editorial artifacts, nine sources carrying zero source notes, the longest
draft stored as a single flat blob with no claim structure beneath it. That failure is
precisely what this architecture was designed against.

Nine real sources are already identified from that pass. Running Vela first therefore gives a
controlled comparison on identical material — same topic, same sources, different system. If
this produces something publishable where the last one produced five retirements, the
architecture is doing its job. If it does not, that is worth learning before anything is
built on top of it.

Everything else — Truth Social, Substack, video scripts, release monitoring, metrics
analysis, pattern detection, multiple accounts — is extension off a proven spine.

**The product test is blunt:** the owner opens the application, produces work worth posting,
posts it manually, and comes back tomorrow.
