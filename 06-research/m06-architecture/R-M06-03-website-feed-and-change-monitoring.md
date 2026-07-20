# R-M06-03 — Website, Feed, and Change Monitoring

## Research status

Research report completed on 2026-07-20.

This report is source material for M06 architecture synthesis. It does not authorize implementation, scraping, crawling, browser automation, monitoring, provider admission, retention of third-party content, or any change to the accepted milestone plan.

No website-ingestion option in this report is approved merely because it is technically feasible. The owner must explicitly approve any later architecture option and any implementation package.

## Question and project boundary

How should The Discrepancy Desk manually ingest public webpages now, and what standards, provenance, safety, change-detection, feed, and monitoring mechanisms could support future automation without creating duplicate truth, unauthorized crawling, noisy change records, or untraceable LLM context?

The report covers:

- manually pasted webpage content;
- human-triggered single-page retrieval;
- RSS and Atom feeds;
- WebSub notifications;
- XML sitemaps;
- HTTP validators;
- canonical URLs and redirects;
- raw HTML versus extracted main content;
- JavaScript-rendered pages;
- meaningful-change classification;
- page deletion and unavailability;
- provider-based extraction;
- future scheduled monitoring;
- prompt-injection and hostile-page risks.

It does not select a parser, crawler, SaaS provider, browser engine, database schema, Qdrant design, or monitoring schedule.

---

# 1. Executive findings

## 1.1 Manual-first website intake is practical

**PROJECT RECOMMENDATION**

The lowest-risk initial website workflows are:

```text
Option A — Paste selected webpage text and source URL
Option B — Human-triggered retrieval of one public URL
```

Both should create a pending ingestion package and require human review before Vault admission.

The system should not initially:

- recursively crawl a site;
- log into websites;
- bypass paywalls or access controls;
- execute arbitrary page scripts;
- follow every discovered link;
- monitor sites automatically;
- treat extracted text as the original page;
- assume public accessibility grants indefinite retention rights.

## 1.2 Feeds are the strongest early monitoring candidate

**VERIFIED FACT**

Atom is a standardized XML syndication format containing feeds and entries with stable identifiers and metadata. RSS 2.0 is also widely used for content syndication. WebSub is a W3C Recommendation that allows publishers and hubs to deliver new or updated content to subscribers using verified HTTP callbacks.

**PROJECT RECOMMENDATION**

After manual ingestion is stable, admitted RSS/Atom feeds should be considered before general webpage polling because they provide publisher-supplied discovery and update metadata.

A feed notification or entry should create a pending source candidate. It should not automatically become accepted Vault knowledge.

## 1.3 HTTP validators reduce transfer but do not prove meaningful change

**VERIFIED FACT**

HTTP conditional requests can use `If-None-Match` with ETags and `If-Modified-Since` with `Last-Modified`. A server may return `304 Not Modified`, avoiding transfer of an unchanged representation. RFC 9110 treats `If-None-Match` as the more accurate validator when both are available.

**INFERENCE**

A changed ETag, Last-Modified value, response body, or HTML hash proves only that a representation changed. It does not establish that the article's substantive claims changed.

## 1.4 Raw capture and readable extraction are different artifacts

**PROJECT RECOMMENDATION**

A webpage ingestion should preserve at least:

```text
request and redirect receipt
response metadata
raw retrieved representation when admitted
normalized URL observations
main-content extraction
parser identity and version
human review decision
```

The raw response is acquisition evidence. The extracted article text is a fallible derivative.

## 1.5 Canonical URLs are hints, not identity truth

**VERIFIED FACT**

Web publishers can signal a preferred URL through redirects, `rel="canonical"`, and sitemap inclusion. Search engines may still choose a different canonical URL. Canonicalization exists because similar or duplicate content often appears under multiple URLs.

**PROJECT RECOMMENDATION**

The system should distinguish:

```text
requested URL
final response URL
publisher-declared canonical URL
normalized comparison URL
source-item identity
```

It should not silently merge records solely because a canonical tag matches.

## 1.6 Website monitoring requires source-by-source admission

**PROJECT RECOMMENDATION**

Every monitored site or feed should require a source-admission record covering:

- exact host and scope;
- approved acquisition method;
- robots and terms review;
- authentication status;
- permitted retention;
- polling or notification method;
- rate and cost limits;
- account ownership;
- failure handling;
- disablement authority;
- change significance rules;
- re-review date.

---

# 2. Evidence labels

