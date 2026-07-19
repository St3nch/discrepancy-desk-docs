# Truth Social Capture Boundaries for The Discrepancy Desk

## Executive Summary

**Bottom line.** The safest current operating posture is **C. Permit URLs, human notes, and owned-content records only**. In plain English: **do not build a Truth Social browser-capture feature right now**, even if it is human-triggered, limited to one page, local-only, and initiated by a deliberate click. That feature would still rely on extension scripts/content scripts to read page content, and Truth Social’s current Terms of Service squarely prohibit access through “automated or non-human means,” prohibit “any automated use of the system,” prohibit “data gathering and extraction tools,” and prohibit using an “automated system” or “unauthorized script or other software” other than standard search-engine or normal browser usage. The same Terms separately prohibit systematic retrieval of data to build any collection, compilation, database, or directory without written permission. A user click does not magically turn a script into a pumpkin. Or, more formally: a human gesture does not erase the automated character of the software action.

**Why this recommendation is narrower than the law might require.** U.S. scraping case law does not clearly make one-page DOM capture illegal under the Computer Fraud and Abuse Act just because the page is public. In *Van Buren*, the Supreme Court treated CFAA access as a gates-based inquiry, focused on whether the user entered forbidden areas rather than merely misused information they could otherwise access. In *hiQ v. LinkedIn*, the Ninth Circuit said public pages generally lack access gates and that accessing publicly available data will not ordinarily be “without authorization” under the CFAA. But those cases do **not** create contractual permission under Truth Social’s own Terms, and *hiQ* itself was preliminary-injunction litigation, not a universal “scraping is fine now, everyone grab a ladle” ruling. So the legal floor and the contractual ceiling are not the same thing.

**What can be built now.** For Truth Social, the defensible zone is the one you already suspected: manual posting through the official UI; official share-composer assistance; manual recording of your own post text, URLs, IDs, and timestamps; manual recording of visible metrics; import/use of an official owner-data export; and human-written notes about third-party posts. Truth Social also offers official bookmarking inside the platform for saving posts, and official publisher tooling for a share button, which reinforces the point that where Truth Social wants off-platform or cross-site functionality, it documents it. I found no official Truth Social guidance authorizing browser extensions, clipboard-to-database tools, read-later archival tools, or local DOM-extraction tools. Silence is not consent; it is just silence with better lighting.

**Current recommendation in one sentence.** Keep Truth Social support in **manual-only mode**: **URLs, human-authored notes, owned-post records, official share tools, official bookmarks, manual metrics, and official owner export; no DOM extraction, no extension capture, no background scripts, no automated timelines, no thread/account harvesting, and no third-party text archive**. Written permission would be worth requesting **only if** Truth Social becomes strategically important after the X workflow is stable and you want to unlock any on-page extraction or third-party archival. Professional legal review is advisable before any monetized or scaled third-party capture, any screenshot archive, or any proposal to retain deleted or sensitive third-party content.

**Recommended current posture.**  
**Recommendation chosen:** **C. Permit URLs, human notes, and owned-content records only.**  
**Not chosen:** A, B, D, and E. A and B are too aggressive under the current Terms; D is too strict because some low-risk manual/owned-content workflows are still defensible without new permission; E is too strict because it would unnecessarily forbid clearly lower-risk manual Truth Social support.

## Truth Social Contractual and Automated Access Analysis

**Truth Social contractual rules that matter most.** The current public Terms say the service is for “educational and personal use,” grant only a limited license to access and use the service and to download or print a copy of content “solely for your personal, non-commercial use,” ban using content for commercial purposes without prior written permission, ban using the service “through automated or non-human means, whether through a bot, script, or otherwise,” ban “any automated use of the system,” ban “data gathering and extraction tools,” ban automated systems such as spiders, robots, scrapers, offline readers, and unauthorized scripts/software except for standard search-engine or normal browser usage, ban using the service/content for revenue-generating endeavors or commercial enterprise, and ban systematically retrieving data to build a collection, compilation, database, or directory without written permission. Those are not edge clauses hiding under a couch cushion; they are the center of the analysis.

