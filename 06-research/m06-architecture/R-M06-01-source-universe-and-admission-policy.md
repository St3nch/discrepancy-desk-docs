# R-M06-01 — Source Universe and Admission Policy

## Status

Research report completed on 2026-07-20.

This report is source material for M06 architecture synthesis. It is not implementation authority, connector admission, permission to scrape, permission to retain third-party content, or approval to automate any source.

## Research Question

What source classes should The Discrepancy Desk be able to ingest, how can each class enter manually at first, how might each become automated later, and what provenance, policy, retention, failure, and human-review conditions must govern admission?

## Executive Finding

The system should not build one ingestion path for X, another unrelated path for YouTube, and a third unrelated path for websites.

The recommended architectural direction is:

```text
source-specific acquisition
        ↓
universal ingestion envelope
        ↓
immutable original artifact or exact supplied input
        ↓
normalized source-specific document package
        ↓
human admission review
        ↓
governed research memory
        ↓
rebuildable retrieval derivatives later
```

Manual and automated acquisition should produce the same downstream envelope. Automation changes who or what acquired the material; it must not create a second truth system.

The source universe should be broad, but admission must be narrow and explicit. Technical feasibility, public accessibility, and search-engine crawlability do not independently establish permission to automate, copy indefinitely, or republish.

---

# 1. Research Method and Evidence Labels

This report uses the following labels:

- **VERIFIED FACT** — directly supported by a cited primary or authoritative source.
- **SOURCE/PROVIDER CLAIM** — a capability or policy claimed by the platform/provider itself.
- **INFERENCE** — a reasoned implication from verified material.
- **PROJECT RECOMMENDATION** — a proposed Discrepancy Desk rule requiring later owner acceptance.
- **OPEN QUESTION** — unresolved and not safe to treat as settled.

Primary-source preference was applied to platform APIs, Internet standards, and publisher documentation.

---

# 2. Governing Distinctions

## 2.1 Source identity is not an observation

**PROJECT RECOMMENDATION**

The system should distinguish:

```text
source identity
source item identity
source occurrence
observation
acquisition event
original artifact
normalized derivative
human assertion
```

Example:

```text
YouTube channel                    source identity
YouTube video ID                   source item identity
video observed on July 20          observation
videos.list API request            acquisition event
returned JSON                      original acquisition artifact
normalized video record            derivative
"the speaker alleged X"            extracted claim candidate
"X is established"                 human-governed assertion
```

One video can have many observations. One webpage can appear at several URLs. One document can be acquired from several occurrences. These must not collapse accidentally.

## 2.2 Public does not mean admitted

**VERIFIED FACT**

RFC 9309 defines robots.txt as a mechanism for service owners to express crawler access preferences. It explicitly states that robots rules are not access authorization. It also describes crawlers as automated clients and provides caching and parsing guidance.

**INFERENCE**

A URL being publicly reachable or allowed by robots.txt does not by itself answer:

- whether automated retrieval is permitted by platform terms;
- whether full content may be retained indefinitely;
- whether media may be downloaded;
- whether derived transcripts may be stored;
- whether content may be republished;
- whether authentication or contractual access conditions apply.

**PROJECT RECOMMENDATION**

Each automated source needs a separate admission record covering technical method, platform/site policy, retention, credentials, rate limits, and owner approval.

## 2.3 Acquisition does not establish truth

**PROJECT RECOMMENDATION**

Every ingested item should be represented as:

```text
Source or provider S returned/stated content C
at observation time T
through acquisition method M.
```

The system must not convert the presence of a claim in a source into acceptance of the underlying claim.

---

# 3. Source Universe

## 3.1 Human-authored manual material

Examples:

- pasted transcript;
- copied article excerpt;
- handwritten or typed research note;
- interview notes;
- correction note;
- URL with a human description;
- source lead not yet verified;
- connection hypothesis;
- quotation supplied by the owner.

### Immediate manual path

```text
Paste text or note
Select source category
Provide source URL/description when available
Select account scope
Label the material as source text, owner note, lead, question, or hypothesis
Review and admit
```

### Future automation path

Not applicable to purely human-originated notes, although speech-to-text or assisted metadata extraction may help create them.

### Admission risks