This report uses:

- **VERIFIED FACT** — directly supported by a primary or authoritative source.
- **SOURCE/PROVIDER CLAIM** — functionality or policy stated by a product provider.
- **INFERENCE** — reasoned implication from verified material.
- **PROJECT RECOMMENDATION** — a proposed rule requiring owner acceptance.
- **OPEN QUESTION** — unresolved and unsafe to treat as settled.

---

# 3. Website source identities and observations

## 3.1 Required distinctions

**PROJECT RECOMMENDATION**

The future architecture should distinguish:

```text
website/source identity
page/source-item identity
URL occurrence
page observation
acquisition attempt
raw response artifact
rendered-page artifact
main-content derivative
change candidate
human-admitted change
```

Example:

```text
example.gov                                  website identity
/report-2026                                 page identity candidate
https://example.gov/report-2026?ref=x        URL occurrence
GET on 2026-07-20                            acquisition attempt
HTTP 200 HTML bytes                          raw response artifact
Readability article result                   extracted derivative
headline changed                             change candidate
human confirms report was revised            admitted change
```

## 3.2 One URL is not necessarily one stable document

A URL may:

- redirect;
- return different content by language, geography, device, cookies, or time;
- change ownership;
- become a login page;
- return an error page with status 200;
- host a revised document;
- serve dynamically rendered content;
- expose different canonical hints.

The system should therefore preserve observation time and response metadata rather than treating URL alone as identity.

---

# 4. Manual ingestion options requiring later owner approval

No option below is approved by this report.

## Option A — Human-pasted text

### Workflow

```text
human opens webpage
→ copies selected text
→ pastes text and URL into app
→ declares what was copied
→ system preserves exact supplied text
→ human reviews normalized record
→ human admits or rejects
```

### Advantages

- simplest implementation;
- no automated website retrieval;
- works for JavaScript-heavy pages, login-visible pages, and awkward layouts;
- user controls the selected scope;
- immediate utility.

### Limitations

- may omit context, headings, links, images, corrections, or surrounding text;
- no automatic response headers or raw HTML;
- copying can alter whitespace and formatting;
- cannot independently prove what the page displayed;
- timestamps and author metadata may be manually entered.

### Recommended provenance class

```text
human_pasted_web_text
```

## Option B — Human-triggered single-page HTTP retrieval

### Workflow

```text
human enters one public URL
→ system validates scheme and destination
→ retrieves one bounded representation
→ preserves request/response receipt
→ extracts candidate main content
→ shows raw/derived metadata and warnings
→ human admits or rejects
```

### Proposed initial constraints

- `https` preferred;
- one URL per explicit action;
- no recursive crawling;
- no login or cookie import;
- no automatic JavaScript execution;
- bounded redirects;
- bounded response size and time;
- content-type allowlist;
- private-network and localhost destinations blocked;
- no download of arbitrary linked resources;
- human review required.

### Advantages

- preserves transport metadata;
- can capture exact HTML bytes when retention is admitted;
- supports deterministic parser testing;
- can record redirects, ETag, Last-Modified, content type, and canonical hints.

### Limitations

- static HTTP may not contain rendered article content;
- raw HTML may include scripts, tracking, navigation, and prompt injection;
- some sites block non-browser clients;
- permission and retention remain source-specific.

### Recommended provenance class

```text
human_triggered_single_page_http
```

## Option C — Human-triggered rendered-page capture

### Workflow

A controlled browser engine loads one explicitly requested page, waits under a bounded policy, captures the rendered DOM or snapshot, and returns it for human review.

### Advantages

- supports JavaScript-rendered pages;
- can capture content absent from initial HTML;
- closer to what the human saw.

### Risks

- page scripts execute;
- greater prompt-injection and hostile-content exposure;
- authentication/cookie leakage risks;
- substantially larger attack surface;
- anti-bot and platform-policy concerns;
- non-deterministic rendering;
- resources may change during load;
- harder evidence/replay model.

### Recommendation

**PROJECT RECOMMENDATION**

Do not include rendered-page capture in the first M06 implementation package. Research it as a later separately approved capability only if static retrieval and manual paste prove insufficient.

## Option D — Feed subscription

### Workflow

```text
owner admits feed URL and scope
→ system retrieves RSS/Atom conditionally or subscribes via WebSub
→ new or updated entry becomes pending candidate
→ optional article retrieval requires its own admitted step
→ human reviews and admits
```

### Advantages

