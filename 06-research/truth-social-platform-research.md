# Truth Social Platform-Admission Research

*Planning-stage research. Research date: July 19, 2026. No implementation, no account creation, no schema, no code. This is not legal advice.*

## 1. Executive Summary

Truth Social has an official licensed product called **Truth API** (announced July 16, 2026), but no public, self-service, general-purpose third-party developer program has been identified. Its published Terms of Service explicitly prohibit automated access, bots, scripts, scrapers, and systematic data retrieval, and there is no documented way to register an application, obtain API keys, or request written developer permission through a public process. **Truth API** is a one-way, read-only, business-to-business licensed data feed. Per TMTG's GlobeNewswire press release (July 16, 2026), "TMTG anticipates that Truth API will be available to **institutional customers** beginning August 1, 2026," and interim CEO Kevin McGurn described it as "a direct, licensed, real-time feed of the platform's most market-moving Truths." Quartz (July 17, 2026) specifies it grants "licensed access to real-time posts from the platform's **top 10 trending accounts**, pushing content to subscribers within milliseconds." It is a data-distribution product for financial firms accessed via a sales/licensing contact — **not** a general developer platform — and it is out of scope and out of budget for The Discrepancy Desk.

What *is* officially supported and safe is **manual publishing assistance** through Truth Social's documented share composer and follow-button, plus **manual recording** of public post URLs and manually observed metrics. Technical similarity to Mastodon is real (the platform is a fork of **Mastodon 3.4.1** with a **Soapbox** front end) but confers **no authorization**: federation is disabled, standard Mastodon clients are not officially supported, and the OAuth/app-registration surface visible in the code is undocumented and unsanctioned for outside use.

The recommended posture is **Option C: Manual publishing support admitted, API integration deferred**, combined with manual recordkeeping. This keeps The Discrepancy Desk fully inside documented, permitted behavior while preserving the "AI drafts, human clears" doctrine, since every publish is a human action taken in the official UI.

## 2. Current Platform-Admission Recommendation

**Chosen outcome: C — Manual publishing support admitted, API deferred** (with manual recordkeeping of URLs and metrics admitted alongside).

**Allowed now:**
- Human-operated posting via the Truth Social web/app composer.
- Use of the official **share URL/composer** (`https://truthsocial.com/share?title=…&url=…`) to prefill a public URL and human-approved text, then a human presses post.
- Manual recording of the resulting published post URL, platform ID, timestamp, and manually observed engagement counts.
- Embedding the official share/follow buttons on The Discrepancy Desk's own web properties.

**Not allowed now:**
- Any automated reading, scraping, or systematic retrieval of Truth Social data (ToS §4(6), §7).
- Any use of undocumented `/api/v1/...` endpoints, the internal OAuth token flow, session-cookie reuse, or third-party scraper APIs/wrappers.
- Automated posting, replying, liking, following, reposting, or DMs.
- Building a database/compilation of Truth Social content without express written permission.

**Evidence that would change the decision:**
- Publication of an official third-party developer program with public registration, documented scopes, and terms that permit read and/or write use (none exists today).
- A loadable, authoritative OpenAPI/Swagger spec at an official URL that grants (not merely describes) permitted third-party use.
- A written grant of permission from TMTG in response to a licensing/permission request.

**Should written permission be requested?** Optional and low-cost: a permission/licensing inquiry to TMTG (the only published channel is the Truth API licensing address, `licensing@tmediatech.com`) could clarify whether any non-financial developer access exists. Treat any response as the governing authority. Do **not** assume silence equals permission — note that CNN (per Quartz) confirmed "No pricing for Truth API was available," access is contact-sales only, and McGurn told Axios (per Quartz, July 17, 2026) that "certain firms had spent months pulling Truth Social data without authorization, frequently running afoul of its terms of service."

**Is a later controlled probe justified?** **No — not now.** No official documentation authorizes a technical probe. Endpoint reachability is not permission. Revisit only if TMTG publishes a developer program or grants written permission.

## 3. Official Developer Access