- copied text without source attribution;
- memory presented as observed fact;
- private or sensitive information;
- accidental account mixing;
- quotations stripped of context;
- LLM-generated text presented as source material.

### Recommended initial status

```text
MANUAL-FIRST — CORE M06 INPUT
```

---

## 3.2 Local files and uploaded documents

Examples:

- TXT and Markdown;
- PDF;
- HTML;
- SRT/VTT/SBV transcript files;
- JSON/JSONL;
- images and screenshots;
- DOCX and presentation files later;
- audio/video files later, subject to admission and storage limits.

### Immediate manual path

Use the existing bounded native file-selection boundary. Copy the selected file into a governed intake area, calculate its hash, detect media type, record size and filename, then hold it in a pending-review state.

### Future automation path

- watched local drop folder;
- approved email attachment import;
- approved cloud-drive connector;
- source-specific export import.

### Admission risks

- malformed or hostile files;
- archive bombs;
- macro-enabled documents;
- misleading filename extensions;
- private or copyrighted material;
- duplicate files;
- OCR errors;
- parser prompt injection;
- excessively large files.

### Recommended initial status

```text
MANUAL-FIRST — CORE M06 INPUT
```

---

## 3.3 X posts and X account observations

**VERIFIED FACT**

X provides official API endpoints for recent Post search and near-real-time Filtered Stream retrieval. Recent search retrieves matching Posts from a bounded recent window. Filtered Stream supports persistent rules and near-real-time delivery, with webhook delivery documented as an option.

### Immediate manual path

- paste an X Post URL;
- paste or import text when permitted;
- store the owner-observed URL, Post ID, account identity, observation time, human note, and optional screenshot/evidence reference;
- retain the existing M01 official API probe and bounded official lookup path for specifically admitted reads.

### Future automation path

- official Post lookup;
- official recent search;
- official Filtered Stream or webhook pilot;
- owned-account metrics and mentions through admitted official endpoints.

### Admission risks

- API cost and quota;
- deleted/edited/unavailable Posts;
- protected or private content;
- expansion/media fields changing over time;
- policy and retention requirements;
- confusing repost/quote/reply relationships;
- ingesting engagement data as timeless truth;
- automated monitoring expanding beyond owner-approved rules.

### Recommended initial status

```text
MANUAL + EXISTING BOUNDED OFFICIAL API
AUTOMATION REQUIRES A SEPARATE CONNECTOR PACKAGE
```

---

## 3.4 YouTube videos, channels, playlists, captions, and transcripts

**VERIFIED FACT**

The official YouTube Data API represents videos, channels, playlists, captions, and other resources as JSON objects. `videos.list` can return video metadata, and a request by video ID has a low documented quota cost. Video resources can include title, description, channel identity, duration, caption availability, status, and statistics depending on requested parts.

**VERIFIED FACT**

The official captions API does not return caption text through `captions.list`. Downloading a caption track uses `captions.download`, requires OAuth authorization, and requires the requesting user to have permission to edit the video. Therefore, it is not a general official transcript-download API for arbitrary public videos.

**VERIFIED FACT**

YouTube supports channel push notifications through PubSubHubbub/WebSub. Notifications can report new uploads and title or description updates and are delivered as Atom feed entries to a callback.

### Immediate manual path

```text
Paste YouTube URL
Safely parse video ID
Optionally retrieve official public metadata
Paste transcript manually or upload TXT/SRT/VTT/SBV/JSON export
Declare transcript provenance
Review metadata and transcript
Admit as source
```

Transcript provenance choices should include:

```text
human copied from visible YouTube transcript
creator-provided transcript
creator-provided caption file
automatic caption export
third-party transcript service
local speech-to-text
human-created transcript
unknown
```

### Future automation path

- official metadata retrieval through `videos.list`;
- official channel upload/update notifications through WebSub;
- approved transcript SaaS API;
- approved creator-owned caption API access;
- locally operated speech-to-text when lawful acquisition of the media is admitted;
- playlist/channel batch intake after cost and policy review.

### Admission risks

- unofficial transcript endpoints;
- transcript SaaS provenance and retention;
- inaccurate automatic captions;
- missing timestamps;
- language and translation errors;
- edited or removed videos;
- live streams and changing archives;
- copyright and media-retention boundaries;
- downloading audio/video without an admitted basis;
- statistics changing after observation.

