# M01 Work Package B — Redacted Configuration Record

## Status

Complete.

Completion date: 2026-07-19.

This record intentionally excludes all secret values, payment details, recovery material, and unredacted credential screenshots.

## Developer Account

```text
Account: @DiscrepancyDesk
Developer access: active
Billing model: Pay Per Use
```

The owner reviewed and accepted the current X Developer Agreement and policy during developer-account enrollment.

## App Configuration

```text
App name: Discrepancy Desk Read Probe
App ID: 33215850
Environment: Development
Status: Active
Permission: Read only
Authentication used for M01: OAuth 1.0a User Context
Authenticated account: @DiscrepancyDesk
```

Configured app description:

```text
Read-only owner-operated X API probe for The Discrepancy Desk. Used to inspect the account's own API payloads for local data-model research.
```

Configured URLs:

```text
Website URL: https://discrepancydesk.com
Callback URI / Redirect URL: http://127.0.0.1:8080/callback
```

The callback value satisfies the console configuration requirement. No callback service, OAuth flow, application code, or API request was implemented or executed during Work Package B.

## Credential Boundary

Credential types generated and retained:

```text
OAuth 1.0a Consumer Key
OAuth 1.0a Consumer Key Secret
OAuth 1.0a Access Token
OAuth 1.0a Access Token Secret
```

Secret-storage method:

```text
Owner-controlled Bitwarden vault entry
```

Confirmed boundary:

- all four OAuth 1.0a values were saved accurately in Bitwarden;
- no OAuth 2.0 credentials were retained for this probe;
- no App-Only Bearer Token was generated for this probe;
- no secret value was stored in Git, Markdown, evidence files, screenshots retained in the repository, or project records;
- no secret value is reproduced in this record;
- regeneration replaced any earlier incomplete or mislabeled credential set.

## Billing Controls

```text
Credits purchased: $5.00
Remaining balance at setup: $5.00
Current spend at setup: $0.00
Billing-cycle spend cap: $5.00
Auto-recharge: unavailable/off
Payment-driven automatic recharge: not enabled
```

The owner explicitly purchased the $5.00 credit balance and set the billing-cycle spending cap to $5.00.

No additional purchase is authorized for M01 without a new owner decision.

## Permission and Feature Exclusions

Not enabled or admitted:

- write permission;
- Direct Message permission;
- automated posting;
- automated replies;
- likes, follows, reposts, or bookmarks;
- App-Only authentication for the controlled probe;
- OAuth 2.0 access-token or refresh-token use;
- webhooks;
- event subscriptions;
- streaming rules;
- recurring polling;
- third-party account management.

## Evidence Basis

Owner-supplied live X Developer Console screenshots and direct owner confirmations established:

- active developer access;
- purchased credit balance;
- $5.00 spending cap;
- auto-recharge off/unavailable;
- app identity and App ID;
- read-only OAuth 1.0 access level;
- OAuth 1.0 access-token generation for `@DiscrepancyDesk`;
- secure storage of the four required values in Bitwarden.

Screenshots containing visible credential material are not copied into either repository.

## Work Package B Completion Result

All Work Package B completion conditions are satisfied:

- developer account active;
- dedicated probe app active;
- read-only permission verified;
- OAuth 1.0a User Context selected;
- four required credentials stored outside the repositories in Bitwarden;
- no secret values recorded in project evidence;
- $5.00 purchase recorded;
- $5.00 spending cap verified;
- auto-recharge disabled/unavailable;
- no API request executed;
- no application or database implementation started.

Next controlled package:

```text
M01 Work Package C — Controlled Read-Only Probe
```

Work Package C requires a separately reviewed execution procedure before any endpoint is called.