- **Public developer registration / developer accounts:** None found. No public developer portal, no documented sign-up. *(Official absence; High confidence.)*
- **Application / OAuth client registration:** Not offered as a documented third-party process. The platform inherits Mastodon's OAuth2 provider capability, and the web client uses a hardcoded `client_id`/`client_secret` embedded in its JavaScript bundle (documented by the third-party `truthbrush` project). This is an internal first-party credential, **not** an invitation for third-party client registration. *(Secondary/technical evidence; Moderate confidence that no sanctioned third-party registration exists.)*
- **API keys / personal access tokens / service accounts:** None documented for third parties.
- **Commercial / partner / written-permission access:** The only commercial data access is **Truth API** (July 16, 2026), a B2B licensed feed. Per the TMTG press release it will "be available to institutional customers beginning August 1, 2026," and StockTitan (July 16, 2026) notes it offers "24/7 coverage, and a historical archive of posts dating back to 2022." It is institutional-only, contact-sales, with **no public sign-up page and no self-serve API keys**; initial customers, per Axios, include "financial news organizations and high-frequency trading firms." *(Official + secondary; High confidence.)*
- **The exact official process, if one exists:** For the Truth API only — an organization emails TMTG's "Truth Social Data Licensing team" (`licensing@tmediatech.com`) to license the feed. There is no analogous process for general developers.

**Bottom line:** Truth API exists but is not a general developer platform, and no public, self-service, general-purpose developer program has been identified. An exposed application-registration endpoint in the Mastodon-derived code must **not** be read as permission to register.

## 4. API Documentation Status

- **`https://truthsocial.com/api/docs`** — Returns a **Swagger UI shell that fails to load its API definition** ("Failed to load API definition."). The HTML title is "Swagger UI" and a CSRF token is present, but no usable OpenAPI spec renders. This is a broken/non-authoritative documentation surface, not usable developer documentation. *(Verified July 19, 2026; High confidence.)*
- **OpenAPI/Swagger spec file:** No loadable spec observed at the docs URL.
- **REST API docs / authentication docs / rate-limit docs / webhook docs / streaming docs / media-upload docs / account-management docs / analytics docs:** **None published** for third-party developers. *(Official absence; High confidence.)*
- **Truth API documentation:** The press release references "familiar, industry-standard delivery methods" but no public technical documentation, OpenAPI spec, or developer reference has been published; access and specs are handled through the licensing sales channel. *(Official; Moderate confidence.)*
- **Permission vs. description:** Even where the Mastodon-derived endpoints are described in third-party analyses, those descriptions document *behavior*, not *permission*. Nothing at an official URL grants third-party API rights.

Do not attempt to bypass the broken docs endpoint or fetch missing spec files.

## 5. Mastodon Compatibility Analysis

Truth Social's server is a **fork of Mastodon version 3.4.1** (identified via `lib/mastodon/version.rb` in the released source), with a **Soapbox** front end (per Wikipedia, the front end is "Soapbox Technology's 'Soapbox'... instead of Mastodon's native front end"); both are AGPLv3. Per the Official Mastodon Blog (Oct 29, 2021, Eugen Rochko), the AGPLv3 requires source disclosure; TMTG released source as a ZIP under its legal "Open Source" page after a 2021 compliance request. boehs.org's AGPL write-up documents that the fork retains Mastodon's `/api/v1/instance` endpoint plus a custom `/app/controllers/api/v1/truth/` directory and an ads endpoint absent from vanilla Mastodon. Independent mirrors exist on GitHub (milesmcc, justjosias, boehs). Notable divergences: **federation/ActivityPub is effectively disabled** (the platform does not federate; RSS feeds are stubbed to return empty; it is mutually defederated from the wider Fediverse), added SMS verification and device registration, Groups, ad endpoints, Pleroma-compatible request handling, and extra moderation actions. Technical inheritance is real but is **not** authorization.

