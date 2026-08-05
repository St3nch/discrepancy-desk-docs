# v1 Repository Inventory

What exists in `St3nch/discrepancy-desk-docs` @ `f04199d`, what question each item answers,
and what v2 should do with it.

**309 markdown files.** 230 of them (131 planning packages, 99 audit records) are milestone
machinery with no v2 value. The ~50 below are the ones worth knowing about.

The old repo is not going anywhere. This is a findability problem, not a preservation
problem — the purpose of this index is that you never re-derive an answer you already paid
for.

**Dispositions:**

- **CARRY** — already inherited into the v2 vision or decisions doc. Nothing to do.
- **CONSULT** — answers a question v2 will ask. Read it when you reach that question.
- **REFRESH** — the question is still live but the answer has decayed. Re-run before trusting.
- **IGNORE** — dead machinery, superseded architecture, or v1 process artifacts.

---

## Tier 1 — The Research That Cost the Most and Still Holds

The nine `R-M06-*` architecture reports, ~8,000 lines total. These are the highest-value
artifacts in the repo. Each one is a real research pass with evidence tiering
(VERIFIED FACT / PROVIDER CLAIM / INFERENCE / OPEN QUESTION) and none of them is about v1's
process — they are about the problem domain, which has not changed.

| Report | Lines | Answers | Disposition |
|---|---|---|---|
| **R-M06-01** Source universe and admission policy | ~900 | Should each source type get its own ingestion path? *(No — one universal envelope; manual and automated acquisition produce the same downstream package.)* Also: technical feasibility ≠ permission. | **CARRY** — the universal envelope is in the v2 vision. Consult for the admission-policy detail. |
| **R-M06-02** YouTube and audiovisual ingestion | 1,394 | What is a video source actually made of? *(Ten distinct things, not one "transcript.")* Acquisition methods, provider posture, segment identity, timestamp handling. | **CONSULT** — the single most valuable deferred-feature document. Read before any video work. |
| **R-M06-03** Website, feed, and change monitoring | 1,095 | Feeds vs. polling; why HTTP validators don't prove meaningful change; raw capture vs. readable extraction as different artifacts; canonical URLs are hints not identity. | **CONSULT** — directly relevant to v2's open-web capture. Read during the tool-surface fog item. |
| **R-M06-04** Document normalization and parser provenance | 1,297 | Why the original stays canonical; element-based not text-only normalization; parser output is an observation, not truth; chunking stays downstream. | **CARRY + CONSULT** — principles are in v2; the parser-provenance detail matters when capture is built. |
| **R-M06-05** Canonical Vault knowledge model | 1,139 | Why a generic "note" object fails; the canonical chain; assertions need attribution + qualifiers + evidence links + review state; annotations target exact source regions. | **CONSULT** — informs the locator model. Note: it argues events should be first-class, which v1 deferred and v2 has not decided. |
| **R-M06-06** Identity, versioning, correction, deduplication | 737 | Layered identity; dedup must not delete provenance; correction is lineage not mutation; cross-account reuse is import lineage not shared truth. | **CARRY** — correction-as-lineage is in v2. Consult for the dedup detail. |
| **R-M06-07** LLM context assembly and governed reasoning | 1,003 | Context is a governed evidence package, not a pile of text. **Retrieved material is data, never instruction.** The model may reason; it may not confer authority. Prompt injection remains open. | **CONSULT — HIGH PRIORITY.** v2 puts the LLM in the research seat, which makes this report *more* relevant than it was in v1, not less. Read before building the research agent. |
| **R-M06-08** Retrieval, chunking, graph, Qdrant readiness | 616 | Retrieval structures are disposable derivatives; chunks derive from canonical elements; account filtering before ranking; hybrid > semantic-only; graph outputs are candidates. | **CONSULT** — only when retrieval becomes a real need. Not before. |
| **R-M06-09** Automation, security, backup, rebuild | 626 | Automation creates candidates not truth; the canonical Vault must rebuild every derivative; provider exit is a design requirement; **backup is not proof of recovery.** | **CONSULT** — read before the first backup design. "Backup is not proof of recovery" is the line that matters. |

Also in that directory: `multi-account-research-and-editorial-policy-layer.md` — **CONSULT**
if multi-account ever becomes real.