### Recommended initial status

```text
MANUAL TRANSCRIPT + OPTIONAL OFFICIAL METADATA
CHANNEL MONITORING AND TRANSCRIPT AUTOMATION DEFERRED
```

---

## 3.5 Podcasts and syndicated audiovisual feeds

**VERIFIED FACT**

Apple documents podcast RSS feeds as publicly addressable RSS 2.0 feeds containing episode metadata, globally unique episode identifiers, enclosure URLs, dates, and media types. Apple also supports provider-supplied transcripts, including VTT or SRT in some workflows, and distinguishes automatically generated transcripts from provider-supplied transcripts.

### Immediate manual path

- paste podcast episode URL or feed URL;
- paste/upload transcript;
- import episode title, show, GUID, publication date, enclosure URL, duration when available, and human notes;
- preserve feed entry identity separately from the current audio URL.

### Future automation path

- poll admitted RSS feeds conditionally;
- use stable episode GUIDs to detect new or updated episodes;
- retrieve provider-supplied transcript links where explicitly published and admitted;
- use approved local or SaaS transcription for episodes without transcripts.

### Admission risks

- media enclosure download and retention;
- feed entries changing while GUID remains stable;
- transcript provenance;
- subscriber/private feeds;
- access tokens embedded in feed URLs;
- copyrighted audio;
- automatic chapter/transcript quality.

### Recommended initial status

```text
MANUAL EPISODE/TRANSCRIPT INPUT
RSS METADATA PILOT AFTER FEED-SPECIFIC ADMISSION
```

---

## 3.6 RSS and Atom feeds

**VERIFIED FACT**

RFC 4287 defines Atom as an XML-based feed format containing entries and metadata. Atom entries have stable `atom:id` values; the specification says the ID must not change merely because an entry is relocated, migrated, syndicated, republished, exported, or imported. Atom also distinguishes published and updated timestamps, while noting that publishers decide which modifications are significant enough to change `atom:updated`.

**VERIFIED FACT**

WebSub is a W3C Recommendation for decentralized HTTP publish/subscribe. Publishers expose a hub, subscribers request subscriptions, and hubs distribute new content to callback endpoints.

### Immediate manual path

- paste a feed URL;
- preview entries;
- select specific entries for admission;
- preserve feed metadata, entry ID/GUID, publication/update times, links, and supplied content/summary.

### Future automation path

- conditional polling;
- WebSub subscription where supported;
- feed-entry deduplication based on stable ID plus content hash;
- alert-only monitoring before automatic ingestion.

### Admission risks

- feeds containing only excerpts;
- mutable entries;
- deleted entries disappearing from the feed;
- reused or malformed GUIDs;
- HTML embedded in XML;
- external enclosures;
- private tokenized feeds;
- feed discovery mistaken for permission to fetch linked full pages.

### Recommended initial status

```text
MANUAL PREVIEW/SELECTION FIRST
AUTOMATED FEED MONITORING IS A STRONG EARLY CANDIDATE
```

---

## 3.7 Public webpages and articles

### Immediate manual path

A bounded first version can support:

```text
one human-supplied public URL
no recursive crawl
no login
no paywall bypass
no CAPTCHA bypass
no browser automation
raw HTTP representation preserved when admitted
main-content extraction shown for review
human admission required
```

The intake should preserve:

- supplied URL;
- final URL after redirects;
- canonical URL declared by the page, if any;
- retrieval time;
- HTTP status;
- content type and encoding;
- ETag and Last-Modified when returned;
- selected response headers;
- raw bytes or approved bounded representation;
- extracted title, byline, dates, headings, text, links, and media references;
- extractor name/version;
- extraction warnings.

**VERIFIED FACT**

RFC 9110 defines conditional HTTP requests. `If-None-Match`/ETag is the more accurate validator when available, while `If-Modified-Since` can avoid transferring a representation when it has not changed.

**VERIFIED FACT**

Google documents sitemaps as publisher-provided files listing important pages/files and metadata such as update times. Google also documents canonicalization as selecting a representative URL among duplicate or substantially similar pages.

**INFERENCE**