**Fact.** Truth Social’s Community Guidelines separately say posting spam and using bots are not allowed and that accounts engaging in such behavior may be removed; they also identify privacy violations, doxxing, and intellectual-property infringement as reportable categories. That does not itself answer the extension question, but it reinforces an anti-bot / anti-abuse posture and a willingness to suspend or remove accounts for uses the platform sees as crossing the line.

**Automated access analysis.** Official browser-extension documentation makes the technical classification pretty straightforward. Chrome’s `activeTab` permission grants temporary access to a tab **after an explicit user gesture**, and then permits the extension to execute scripts in that tab; MDN explains that a content script runs in the context of the web page, can read and modify page content, can access the DOM, and can be injected at runtime into a tab when the user clicks the extension. So, from a browser-platform perspective, a human-triggered capture extension is still **software-assisted script execution on page content**. It is not the same as a user manually copying a URL or typing a note. In Truth Social’s Terms vocabulary, that puts it much closer to a “script,” “automated use,” or “data gathering and extraction tool” than to ordinary browser viewing.

**Inference.** A deliberately clicked extension is therefore best classified, for Truth Social policy purposes, as **an unaddressed but high-risk scripted extraction method**, not as clearly permitted “human-operated assistance.” The user gesture helps on the browser-security side by limiting when the extension gets access, but it does **not** neutralize the Terms’ separate prohibition on scripts, automated/non-human access, automated systems, and extraction tools. The Terms also carve out only “standard search engine or Internet browser usage.” A custom content script is not standard browser usage; it is extra software layered onto the browser.

**Systematic retrieval analysis.** Truth Social draws an unusually important line around creating a collection, compilation, database, or directory. That means the *volume*, *regularity*, *database destination*, and *purpose* matter at least as much as the mechanics of capture. A single note is one thing; a recurring archive is another. And yes, “I only did it one post at a time” is not the sort of argument I would trust to save the day if the product quietly evolves into a dossier machine.

| Scenario | Risk | Why |
|---|---|---|
| Saving one third-party post URL | Low | A URL is a pointer, not a copied content body; Truth Social also officially supports bookmarking posts inside the platform. |
| Saving a URL plus a human-written note | Low | Still primarily pointer-plus-commentary, with minimal copying. |
| Copying selected text manually with normal clipboard actions | Moderate | Manual, but still copies expressive content off-platform; terms and copyright risk rise if stored for reuse. |
| Clicking an extension button that extracts visible post text | High | Scripted extraction of on-page content fits the Terms’ anti-script / anti-extraction language. |
| Saving visible author, timestamp, and engagement counts | Moderate | Lower copyright exposure than full text, but still extracting “data” off-platform; repeated tracking can become systematic retrieval. |
| Saving one screenshot manually | Moderate | A screenshot is still a copy, often copying the whole post and any attached media visible in-frame. |
| Saving ten individual posts over several weeks | Moderate | Volume starts to look archival, especially if content bodies or screenshots are retained. |
| Saving hundreds of posts into a research archive | Clearly prohibited | Directly resembles compiling a collection/database of platform content without written permission. |
| Saving an entire thread | High | Multi-post aggregation sharply increases systematic-retrieval and copying concerns. |
| Capturing every post viewed during a browsing session | Clearly prohibited | Functionally a session scraper, even if initiated from within the browser. |
| Capturing all posts from one account | Clearly prohibited | Account-level compilation squarely tracks the Terms’ database/collection language. |
| Capturing search-result pages or timelines | Clearly prohibited | Search/timeline capture is classic systematic retrieval. |
| Capturing only posts published by The Discrepancy Desk itself | Low if manual or official-export based; Moderate if extension-extracted | Ownership lowers copyright friction, but scripted DOM extraction still sits badly with the Terms. |

**Owned-content versus third-party-content analysis.** Truth Social’s Terms say users represent that they are the creator/owner of their Contributions or have the necessary rights, and the Terms expressly say users retain full ownership of their Contributions even while granting Truth Social a broad license to use them. That materially changes the copyright posture for your own posts. It does **not** erase the anti-automation clauses, but it does make it much easier to justify storing your own approved post text, your own published URLs, and your own owner-export data. By contrast, third-party post text, screenshots, media, and profile details remain subject to the Terms’ copying/aggregation/commercial-use restrictions and to ordinary copyright/privacy risk.