| Capability | Standard Mastodon behavior | Truth Social documented behavior | Truth Social observed / publicly evidenced behavior | Permission status | Confidence |
|---|---|---|---|---|---|
| Account lookup | `GET /api/v1/accounts/lookup` etc., open | Not documented for third parties | Public for prominent accounts; auth required for most since ~Aug 2025 | Not addressed / Likely prohibited for automation | Moderate |
| Status lookup | Open REST | Not documented | Public statuses reachable; used by third-party tools | Not addressed / Likely prohibited for automation | Moderate |
| Timeline reads | Public/home timelines | Not documented | "Feeds" reworked (Following, Groups); public reads partially gated | Not addressed | Moderate |
| Search | `/api/v2/search` | Not documented | `search` present (accounts/statuses/hashtags/groups) | Not addressed | Moderate |
| Media retrieval | Media URLs in payloads | Not documented | Media served from `static-assets-*.truthsocial.com` | Personal use only; no commercial reuse w/o written permission | Moderate |
| Notifications | `/api/v1/notifications` | Not documented | Present via inherited code | Not addressed | Low |
| Posting | `POST /api/v1/statuses` | Not documented for third parties | Endpoint exists (Mastodon inheritance) | Not authorized for third parties | Moderate |
| Replying | Status with `in_reply_to_id` | Not documented | Present | Not authorized | Moderate |
| Boosting/reposting (ReTruth) | `/reblog` | Not documented | Present (ReTruth) | Not authorized | Moderate |
| Liking/favoriting | `/favourite` | Not documented | Present (likes) | Not authorized | Moderate |
| Following | `/follow` | Not documented; official *follow button* embed exists | Follow embed documented; API follow not sanctioned | Follow button: permitted / API follow: not authorized | Moderate |
| Direct messages | Mastodon has DMs via statuses | DM help docs exist (UI); 500-char, auto-delete | UI feature; no third-party API sanction | Not authorized for automation | Moderate |
| Bookmarks | `/bookmark` | UI bookmarks documented | Present | Not authorized for automation | Low |
| Lists | `/api/v1/lists` | Not documented | Uncertain | Unknown | Low |
| Polls | Question type | UI polls documented | Present | Not authorized for automation | Low |
| Streaming | Mastodon streaming API | Not documented for third parties | Truth API uses "industry-standard delivery" for institutions only | Institutional license only | Moderate |
| OAuth | OAuth2 provider, app registration | Not documented for third parties | `oauth/token` works with embedded first-party creds | Not authorized for third parties | Moderate |
| App registration | `POST /api/v1/apps` open on vanilla Mastodon | Not documented | Endpoint likely present but unsanctioned | Not authorized; existence ≠ permission | Moderate |
| Metrics | Counts in payloads | No analytics API documented | Engagement counts visible in UI/payloads | Manual observation only | Moderate |
| Account export | Mastodon data export/archive | Privacy policy: user may download own shared info | Inherited settings export likely | Owner self-export permitted | Moderate |

**Standard Mastodon clients:** Not officially supported or endorsed; federation is off, so cross-instance Mastodon interaction does not work. There is no official statement permitting third-party Mastodon clients. Do not rely on them.

## 6. Terms and Automation Boundaries

Key ToS provisions (T MEDIA TECH LLC; governed by Florida law; ©2026):
- **§4(6):** users warrant they "will not access the Service through automated or non-human means, whether through a bot, script, or otherwise."
- **§7(1):** no systematic retrieval "to create or compile … a … database, or directory without written permission from us."
- **§7(9):** no "automated use of the system, such as using scripts … or using any data mining, robots, or similar data gathering and extraction tools."
- **§7(22):** no "spider, robot, cheat utility, scraper, or offline reader," except standard search-engine/browser use.
- **§7(5)/§7(16):** no circumventing or bypassing access-control/security features.
- **§7(14)/§7(15):** no use in a competing/revenue-generating enterprise; no reverse engineering.
- **§2:** content is provided for personal, non-commercial use; no reproduction/aggregation "for any commercial purpose … without our express prior written permission." A limited personal-use license is granted.

| Activity | Clearly allowed | Allowed w/ conditions | Requires permission | Not addressed | Likely prohibited | Clearly prohibited |
|---|---|---|---|---|---|---|
| Manual posting (human in UI) | ✔ | | | | | |
| Manual copy/paste of approved text | ✔ | | | | | |
| Official share button/composer | ✔ (documented) | | | | | |
| Prefilled composer link (`/share`) | ✔ (documented) | | | | | |
| Manual capture of own posts | | ✔ (personal use) | | | | |
| Manual recording of public URLs | | ✔ | | | | |
| Manual metrics recording (human-observed) | | ✔ | | | | |
| Approved API **read** access | | | ✔ (no program exists) | | | |
| Approved API **posting** | | | ✔ (no program exists) | | | |
| Automated timeline reads | | | | | | ✔ (§4(6),§7(9),§7(22)) |
| Automated search | | | | | | ✔ |
| Automated metrics retrieval | | | | | | ✔ |
| Scraping public profiles | | | | | | ✔ (§7(22)) |
| Browser automation | | | | | | ✔ |
| Headless browser collection | | | | | | ✔ |
| Session-cookie API access | | | | | | ✔ (§7(5),§7(16)) |
| Reverse-engineered mobile API | | | | | | ✔ (§7(15)) |
| Building a compilation/database of TS content | | | ✔ (written permission, §7(1)) | | | |
| Commercial reuse of content | | | ✔ (written permission, §2) | | | |