- publisher-supplied discovery;
- stable entry IDs often available;
- update timestamps and links supplied;
- lower noise than crawling an entire site;
- WebSub can reduce polling.

### Limitations

- feeds may contain excerpts only;
- entries may be removed from the rolling feed;
- IDs and timestamps can be malformed or reused;
- feed updates do not prove article-body changes;
- article retrieval remains a separate operation.

### Recommendation

Treat feed discovery and article acquisition as separate records.

## Option E — Scheduled single-page monitoring

### Workflow

An admitted source record schedules conditional requests for a fixed URL. New observations are compared against prior observations. Only candidate changes enter review.

### Recommendation

Do not approve this until:

- source admission exists;
- conditional retrieval is implemented and tested;
- change classification is defined;
- monitoring budgets and disablement are governed;
- backup/recovery and stale-worker behavior are proven.

## Option F — Sitemap discovery

Sitemaps can enumerate site URLs and optional modification dates.

### Recommendation

Sitemaps should be treated as discovery hints, not authorization to retrieve every listed page and not proof that `<lastmod>` is accurate. Sitemap-based expansion should require a separately approved source scope and candidate limits.

## Option G — Extraction SaaS/provider

Potential providers can return cleaned Markdown, structured article fields, screenshots, or rendered content.

### Advantages

- broad website compatibility;
- reduced local browser/parser complexity;
- structured output;
- possible anti-bot handling.

### Risks

- unclear acquisition methods;
- third-party retention;
- content sent outside the local system;
- pricing and rate limits;
- provider-specific output drift;
- difficulty reproducing extraction;
- platform/site policy concerns;
- vendor-generated content may be mistaken for source truth.

### Recommendation

No provider should be selected in R-M06-03. A later provider-admission review must compare local static retrieval, local parsing, rendered-browser services, and extraction SaaS options.

---

# 5. Feed standards and update delivery

## 5.1 Atom

**VERIFIED FACT**

RFC 4287 defines Atom feeds and entries. Atom entries carry IDs, update timestamps, links, content or summaries, authorship metadata, and extensible elements.

**PROJECT IMPLICATION**

An Atom entry ID should be preserved as publisher-supplied identity evidence, but the system should still retain feed URL, entry link, observation time, and content hash because publishers can make mistakes.

## 5.2 RSS

**VERIFIED FACT**

RSS 2.0 provides channel and item structures commonly containing title, link, description, GUID, publication date, author, and enclosure information.

**PROJECT IMPLICATION**

RSS GUIDs should be treated as publisher-supplied identifiers, not globally guaranteed UUIDs.

## 5.3 WebSub

**VERIFIED FACT**

WebSub defines publisher, hub, and subscriber roles. Subscribers request subscriptions through hubs, hubs verify callbacks, and hubs distribute new or updated content. Authenticated delivery can use a shared secret and signature header.

**SECURITY IMPLICATION**

A future WebSub implementation would require:

- public callback reachability or a relay;
- challenge verification;
- signature verification when configured;
- topic/hub binding;
- replay and duplicate handling;
- payload size limits;
- malicious hub and topic defense;
- subscription renewal and expiration handling.

**PROJECT RECOMMENDATION**

WebSub belongs to future monitoring architecture, not initial M06 manual ingestion.

---

# 6. HTTP retrieval and validators

## 6.1 Conditional requests

**VERIFIED FACT**

A client can send:

```text
If-None-Match: <prior ETag>
If-Modified-Since: <prior Last-Modified>
```

A server may respond `304 Not Modified`. When both are present, `If-None-Match` takes precedence.

## 6.2 Proposed observation fields

**PROJECT RECOMMENDATION**

A webpage acquisition receipt should preserve:

```text
requested_url
request_time
request_method
request_headers_policy
redirect_chain
final_url
status_code
response_time
content_type
content_encoding
content_length_observed
etag
last_modified
cache_control
content_language
link headers
publisher canonical hint
robots observation reference
raw artifact hash
retrieval instrument and version
```

Sensitive transient headers should not be stored by default.

## 6.3 Validator limitations

ETags may be:

- weak or strong;
- regenerated for non-substantive changes;
- absent;
- shared across compressed variants unpredictably;
- altered by intermediaries.

Last-Modified may be:

- missing;
- rounded;
- inaccurate;
- changed by deployment rather than content revision.

The system must not convert validator change into a substantive-content conclusion.

---

# 7. Robots, terms, and access boundaries

## 7.1 Robots exclusion