**Browser-extension-specific analysis.** I found **no official Truth Social page** that squarely addresses browser extensions, clipboard-to-db tools, read-later archival tools, accessibility capture tools, save-to-note tools, or local research extensions. The help center publicly surfaces legal pages, bookmarking docs, publisher share/follow tools, advertising docs, and open-source disclosures, but not extension capture guidance. That absence is important only in a negative way: it means there is no official safe harbor I could locate for the proposed DOM-capture workflow. It does **not** create implied permission.

## Content, Commercial, Copyright, Privacy, Screenshots, Metrics, and Account Export

**Owned content versus third-party content.** The project may safely store its **own approved post text**, **its own published-post URLs**, **its own post IDs/timestamps**, **its own manually observed metrics**, and **its own official account export**, because the Terms preserve user ownership of Contributions and the Privacy Policy expressly contemplates user download of information shared with the website/app through settings. The safer boundary for third-party material is much narrower: **manual notes about third-party posts** are defensible; **URLs** are defensible; **short quotations** can be justified only on a limited, commentary-linked basis; **full third-party post text**, **third-party media**, and **systematic profile/content archives** are not presently safe; and **third-party screenshots** belong in a limited evidentiary bucket, not a routine ingestion pipeline.

**Personal versus commercial use.** Truth Social’s Terms are broad enough to create real commercial-use friction. They restrict content to personal/non-commercial use, forbid commercial exploitation without prior written permission, and forbid using the service/content as part of a revenue-generating endeavor or commercial enterprise. At the same time, Truth Social separately publishes official advertising pathways through the Rumble Advertising Network and, as of July 16, 2026, Trump Media publicly advertised a licensed “Truth API” product for financial-services partners. The practical read is that commerce is not forbidden in the abstract; rather, commerce appears to be allowed through **authorized channels** and denied through **unauthorized reuse and extraction**. For this project, future monetization increases the risk that an internal research database would be seen as commercially connected, especially if it stores copied third-party text or screenshots at scale. Internal notes and pointers are easier to defend than a reusable archive of platform content.

**Copyright, quotation, and commentary boundaries.** U.S. copyright protects original works once fixed, including blog posts, short online literary works, photographs, and other creative expression. The U.S. Copyright Office recognizes fair use for limited quotations and uses tied to commentary, criticism, news reporting, and scholarship, but it also emphasizes that fair use has **no magic word count** and depends on the four statutory factors: purpose/character, nature of the work, amount/substantiality, and market effect. That means a short quotation used as evidence in commentary is materially easier to defend than storing the full post text, and a factual summary is easier than copying the whole expression. A screenshot of an entire post is also a copy; it may be more justifiable when used privately as evidence for commentary or compliance review, but it usually copies everything at once, which weakens the amount/substantiality factor. None of that overrides the separate contractual problem: a use can be arguable under fair use and still be restricted by Truth Social’s Terms.

**Recommended project practice for quotations and republication.** Keep third-party preservation to the minimum reasonably necessary to support commentary or fact-checking. Prefer this order: **URL → human summary → short quote → cropped screenshot**, and treat **full post text**, **full-post screenshots**, and **attached media** as higher-risk exceptions requiring human review. For public republication, use only what is needed to make the commentary intelligible, keep attribution/source context, and avoid reproducing attached third-party images/video unless the use is clearly necessary and separately reviewed.

**Privacy and sensitive-data boundaries.** Truth Social’s Privacy Policy defines personal data broadly, including names, location data, identification-related information, online identifiers, and account information such as name, email, mailing address, date of birth, location, phone number, gender, profession, company, and similar details. The Community Guidelines separately identify privacy violations and doxxing as reportable misconduct. For The Discrepancy Desk, that means third-party Truth Social capture should be run under a data-minimization rule: retain only what is necessary, avoid building dossiers on private individuals, and create a strong review trigger for real names, profile photos, location information, political or religious views, health information, contact information, minors, deleted posts, and other sensitive content.

