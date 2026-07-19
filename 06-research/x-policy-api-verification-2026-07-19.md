# X Policy and API Verification — 2026-07-19

## Purpose

Record the current official X facts required to begin Milestone 01 without relying on old third-party estimates or chat history.

Research date: 2026-07-19.

Primary sources only:

- X Help — Authenticity
- X Help — Profile labels
- X Developer Docs — Pricing
- X Developer Docs — Rate limits
- X Developer Docs — Usage and billing
- X Developer Console documentation

These findings must be rechecked before any API credit purchase or if X materially changes its policies or pricing.

## Finding X001 — PCF Display-Name Requirement

X permits compliant Parody, Commentary, and Fan accounts that depict another person, group, or organization, but requires all of the following:

- a PCF profile label;
- a non-identical avatar;
- a term such as `parody`, `fake`, `fan`, or `commentary` at the beginning of the account display name; and
- a matching disclosure term in the bio/profile description.

### Project posture

The Discrepancy Desk is an original fictional brand rather than an account depicting a specific real person, group, agency, or organization. The official PCF display-name rule is therefore recorded as a conditional platform requirement, not automatically treated as controlling this account.

The owner-approved live profile posture is:

```text
Display name: Quinton Clearance
Handle: @DiscrepancyDesk
Bio: Every anomaly becomes paperwork.
Location: Basement 1, Level 7
```

The owner rejected adding `Commentary |` or similar wording to the display name. The account must instead avoid affiliation confusion through its original fictional identity, artwork, public content, and refusal to use real government seals, insignia, agency branding, employment claims, or claims of classified access.

Re-evaluate PCF-label applicability only if X provides account-specific guidance, issues a warning, or materially changes the policy.

Confidence: High — official X policy.

Recheck trigger: before public launch and after any X warning or policy change.

## Finding X002 — Current API Pricing Model

X API v2 currently uses prepaid, credit-based, pay-per-use pricing with no subscription or minimum commitment.

Official documented rates relevant to the probe include:

| Resource or action | Current documented cost |
|---|---:|
| Post read | $0.005 per returned Post |
| User read | $0.010 per returned User |
| Owned read | $0.001 per returned resource |
| Content create | $0.015 per request in the detailed pricing documentation |
| Content create with URL | $0.200 per request |

Owned-read pricing applies when the authenticated user is the owner of the developer app and the endpoint reads that user's own posts, mentions, bookmarks, followers, following, likes, or related owned resources.

The X marketing landing page and detailed pricing documentation have shown inconsistent content-create figures. This does not affect M01 because all writes are prohibited. Treat the Developer Console as the final price authority before any future write-capability decision.

### Cost controls required for M01

- No automatic recharge.
- Owner approves the initial credit purchase.
- Set a low spending limit in the Developer Console.
- Prefer owned-read endpoints.
- Request the smallest useful result counts and fields.
- Record actual credits consumed in the probe manifest.
- Stop on unexpected billing behavior.

Confidence: High for read and owned-read prices; Moderate for irrelevant write prices due to official-page inconsistency.

Recheck trigger: immediately before buying credits.

## Finding X003 — Relevant Rate Limits

Current official rate-limit documentation lists:

| Endpoint | Per-app limit | Per-user limit |
|---|---:|---:|
| `GET /2/users/me` | — | 75 per 15 minutes |
| `GET /2/users/:id/tweets` | 10,000 per 15 minutes | 900 per 15 minutes |
| `GET /2/users/:id/mentions` | 450 per 15 minutes | 300 per 15 minutes |
| `GET /2/tweets` | 3,500 per 15 minutes | 5,000 per 15 minutes |
| `GET /2/tweets/:id` | 450 per 15 minutes | 900 per 15 minutes |
| `GET /2/tweets/search/recent` | 450 per 15 minutes | 300 per 15 minutes |
| `GET /2/tweets/:id/quote_tweets` | 75 per 15 minutes | 75 per 15 minutes |

Rate limits are not spending permissions. The M01 probe should remain far below them.

The pay-per-use plan is documented with a monthly cap of 2 million Post reads. This cap is irrelevant to the tiny M01 probe but should remain recorded for completeness.

Confidence: High — official X API documentation.

Recheck trigger: before probe execution.

## Finding X004 — Credential and Billing Boundary

The Developer Console is the official place to:

- create the developer app;
- generate credentials;
- purchase credits;
- set spending limits;
- monitor usage and costs.

Credentials are shown once and must not be stored in Markdown, Git, screenshots, probe payloads, manifests, or logs.

Use local environment variables or another approved local secret mechanism. Redact authorization headers and tokens from all evidence.

## M01 Implication

The policy/pricing verification portion of the M01 entry gate is satisfied as of 2026-07-19, subject to a final same-day Developer Console price check before any credit purchase.

This verification does not authorize spending, API writes, database design, or implementation beyond the bounded read-only probe.