---

## Tier 2 — Live Questions, Decayed Answers

Everything here is time-sensitive. The question is still good; the answer has aged.

| Document | Answers | Disposition |
|---|---|---|
| `x-policy-and-counting-refresh-2026-07-28.md` (285 lines) | X character counting rules, longer posts, Articles, media facts. Includes a worked counting rule. | **REFRESH** — counting rules move. Re-derive before composer work; use the old doc as the shape of the answer. |
| `x-operational-refresh-2026-07-29-p4.md` (76 lines) | Current X API pricing/operational facts; why manual publishing avoids per-post charges. | **REFRESH** — pricing moved twice in 2026 alone. |
| `x-capture-policy-refresh-2026-07-29-p5.md` (106 lines) | Intake hierarchy; one internal model, multiple observations; reconciliation direction. | **REFRESH** for policy; the *one-model-many-observations* pattern is durable — **CARRY** that part. |
| `x-policy-api-verification-2026-07-19.md` | Original X policy/API verification. | **IGNORE** — superseded by the two refreshes above. |
| `truth-social-platform-research.md` (309 lines) | Official developer access, API documentation status, Mastodon compatibility, automation boundaries. | **REFRESH** before any Truth Social work. The Mastodon-compatibility analysis is the durable part. |
| `truth-social-capture-boundaries.md` (228 lines) | Contractual and automated-access analysis; scenario matrix and risk register. | **REFRESH** — but the risk-register *structure* is reusable as-is. |
| `sqlite-backup-and-wal-safety-verification-2026-07-27.md` (86 lines) | WAL-mode backup safety; what is actually verified vs. assumed. | **CONSULT** — short, specific, and still true unless SQLite's WAL semantics changed. Read before backup design. |

---

## Tier 3 — Doctrine and Design, Already Inherited

All **CARRY** — these are the source of the v2 vision. Listed so you can check the extraction
was faithful, not because they need porting.

- `00-doctrine/operating-doctrine.md` — the four core lines, Telescope Rule, Raw-First Rule
- `00-doctrine/editorial-anomaly-archive-direction.md` — record/label/connect/never-fabricate
- `00-doctrine/editorial-standard.md` — research sequence, publishability gate, Quinton voice
- `00-doctrine/human-approval-policy.md` — approval state machine, exact-text binding
- `03-system-design/the-record-and-angle-room.md` — the three layers, cross-database bridge
- `03-system-design/evidence-classification-model.md` — six dimensions, no-score rule
- `04-platform-rules/connection-publication-boundary.md` — publication-risk classes, fail-closed
- `02-product/no-coincidences.md` — the pattern feature's full design

**Partial carry:**

- `03-system-design/data-model-planning.md` — **CONSULT** for the exact raw-evidence boundary
  section; the rest is v1 candidate naming.
- `02-product/module-map.md`, `product-overview.md`, `workflow-overview.md`, `mvp-scope.md`,
  `non-goals.md` — **IGNORE**. v2 supersedes all five.
- `03-system-design/obsidian-qdrant-sqlite-plan.md` — **IGNORE**, self-marked superseded.
- `03-system-design/multi-account-model.md` — **CONSULT** only if multi-account activates.
- `00-doctrine/audit-artifact-boundary.md` — **CARRY the rule** (never commit live review
  prompts to the repo; a reviewer will treat them as project authority). Cheap, still right.

---

## Tier 4 — Brand

All **CARRY**, and the only category where v1's work is finished rather than superseded.
Seven files, all short. The v2 vision compresses these into one section; the originals hold
more usable material than the summary.

- `quinton-clearance-persona.md` — voice examples worth keeping verbatim
- `voice-and-style-guide.md` — the full style rules
- `brand-identity.md` — brand lines, B1-L7 numerology, tone, audience promise
- `bread-baker-canon.md` — the crumbs double-meaning, in full
- `avatar-banner-direction.md`, `profile-bio-options.md`, `x-launch-post-package.md` — asset
  and launch material; useful if any account is rebuilt

---

## Tier 5 — Operational History