**Recommended privacy controls.** Do not store private-account content, blocked-account content, or anything obtained after a post/account is no longer viewable through ordinary authorized browsing; do not retain deleted posts absent a clearly documented public-interest reason or legal-hold reason; and do not preserve sensitive-person material without specific human approval. Set retention limits, permit redaction, support deletion-on-request review, mark sensitive-person records, and prefer URL-plus-summary over copied text whenever the subject is a private individual rather than a public figure or institutional actor.

**Screenshot analysis.** Truth Social’s own Terms permit downloading or printing a copy of content for personal, non-commercial use, and its law-enforcement guidelines say the company may preserve a temporary screenshot of relevant account records for 90 days in response to valid preservation requests. That does **not** create broad user permission to build an off-platform screenshot archive, but it does show that screenshots are understood as copies/records of platform content. The safest screenshot policy is therefore narrow: **one manually triggered screenshot of a specific public post, for private evidentiary review, cropped to the minimum necessary, retained for a limited period, no bulk archive, no auto-capture, no attached-media harvesting, and no public republication without separate review**. Screenshotting your own post is much lower risk than screenshotting a third-party post; automatic screenshots or archives of third-party posts are much higher risk.

**Metrics capture.** A visible like count or timestamp is lower-risk than copying a full text post, but it is still “data” from the service. The Terms do not carve out a free pass for engagement metrics. The safe boundary is: **manual observation and manual transcription of visible metrics** is acceptable for your own posts and occasionally defensible for third-party posts; **DOM extraction of metrics** is high-risk because it is still extraction by script; **automated polling** is out; and **repeated manual snapshots** can themselves become systematic retrieval if done at scale, across many posts/accounts, or over time to build a trend archive. If the project wants historical Truth Social metrics, the right current approach is manual and sparse, not a recurring machine process.

**Account export and owner-data retrieval.** The public Privacy Policy says users can “download the information you have shared with the Website and App” through settings. The same policy also describes data-portability rights, says portability responses should be in a “readily useable” format and, under the GDPR section, says users may request transfer of their personal data in a “structured, commonly used, machine-readable format.” For California portability requests, the policy says Truth Social will disclose and deliver required information free of charge within 45 days of a verified request, with one 45-day extension when reasonably necessary. What the public docs **do not** specify is the exact click-path in settings, exact file format, whether media/followers/following are included, or any public frequency limit. Official owner export is in scope and should be treated separately from third-party capture, but the public documentation is high-level rather than operationally precise.

## Scenario Matrix and Risk Register

| Scenario | Human initiated? | Software extraction? | Third-party content? | Stored in database? | Commercial connection? | Terms risk | Copyright risk | Privacy risk | Overall risk | Recommended posture |
|---|---:|---:|---:|---:|---:|---|---|---|---|---|
| Save own post URL | Yes | No | No | Yes | Possible | Low | Low | Low | Low | Allow now |
| Save own post text | Yes | No | No | Yes | Possible | Low | Low | Low | Low | Allow now |
| Save own visible metrics | Yes | No | No | Yes | Possible | Low | Low | Low | Low | Allow now if manual |
| Use official owner export | Yes | No | No | Yes | Possible | Low | Low | Low | Low | Allow now |
| Save third-party URL | Yes | No | Yes | Yes | Possible | Low | Low | Low | Low | Allow now |
| Write a manual note about a third-party post | Yes | No | Indirectly | Yes | Possible | Low | Low | Moderate | Low to Moderate | Allow now with minimization |
| Copy a short quotation manually | Yes | No | Yes | Yes | Possible | Moderate | Moderate | Moderate | Moderate | Allow with limits |
| Copy full third-party post manually | Yes | No | Yes | Yes | Possible | High | High | Moderate | High | Exclude now |
| Extension extracts third-party post text | Yes | Yes | Yes | Yes | Possible | High to Clearly prohibited | High | Moderate | High | Exclude now |
| Extension extracts author and timestamp | Yes | Yes | Yes | Yes | Possible | High | Low to Moderate | Moderate | High | Exclude now |
| Extension extracts visible metrics | Yes | Yes | Yes | Yes | Possible | High | Low | Moderate | High | Exclude now |
| Manual screenshot of one third-party post | Yes | No | Yes | Yes | Possible | Moderate | Moderate to High | Moderate | Moderate | Allow with limits |
| Bulk screenshot archive | Yes | Maybe | Yes | Yes | Possible | Clearly prohibited | High | High | High | Exclude now |
| Capture an entire thread | Yes | Maybe | Usually yes | Yes | Possible | High to Clearly prohibited | High | Moderate | High | Exclude now |
| Capture all posts from one account | Yes | Maybe | Yes | Yes | Possible | Clearly prohibited | High | Moderate to High | High | Exclude now |
| Capture search results | Yes | Maybe | Yes | Yes | Possible | Clearly prohibited | High | Moderate | High | Exclude now |
| Capture timeline content | Yes | Maybe | Yes | Yes | Possible | Clearly prohibited | High | Moderate | High | Exclude now |
| Automated recurring metrics snapshots | No / partially | Yes | Mixed | Yes | Possible | Clearly prohibited | Low to Moderate | Moderate | High | Exclude now |
| Human-triggered occasional metrics entry | Yes | No | Mixed | Yes | Possible | Low for own; Moderate for third-party at scale | Low | Low to Moderate | Low to Moderate | Allow manually with volume limits |

