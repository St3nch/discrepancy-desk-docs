# The Discrepancy Desk Editorial Control Room — Product Definition

## 1. Executive Product Judgment
The owner should authorize a tightly-scoped **M04 "X Operating Workflow" functional web control room** that proves ONE complete human editorial loop — capture → research → draft (AI-proposed) → human review/approval → manual publish → reconciliation → manual metrics — plus a Command Center, a rolling-90-day scheduler with a first-class Unscheduled Reserve, and a bounded Release Watch intake. Everything else (polished calendar UX, dossiers-as-product, Reply Desk, Article Room, semantic retrieval, Hermes) should be deferred behind stable contracts. The disposable FastAPI/Jinja/HTMX UI must NOT be polished; it is a route-authority test surface and a contract reference for the future Tauri desktop client, which is where product-grade UX investment belongs.

The single most important design principle: the app is the operational brain, but it must open to a Command Center that answers "what matters now?" in under five seconds without any database knowledge. The records-first doctrine ("AI drafts. Human clears. Database remembers. Metrics judge.") is sound and should be preserved literally in the interface: the approval row is the authorization token, agents propose candidates only, and no LLM may approve, mutate accepted truth, or touch the database directly. The proposed "Archive / Docket / Flash Release" lanes are a good editorial spine and should be adopted with refinement. "Hermes Agent" should be treated as a possible future runtime behind an agent-neutral business API, not a near-term dependency.

## 2. Repository Snapshot at Research Start
**Dated snapshot (verified on 2026-07-20; research context, not continuing live authority):** App repo `discrepancy-desk` at HEAD `08ce68212923bc903d12bb21ab301fb342c170b6`, latest commit "Add executable M03 hammer closure evidence," clean, public. Docs repo `discrepancy-desk-docs` at HEAD `0631e28c06d7ffea74be62f87d8179dea8ab6640`, latest commit "Record M03 executable hammer closure review," clean at the start of this research. Later working-tree changes must be read from live repository state, not inferred from this snapshot.

**Current milestone:** M03 "Local Control Room MVP" — technical exit gate PASS, awaiting owner closure acceptance. Validation: 54 passing pytest tests, clean Ruff, executable HT matrix 19/19 in-scope invariants passed; HT-14 (mention classification) deferred by approved scope.

**Implemented persistence contracts (SQLite STRICT, Alembic-migrated):** `owned_accounts`; `work_items` with a 12-state lifecycle (captured, research_needed, research_ready, drafting, human_review_needed, approved, manual_ready, published, rejected, withdrawn, publication_mismatch, evidence_blocked); `evidence_refs` (sha256, byte_size, verification_state, path-escape CHECK constraints); `revisions` (authored_text BLOB, binding_sha256 exact-byte binding, supersedes_revision_id lineage); `approvals` (decision enum incl. superseded/consumed; action_id UNIQUE — the authorization token); `publications` (external_post_id, verification_state incl. verified_match/verified_mismatch, replaces_publication_id + resolution_kind); `metric_snapshots` (observation_method manual/api; observation_state vocabulary observed_value/observed_empty/not_requested/not_returned/unavailable/withheld/malformed/errored/unsupported; corrects_snapshot_id); `source_records` (source_kind url/manual_note/owned_post/api_evidence); `operation_keys` (idempotency: operation_type+operation_key PK, request_sha256); `audit_events` (hash-chained previous_chain_sha256/event_sha256/chain_sha256, append-only via BEFORE UPDATE/DELETE triggers).

**Governed operation surface** in `operator_service.py` (create_owned_account, capture_work_item, add_source_record, reject_review, get_control_room_item, run_matched_operator_loop) exposed via `web.py` FastAPI routes with route-level authority/rejection tests. Templates base/index/work_item/health/error — thin.

**Pre-revision roadmap snapshot used by this research:** M00 planning DONE; M01 X identity + read-only API probe DONE (App ID 33215850, read-only OAuth 1.0a, $5 cap); M02 persistence contract + hammer tests DONE; M03 ACTIVE/technically done. At research start, M04 "X Operating Workflow and Manual Metrics" was next and the MVP boundary was END OF M04. The then-current later sequence was M05 Anomaly Vault, M06 Human-Triggered Capture Helper, M07 Truth Social secondary manual-only, M08 Metrics Ledger, M09 "No Coincidences," M10 Qdrant, and M11 Hardening/backup/recovery. This historical sequence is research context, not the later proposed M04–M15 roadmap ruling. Non-goals at MVP: React/Postgres/Redis/Celery/multi-agent orchestration.

**Platform:** X first (@DiscrepancyDesk, "Quinton Clearance," bio "Every anomaly becomes paperwork.," location "Basement 1, Level 7," blue verification, discrepancydesk.com owned; first + pinned posts published). Truth Social secondary/manual/deferred (D013/D014); Truth Social DOM/browser capture NOT admitted. Persona is fictional/parody/commentary.

**Missing product layers (net-new, must be designed here):** Command Center dashboard; calendar/scheduler + Unscheduled Reserve; multi-view WIP pipeline; Ready-to-Post/Need-a-Post decision support; editorial lanes; topic dossiers; Release Watch intake; composer/review desk UX; reply desk; article package; asset/evidence separation UX; notifications; global search/saved views/command palette; metrics/monetization views; state-aware action maps; the agent-neutral business API. The MCP "gpt-mcp" FastMCP steward (decision D012) is a filesystem/Git surface for ChatGPT and is DISTINCT from the governed business-operation agent API this report recommends.

## 3. Comparable Product Findings (with citations, each pattern Adopt/Adapt/Reject/Defer)

**Social publishing/scheduling.**
- **Buffer** — drag-and-drop weekly/monthly calendar; a dedicated Drafts area separate from the queue; pre-scheduled drafts act as "placeholders" that never publish until moved to the queue; overdue drafts are flagged; "move to drafts" returns a queued item to unscheduled; tags for campaigns; hover cards on the calendar (Buffer Help Center, "How to use Buffer's calendar feature" and "Saving and scheduling draft posts," accessed 2026). **Adopt** the draft-as-placeholder-that-never-auto-publishes model (it maps exactly to our approval/manual-publish doctrine); **Adopt** move-back-to-unscheduled preserving the item; **Adopt** overdue flagging.
- **Sprout Social** — separate left-nav queues: Drafts, Needs Approval, Rejected, Failed Posts; rejection requires an optional note recorded in "Approval Activity" and emailed to author; rejected posts are never lost and can be edited/resubmitted; a message submitted as a draft must be moved to scheduled/queued before it can be approved; Reply Approvals with multiple candidate replies; an Asset Library (images/video/text "saved replies," alt-text indicator, folders, campaign tags); daily digest of posts needing review in next 24h (Sprout Social Support, "Message Approval Workflows," "Drafts," "Reply Approvals," "Asset Library," "Failed post notifications," accessed 2026). **Adopt** the distinct Drafts/Needs-Approval/Rejected/Failed queues and the approval-activity log; **Adopt** rejection-with-reason preserved; **Adopt** Failed-Posts queue concept (our analogue: publication_mismatch + manual-publish failures); **Adapt** Asset Library (we must split Assets from Evidence — see §13); **Defer** reply approvals to post-M04.
- **Typefully** — X-first thread composer with auto-split, realistic thread preview, drafts synced across devices, X Articles drafting/scheduling/preview (publishing requires X Premium), command bar (Typefully product pages and docs.x.com success story, accessed 2026). **Adopt** thread composer with live X preview and Article outline; **Adapt** command bar → command palette.
- **Thread-scheduling caveat**: X still does not support native thread scheduling (OpenTweet blog, March 2026, accessed 2026) — this reinforces our **manual-publish-with-reminder** model for threads rather than promising API auto-posting.