| Document | What it is | Disposition |
|---|---|---|
| `09-operations/vela-incident-first-live-pipeline-pass.md` (572 lines) | The first and only live research-to-publication pass, on the 1979 Vela Incident. Contains real researched sources (National Security Archive briefing books, Wilson Center oral history, De Geer & Wright 2018) with per-source notes. | **CONSULT — see below.** |
| `08-audits/vela-findings-disposition.md` | Why it failed: 5 work items, 9 sources, **9 of 9 source notes null**, 0 approvals, 0 publications, 0 of 10 refusal tests run, largest revision stored as one 1,195-char flat blob. Reached the drafting boundary and stopped. | **CONSULT** |
| `08-audits/claude-vela-product-fit-review.md`, `claude-vela-deep-research-editorial-review.md` | The two independent reviews that diagnosed it. | **CONSULT** |
| `09-operations/mcp-tooling-plan.md` | v1 MCP steward tooling. | **IGNORE** — Windows-era. |
| `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md` (471 lines) | The full app product definition: Command Center, calendar, lanes, screens, wireframes, comparable-product analysis. | **CONSULT** — the UX source material. Read during the web-UX fog item. |
| `06-research/future-topic-research-list.md` (49 lines) | Core topic lanes, research surfaces, early content principle. | **CONSULT** — candidate first cases. |
| `06-research/deferred-humalike-quinton-reply-evaluation.md` (268 lines) | Preserved design for a reply capability, with a post-launch maturity gate. | **CONSULT** — only if replies ever activate. |
| `04-platform-rules/platform-risk-register.md` (55 lines) | Platform risk register. | **REFRESH** — structure reusable, contents aged. |
| `06-research/naming-research.md`, `domain-social-handle-notes.md` | Naming and handle research. | **IGNORE** — decided. |

### Vela is worth more than its disposition suggests

It is a **ready-made first case for v2's destination.** Nine real sources are already
identified and documented. The topic is genuinely good — a 1979 unexplained double-flash
detection with a live official-versus-scientific dispute, which is precisely the Desk's
material.

It is also a **benchmark.** v1 produced five drafts on this material and retired all five as
failed editorial artifacts. The recorded failure mode is exactly what v2's architecture was
designed against: nine sources with zero source notes, and a revision stored as one flat blob
with no claim structure underneath it.

Running Vela as v2's first case gives a direct comparison on identical material. If v2
produces something publishable where v1 produced five retired drafts, the architecture is
doing its job. If it doesn't, that is worth knowing before building anything else.

---

## Tier 6 — Ignore Entirely

**`05-implementation-planning/` (131 files)** — exact implementation packages, path envelopes,
adversarial test matrices, correction packages, milestone files. All bound to v1's commit
history, path structure, and Windows/Tauri architecture. Two exceptions:

- `m13-governed-discrepancy-detection-planning-direction.md` — **CONSULT** if No Coincidences
  ever opens. It is the complete design: corpus manifests, detector admission, two-axis
  disposition, candidate fingerprints, kill criteria.
- `hammer-test-strategy.md` — **CONSULT** for the adversarial-testing philosophy, not the
  matrices.

**`08-audits/` (99 files)** — audit reports, dispositions, correction returns, owner closures.
Historical process record. Three exceptions, all noted above (the two Vela reviews and the
findings disposition).

**`99-decisions/decision-log.md`** — D001 through D162. **IGNORE as authority.** Worth one
skim for the handful of decisions whose *reasoning* isn't captured elsewhere, then close it.

**`STATUS.md`, `LLM_MAP.md`, `PROJECT_BRIEF.md`, `README.md`, `bootstrap_git.ps1`** —
**IGNORE.** v1 navigation and a PowerShell bootstrap script.

---

## What This Changes for v2

Three items move from "we'll figure it out" to "the answer is already written":

1. **R-M06-07 (LLM context assembly)** should be read *before* the research agent is
   designed, not after. v2 hands the LLM far more autonomy than v1 ever did, and that report
   is where the prompt-injection and data-not-instruction boundaries were worked out.
2. **R-M06-03 (website and feed monitoring)** directly answers part of the open-web capture
   fog item.
3. **Vela** is the first case, and a benchmark against v1's documented failure.

Add one line to the v2 vision pointing at this index. That is the entire remedy for the
worry that started this — not porting, just a pointer.