| Risk | Trigger | Likelihood | Impact | Mitigation | Stop condition | Recheck date |
|---|---|---|---|---|---|---|
| Terms violation | Any scripted DOM read on Truth Social | Medium to High | High | Disable Truth Social DOM capture entirely | Any proposal to read rendered page content by extension | 2026-10-19 |
| Account suspension | Repeated capture behavior or flagged automation | Medium | High | Keep Truth Social manual-only; no extension on truthsocial.com | First platform warning or complaint | 2026-10-19 |
| Database-compilation allegation | Archive grows from notes to content library | Medium | High | Volume caps, audit log, URL-first design | More than isolated copied third-party records | 2026-09-19 |
| Commercial-use allegation | Monetization plus reused platform content | Medium | High | Keep third-party storage minimal; seek permission before scaling | Paid use depends on preserved third-party content | 2026-09-19 |
| Extension reclassified as scraper | Human-click tool evolves into page reader | High if built | High | Do not ship Truth Social parser or content script | Any code path reads post DOM on truthsocial.com | 2026-08-19 |
| Capture-volume drift | One-off use becomes routine ingestion | High over time | High | Hard caps, warnings, monthly audit | More than de minimis third-party copies retained | 2026-08-19 |
| Background automation drift | Convenience features add listeners/auto-run | Medium | High | No background scripts for Truth Social capture | Any background listener tied to truthsocial.com | 2026-08-19 |
| Retention creep | Screenshots/quotes never deleted | High | Medium to High | Default retention windows and deletion controls | Records exceed authorized retention | 2026-08-19 |
| Sensitive-person profiling | Archive clusters data about private individuals | Medium | High | Ban dossier-building; require human escalation | Multiple retained records about same private person | 2026-08-19 |
| Documentation change | Terms/help/export docs change | High over time | Medium to High | Quarterly policy recheck; feature flag remains off | Relevant policy update or new developer program | 2026-10-19 |

## Product Boundaries and Architecture Implications

### Allowed now

- Manual URL recording for owned and third-party posts.
- Human-authored notes about third-party posts.
- Recording owned approved post text, URLs, IDs, timestamps, and manually observed metrics.
- Importing and studying an official owner-data export.
- Official share-composer assistance and other official publishing tools.
- Official Truth Social bookmarks.

### Allowed with limits

- Short quotations only when necessary for commentary, fact-checking, or source verification; human-entered, source-linked, and reviewed.
- One-off manual screenshots as limited private evidence; cropped, retained briefly, and never collected in bulk.
- Occasional manual metric snapshots, especially for owned posts.

### Requires written permission

- Any Truth Social DOM extraction, including owned content.
- Any browser-extension integration that reads rendered Truth Social page content.
- Any archive of third-party post bodies, screenshots, or metrics beyond isolated case records.
- Any account-, thread-, search-, or timeline-level compilation.
- Any monetized reuse depending on copied platform content rather than URLs, notes, or commentary.

### Excluded now

- Automated reads or writes.
- Scraping or browser automation.
- Session-cookie reuse outside normal page loading.
- Undocumented endpoint calls.
- Bulk capture or automatic screenshots.
- Background monitoring.
- Thread, account, timeline, or search harvesting.
- Recurring metrics polling.
- Any extension, content script, or parser that reads Truth Social DOM content.