**Editorial/content-ops CMS.**
- **Contentful** — Tasks app gates publish (an entry can't be published until all tasks resolved), due dates with a two-day "urgent" escalation; Launch groups entries into a "Release" with a scheduled publish/unpublish and a Release Calendar; the newer Timeline stages multiple future versions of the same entry ("Ideations" = safe draft spaces); optimistic locking returns 409 on concurrent edits; scheduled actions must be cancelled/updated if content changes (Contentful Help Center + blog "Introducing Timeline," "Tasks and comments," accessed 2026). **Adopt** task-gated publish (our evidence/claim gates), release-grouping (Article packages, campaigns), and the "editing scheduled content must invalidate the schedule" rule (maps to our approval-supersession contract); **Adapt** Ideations → duplicate-as-new-candidate.
- **WordPress editorial (PublishPress/Edit Flow)** — custom statuses (Pitch/Assigned/In-progress/Reviewed), a Revision Queue where changes to published posts are held for approve/reject/schedule, Compare Revisions diff UI, role separation so contributors submit but cannot publish (PublishPress docs and WordPress.org plugin page, accessed 2026). **Adopt** revision-queue + side-by-side diff for review; **Adopt** correction-as-controlled-revision (maps to replaces_publication_id/resolution_kind). **Reject** free-form unlimited custom statuses — our 12 DB states are enough; do not let operators invent statuses.

**Work/project tools.**
- **Linear** — board layout toggled with a shortcut; **swimlanes** created by grouping into Rows (by team, project, assignee, target date); keyboard-first (peek with Space, filter with F, command menu Cmd-K, G-then-key navigation), saved views, "Triage" inbox (Linear Docs "Board layout," Swimlanes changelog 2024-04, ShortcutFoo cheat sheet, accessed 2026). **Adopt** saved views + command palette + keyboard scheduling + peek; **Adapt** swimlanes for lane/topic grouping; **Reject** WIP-limit enforcement machinery (irrelevant at solo scale).
- **Airtable/Notion** — calendar drag-to-reschedule updates the underlying date field; dragging a card from a sidebar onto a date schedules it, and dragging back to the sidebar clears the date ("unscheduling"); Timeline view groups records into swimlanes and scrolls horizontally; interface calendars are only single-table and lack real-time alerts (Airtable Support "Getting started with calendar views," "Interface Designer: Calendar Element," Notion Help "Timeline view," accessed 2026). **Adopt** the sidebar-rail + drag-onto-date + drag-back-to-unscheduled model verbatim — it is the exact interaction our Unscheduled Reserve needs; **Adopt** record-coloring by lane/status.

**Scheduling-horizon best practice.** Digital Applied's 2026 Content Calendar Planning guide states: "An effective content calendar framework operates simultaneously across three planning horizons: strategic (90 days), tactical (30 days), and operational (7 days). Each horizon requires different information density and update frequency" (accessed 2026); the same firm's Content Calendar Template 2026 adds: "Refresh as a first-class citizen. Allocate 15-25% of weekly capacity to updating existing posts, not only new production." Multiple 2026 guides (Content Pipeline/airfleet.co, Hootsuite blog, accessed 2026) likewise warn that over-scheduling relative to production capacity is the most common failure. This validates the proposed rolling-90-day active horizon (see §7) and the first-class Reserve for evergreen refresh work.

**Notification design.** Convergent best practice: default to silence, four tests for whether an event earns a notification (user asked; time-sensitive; requires a decision only the user can make; contains context unavailable elsewhere); tiered severity with critical alerts bypassing quiet hours; batching into daily digests; throttling/rate caps; in-product inbox as a first-class "muted-state" channel (Eleken "Notification UX: 8 Best Practices," Courier "How to Reduce Notification Fatigue," Smashing Magazine "Design Guidelines for Better Notifications UX," accessed 2026). **Adopt** wholesale (see §16).

**Government/official-source APIs (avoid scraper-farm).** Federal Register API (no key required, REST CSV/JSON, search + Public Inspection endpoints); National Archives Catalog API v2 (read-only key by email; bulk export; AWS Registry of Open Data mirror); FBI Wanted API (simple REST, `api.fbi.gov/wanted/v1/list`); FBI Crime Data API (UCR/NIBRS, key from CDE team); DOJ News API (14,000+ press releases, filter by date/component/topic, JSON/XML); FARA API; NCVS API (Federal Register Developer Resources, archives.gov/developer, fbi.gov/wanted/api, justice.gov/developer, accessed 2026). **Adopt** official APIs/RSS/bulk data as the ONLY admitted Release Watch intake; **Reject** DOM/browser scraping (already excluded for Truth Social); respect robots.txt and rate limits.

**Agent governance patterns.** Recent literature converges on: agent-first tool contracts returning normalized decision-support structures (confidence, ambiguity indicators, evidence references, next-step suggestions, machine-readable recoverable errors incl. `scope_violation`/`idempotency_conflict`); mandatory caller-supplied idempotency keys with a dedup window; dry-run/preview before commit; short-lived per-agent workload identity; every consequential action bound to an explicit delegation/policy decision; signed action envelopes hash-chained into an append-only journal (arXiv "Agent-First Tool API" 2605.10555; "Beyond Autonomy: Dynamic Tiered AgentRunner" 2605.10223; "From Agent Traces to Trust" 2606.04990; Zylos Research "Agent Identity and Signed Provenance" 2026, accessed 2026). **Adopt** for the future agent API (our audit_events already provides the hash-chained journal; operation_keys already provides idempotency).

## 4. Operator Personas and Work Sessions

**The Daily Human Operator (primary; solo).** Opens the app on a laptop for a 5-minute morning check ("what needs me now, what posts today, anything overdue/blocked?"), a 30-minute working session (clear reviews, approve/reject drafts, fill an empty slot, action a release candidate), occasional multi-hour deep work (build a dossier, draft a thread, reconcile metrics), an end-of-day shutdown ("anything still awaiting me? anything scheduled for tomorrow I should sanity-check?"), and a return-after-several-days "what changed" briefing. Occasionally checks status on a phone but never does deep editorial work there. Needs the app to represent DB truth honestly without requiring DB knowledge.

**The Connected LLM (working inside the app, via governed operations only).** Receives a bounded Context Assembly packet, proposes candidates (research summaries, draft alternatives, source validations, schedule recommendations, release-candidate classifications), and surfaces an operator-visible reasoning summary. It never approves, never mutates accepted truth, never touches the DB directly, and always leaves work in a "awaits human review" state. Its perspective demands: explicit tool permissions/scopes, a bounded work queue the human can pause/reorder/cancel, and full provenance logging.

**Editorial-operations designer view:** the system must make the pipeline legible and prevent both junk-drawer accumulation and weak-filler pressure. **Security/governance view:** the approval row is the only authorization token; every agent action is attributable, idempotent, and budget-capped; no hidden LLM memory is project truth.

## 5. Information Architecture

**M04 functional web control room (thin, disposable) — minimum viable IA:**
1. Command Center (home)
2. Calendar (rolling 90-day) + Unscheduled Reserve rail
3. Pipeline (work items, multiple saved views)
4. Work Item detail (composer + review desk inline)
5. Release Watch (intake queue)
6. Metrics (manual snapshots + monetization pace)
7. System Health (existing health.html, extended)

Dossiers, Assets/Evidence browsers, Reply Desk, and Article Room are **links/stubs** in M04, not full screens.

**Future Tauri app — recommended top-level areas (cleaner than the 14-area proposal).** The proposed 14 areas (Command Center/Calendar/Pipeline/Release Watch/Dossiers/Draft Desk/Review Desk/Reply Desk/Articles/Assets/Evidence Vault/Metrics/Ghost Activity/System Health) is too broad and will fragment attention. Collapse to **8 primary areas** with sub-tabs:
1. **Command Center**
2. **Calendar** (with Unscheduled Reserve)
3. **Pipeline** (Draft Desk + Review Desk are states/panels within a work item, not separate top-level areas)
4. **Release Watch**
5. **Dossiers** (Reply candidates and Articles live as item types surfaced here and in Pipeline)
6. **Library** (Assets + Evidence Vault as two tabs — separated data, one nav home)
7. **Metrics**
8. **System** (Health + Ghost/LLM Activity + audit log)

Reply Desk and Article Room become **views/filters** inside Pipeline/Dossiers rather than standalone areas until usage justifies promotion. This keeps the desk feeling like one operational surface, not a suite.

## 6. Command Center Specification
The home screen answers every key question above the fold. Sections, top to bottom:

- **Today** — date/time, X account status (connected/verified), what is scheduled to post today (with exact times and manual-publish reminders), and a one-line "on pace / behind" monetization ribbon.
- **Needs Attention (prioritized queue)** — a single ranked list, not scattered badges. Ranking: (1) overdue manual posts, (2) publications awaiting reconciliation, (3) publication_mismatch, (4) drafts in human_review_needed, (5) evidence_blocked items, (6) release candidates needing authority review, (7) items nearing the 90-day edge, (8) blocked-7+-days. Each row: what it is, why it's here, the single primary action, and age.
- **Need a Post (empty/weak-slot detection)** — shows today's/tomorrow's gaps and proposes specific ready content with rationale (see §9). Must never manufacture pressure to post weak filler.
- **Ready-to-Post queue** with sub-states: Approved-but-unscheduled · Scheduled-and-ready · Manual-ready-now · Overdue-for-manual-posting · Posted-awaiting-reconciliation.
- **Work-in-Progress by stage** — counts per lifecycle stage (captured/research/drafting/review/approved), each a link to the filtered Pipeline view.
- **Pipeline Health** — schedule coverage over 7/14/30/60/90 days (e.g., "next 7 days: 4 of 7 slots filled"), reserve depth by readiness, lane balance (Archive/Docket/Flash), topic-repetition warnings, format balance (single/thread/article), and blocked-age.
- **Monetization Progress** — rolling-90-day organic impressions vs. the 5M threshold, verified-follower count vs. 500 (Ads/Creator Revenue Sharing) / 2,000 (Subscriptions), threshold pace vs. actual pace, with explicit anti-correlation-as-causation warnings (see §15).

Primary actions surfaced on the Command Center: "Review next draft," "Fill this slot," "Reconcile publication," "Action release candidate," "Record metric." Everything else is one click away.

## 7. Calendar and 90-Day Scheduler Specification
**Views:** day, week, month, rolling-90-day timeline, agenda (list), and a coverage heatmap. A persistent **Unscheduled Staging Rail** sits beside the calendar (Airtable/Buffer pattern).

**Scheduling rules (recommendation on the 90-day boundary):** The rolling-90-day maximum active horizon is **correct and should be kept**, because it matches the industry-standard strategic quarter while staying responsive to a news-reactive parody desk. Exact behavior:
- Nothing may be *actively scheduled* (assigned an exact publish time) more than 90 days out. Ideas/dossiers/evergreen persist **indefinitely** in the Reserve, never occupying calendar slots.
- The calendar renders the next ~90 days; items can be dragged onto a date, dragged to reschedule, or dragged back to the Reserve. Moving preserves full history (audit_events).
- **Editing approved content invalidates/supersedes the approval** per the revision contract (new revision, prior approval → superseded). This is enforced, visible, and explained at the moment of edit.
- **Changing publication TIME alone must not alter approved TEXT** — time is a schedule-slot attribute, not part of the binding_sha256-bound revision. The UI states this explicitly.
- Items within ~7 days of the 90-day edge are surfaced for review/move/return-to-Reserve/mark-dormant.

**Interface distinguishes six date concepts** (do not collapse): preferred publication window · exact scheduled time · earliest useful date · expiration/stale-after date · hard deadline · flexible evergreen status.

**Drag/drop & bulk:** drag onto date; drag to reschedule; drag back to unscheduled; multi-select move; **duplicate-as-new-candidate (never clone an approval)** — a duplicate is a fresh unapproved revision; preview; side-panel open (peek); keyboard scheduling.

**Filters:** lane, status, topic, format, campaign, risk, required-action.

**On-calendar intelligence (warnings, not blocks):** collision display (two items same slot), empty-window display, topic-repetition warning, format-imbalance warning, expiration warning (item will go stale before its slot), and approval-invalidation warning (dragging/editing will supersede approval).

**Card fields:** visible on card — lane color, title/first line, format icon, exact time, approval state; on hover/peek — sources attached, claim classifications, expiration date, risk flag, "changes here will invalidate approval."

## 8. Pipeline and Work-in-Progress Specification
Provide **multiple saved views over one work-item table**, not a single Kanban. Recommend for **M04 the smallest useful set** (three) and defer the rest:

**M04 (build):**
1. **Status board** — columns by the 12 lifecycle states (grouped: Intake → Research → Drafting → Review → Ready → Published → Closed).
2. **My Next Actions** — everything awaiting the human, ranked (mirrors Command Center Needs Attention).
3. **Human Review queue** — drafts in human_review_needed with the review desk one click away.

**Later web / Tauri (defer):** editorial-lane board (swimlanes by Archive/Docket/Flash), topic/dossier view, priority queue, blocked-work view, aging-work view, LLM-assigned work view, release-candidate triage board. Swimlane grouping (Linear pattern) is a Tauri-product feature.

**Work-item detail fields:** id; lane; topic/dossier link; lifecycle state (+ permitted actions, see §17); format; capture source; source_records + evidence_refs (with verification_state); revisions (with binding_sha256, supersedes lineage); current approval (action_id, decision); schedule slot (six date concepts); publication (external_post_id, verification_state, replacement lineage); metric snapshots; claim classifications; risk flags; blockers; dependencies; age-in-state; assigned LLM jobs; audit trail.

**Blockers/dependencies/aging:** an item may be blocked-on-evidence (→ evidence_blocked state), blocked-on-external-release (waiting-on Release Watch), or blocked-on-human-answer (LLM waiting). Aging is age-in-current-state; blocked-7+-days is a saved view and a Needs-Attention trigger. Dependency warnings appear when a scheduled item depends on an unpublished prerequisite (Contentful reference-integrity lesson).

## 9. Ready-to-Post and Need-a-Post Specification
**Ready-to-Post eligibility (a work item qualifies only if ALL hold):** current revision has a live (non-superseded, non-consumed) approval; required evidence_refs are verification_state-verified; claim classifications are set; no unresolved factual-risk flag; not expired. Sub-states as in §6.

**"Need a Post" decision support.** When a slot is empty/weak, the system inspects: schedule gaps; ready inventory; lane balance; topic repetition; recent performance; expiration risk; open dossiers; recurring series due; official releases pending; follow-up candidates; audience-fatigue signals; and operator time available. **Every recommendation carries:** rationale ("why this, why now, why this slot"); evidence readiness; expected effort (post-as-is vs. needs 10-min edit vs. needs image); urgency; risk; whether a live approval already exists; whether any change would invalidate that approval; and lane/format fit.

**Anti-pressure rule (explicit product requirement):** if nothing genuinely fits, the system says so ("No strong candidate for this slot — leaving it empty is the correct call") and never ranks weak filler as if it were ready. Empty slots are acceptable; low-quality posting to chase cadence is not. This directly counters the "metrics-driving-low-quality-content" anti-pattern (§23).

## 10. Editorial Lanes and Dossiers
**Evaluate Archive / Docket / Flash Release.** These are net-new proposals; adopt with clarified definitions:
- **The Archive** — evergreen, reference, and "resurfaceable" records with no time pressure; lives mostly in the Reserve. (Analogy: Buffer drafts backlog / evergreen.)
- **The Docket** — active tracked matters with ongoing work, scheduled posts, and follow-ups (the working caseload).
- **The Flash Release** — time-sensitive rapid-response items triggered by an official-source release (the fast path from Release Watch, §11).

Recommendation: keep the three lanes as a first-class `editorial_lane` concept used for filtering, coloring, and balance metrics — but make lane a **property of a work item**, not a separate pipeline. Lane balance is a Pipeline Health metric. The B1-L7/"paperwork" persona maps naturally: Archive = filed records, Docket = open case files, Flash Release = urgent memos.

**Topic Dossiers.** A dossier is a durable research memory for a subject. Fields: overview/scope; chronology; primary sources; extracted claims; conflicting claims; witnesses/orgs; unresolved questions; accepted conclusions; rejected conclusions; related publications; scheduled future work; anniversaries/expected releases; terminology changes; LLM-candidate connections; human notes. **Every assertion must be typed:** Source-fact / Extracted-claim / Allegation / Inference / Parody-framing / Accepted-conclusion / Disputed-conclusion / Unknown. This typing is the anti-misinformation backbone and prevents parody from being recorded as fact.

**How dossiers relate to work items without a second uncontrolled knowledge base:** a dossier is a *view/index* over governed records (source_records, evidence_refs, revisions, publications) plus typed human/LLM notes — NOT a parallel truth store. Accepted conclusions in a dossier must trace to evidence and are still subject to the approval contract before they can appear in a published post. Recommendation: model dossier as a lightweight entity that *links* to existing records; forbid it from holding un-sourced "facts." Build dossiers as a **Later/Tauri** feature (M05 Anomaly Vault, Obsidian-compatible Markdown, is the natural home); in M04 a dossier is just a topic tag + a notes field.

## 11. Release Watch and Rapid Filing
**Intake queue** distinguishing states: unseen · possible-duplicate · known-update · recycled · substantive-new · irrelevant · needs-authority-review · accepted · rejected · deferred.

**Candidate fields:** source org; URL; release title; release timestamp; discovery timestamp; content hash; doc/media type; **authority classification; novelty classification; relevance classification; urgency classification** (kept as FOUR separate axes — do NOT collapse into one mysterious score); related dossier; related prior Desk posts; agent summary; human decision.

**Conversion outcomes** when a release is accepted: → Flash Release work item · → Docket update · → Archive candidate · → source for an existing item · → dismissed duplicate · → watch-only record.

**Anti-scraper-farm rule:** intake sources are limited to official APIs/RSS/bulk data — Federal Register API (no key, REST JSON/CSV), National Archives Catalog API v2, FBI Wanted API, FBI Crime Data API (UCR/NIBRS), DOJ News API, FARA/NCVS where relevant, and FOIA reading rooms. Respect robots.txt and documented rate limits. No DOM/browser scraping. In M04, Release Watch is a **manual + single-API** intake (recommend Federal Register API first — it needs no key and is fully RESTful); additional feeds and any agent-assisted triage come later.

## 12. Draft, Review, Reply, and Article Workspaces

**Post Composer.** Multiple draft alternatives (each a revision); exact revision history with supersedes lineage; char count; X single/thread/Article previews (Typefully pattern); media preview + alt-text; source refs; per-claim classification; tone/"Quinton-voice-strength" control; warnings (factual-risk, unsupported-claim, duplicate-language, similarity-to-recent); link validation; media-requirement checks. AI drafts here as candidates only.

**Review Desk (the approval gate).** The screen must answer: *what exactly am I approving* (the exact bytes bound by binding_sha256); *what sources support it* (evidence_refs + verification_state); *what changed since the prior revision* (side-by-side diff, WordPress Compare-Revisions pattern); *what remains uncertain* (LLM uncertainty summary + open questions); *what happens if I edit after approving* (edit → approval superseded). Actions: approve (writes the approval row = the authorization token, action_id UNIQUE), reject-with-reason (preserved, item returns to drafting; Sprout pattern), request successor revision. Publication mismatch and replacement-publication flows are handled here (see §17).

**Reply Desk.** Candidate conversation list; account/thread context; why it fits; prior interactions; related dossier; suggested reply alternatives; factual support; risk; expiration/urgency; the approved *exact* reply text (Sprout's "replies can't be edited after submission" discipline maps to our exact-byte binding); manually-posted reply URL; follow-up. Distinguish reply types: useful-factual-addition / discrepancy-observation / source-pointer / respectful-correction / Quinton-character / promotional / irrelevant-trend-chasing. Avoid engagement-bait. **Recommendation: Reply Desk is post-M04** (M06+), because replies multiply governance surface (each reply is a candidate needing approval + manual posting) and the core loop must be proven first.

**Article Room.** Treat an Article as a content **package**: idea/thesis; outline; research packet; per-section status; citations; media; revisions; fact-check; approval; target window; excerpt-post plan; launch thread; follow-up posts; metrics; evergreen resurfacing. Dependent posts (launch thread, excerpts) are child work items with their own approvals. X Articles require Premium to publish (Typefully/AlternativeTo, 2026). **Recommendation: Article Room is Later/Tauri**; in M04 an "article" is just a long work item.

## 13. Asset and Evidence Organization
**These are two different things and must be separated (Sprout's Asset Library is the model for Assets, but Evidence is stricter).**
- **Assets** (reusable creative): images/banners/diagrams/approved-screenshots/templates/graphics/reusable elements; metadata alt-text, usage rights, dimensions, crops, last-used, related items. Mutable, versionable, meant for reuse.
- **Evidence** (immutable proof): source refs, sha256 hashes, stored files, official URLs, capture metadata, provenance, verification_state, related claims. Backed by the existing `evidence_refs` + `source_records` tables. **Immutable and append-only.**

**An image can be both.** Recommendation: store the bytes once; represent Asset and Evidence as two *roles* linking to the same stored file by hash. An official screenshot used as proof is Evidence (immutable, hash-bound, verification_state) and may ALSO be registered as an Asset (reusable, alt-text, crops) — but the Asset role can never overwrite or re-hash the Evidence role. **Publication behavior:** when a post publishes, the exact media used is bound to the publication record; later edits to the Asset (new crop) create a new asset version and do not alter the historical evidence/publication binding. Build the Evidence Vault properly (it already exists in schema); build the Asset Library **Later/Tauri**.

## 14. Connected LLM Experience
**Bounded Context Assembly packet** (only what's relevant): applicable doctrine + voice guide; the specific work-item state; accepted sources; candidate sources; prior related publications; approved conclusions; unresolved questions; schedule constraints; performance history; operator preferences; tool permissions/scopes. Nothing else — no ambient access to the whole DB.

**Suggested Actions list** — the LLM proposes a short ranked list of next candidate operations, each mapped to a governed business operation it is permitted to call.

**Operator-VISIBLE reasoning summary** (NOT private chain-of-thought): evidence used; assumptions made; unresolved uncertainty; why suggested; what could make it wrong; the specific human decision required. This is a product requirement, echoing agent-provenance research (arXiv 2606.04990) that runtime trust requires visible evidence/assumptions, not raw traces.

**Bounded LLM work queue** with states: ready-for-research · comparison · drafting · source-validation · alternative-angles · schedule-recommendation · performance-review · waiting-for-human-answer. The human can pause/reorder/cancel/reject any job.

**LLM Activity / Provenance panel ("Ghost Activity"):** agent identity; provider; model; job/session id; operation invoked; source records used; start/completion timestamps; token counts; cost; result; rejection; retry count; memory/skills availability; and whether the output awaits review. Backed by audit_events (hash-chained) and operation_keys (idempotency).

**Features making strong use of LLM capability (all candidate-only):** ranking the Reserve for a specific empty slot *with per-item rationale*; drafting multiple angle alternatives; validating that each claim maps to an evidence_ref; classifying release candidates on the four axes; proposing dossier connections labeled as candidate-inferences; summarizing performance patterns labeled observation/correlation/hypothesis. **Hard limits:** no self-approval; no mutation of accepted truth; no direct DB writes; candidates only through governed operations; no reliance on hidden LLM memory as project truth.

## 15. Metrics and Monetization
**Useful measurements (manual-first, honest about gaps):** impressions; engagements; replies; reposts; likes; bookmarks (where available); profile visits; follower changes; Article reads; link clicks (where available); verified-user engagement (where available). Every value carries an **observation_state** from the existing vocabulary (observed_value/observed_empty/not_requested/not_returned/unavailable/withheld/malformed/errored/unsupported) and observation_method (manual/api). "Unavailable" and "unknown" are first-class, never silently zero.

**Rolling-90-day monetization progress (using verified official criteria).** X's current official **Creator Revenue Sharing** eligibility (help.x.com/en/using-x/creator-revenue-sharing, accessed July 20, 2026): active Premium/Premium Business/Premium Organizations subscription; **at least 5M organic impressions within the last 3 months**; **at least 500 verified followers**; supported country; User Agreement compliance. Payouts are every two weeks, **$30 minimum**, via Stripe. **Creator Subscriptions** eligibility (help.x.com/en/using-x/subscriptions-creator and content-monetization-standards, accessed July 20, 2026): 18+; active in past 30 days; **at least 2,000 verified followers**; **at least 5M organic impressions within the last 3 months**. Important verified nuance: the threshold is **"verified followers"** (followers who are themselves Premium/Premium Business/Premium Organizations subscribers), not raw followers — a common third-party imprecision (many blogs cite "500 followers" unqualified). The Monetization panel should track both programs' thresholds separately and show pace-vs-threshold on the impressions (5M/90-day) and verified-follower (500 / 2,000) axes.

**X API cost context.** As of 2026 the X API uses pay-per-use pricing with the free tier discontinued for new developers: $0.015 per post created, $0.20 if the post contains a URL, and $0.005 per post read, capped at 2 million post reads per monthly billing cycle (docs.x.com/x-api/getting-started/pricing: "Pay-per-usage plans are capped at 2 million Post reads per monthly billing cycle"; pay-per-use default took effect February 2026, URL pricing added April 20, 2026 — the URL rate rose roughly 1,900% in that repricing). The project's **manual-publish model avoids the $0.015-per-post write charge (and the $0.20 charge on any post containing a URL)** entirely and keeps operations within the documented $5-style cap. Any future API-assisted publishing must budget explicitly against these rates, especially for link posts.

**LLM pattern labeling (mandatory):** every LLM-surfaced pattern is tagged observation / correlation / hypothesis / experiment / accepted-conclusion. **Anti-overclaiming rules:** the app must display an explicit "correlation-is-not-causation" caution on monetization-pace and performance panels, must never present a correlation as a cause, and must never let a metrics target justify posting weak content.

**Metric views:** by post, topic, format, publication time, recurring series, follower conversion, rolling-90-day monetization, follow-up, evergreen longevity. **Recommendation:** M04 ships manual snapshot capture + the rolling-90-day monetization ribbon + per-post metrics; richer analytics views are Later-web; the polished metrics dashboard is Tauri-product.

## 16. Notifications, Search, Saved Views, and Keyboard Workflow
**Notification levels:** Critical · Action-required · Time-sensitive · Informational · Silent-log. **Controls:** snooze/dismiss/resolve/convert-to-task/suppress-repeats/quiet-hours/daily-briefing/"do-not-notify-unless-actionable." Apply the four-tests rule (§3): default to silence; only Critical bypasses quiet hours; batch the rest into a daily briefing; the in-app Needs-Attention queue is the primary muted-state channel. The app must not become noisy — this is a first-class requirement given a solo operator.

**Global search** across work items, dossiers, sources, claims, revisions, publications, metrics, assets, release candidates, schedules, and audit history.

**Saved views** (ship these named views): Today · Needs-my-approval · Ready-but-unscheduled · Need-a-post · Blocked-7+-days · Flash-Releases · Evergreen-reserve · Publication-mismatches · Metrics-due · High-performing-topics · No-recent-activity · LLM-waiting-for-me · Unresolved-allegations · Evidence-missing.

**Keyboard/command palette** (Linear model): Cmd-K command palette; peek with Space; G-then-key navigation; keyboard scheduling; quick approve/reject. Keyboard-first is a **Tauri-foundation** priority; in M04 provide a basic command palette and the saved views.

## 17. State-Aware Actions and Error Recovery
**State-action map (operator need not know DB states, but UI represents them truthfully).** For each of the 12 states, define permitted human actions, permitted LLM proposals, blocked actions (+ plain explanation), required evidence, next recommended action, urgency, relevant views. Examples:
- **captured:** human can send to research or reject; LLM may propose research plan; cannot draft yet (no research). Next: assign research.
- **research_ready:** human/LLM may draft; LLM proposes draft alternatives; approval blocked (no revision). Next: draft.
- **human_review_needed:** human approves/rejects/requests-successor; LLM may not approve; editing creates a new revision. Next: review.
- **approved:** human may schedule or manual-publish; **editing supersedes approval** (explained); LLM may not alter. Next: schedule/publish.
- **manual_ready → published:** human records external_post_id; system moves to awaiting-reconciliation.
- **published → publication_mismatch:** if verification finds verified_mismatch, offer replacement-publication flow (replaces_publication_id/resolution_kind).
- **evidence_blocked:** all drafting/approval blocked until evidence verified; explanation names the missing/failed evidence.

**Error/Recovery/Trust — every error explains: what happened / what was preserved / what was NOT changed / what you can safely do next; never expose raw SQLite or stack traces.** Cases: failed save (nothing committed, retry safe via operation_keys idempotency); idempotent retry (duplicate suppressed, original result returned); stale revision (someone/something superseded it — show the new one, Contentful 409 lesson); approval invalidation (edit superseded approval — re-review required); malformed LLM output (rejected, not stored as truth, retry count incremented); missing/changed source (evidence_blocked, publication flagged); duplicate candidate (flagged, offered as new revision not a clone); schedule collision (warned, not blocked); past-due schedule (moved to overdue-manual, Buffer pattern); publication mismatch (reconciliation flow); metric-observation failure (recorded with observation_state errored/unavailable); DB integrity warning; migration dirty state (safe read-only mode + guidance); backup failure; agent offline; provider outage; cost limit reached (respect the $5-style cap; queue paused, human notified).

## 18. Conceptual Data Model (recommend, not implement; no physical schema)
Entities and their status against the current schema:

- **Already EXIST (reuse):** work-item (`work_items`), source (`source_records`), evidence (`evidence_refs`), draft-revision (`revisions`), approval (`approvals`), schedule info partially (see below), publication (`publications`), metric-observation (`metric_snapshots`), agent-request/idempotency (`operation_keys`), audit (`audit_events`), owned-account (`owned_accounts`).
- **MISSING (add as real entities):** editorial-lane (small enum/table); topic-dossier (lightweight, links only); release-candidate (with the four classification axes); task (evidence/claim gates); dependency (between work items); reply-candidate (defer build); article-package (defer build); asset (defer build; distinct from evidence); LLM-job; operator-preference; saved-view; notification.
- **Should be VIEWS, not tables:** Ready-to-Post, Need-a-Post, Pipeline Health, Monetization Progress, most saved views, lane balance — all derived from existing rows. Do not persist derived state.
- **schedule-slot:** recommend a real but thin entity (six date concepts + link to work item + link to approved revision); crucially, schedule time is NOT part of the revision binding.
- **claim / research-note:** claim classification should attach to revisions/dossier assertions with the typed vocabulary (§10); research-note belongs to the dossier index, must link to governed records, never hold un-sourced facts.
- **Duplication-risk hotspots:** dossier vs. work item (dossier must be an index, not a second truth store); asset vs. evidence (same bytes, two roles, evidence immutable); LLM memory vs. DB (DB is truth, LLM memory is never authoritative).

## 19. Web Versus Tauri Scope

| Feature | M04 web | Later web | Tauri foundation | Tauri product | Agent milestone | Deferred |
|---|---|---|---|---|---|---|
| Command Center (core queues) | ✅ | | | polish | | |
| Rolling-90-day calendar (basic) | ✅ | | | | | |
| Calendar polished drag/drop, heatmap, multi-select | | partial | | ✅ | | |
| Unscheduled Reserve (basic subdivisions) | ✅ | | | rich | | |
| Pipeline: 3 core saved views | ✅ | | | | | |
| Pipeline: swimlanes, extra boards | | | | ✅ | | |
| Work-item detail + review desk | ✅ | | | polish | | |
| Composer with X/thread/Article preview | basic | | | ✅ | | |
| Ready-to-Post / Need-a-Post logic | ✅ | | | UX polish | | |
| Release Watch (manual + Federal Register API) | ✅ | more feeds | | | agent triage | |
| Dossiers | tag+notes stub | | | ✅ (M05) | | |
| Reply Desk | | | | ✅ | approvals | M06+ |
| Article Room | | | | ✅ | | |
| Assets library | | | | ✅ | | |
| Evidence Vault | reuse schema | browser | | polish | | |
| Metrics: manual snapshot + monetization ribbon | ✅ | more views | | dashboard | | |
| Notifications | in-app queue | digest | native/tray | | | |
| Global search + saved views | basic | | | ✅ | | |
| Command palette / keyboard-first | basic | | ✅ | | | |
| Desktop shell, panes, tray, native files, secure creds | | | ✅ | | | |
| Ghost/LLM Activity panel | read-only stub | | | ✅ | ✅ | |
| Agent-neutral business API (identity/scopes/provenance/budgets/mock-agent) | | | | | ✅ | |
| Hermes pilot (one restricted Release-Watch write profile) | | | | | after API | ✅ until then |
| Semantic retrieval (Qdrant) | | | | | | ✅ (M10) |
| Truth Social | | | | | | ✅ (M07) |

## 20. Suggested Screen Inventory
For each proposed screen: purpose · primary user question · main objects · primary actions · secondary actions · LLM assistance · empty state · error state · mobile relevance · web/Tauri target.

1. **Command Center** — orient the operator instantly · "What matters now?" · queues, health, monetization ribbon · review/fill/reconcile/record · dismiss/snooze · ranks Needs-Attention & proposes slot fills · "All clear — nothing needs you" · degrades to cached read-only with banner · high (phone status) · M04 web + Tauri product.
2. **Calendar + Reserve** — plan the next 90 days · "What posts when, where are gaps?" · schedule-slots, work items, reserve · drag/schedule/reschedule/duplicate-as-new · filter/peek · ranks reserve for a slot with rationale · "No scheduled posts — drag from Reserve" · collision/expiry warnings inline · medium · basic M04, product Tauri.
3. **Pipeline** — manage WIP · "What's in flight and what's mine?" · work items · advance state/assign/open · filter/save view · summarizes blockers · "No active work" · shows stuck-state reasons · medium · M04 web + Tauri.
4. **Work Item / Review Desk** — decide on exact content · "What am I approving and on what evidence?" · revisions, evidence, approval, publication · approve/reject/request-successor/schedule/record-post · diff/preview · draft alternatives, uncertainty summary · "No revision yet — draft or ask LLM" · stale-revision & approval-invalidation messages · low · M04 web + Tauri.
5. **Need a Post** — fill gaps without weak filler · "What should I post now?" · slot + ranked candidates · pick/edit/schedule · dismiss · ranks with rationale/effort/risk · "No strong candidate — leaving empty is correct" · n/a · M04 web + Tauri.
6. **Release Watch** — triage official releases · "What official news needs attention?" · release candidates · accept→convert/reject/defer · classify four axes · agent summary/classification · "No new releases" · feed-error banner naming source · low · M04 web + Tauri.
7. **Dossier** — durable subject memory · "What do we know and what's unresolved?" · typed assertions, sources, linked items · add note/link record · propose connections (candidate) · "New dossier — add scope" · n/a · Tauri product (stub M04).
8. **Metrics** — judge performance honestly · "What's performing, are we on pace?" · snapshots, monetization pace · record metric · filter by view · labels patterns · "No metrics yet" · shows observation_state gaps, never fake zeros · medium · M04 basic + Tauri.
9. **Ghost/LLM Activity** — supervise the agent · "What did the LLM do, at what cost, awaiting me?" · LLM jobs, provenance · pause/reorder/cancel/reject · inspect provenance · self-reports reasoning summary · "No agent activity" · provider-outage/cost-cap notices · low · read-only stub M04, product Tauri + agent milestone.
10. **Library (Assets/Evidence)** — organize reusable creative vs. immutable proof · "Where's the file/proof?" · assets, evidence_refs · register/link/verify · crop/alt-text (asset only) · alt-text suggestions · "Empty library" · verification-failure notices · low · Evidence reuse M04, Assets Tauri.
11. **System Health** — trust the machine · "Is the desk healthy?" · DB/migration/backup/agent status · run check/backup · view audit chain · n/a · "All systems nominal" · integrity/migration-dirty guidance · low · M04 (extend health.html) + Tauri.

## 21. Text Wireframes

**Command Center**
```
┌ THE DISCREPANCY DESK ─ Command Center ───────── Basement 1, Level 7 ┐
│ Today: Sun 20 Jul 2026  •  @DiscrepancyDesk ✓ connected            │
│ Monetization: 3.1M/5M impressions (90d) • 380/500 verified • BEHIND│
├────────────────────────────────────────────────────────────────────┤
│ NEEDS ATTENTION (7)                                                 │
│  ⚠ 1 OVERDUE manual post — "Memo re: missing minute"   [Post now]   │
│  ⦿ 2 awaiting reconciliation                            [Reconcile] │
│  ✎ 1 draft awaits your approval                         [Review]    │
│  ⛔ 1 evidence_blocked — source hash unverified          [Open]      │
│  ⧗ 1 release candidate needs authority review           [Triage]    │
├────────────────────────────────────────────────────────────────────┤
│ NEED A POST — Today 5:00pm slot EMPTY                               │
│  Best fit: "Anniversary of File 7-B" (Archive, approved, ready)     │
│   why: fills gap, no topic repeat 14d, evergreen, effort=post-as-is │
│  [Schedule]  [See 2 more]   ·  No weak filler suggested.            │
├────────────────────────────────────────────────────────────────────┤
│ READY TO POST   Approved·unsched 3 | Scheduled 4 | Manual-now 1     │
│ WIP  Captured 2 · Research 3 · Drafting 2 · Review 1 · Approved 3   │
│ PIPELINE HEALTH  7d ████░ 4/7 · 30d 62% · Reserve 18 · Flash 1      │
└────────────────────────────────────────────────────────────────────┘
```

**90-Day Calendar**
```
┌ Calendar ─ [Day][Week][Month][90-Day][Agenda][Heatmap] ── Filters ▾ ┐
│ UNSCHEDULED RESERVE ▸        │  JUL 2026                            │
│ ┌ Ready·approved (3) ─────┐  │  Mo Tu We Th Fr Sa Su               │
│ │ ▪ File 7-B anniversary  │  │  .. .. .. .. .. .. 20●              │
│ │ ▪ Docket update: FR-291 │  │  21 22◆ 23 24● 25 26 27             │
│ ├ Nearly-ready (5) ───────┤  │  28 29 30 31                        │
│ ├ Evergreen (7) ──────────┤  │  ● scheduled  ◆ collision  ○ empty  │
│ ├ Needs-image (2) ────────┤  │  ⌁ item near 90d edge → review      │
│ └ Dormant (1) ────────────┘  │  drag ▪ onto a date to schedule     │
│  drag back here to unschedule│  editing approved text = supersede  │
└──────────────────────────────┴──────────────────────────────────────┘
```

**Work Pipeline**
```
┌ Pipeline ─ View: [Status ▾] Saved: My-Next-Actions | Review queue ──┐
│ INTAKE     RESEARCH     DRAFTING    REVIEW      READY      PUBLISHED │
│ ▫cap-2    ▫res-3       ▫drf-2      ▫rev-1      ▫apr-3     ▫pub-9     │
│ [File-9]  [File-7B]    [Memo-12]   [Minute-4]  [Anniv]    [Pinned]  │
│           ⛔evi-block                ← awaits you                     │
│ filters: lane· status· topic· format· campaign· risk· action        │
└──────────────────────────────────────────────────────────────────────┘
```

**Work Item**
```
┌ Work Item #0142 ─ Lane: Docket ─ State: human_review_needed ─────────┐
│ Topic/Dossier: File 7-B  •  Format: thread (3)  •  Risk: LOW        │
│ REVISIONS  r3 (current) ← r2 ← r1     [Diff r2→r3]                  │
│ EVIDENCE  ✓ ev#88 sha256… verified · ✓ ev#89 verified               │
│ CLAIMS    [Source-fact][Extracted-claim][Parody-framing]            │
│ APPROVAL  none live — editing after approval will supersede it       │
│ SCHEDULE  earliest:22Jul · preferred:eve · expires:15Sep · slot:—   │
│ LLM       uncertainty: "date of memo unconfirmed; framed as query"  │
│ [Approve r3]  [Reject w/ reason]  [Request successor]  [Preview]     │
└──────────────────────────────────────────────────────────────────────┘
```

**Ready to Post**
```
┌ Ready to Post ──────────────────────────────────────────────────────┐
│ ⏰ Manual-ready NOW (1): "Memo re: missing minute" [Post→record URL] │
│ ⚠ OVERDUE (1): "Filing 3-A" was due 09:00 [Post now][Reschedule]     │
│ ✓ Scheduled & ready (4)   ▫ Approved·unscheduled (3) [Schedule]      │
│ ⦿ Posted·awaiting reconciliation (2) [Enter external_post_id]        │
└──────────────────────────────────────────────────────────────────────┘
```

**Need a Post**
```
┌ Need a Post ─ Slot: Today 17:00 (empty) ────────────────────────────┐
│ 1. Anniversary File 7-B  fit:evergreen+no-repeat  effort:as-is       │
│    approved✓  risk:LOW   [Schedule here]                             │
│ 2. Docket FR-291 update  fit:timely  effort:10-min edit (supersedes) │
│    approved✗  risk:LOW   [Open to finish]                            │
│ ── No other candidate meets the quality bar. Empty is acceptable. ──│
└──────────────────────────────────────────────────────────────────────┘
```

**Release Watch**
```
┌ Release Watch ─ Source: Federal Register API (RSS/JSON) ────────────┐
│ NEW (3)                                                              │
│  ▸ "Notice 2026-291" FR • authority:HIGH novelty:NEW rel:MED urg:MED │
│     agent: "overlaps File 7-B chronology"   [Accept▾][Defer][Reject] │
│        Accept as → Flash Release | Docket update | Archive | Source  │
│  ▸ "EO amendment" possible-duplicate of #0140                        │
│ ── intake limited to official APIs/RSS/bulk data; robots.txt honored │
└──────────────────────────────────────────────────────────────────────┘
```

**Dossier**
```
┌ Dossier: File 7-B ──────────────────────────────────────────────────┐
│ Scope · Chronology · Sources · Claims · Unresolved · Conclusions     │
│ ASSERTIONS (typed)                                                   │
│  [Source-fact] FR notice dated 12 Jun (ev#88)                        │
│  [Allegation] minute went missing — per thread, UNVERIFIED           │
│  [Parody-framing] "every anomaly becomes paperwork"                  │
│  [Unresolved] who authored the missing minute?                       │
│ LINKED WORK ITEMS: #0140 (pub), #0142 (review)                      │
│ LLM candidate connection: overlaps Notice 2026-291 (unconfirmed)    │
└──────────────────────────────────────────────────────────────────────┘
```

**Review Desk**
```
┌ Review Desk ─ #0142 r3 ─────────────────────────────────────────────┐
│ WHAT YOU'RE APPROVING (exact bytes, binding_sha256 e3a1…):          │
│  "1/ A minute is missing from File 7-B. The Desk has questions…"     │
│ CHANGED SINCE r2: +"(dates unconfirmed)"  [full diff]               │
│ SOURCES: ev#88 ✓verified  ev#89 ✓verified                           │
│ UNCERTAIN: memo date unconfirmed; framed as question, not claim      │
│ IF YOU EDIT AFTER APPROVING: approval is superseded, re-review req'd │
│ [APPROVE]   [REJECT + reason]   [REQUEST SUCCESSOR]                  │
└──────────────────────────────────────────────────────────────────────┘
```

**Metrics**
```
┌ Metrics ─ [Post][Topic][Format][Time][90-Day Monetization] ─────────┐
│ ROLLING 90-DAY                                                       │
│  Impressions 3.10M / 5.00M  ███████░░░  pace: BEHIND (-0.4M)         │
│  Verified followers 380/500 (Rev-Share)  •  380/2000 (Subs)         │
│  ⚠ correlation ≠ causation: pace shown, not attributed to any post   │
│ PER-POST (last)  Pinned: impr 42,110 · eng 1,204 · reposts 88        │
│  Bookmarks: withheld (observation_state)  Link clicks: n/a          │
└──────────────────────────────────────────────────────────────────────┘
```

**Ghost Activity**
```
┌ Ghost / LLM Activity ───────────────────────────────────────────────┐
│ Agent: draft-assistant  Provider: <configured>  Model: <configured> │
│ Job #JQ-77 drafting #0142  started 14:02  done 14:02  tok 3,120      │
│  cost $0.01  result: 2 candidate revisions → AWAITS REVIEW           │
│  sources used: ev#88, ev#89   retries: 0                            │
│ Queue: [research:1][drafting:0][waiting-for-human:1]                 │
│ [Pause] [Reorder] [Cancel] [Reject output]   Cost cap: $5 (ok)      │
└──────────────────────────────────────────────────────────────────────┘
```

## 22. Feature Prioritization
- **Must have (M04):** Command Center core queues; rolling-90-day calendar (basic) + Unscheduled Reserve; 3 pipeline saved views; work-item detail + Review Desk with diff and approval binding; Ready-to-Post + Need-a-Post logic with anti-filler rule; manual metric snapshots + monetization ribbon (verified official thresholds); Release Watch (manual + Federal Register API) with four-axis classification; Evidence Vault (reuse schema); in-app Needs-Attention notifications; basic command palette + saved views; state-action map + trustworthy error messages; extended System Health.
- **Should have (Later web / Tauri foundation):** daily-briefing notifications; more saved views; richer Reserve subdivisions; Ghost Activity read-only panel; desktop shell/keyboard-first; more Release Watch feeds.
- **Could have (Tauri product):** polished drag/drop calendar + heatmap + swimlanes; Dossiers as product (M05); Assets Library; Article Room; full metrics dashboard; Reply Desk.
- **Do not build yet (Deferred):** agent-neutral business API (its own milestone, before any agent write); Hermes pilot (one restricted Release-Watch write profile, only after the API exists); Qdrant semantic retrieval (M10); Truth Social (M07); multi-agent orchestration; React/Postgres/Redis/Celery.

## 23. Risks and Anti-Patterns
- **Dashboard clutter / too many statuses** — mitigate with one ranked Needs-Attention queue, the 12 fixed DB states surfaced as grouped stages, and NO user-invented statuses (reject WordPress-style status sprawl).
- **Calendar junk drawer** — the Reserve must have readiness subdivisions and dormancy/aging so it never becomes an undifferentiated dump; LLM can rank it but must justify each item.
- **Weak filler-content pressure** — Need-a-Post must be allowed to say "leave it empty"; cadence never overrides quality.
- **LLM overreach** — hard boundaries: candidates only, no self-approval, no truth mutation, no direct DB writes, bounded context packet, visible reasoning, cost caps.
- **Hidden state / duplicate truth stores** — dossiers and LLM memory are indexes/aids, never authoritative; the DB is the single truth; derived screens are views, not tables.
- **Approval bypass** — the approval row (action_id UNIQUE) is the only authorization token; editing supersedes approval; changing time alone never alters approved text.
- **Over-polishing the disposable web UI** — explicitly forbidden; invest polish in Tauri. The web harness is a contract/test surface.
- **Premature multi-agent complexity** — one connected LLM through governed operations; no orchestration at MVP.
- **Metrics driving low-quality content** — anti-overclaiming labels; correlation≠causation warnings; no target justifies weak posts.
- **Excessive notifications** — default silence, four-tests rule, quiet hours, daily briefing.
- **Scraper-farm drift** — official APIs/RSS/bulk data only; robots.txt respected; no DOM capture.
- **X API cost surprise** — as of 2026 the X API is pay-per-use with no free tier for new developers ($0.015/post created, $0.20 if it contains a URL, $0.005/read, capped at 2M reads/month per docs.x.com); the URL rate rose ~1,900% in the April 2026 repricing. The manual-publish model avoids per-post write costs and keeps the project within its documented $5-style cap.

## 24. Recommended Roadmap (next four milestones)
**M04 — X Operating Workflow & Manual Metrics (functional web control room).**
- Purpose: prove ONE complete human editorial loop end-to-end and establish stable contracts.
- Admitted scope: Command Center; rolling-90-day calendar + Reserve (basic); 3 pipeline views; composer + Review Desk (diff, approval binding, supersession); Ready/Need-a-Post logic; manual metrics + monetization ribbon; Release Watch (manual + Federal Register API); Evidence Vault reuse; in-app notifications; saved views; state-action map; trustworthy errors.
- Excluded: Reply Desk, Article Room, Assets library, Dossiers-as-product, agent write access, Tauri, Truth Social, semantic retrieval, polished UX.
- Exit gate: an operator can capture → research → AI-draft → review/approve → manual-publish → reconcile → record metrics for a real post, with full audit chain, idempotent operations, and no approval bypass; tests green; Ruff clean; hammer invariants pass.
- Components: web only.

**M-Tauri-1 — Desktop Foundation.**
- Purpose: replace the disposable web shell with a real desktop app.
- Admitted: Tauri shell, persistent nav (8 areas), resizable panes/side inspectors, command palette + keyboard-first, local notifications + tray, native file selection + drag-drop, secure credential storage, offline-safe viewing, window restoration, background-service + health/LLM-activity status, B1-L7 "operational records desk" visual identity (not a game HUD, not a website-in-a-window). Tauri 2.0 supports these natively (system tray, native notifications, secure/encrypted storage, updater; per tauri.app).
- Excluded: new business logic (reuse M04 contracts), agent write access.
- Exit gate: feature-parity with the M04 loop in a native window, offline-safe, credentials secure.
- Components: Tauri foundation.

**M-Agent-1 — Agent-Neutral Business API.**
- Purpose: a governed, agent-neutral interface so ANY LLM runtime can propose candidates safely.
- Admitted: agent identity + scopes; idempotency (reuse operation_keys); provenance (reuse audit_events hash chain); budgets/cost caps; normalized decision-support responses with recoverable errors (scope_violation, idempotency_conflict); dry-run/preview; a mock-agent test harness. No self-approval, no truth mutation, no direct DB writes — candidates only.
- Excluded: any specific vendor runtime; autonomous posting.
- Exit gate: a mock agent can propose research/draft/classification candidates through the API, fully logged, idempotent, budgeted, with zero ability to approve or write truth.
- Components: agent milestone (web + Tauri surfaces).

**M-Hermes-Pilot — One Restricted Candidate-Write Profile.**
- Purpose: pilot Hermes behind the agent API on the lowest-risk surface. (Hermes Agent, released February 25, 2026 by Nous Research — makers of the Hermes, Nomos and Psyche models — is MIT-licensed and crossed ~175,000 GitHub stars in under four months; per its docs it runs "as a persistent daemon on your own infrastructure, accumulates memory across sessions, runs scheduled cron tasks, connects to 16+ messaging platforms, and writes its own reusable skills from experience," and offers a "Blank Slate" minimal-toolset mode — a good fit for least-privilege piloting.)
- Admitted: ONE restricted profile — Release-Watch candidate summarization/classification only, producing candidates a human must accept; blank-slate/minimal toolset; cost-capped; every action provenance-logged.
- Excluded: drafting/approval/posting; broad DB access; multi-agent; anything Hermes cannot do through the governed API.
- Exit gate: Hermes proposes Release-Watch candidates that appear in the human intake queue with full provenance and zero write authority beyond candidate creation; human acceptance still required for every downstream effect.
- Components: agent milestone + Tauri Ghost Activity panel.

## 25. Final Recommendation
Immediately after closing M03, the owner should **authorize M04 as scoped in §24** — the functional web control room that proves the complete human editorial loop and locks the contracts — and should **explicitly forbid polishing the web UI** beyond what the workflow and route-authority tests require. Treat Tauri as the real product surface and sequence it as M-Tauri-1 only after M04's contracts are stable. Sequence the agent capability strictly: build the **agent-neutral business API (M-Agent-1) before granting any LLM write access**, and only then run the **single restricted Hermes Release-Watch pilot (M-Hermes-Pilot)**. Keep the doctrine literal throughout — AI drafts, human clears (the approval row is the token), the database remembers (hash-chained, append-only), metrics judge (labeled, never overclaimed) — and keep the Command Center honest enough that one person can open the desk and, in five seconds, know exactly what matters.