## 7. Authentication and Permissions

Truth Social inherits Mastodon's OAuth2 provider. Third-party analyses show a working `POST https://truthsocial.com/oauth/token` (password grant) using a **hardcoded first-party `client_id`/`client_secret`** extracted from the web client's JavaScript, followed by `GET /api/v1/accounts/verify_credentials`. This is the platform's own client credential, not a sanctioned third-party mechanism. There is **no documented third-party app-registration flow, no published scopes, no PKCE guidance, no refresh/expiration/revocation documentation** for outside developers. **Application registration status for third parties: undocumented / effectively unavailable.** Do not create an account or register an application. Do not reuse embedded credentials.

## 8. Read Capabilities

Mastodon-derived read endpoints exist and some public reads work without auth (e.g., prominent accounts, trends), but since roughly August 27, 2025, third-party tooling reports that Truth Social gates most non-prominent profiles behind authentication. Regardless of reachability:

| Read target | Officially documented? | Permission required? | Auth required? | Likely payload | Rate limit known? | Retention limits |
|---|---|---|---|---|---|---|
| Own authenticated profile | No | N/A (owner UI) | Yes | Mastodon-style JSON | No | Personal-use only |
| Own posts | No | Owner may capture own | Yes | JSON | No | Personal-use only |
| Individual public post | No | Automation prohibited | Sometimes | JSON | No | No compilation w/o permission |
| Replies / mentions | No | Automation prohibited | Often | JSON | No | Same |
| Notifications | No | Automation prohibited | Yes | JSON | No | Same |
| Followers / following | No | Automation prohibited | Often | JSON | No | Same |
| Public / hashtag timelines | No | Automation prohibited | Partial | JSON | No | Same |
| Search | No | Automation prohibited | Partial | JSON | No | Same |
| Media metadata | No | Automation prohibited | Partial | JSON | No | Same |
| Engagement counts | No | Manual only | Partial | JSON | No | Same |
| Post/account analytics | No analytics API | Manual only | Yes (owner UI) | UI only | No | Same |

**Do not collect merely because an endpoint is reachable.** ToS prohibits automated reads and systematic retrieval.

## 9. Write Capabilities

Because it is a Mastodon fork, write endpoints (create post/reply, upload media, create poll, delete/edit, reblog, favourite, follow, DM, schedule) are **technically present**. However:
- **Officially supported for third parties:** No.
- **Permitted by policy:** No (automation prohibited; §4(6), §7, §11 app license bars automated queries).
- **Appropriate for this project:** No — The Discrepancy Desk's doctrine requires human clearance and explicitly forbids autonomous posting/replies/likes/follows/reposts/DMs and coordinated amplification.

All writes should occur **only** as deliberate human actions in the official UI. The Truth API does **not** provide any write/post-back capability; it is strictly an outbound data feed.

## 10. Manual Publishing Options