| Constraint | Classification |
|---|---|
| Platform feature flags | Required now |
| Truth Social capture disabled by default | Required now |
| URL-and-note-only mode for Truth Social | Required now |
| Truth Social parser disabled | Required now |
| No background scripts tied to Truth Social capture | Required now |
| No auto-run content script on Truth Social | Required now |
| Explicit human confirmation before save | Required now |
| Source attribution field | Required now |
| Manual metrics mode only | Required now |
| Audit log for all Truth Social records | Required now |
| Retention controls | Required now |
| Delete-on-request/source-removal review | Required now |
| Capture-volume warnings | Recommended |
| Sensitive-person review gate | Recommended |
| Owned-content-only import from official export | Recommended |
| Owned-content-only DOM mode | Deferred pending written permission or counsel signoff |
| Third-party DOM extraction | Not admitted |
| Automatic screenshots | Not admitted |
| Timeline/search/thread capture | Not admitted |

## Permission Path, Unknowns, and Recheck Triggers

Truth Social does not publish a dedicated browser-extension approval route, public developer intake for this use case, or a specific licensing form for DOM extraction. Public contact points include `support@truthsocial.com` for service/content questions and `legal@tmediatech.com` for legal/privacy contact. No documented response time was found.

Written permission is not worth requesting yet unless Truth Social becomes materially important after X is stable and the project wants capabilities beyond URL/note/manual metrics/owner-export support.

Unknowns remain around whether Truth Social would ever permit a narrow user-clicked reader for owned content and the exact scope and format of owner exports. These unknowns are not loopholes.

Recheck immediately if Truth Social changes its Terms or Privacy Policy, publishes a public developer or extension policy, expands export documentation, or the project begins monetizing based on preserved third-party material. Otherwise recheck by **2026-10-19**.

## Permission-Request Template

> Subject: Request for written permission clarification regarding limited local research capture
>
> Hello,
>
> I am seeking written clarification regarding a proposed Truth Social workflow for a single account holder’s internal research and recordkeeping.
>
> Proposed workflow:
> - a human user manually navigates to a specific Truth Social post already being viewed in the browser;
> - the human manually clicks a browser-extension button;
> - the tool would read only limited information already rendered on that page;
> - the captured information would be sent only to a local application for immediate human review and editing;
> - no timelines, search results, profile crawls, or thread crawls would be captured;
> - no background operation, browser automation, endpoint probing, cookie reuse, or automated publishing would occur.
>
> We request written confirmation on whether this limited on-page capture would be permitted, and if not, whether Truth Social would permit any narrower variant, such as owned-content-only capture or author/timestamp/URL-only capture.
>
> If written permission is required, please advise the appropriate contact, required information, and any commercial or technical conditions that would apply.
>
> Thank you.

## Sources Reviewed

Principal authorities reviewed in the underlying research included:

- Truth Social Terms of Service.
- Truth Social Privacy Policy.
- Truth Social Community Guidelines.
- Truth Social Bookmarks documentation.
- Truth Social Share Button documentation.
- Truth Social Contact and Brand/Press pages.
- Truth Social Advertising documentation.
- Truth Social Law Enforcement Guidelines.
- Truth Social Open Source page.
- Trump Media notices concerning licensed Truth API access.
- *Van Buren v. United States*.
- *hiQ Labs, Inc. v. LinkedIn Corp.*.
- Chrome Extensions `activeTab` documentation.
- MDN Content Scripts and Background Scripts documentation.
- U.S. Copyright Office fair-use materials.
- FTC and NIST privacy-minimization guidance.

Access date for the underlying research: **2026-07-19**.

## Final Recommendation

**Choose C: Permit URLs, human notes, and owned-content records only.**

Build now: manual URL recording, human-written third-party notes, owned-post records and manual metrics, official share-composer assistance, and official owner-export handling.

Do not build now: any Truth Social extension/parser that reads page content, third-party DOM extraction, timeline/search/thread/account capture, automated metrics polling, or automated screenshots.

A later decision requires written permission, materially revised official rules, or professional legal advice supporting a narrower owned-content-only path.