Sitemaps and canonical tags are valuable discovery and deduplication signals, but they are publisher/search signals—not proof that content is unchanged, complete, or authorized for unrestricted retention.

### Future automation path

- admitted RSS/sitemap discovery;
- conditional single-page retrieval;
- bounded domain monitor;
- API/webhook when the publisher provides one;
- browser rendering only after a separate policy and security review;
- meaningful-change classification after preserving raw observations.

### Admission risks

- site terms and robots rules;
- dynamically rendered pages;
- login/paywall/session content;
- anti-bot controls;
- canonicalization errors;
- duplicate URLs;
- malicious HTML and prompt injection;
- embedded tracking or secrets;
- copyright and retention;
- large or infinite pages;
- pages changing between retrieval and review.

### Recommended initial status

```text
BOUNDED MANUAL SINGLE-URL IMPORT IS A HIGH-PRIORITY M06 CANDIDATE
AUTOMATED MONITORING REQUIRES SOURCE-SPECIFIC ADMISSION
```

---

## 3.8 Sitemaps and site-discovery documents

### Immediate manual path

- paste sitemap URL;
- preview URLs and metadata;
- manually select items for observation;
- do not recursively ingest everything by default.

### Future automation path

- scheduled sitemap diff;
- discover new/updated candidate URLs;
- queue candidates for human review;
- optionally feed admitted single-page monitors.

### Admission risks

- huge URL sets;
- stale or inaccurate `lastmod` values;
- sitemap indexes recursively expanding scope;
- media URLs;
- URLs disallowed by policy or source admission;
- discovery silently becoming crawling authority.

### Recommended initial status

```text
DISCOVERY-ONLY CANDIDATE
NO AUTOMATIC CONTENT ADMISSION
```

---

## 3.9 Official public APIs and structured datasets

Examples:

- government open-data APIs;
- legislative records;
- court/public-record systems where access is permitted;
- scientific literature metadata;
- public archives;
- media/provider APIs;
- structured organization datasets.

### Immediate manual path

- upload saved JSON/CSV/XML response;
- record API/documentation URL and request description;
- preserve response bytes and human notes;
- admit selected records.

### Future automation path

- source-specific API connector;
- pagination and checkpointing;
- request/response receipts;
- cost/quota budget;
- schema-version tracking;
- changed-record observations.

### Admission risks

- license and attribution;
- API keys and secrets;
- pagination gaps;
- mutable schemas;
- incomplete or delayed data;
- bulk ingestion cost;
- rate limits;
- private data appearing in otherwise public systems;
- interpreting provider records as universal truth.

### Recommended initial status

```text
MANUAL STRUCTURED IMPORT
EACH AUTOMATED API REQUIRES ITS OWN ADMISSION PACKAGE
```

---

## 3.10 ActivityStreams and federated social data

**VERIFIED FACT**

ActivityStreams 2.0 is a W3C Recommendation defining a JSON/JSON-LD model for objects, links, activities, actors, collections, and ordered collections.

### Immediate manual path

- upload an ActivityStreams JSON document or paste a public object URL after source-specific review;
- preserve exact JSON and object/actor identifiers;
- normalize only supported fields.

### Future automation path

- admitted ActivityPub/federated-source connector;
- collection/page traversal with strict limits;
- actor/object update observations.

### Admission risks

- federation-specific privacy expectations;
- object deletion and tombstones;
- audience fields;
- remote attachments;
- recursive collection expansion;
- extensions and unknown vocabularies;
- cross-server identity and impersonation.

### Recommended initial status

```text
FUTURE SOURCE CLASS — NOT AN M06 IMPLEMENTATION REQUIREMENT
```

---

## 3.11 Email, newsletters, and private correspondence

### Immediate manual path

- owner manually exports or pastes material they are authorized to retain;
- label it private/restricted;
- separate sender claims from verified facts;
- redact secrets before admission when appropriate.

### Future automation path

- explicit Gmail/mail connector;
- label/folder-scoped import;
- attachment intake;
- unsubscribe and deletion handling.

### Admission risks

- personal/private data;
- confidential material;
- credentials and tokens;
- third-party consent;
- legal retention obligations;
- account mixing;
- prompt injection in message content.

### Recommended initial status