This is the sanctioned path and the recommended fallback:
- **Official share composer:** Construct `https://truthsocial.com/share` with URL-encoded `title` (prefills the composer's first line) and `url` (prefills the last line). Documented on Truth Social's Publishers help page. The page states the prefill text must not exceed the **500-character limit** and parameters must be URL-encoded.
- **Share button embed:** `<a class="truthsocial-share"></a><script src="https://truthsocial.com/instance/share.js"></script>`.
- **Follow button embed:** `<div data-truth-social-follow-button data-username="USERNAME"></div><script async src="https://embed.truthsocial.com/embed.js"></script>`.
- **Workflow The Discrepancy Desk may safely use:** (1) AI drafts text; (2) human clears it; (3) human opens Truth Social (or the official share composer prefilled with an approved public URL/text); (4) human presses post; (5) human copies the resulting published post URL and records it. Every step is a documented, human-initiated action.

Deep links / mobile app links / link-preview behavior follow standard OpenGraph handling; no special API is required. Media handoff should be manual (human attaches approved media in the composer).

## 11. Content and Media Limits

| Item | Documented limit | Source basis | Notes |
|---|---|---|---|
| Post ("Truth") text | **1,000 characters** | Official help "How do I use Truth Social?": "Type your Truth in the new dialogue box. There is a 1000-character limit." | Some third parties cite 500; the documented composer limit is 1,000. Very long presidential posts suggest higher limits may apply to some accounts (observed, not documented). |
| Share-composer prefill text | **500 characters** | Official Publishers/share-button page | Applies to the prefilled `/share` text specifically. |
| Photos per post | **Up to 4** | Official help | |
| Video per post | **1 video** | Official help | |
| Video size / duration | **Up to 450 MB, max 15 minutes** (effective June 28) | Official photo/video FAQ | |
| DM length | **500 characters** | Official DM Quick Facts | Auto-delete default 14 days, up to 90 days. |
| GIF / poll / alt text / content warnings / editing / scheduling | Inherited Mastodon-style features present in UI | Source-code inheritance + UI | Specific documented limits not published; label as inferred where not in help docs. |
| Hashtags / mentions | Supported (Mastodon-style) | UI/source | Trending is hashtag-based; no documented count cap. |

Clearly label inferred limits (poll options, alt-text length, edit windows) as **inferred from Mastodon lineage**, not confirmed by Truth Social documentation.

## 12. Rate Limits

**No third-party rate limits, retry-header behavior, 429 guidance, or backoff policy are documented.** Truth Social fronts traffic with **Cloudflare BotManagement** (stated in its Privacy Policy) to "protect its website from harmful automated attacks." Do **not** estimate "production-safe" request rates — there is no evidentiary basis, and automated access is prohibited regardless.

## 13. Data Storage and Retention

- ToS **§7(1)** bars compiling a database/collection from the Service **without written permission**; **§2** bars commercial reproduction/aggregation without written permission; **§9** grants TMTG a broad license over user Contributions.
- **Legal permission:** U.S. law is unsettled on public-data scraping; the ToS contractual bar is the binding constraint here.
- **Contractual permission:** Storing raw API payloads or building a compilation is **not** permitted without written permission.
- **Technical feasibility:** High (irrelevant to permission).
- **Privacy risk:** Storing third-party user data (followers, repliers) raises privacy exposure; minimize.
- **Recommended project practice:** Store only what a human legitimately records for its **own** published posts — post URL, platform ID, timestamp, the approved source text, manually entered metrics, and source attribution. Do **not** store scraped/raw third-party API payloads, follower lists, or bulk search results. Never store access tokens or reused session cookies.

## 14. Account Export

Truth Social's **Privacy Policy** states users "can also download the information you have shared with the Website and App" via account settings — an **owner self-export**, consistent with Mastodon's data-download feature. Format, scope, and frequency are **not documented publicly**; assume a Mastodon-style archive. This owner export is the **only sanctioned bulk source** and may legitimately inform The Discrepancy Desk's understanding of its own data shape — but this is a planning-stage note, not a schema-design step. No follower/following export API for third parties is sanctioned.

## 15. Metrics Availability

| Metric | Public UI | Owner UI | Official API | Export | Manual observation | Notes |
|---|---|---|---|---|---|---|
| Replies count | ✔ | ✔ | ✖ (no sanctioned API) | Possibly | ✔ | Mastodon `replies_count` |
| ReTruths (reposts) | ✔ | ✔ | ✖ | Possibly | ✔ | `reblogs_count` |
| Likes/favorites | ✔ | ✔ | ✖ | Possibly | ✔ | `favourites_count` |
| Quotes | ✔ | ✔ | ✖ | Possibly | ✔ | |
| Views/impressions | Limited/none | Limited | ✖ | ✖ | Partial | Not consistently exposed |
| Profile visits | ✖ | Limited | ✖ | ✖ | ✖ | Not documented |
| Follower changes | ✔ (count) | ✔ | ✖ | Possibly | ✔ | Track manually over time |
| Video views | Sometimes | Sometimes | ✖ | ✖ | Partial | |
| Engagement rate | ✖ (computed) | ✖ | ✖ | ✖ | ✔ (compute yourself) | |

**No official analytics/metrics API exists for third parties.** The project should **begin with manual metrics capture** by a human, recorded alongside the post URL — satisfying the "Metrics judge" doctrine without any automation.

## 16. Security Requirements

If official API access ever becomes available (e.g., a written permission grant), require:
- Secrets in environment variables or a secrets manager; **never** in Markdown, Git, screenshots, or raw logs.
- Encrypted storage at rest; least-privilege scopes (read-only before any write).
- A **separate development account**, distinct from the production brand account.
- Secret rotation on a schedule and on suspected exposure; documented revocation testing.
- Log redaction (strip tokens/PII from logs).
- **No session-cookie reuse, no browser credential extraction, no embedded first-party client-secret reuse.**
- Until then, the only "credential" is a human's normal login used in the official UI — protect it with a strong password and MFA if offered.

## 17. Account and Parody Rules

- Truth Social **prohibits impersonation** and "Unauthorized or Impersonator Accounts … that is not a parody," and bans username squatting (Community Guidelines; ToS §5, §7(11)).
- Parody/"trolling" is **nominally permitted** by the Community Guidelines, but reporting (Daily Dot; PBS) shows **inconsistent enforcement** — parody accounts have been suspended, and moderation is discretionary; TMTG reserves sole-discretion termination (ToS §19).
- A **Supplemental TOS for U.S. Federal Government Agency Official Use** exists; impersonating a government official/agency would violate impersonation rules and could invite takedown.
- **Assessment of the proposed bio** ("Fictional records custodian. Commentary/parody. Not affiliated with any government agency."): This framing is **helpful and likely adequate** to satisfy the parody-labeling expectation, provided the persona ("Quinton Clearance") and the handle/display name do **not** mimic a real official, agency seal, or a real person, and provided posts do not present speculation as confirmed fact or claim real classified access. Given inconsistent enforcement and the government-adjacent theme, **professional legal review is warranted before launch** — this report identifies risks only and is not legal advice.

## 18. Cross-Platform Publishing

No Truth Social rule specifically prohibiting cross-posting the *same brand's* content to a *different* platform was found; the binding constraints are the **spam/bot prohibitions** (Community Guidelines) and the **automation ban** (ToS §4(6)). Identical, machine-syndicated, simultaneous posting is the risky pattern. Recommended safe early approach:
- One approved **source draft**, then **platform-specific, human-reviewed variants** for X and Truth Social.
- **Manual publishing** on each platform (no third-party scheduler/syndication API into Truth Social).
- **Separate post URLs and separate metrics** per platform; no cross-platform engagement manipulation, no engagement pods, no coordinated amplification.
- Stagger timing and tailor wording/hashtags to each platform to avoid duplicate/spam classification.

## 19. Evidence Matrix

| Question | Finding | Official source | Secondary source | Source date | Current as of | Permission status | Confidence | Unknowns | Project implication |
|---|---|---|---|---|---|---|---|---|---|
| Public developer program? | No public, self-service, general-purpose program identified | Absence across help.truthsocial.com | 1322.io guide ("No public one") | 2026 | Jul 19 2026 | Not addressed | High | Whether private program exists | Defer API |
| Official API? | Truth API — B2B read-only feed, institutional-only, top-10 accounts, milliseconds | TMTG press release (GlobeNewswire) | CNBC, CBS, Hill, Quartz, StockTitan | Jul 16–17 2026 | Jul 19 2026 | Requires written approval (licensing) | High | Price, exact tech, latency semantics | Out of scope for project |
| API docs load? | Swagger UI shell, spec fails to load | truthsocial.com/api/docs | — | Jul 19 2026 | Jul 19 2026 | Not addressed | High | Whether ever authoritative | Cannot rely on it |
| Mastodon base | Fork of Mastodon 3.4.1 + Soapbox front end | Released AGPLv3 source; Mastodon Blog | jasminchen.dev, boehs.org, Wikipedia | 2021–2024 | Jul 19 2026 | N/A (code) | High | Current exact version | Similarity ≠ permission |
| Federation | Disabled / defederated | Source (stubbed RSS) | Wikipedia, HN | 2022–2024 | Jul 19 2026 | N/A | High | — | No Fediverse interop |
| Automation banned? | Yes | ToS §4(6),§7 | Time, Wikipedia | ©2026 ToS | Jul 19 2026 | Clearly prohibited | High | — | No bots/scrapers |
| Systematic retrieval | Needs written permission | ToS §7(1) | — | ©2026 | Jul 19 2026 | Requires written approval | High | — | No DB compilation |
| Share composer | Documented `/share` prefill, 500 chars | Publishers help page | Shareaholic | 2026 | Jul 19 2026 | Explicitly permitted | High | — | Core manual path |
| Post length | 1,000 chars ("There is a 1000-character limit") | "How do I use TS" help | LinkedIn (500 claim) | 2026 | Jul 19 2026 | N/A | High | 500 vs 1,000 third-party discrepancy | Draft within 1,000 |
| Media limits | 4 photos / 1 video; 450 MB / 15 min | Photo-video FAQ | — | eff. Jun 28 | Jul 19 2026 | N/A | High | Image size/format cap | Plan media manually |
| Rate limits | Undocumented; Cloudflare bot mgmt | Privacy Policy | — | 2026 | Jul 19 2026 | Not addressed | High | Actual limits | Don't estimate |
| Owner data export | "Download info you have shared" | Privacy Policy | — | 2026 | Jul 19 2026 | Conditionally permitted (owner) | Moderate | Format/scope | Only sanctioned bulk source |
| Analytics API | None for third parties | Absence | — | 2026 | Jul 19 2026 | Not addressed | Moderate | — | Manual metrics |
| Parody allowed? | Nominally yes; enforcement inconsistent | Community Guidelines | Daily Dot, PBS | 2025–2026 | Jul 19 2026 | Conditionally permitted | Moderate | Enforcement discretion | Label clearly; legal review |
| Impersonation | Prohibited | Community Guidelines, ToS §7(11) | Daily Dot | 2026 | Jul 19 2026 | Clearly prohibited | High | — | No gov/official mimicry |

## 20. Risk Register

| Risk | Trigger | Likelihood | Impact | Evidence | Mitigation | Stop condition |
|---|---|---|---|---|---|---|
| Assuming Mastodon compatibility | Treating fork as vanilla Mastodon; using standard clients/endpoints | Med | High (bans, broken assumptions) | Fork of 3.4.1, federation off | Prove each behavior; never assume | Any reliance on unproven endpoint |
| Undocumented API access | Calling internal `/api/v1/*` or OAuth token flow | Med | High (ToS breach, suspension) | ToS §4(6),§7 | Manual-only; no probes | Any programmatic call planned |
| ToS violation | Automated read/write/scrape | Med-High | High (legal action per §19) | ToS §7, §19 | Human-in-UI only | Any automation proposed |
| Account suspension | Bot detection / parody dispute | Med | High (loss of brand handle) | Cloudflare bot mgmt; parody bans | Manual use; clear parody labels | Warning/label request from TMTG |
| Credential exposure | Secrets in Git/logs/screenshots | Low-Med | High | — | Env vars/secrets mgr; MFA | Any secret committed |
| Rate-limit violation | Guessing safe request rates | Low (if manual) | Med | Undocumented | No automation | Any batch calling |
| Data-retention violation | Storing scraped payloads / building DB | Med | High (§7(1),§2) | ToS | Store only own-post records | Any bulk store of TS data |
| Privacy concerns | Storing third-party user data | Med | Med-High | Privacy law | Data minimization | Storing follower/replier PII |
| Unofficial client dependence | Relying on truthbrush/scrapers | Med | High (ToS breach, breakage) | Third-party tools gated Aug 2025; McGurn's "ran afoul of ToS" remark | Avoid entirely | Any dependency added |
| Documentation drift | Rules/endpoints change silently | High | Med | "may change without notice" | Re-verify at each stage | Stale citation used |
| API shutdown | Internal endpoints locked | Med | Med | 1322.io; historical gating | Manual path unaffected | Reliance on any endpoint |
| Cross-posting spam classification | Identical simultaneous syndication | Low-Med | Med | Spam guidelines | Platform-specific variants, staggered | Duplicate-mass posting |
| Parody/impersonation confusion | Persona reads as real official | Med | High (takedown, legal) | Impersonation rules | Clear labels; no seals/real names; legal review | Any confusion complaint |

## 21. Unknowns Requiring Clarification

- Whether TMTG offers **any** non-financial developer or written-permission access beyond the institutional Truth API. *(Unresolved.)* Note: McGurn told Axios (per Quartz) TMTG "is in discussions to license Truth Social data to artificial intelligence companies for large language model training" — a forward-looking statement, not a current developer offering.
- Truth API's exact **delivery technology, latency semantics (before vs. simultaneous with public posting), and pricing** — TIME (July 17, 2026) noted "exact posting latency" was not disclosed, and CNN (per Quartz) confirmed no pricing was available. *(Unresolved; not disclosed.)*
- The **current** exact Mastodon fork version powering production (source releases lag; uploads reportedly stopped after December 2022). *(Unresolved.)*
- The true **post character limit** (documented 1,000 vs. third-party 500 claims vs. very long observed posts). *(Largely resolved to 1,000 via official help; residual ambiguity for high-profile accounts.)*
- Whether the broken **Swagger docs** endpoint was ever intended to be authoritative or is a first-party internal artifact. *(Unresolved.)*
- Owner **data-export** format/scope/frequency. *(Unresolved.)*

## 22. Recommended Next Bounded Step

Adopt **Option C**: stand up a **manual, human-cleared publishing workflow** for Truth Social using the official share composer and manual URL/metric recordkeeping, mirroring the "AI drafts, human clears, database remembers, metrics judge" doctrine with a human as the sole publish actor. In parallel, **send one written permission/licensing inquiry** to TMTG (`licensing@tmediatech.com`) to ask whether any sanctioned read access exists for a small research/publishing project, and treat the answer as governing. Obtain **professional legal review** of the parody/persona framing before launch. Do **not** build any API integration, run any probe, or store any scraped data. Re-verify all rules at the point of any future change, because Truth Social states its rules and endpoints "may change without notice."

## 23. Sources

- Truth Social Terms of Service — help.truthsocial.com/legal/terms-of-service (©2026; accessed Jul 19 2026; official).
- Truth Social Community Guidelines — help.truthsocial.com/community-guidelines-page (accessed Jul 19 2026; official).
- Truth Social Privacy Policy — help.truthsocial.com/legal/privacy-policy (accessed Jul 19 2026; official).
- Truth Social Publishers / Share Button — help.truthsocial.com/publishers/share-button (accessed Jul 19 2026; official).
- Truth Social Follow Button — help.truthsocial.com/publishers/follow-button (official).
- Truth Social "How do I use Truth Social?" and Photo/Video size FAQ — help.truthsocial.com/frequently-asked-questions (official).
- Truth Social DM Quick Facts — help.truthsocial.com/direct-messages/quick-facts (official).
- Truth Social Supplemental TOS for U.S. Federal Gov Agency Use — help.truthsocial.com/legal (official).
- truthsocial.com/api/docs — Swagger UI shell, "Failed to load API definition" (accessed Jul 19 2026; official surface, non-functional).
- TMTG press release, "Trump Media and Technology Group Launches Truth API, a New Licensed Data Service for Financial Services Partners" — GlobeNewswire (Jul 16 2026; official).
- CNBC, CBS News, The Hill, NBC/Reuters, TIME, Quartz, StockTitan, Axios (via Quartz) — Truth API coverage (Jul 16–17 2026; secondary).
- Mastodon Blog, "Trump's new social media platform found using Mastodon code" (Oct 29 2021; comparison).
- jasminchen.dev source-code analysis (2024) and boehs.org AGPL compliance write-up (secondary/technical).
- GitHub source mirrors: milesmcc/truthsocial, justjosias/truth-social, boehs/truthsocial (secondary/technical).
- stanfordio/truthbrush (archived Apr 27 2026) — third-party client documenting endpoints/OAuth (secondary; not endorsed).
- Daily Dot and PBS — parody-account enforcement reporting (secondary).
- Wikipedia — Truth Social / Mastodon; Keysight traffic analysis (background/secondary).

---
*Confidence labels used: High / Moderate / Low / Unresolved. Permission labels used: Explicitly permitted / Conditionally permitted / Not addressed / Requires written approval / Likely prohibited / Explicitly prohibited. All findings distinguish officially documented behavior, technically observable behavior, Mastodon-derived behavior, third-party claims, historical information, and current verified information as of July 19, 2026. Technical similarity to Mastodon is never treated as authorization; endpoint existence is never treated as permission.*