**VERIFIED FACT**

RFC 9309 defines robots rules for automated crawlers and explicitly states that these rules are not access authorization.

## 7.2 Project rule

**PROJECT RECOMMENDATION**

For automation, the system should record robots observations but must also separately consider:

- site terms;
- API availability;
- authentication restrictions;
- copyright and retention;
- frequency and load;
- source owner expectations;
- contractual restrictions;
- applicable law.

A robots allowance is not sufficient admission. A robots disallow should normally block automated retrieval unless a separate explicit authority applies and is owner-approved.

## 7.3 Manual human-visible content

Human access to a page does not automatically authorize automated copying, bulk retention, or monitoring. Manual pasted text should preserve that it was supplied by the human rather than implying system acquisition permission.

---

# 8. Canonicalization, redirects, and duplicate handling

## 8.1 Canonical signals

**VERIFIED FACT**

Common canonicalization signals include:

- permanent redirects;
- HTML `rel="canonical"`;
- HTTP `Link` header with `rel="canonical"`;
- sitemap inclusion.

These are signals or hints rather than infallible identity rules.

## 8.2 Proposed identity treatment

**PROJECT RECOMMENDATION**

Store separately:

```text
requested URL
observed redirect chain
final URL
publisher canonical URL
feed entry URL
sitemap URL
normalized comparison URL
human-resolved source-item identity
```

Potential duplicates should become review candidates.

## 8.3 URL normalization limits

Safe mechanical normalization may include:

- lowercase scheme and host;
- default-port handling;
- fragment removal for HTTP retrieval identity;
- percent-encoding normalization where semantics are preserved.

The system should not automatically remove query parameters or reorder them without source-specific rules because parameters may change content.

---

# 9. Static HTML, rendered DOM, and main-content extraction

## 9.1 Static response

The raw response is what the server returned to the admitted client under a specific request context.

It may not match what a browser renders.

## 9.2 Rendered page

A browser may execute scripts, fetch additional resources, personalize content, and mutate the DOM.

A rendered capture requires its own instrument identity and timing policy.

## 9.3 Main-content extraction

**SOURCE/PROVIDER CLAIM**

Mozilla Readability, used by Firefox Reader View, can extract fields such as title, processed content, plain text, excerpt, byline, site name, language, and publication time from a DOM document.

Mozilla explicitly notes that:

- extraction uses heuristics;
- readerability checks can produce false positives and negatives;
- untrusted output must be sanitized;
- JavaScript and remote-resource execution should remain disabled in server-side DOM processing unless explicitly needed.

**PROJECT RECOMMENDATION**

A future local static-page parser option should evaluate Mozilla Readability or equivalent as one candidate, while preserving:

```text
raw HTML
parser version
parser configuration
extracted HTML
extracted plain text
metadata fields
warnings
```

The parser output must never replace the raw acquisition artifact.

---

# 10. Meaningful-change model

## 10.1 Change layers

**PROJECT RECOMMENDATION**

Future monitoring should distinguish:

```text
transport_change
raw_representation_change
metadata_change
layout_or_template_change
main_content_change
substantive_text_change_candidate
removal_or_unavailability
redirect_change
canonical_hint_change
```

## 10.2 Proposed comparison stages

```text
Stage 1 — HTTP validator result
Stage 2 — raw-byte/content hash
Stage 3 — normalized DOM or main-content hash
Stage 4 — structured field comparison
Stage 5 — text diff
Stage 6 — human significance review
```

An LLM may later summarize differences as an advisory candidate, but it may not decide that a correction, contradiction, or significant event occurred.

## 10.3 Noise examples

Non-substantive differences may include:

- timestamps generated on every request;
- ad rotation;
- navigation changes;
- recommendation widgets;
- analytics identifiers;
- cookie banners;
- unrelated sidebar changes;
- formatting changes;
- reordered attributes;
- dynamic counters.

## 10.4 Substantive candidate examples

- headline changed;
- author changed;
- publication/update date changed;
- paragraph added or removed;
- numerical claim changed;
- correction notice added;
- linked source changed;
- document replaced;
- page removed or redirected;

These remain candidates until reviewed.

---

# 11. Page deletion, errors, and availability

A failed request should not mean the source item is deleted.

The system should distinguish:

```text
DNS failure
connection timeout
TLS failure
robots refusal
HTTP 401/403
HTTP 404/410
HTTP 429
HTTP 5xx
soft-404 page
login/interstitial response
challenge page
content-type change
redirect to new location
successful retrieval with empty extraction
```