```text
MANUAL RESTRICTED INPUT ONLY
AUTOMATION REQUIRES A PRIVACY-SPECIFIC GATE
```

---

## 3.12 Screenshots, images, diagrams, and scanned documents

### Immediate manual path

- select file;
- preserve original bytes and hash;
- record who captured/provided it, source URL when known, timestamp, and whether it is cropped/edited;
- allow human transcription or annotation;
- mark OCR as derived and potentially erroneous.

### Future automation path

- browser-helper screenshot capture;
- OCR/layout extraction;
- image metadata extraction;
- visual similarity indexing later.

### Admission risks

- manipulated images;
- lost context;
- private information;
- OCR hallucination/errors;
- screenshots treated as canonical webpages;
- embedded EXIF/location data;
- copyrighted media.

### Recommended initial status

```text
MANUAL-FIRST — EVIDENCE WITH DERIVED OCR
```

---

# 4. Source Admission States

**PROJECT RECOMMENDATION**

Every source definition should have one explicit state:

```text
proposed
manual_only
manual_admitted
automation_research
pilot_authorized
automated_admitted
suspended
retired
prohibited
```

Meaning:

- `proposed` — identified but not admitted.
- `manual_only` — human may supply material; the system does not retrieve it.
- `manual_admitted` — a bounded manual retrieval/import workflow is approved.
- `automation_research` — automation is being investigated; no live automation.
- `pilot_authorized` — one bounded monitored/connector pilot is approved.
- `automated_admitted` — recurring connector behavior is approved within fixed limits.
- `suspended` — acquisition stops while retained records remain governed.
- `retired` — connector no longer runs; historical provenance remains.
- `prohibited` — the source/method may not be used.

No source should progress automatically between states.

---

# 5. Acquisition-Method Vocabulary

**PROJECT RECOMMENDATION**

A closed initial acquisition vocabulary should include:

```text
human_paste
human_note
human_file_upload
human_url_request
human_browser_capture
official_api
publisher_feed
publisher_webhook
approved_http_retrieval
provider_api
local_transcription
human_transcription
structured_import
```

Avoid vague values such as `scrape`, `downloaded`, or `internet` because they conceal the actual method and authority basis.

---

# 6. Universal Ingestion Envelope — Research Candidate

Every intake method should emit a source-neutral envelope before normalization.

```json
{
  "schema_version": 1,
  "ingestion_id": "ingest_...",
  "account_id": "account_...",
  "source_class": "youtube_video",
  "source_identity": {
    "platform": "youtube",
    "item_id": "video_id",
    "supplied_url": "...",
    "canonical_url": "..."
  },
  "acquisition": {
    "method": "human_paste",
    "actor_type": "human",
    "actor_id": "owner",
    "provider": null,
    "acquired_at": "...",
    "request_receipt_id": null
  },
  "original_artifacts": [
    {
      "artifact_id": "artifact_...",
      "role": "supplied_transcript",
      "relative_path": "...",
      "media_type": "text/plain",
      "byte_size": 1234,
      "sha256": "..."
    }
  ],
  "policy": {
    "source_admission_id": "source_admission_...",
    "retention_class": "research_source",
    "sensitivity": "public",
    "automation_state": "manual_only"
  },
  "review": {
    "state": "pending",
    "warnings": []
  }
}
```

This is not a final schema. R-M06-04 through R-M06-06 must test and refine it.

---

# 7. Required Acquisition Receipt

**PROJECT RECOMMENDATION**

Automated or provider-assisted acquisition should record:

- source admission ID;
- source class and item identity;
- acquisition method;
- human or worker identity;
- request time and completion time;
- endpoint or feed identity without secrets;
- request parameters with secret redaction;
- HTTP status and selected headers;
- response media type and byte count;
- quota/cost estimate where applicable;
- provider request ID;
- retry count;
- original artifact hashes;
- parser/extractor version;
- final disposition;
- error classification;
- whether any content was retained after failure.

Manual paste/upload should record a simpler receipt but must still preserve actor, time, input hash, source description, and admission decision.

---

# 8. Manual-First Product Recommendation

The first M06 ingestion surface should support:

```text
1. Paste text/transcript
2. Upload file
3. Add one webpage URL
4. Add YouTube URL plus pasted/uploaded transcript
5. Add manual research note or question
```