A single transient failure must not erase prior observations or create a deletion conclusion.

A future monitor should require repeated or source-specific confirmation before marking an item unavailable.

---

# 12. Security and hostile-content boundary

## 12.1 Server-side request forgery

**PROJECT RECOMMENDATION**

Any URL retrieval capability must block:

- localhost and loopback;
- private and link-local networks;
- cloud metadata endpoints;
- non-HTTP schemes;
- DNS rebinding and redirect escape to blocked ranges;
- unbounded redirects;
- oversized responses;
- decompression bombs;
- unsupported content types.

## 12.2 Script and active-content safety

Raw or extracted HTML may contain:

- scripts;
- event handlers;
- iframes;
- forms;
- tracking URLs;
- malicious CSS;
- prompt-injection text;
- misleading hidden content.

The application should never render untrusted HTML with active privileges. Human-readable projections should be sanitized or rendered as inert text.

## 12.3 LLM prompt injection

Web content may contain instructions aimed at an LLM, such as requests to ignore system rules, expose secrets, or alter records.

The future context compiler must label retrieved webpage content as untrusted source material and prevent it from becoming tool authority.

## 12.4 Credentials

The first webpage importer should not accept browser cookies, passwords, session exports, or authenticated profiles.

Authenticated-source ingestion requires a later dedicated security and retention review.

---

# 13. Candidate manual-first website package

**PROJECT RECOMMENDATION — requires owner approval later**

A pending webpage ingestion package could contain:

```text
manifest.json
original/
  supplied-text.txt OR response-body.html
receipts/
  acquisition.json
normalized/
  page.json
  extracted-content.html
  extracted-text.txt
review/
  admission-decision.json
```

Candidate manifest fields:

```text
ingestion_id
account_id
source_type
acquisition_mode
requested_url
final_url
publisher_canonical_url
acquired_at
retrieval_instrument
original_artifact_hash
normalizer_name
normalizer_version
review_status
```

This is a research recommendation, not an approved schema.

---

# 14. Source-admission state candidate

A monitored website/feed could move through:

```text
proposed
policy_review_needed
manual_only
approved_single_page_manual
approved_feed_monitoring
approved_fixed_url_monitoring
suspended
revoked
```

No state or vocabulary is approved by this report.

The record should answer:

- Who approved it?
- What exact host/feed/path is covered?
- What method is allowed?
- Is full content retained or only metadata/excerpts?
- What is the frequency or notification mode?
- What budgets apply?
- What failures suspend it?
- When must it be re-reviewed?

---

# 15. Candidate architecture options for owner review later

## Architecture W1 — Paste-only website intake

```text
human-supplied text + URL
→ immutable supplied artifact
→ review
→ Vault admission
```

**Best for:** fastest safe utility.

**Weakness:** limited provenance and context.

## Architecture W2 — Local static single-page importer

```text
explicit URL
→ bounded HTTP retrieval
→ raw response preservation
→ local main-content extraction
→ review
```

**Best for:** ordinary public articles and documents.

**Weakness:** fails on many JavaScript-heavy pages.

## Architecture W3 — Local static importer plus feed monitor

```text
W2
+ admitted RSS/Atom feeds
+ conditional polling
+ pending candidates
```

**Best for:** early automation with limited scope.

**Weakness:** feed quality varies; article retrieval remains separate.

## Architecture W4 — Provider-assisted extraction

```text
explicit URL
→ admitted external provider
→ provider response + provenance
→ review
```

**Best for:** broad compatibility and reduced local complexity.

**Weakness:** privacy, retention, cost, reproducibility, and provider-method uncertainty.

## Architecture W5 — Controlled browser rendering

```text
explicit URL
→ isolated browser worker
→ rendered capture
→ extraction
→ review
```

**Best for:** dynamic sites that cannot be handled statically.

**Weakness:** highest security and operational complexity.

## Research recommendation

Present W1–W5 to the owner during M06 synthesis. Do not silently combine them. A plausible staged order is W1, then W2, then feed monitoring, with W4/W5 only after demonstrated need—but the owner must approve that sequence.

---

# 16. Rejected or unsafe shortcuts

The report rejects:

- treating robots.txt as legal permission;
- crawling all links from an imported page;
- importing every URL in a sitemap automatically;
- storing only extracted Markdown and discarding the source artifact;
- treating `rel="canonical"` as conclusive identity;
- treating an ETag change as substantive change;
- treating feed removal as deletion;
- logging into arbitrary sites with browser cookies;
- executing page scripts in the desktop app;
- rendering unsanitized HTML;
- allowing URLs to reach private networks;
- letting webpage text instruct the LLM's tools;
- automatic monitoring without a source-admission record;
- automatic Vault admission from feed or monitor events;
- selecting a scraping/extraction SaaS without owner approval.

---

# 17. Open questions

1. Should M06 initially include only paste-only intake, or also bounded static URL retrieval?
2. If static retrieval is approved, may raw HTML be retained indefinitely or only as evidence under source-specific policy?
3. Should the importer preserve response headers verbatim or an allowlisted normalized receipt?
4. Which content types belong in initial URL retrieval: HTML only, or also plain text, JSON, XML, and PDF?
5. Should feeds be implemented in M06 or deferred to a later connector milestone?
6. Is local Mozilla Readability the preferred extraction candidate, or should multiple parsers be benchmarked?
7. What level of failed-extraction review UI is required?
8. How should canonical and duplicate candidates be resolved?
9. What source-specific retention states are required?
10. When does a page observation become a document version rather than a new occurrence?
11. What threshold or rules should create a substantive-change candidate?
12. Should a future LLM diff summary be generated automatically or only on human request?
13. How should screenshots and visual evidence relate to static HTML observations?
14. Should rendered-browser capture belong to M06, M07, or a later dedicated source-connector milestone?
15. Which provider candidates, if any, deserve a separate admission comparison?

---

# 18. Inputs required by later reports

## R-M06-04

Needs:

- raw-versus-derived artifact distinction;
- static HTML and rendered DOM distinction;
- extraction provenance;
- content-type and parser routing;
- sanitization and hostile-document requirements.

## R-M06-05

Needs:

- source identity, page identity, URL occurrence, and observation distinction;
- canonical-hint treatment;
- feed-entry and webpage relationships;
- change-candidate representation.

## R-M06-06

Needs:

- URL normalization limits;
- redirect/canonical evidence;
- content hashes and version behavior;
- duplicate and revision handling.

## R-M06-07

Needs:

- untrusted webpage content boundary;
- prompt-injection defenses;
- competing observations and correction notices;
- bounded source excerpts.

## R-M06-08

Needs:

- heading and main-content structure;
- page-version invalidation;
- chunk provenance;
- current/superseded/deleted filtering.

## R-M06-09

Needs:

- source-admission state;
- SSRF and browser isolation;
- polling/WebSub controls;
- budgets, retries, suspension, backup, and rebuild.

---

# 19. Recommended direction for synthesis consideration

**PROJECT RECOMMENDATION — not approved architecture**

The future synthesis should consider this staged path:

```text
Stage 0 — Human-pasted webpage text
Stage 1 — Human-triggered bounded static retrieval of one URL
Stage 2 — Local main-content extraction with raw artifact preservation
Stage 3 — Admitted RSS/Atom feed discovery
Stage 4 — Conditional fixed-page monitoring
Stage 5 — WebSub subscriptions
Stage 6 — Provider-assisted or rendered-browser retrieval where justified
```

Every stage requires explicit owner approval and its own test and policy boundary.

The first implementation should remain useful even if all automation stages are rejected.

---

# 20. Primary sources and access dates

Accessed 2026-07-20.

- RFC 9110 — HTTP Semantics
  https://datatracker.ietf.org/doc/rfc9110/

- RFC 9309 — Robots Exclusion Protocol
  https://datatracker.ietf.org/doc/rfc9309/

- RFC 4287 — The Atom Syndication Format
  https://datatracker.ietf.org/doc/rfc4287/

- W3C WebSub Recommendation
  https://www.w3.org/TR/websub/

- Sitemaps protocol
  https://www.sitemaps.org/protocol.html

- Google Search Central — Canonicalization
  https://developers.google.com/search/docs/crawling-indexing/canonicalization

- Google Search Central — Consolidate duplicate URLs
  https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls

- Google Search Central — JavaScript SEO basics
  https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics

- Mozilla Readability
  https://github.com/mozilla/readability

## Provider candidates noted but not admitted

The following classes require a later separate provider comparison if the owner chooses to evaluate them:

- article extraction APIs;
- rendered browser APIs;
- crawling services;
- change-monitoring SaaS;
- hosted feed/WebSub infrastructure.

No provider is recommended or approved by this report.