Every workflow should:

1. select the owning account;
2. create a pending ingestion record;
3. preserve the supplied original;
4. normalize without overwriting the original;
5. display provenance and warnings;
6. detect likely duplicates;
7. allow correction before admission;
8. require a human admission decision;
9. emit audit history;
10. create no Qdrant point yet.

---

# 9. Automation Admission Checklist

**PROJECT RECOMMENDATION**

No connector, monitor, feed subscription, crawler, or transcript provider should run until its package answers:

## Source and authority

- What exact source is being accessed?
- Is the method official, publisher-provided, standards-based, provider-mediated, or custom?
- What terms, license, robots, authentication, or contractual rules apply?
- Does public access differ from automation permission?

## Scope

- Which accounts own the resulting records?
- Which URLs, IDs, channels, feeds, queries, or rules are admitted?
- Is recursive traversal allowed?
- Are private/authenticated resources excluded?

## Data and retention

- What original artifacts are retained?
- What normalized derivatives are created?
- Are media files retained or only metadata/transcripts?
- What deletion, expiration, correction, or provider-retention rules apply?

## Cost and reliability

- What are quotas and prices?
- What are hard per-run/daily/monthly ceilings?
- What are retry/backoff rules?
- How are partial failures and duplicate deliveries handled?

## Security

- Where are credentials stored?
- How are hostile content and prompt injection contained?
- What size/type limits apply?
- How are secrets and private information detected?

## Review and authority

- Does acquisition create only pending source records?
- Which derived fields are candidates rather than accepted facts?
- Who can promote material into governed memory?
- What stop conditions disable the connector?

## Rebuildability

- Can normalized derivatives be rebuilt from preserved originals?
- Can later retrieval indexes be destroyed and recreated?
- How are parser/provider versions recorded?

---

# 10. Initial Source-Admission Matrix

| Source class | Manual path now | Future automation | Initial recommendation |
|---|---|---|---|
| Human note/paste | native form | assisted transcription only | admit in M06 |
| Local document | native upload | watched folder/cloud connector later | admit bounded formats in M06 |
| X Post | URL/manual capture + existing bounded API | official lookup/search/stream package | retain current limits |
| YouTube video | URL + pasted/uploaded transcript | metadata API, WebSub, approved transcript provider | high-priority M06 manual path |
| Podcast episode | URL/feed metadata + transcript upload | RSS polling and approved transcript path | manual first |
| RSS/Atom entry | feed preview and selected entry | conditional polling/WebSub | strong early automation candidate |
| Public webpage | one URL, human invoked | conditional bounded monitor | high-priority M06 manual path |
| Sitemap | preview URLs | candidate discovery/diff | discovery only |
| Public API/dataset | upload saved response | source-specific connector | manual import first |
| ActivityStreams | upload JSON | federated connector later | defer |
| Email/newsletter | restricted paste/export | privacy-specific connector | manual only |
| Image/screenshot | native upload | browser helper/OCR later | admit with provenance |
| Audio/video file | upload only if admitted | local transcription later | defer broad support pending R-M06-02/04 |

---

# 11. Stop Conditions

**PROJECT RECOMMENDATION**

Acquisition must fail closed when:

- source admission is absent, suspended, retired, or prohibited;
- account scope is missing or ambiguous;
- a URL uses a disallowed scheme or resolves to a prohibited/private target;
- authentication is unexpectedly required;
- redirect scope exceeds policy;
- content type or size exceeds admitted limits;
- robots/site/platform policy has not been reviewed for automation;
- credentials could be written into evidence or logs;
- the response is partial or malformed and cannot be represented honestly;
- original artifact hashing fails;
- parser output cannot be linked to the original;
- a provider returns data outside the requested source scope;
- cost/quota ceiling would be exceeded;
- duplicate identity conflicts cannot be reconciled;
- human admission is required but absent.

Failure must not leave a record that appears admitted or complete.

---

# 12. Findings for M06 Architecture

## Verified findings

1. Official APIs and standards exist for several high-value discovery/metadata paths: X Post search/stream, YouTube metadata, YouTube channel notifications, RSS/Atom, WebSub, ActivityStreams, and conditional HTTP retrieval.
2. Official YouTube caption download is permission-bound and does not solve arbitrary public-video transcript acquisition.
3. Feeds provide stable item identifiers and update metadata, but publishers control update semantics and feeds can be incomplete.
4. HTTP validators can reduce unnecessary transfers but do not establish semantic equivalence or permission.
5. Robots rules communicate crawler preferences but are not access authorization.
6. Podcast RSS feeds expose stable episode identity and media metadata; transcript availability and provenance vary.

## Project recommendations requiring later owner acceptance

1. Adopt one universal ingestion envelope across manual and automated acquisition.
2. Make manual paste, file upload, one-page webpage import, and YouTube URL plus transcript the first M06 ingestion workflows.
3. Require one explicit source-admission record for every future automated connector/monitor.
4. Treat RSS/Atom monitoring as an early automation candidate because it is publisher-provided, structured, identity-bearing, and naturally incremental.
5. Treat webpage monitoring as source-specific and conditional, not a generic crawler permission.
6. Preserve exact originals and acquisition receipts before normalization.
7. Keep every ingestion pending until a human admission decision.
8. Do not add Qdrant during M06 ingestion implementation.

---

# 13. Open Questions for Later Reports

1. What exact file types and maximum sizes should M06 admit?
2. Should bounded webpage import retain full HTML bytes, a WARC-style response package, selected headers, or another format?
3. What SSRF and private-network defenses are required for URL ingestion?
4. Which transcript SaaS providers expose stable, lawful, automatable APIs with acceptable retention and provenance?
5. When may local speech-to-text acquire or process third-party media?
6. How should changing YouTube statistics and descriptions create observations?
7. How should feed entry deletion be represented?
8. What constitutes the same document across URL, feed, API, and uploaded-file occurrences?
9. What material requires expiration or restricted sensitivity labels?
10. Which source classes may share canonical evidence across account vaults without sharing account-specific interpretations?
11. Should the application ever retain full third-party audio/video, or only owner-supplied files, metadata, and transcript derivatives?
12. How should monitoring distinguish transport change, raw-byte change, extracted-content change, and meaningful-claim change?

---

# 14. Recommended Next Research

Proceed to:

```text
R-M06-02 — YouTube and Audiovisual Ingestion
```

That report should deeply evaluate:

- official YouTube metadata and notification paths;
- transcript acquisition options;
- manual transcript formats;
- transcript SaaS APIs;
- local speech-to-text;
- timestamp and language preservation;
- video/channel/version identity;
- audiovisual retention and copyright boundaries;
- change monitoring;
- exact manual-first M06 product workflow.

No source connector or automated monitor is authorized by this report.

---

# 15. Primary Sources Consulted

- X API, Search recent Posts: https://docs.x.com/x-api/posts/search-recent-posts
- X API, Filtered Stream: https://docs.x.com/x-api/posts/filtered-stream/introduction
- YouTube Data API, Videos list: https://developers.google.com/youtube/v3/docs/videos/list
- YouTube Data API, video resource: https://developers.google.com/youtube/v3/docs/videos
- YouTube Data API, Captions list: https://developers.google.com/youtube/v3/docs/captions/list
- YouTube Data API, Captions download: https://developers.google.com/youtube/v3/docs/captions/download
- YouTube push notifications: https://developers.google.com/youtube/v3/guides/push_notifications
- RFC 4287, Atom Syndication Format: https://datatracker.ietf.org/doc/html/rfc4287
- W3C WebSub Recommendation: https://www.w3.org/TR/websub/
- RFC 9110, HTTP Semantics: https://datatracker.ietf.org/doc/html/rfc9110
- RFC 9309, Robots Exclusion Protocol: https://datatracker.ietf.org/doc/html/rfc9309
- Google Search Central, Sitemaps: https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview
- Google Search Central, Canonicalization: https://developers.google.com/search/docs/crawling-indexing/canonicalization
- Apple Podcasts, RSS feed requirements: https://podcasters.apple.com/support/823-podcast-requirements
- Apple Podcasts, Transcripts: https://podcasters.apple.com/support/5316-transcripts-on-apple-podcasts
- W3C ActivityStreams 2.0: https://www.w3.org/TR/activitystreams-core/
