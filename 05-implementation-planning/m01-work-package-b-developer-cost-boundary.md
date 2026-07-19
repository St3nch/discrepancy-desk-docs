# M01 Work Package B — Developer and Cost Boundary

## Status

Complete.

Completed on 2026-07-19. Governing completion evidence:

```text
05-implementation-planning/m01-work-package-b-redacted-configuration-record.md
```

This document did not authorize an API request, write permission, or credential storage in either repository. The owner separately approved and completed the exact $5.00 credit purchase and configuration steps recorded in the redacted completion record.

## Purpose

Establish the smallest safe X developer configuration needed for the M01 read-only probe before any credentials are generated or credits are purchased.

## Current Official Boundary Recheck

Rechecked from official X documentation on 2026-07-19.

Current documented model:

- prepaid, credit-based, pay-per-use access;
- no subscription or minimum commitment documented;
- Post read: `$0.005` per returned resource;
- User read: `$0.010` per returned resource;
- qualifying Owned Read: `$0.001` per returned resource;
- same-resource requests are generally deduplicated within a UTC day, but deduplication is a soft guarantee and is not a spending control;
- auto-recharge is optional and configurable in the Developer Console;
- a billing-cycle spending limit can be configured;
- live Developer Console prices remain the final authority before purchase.

Official references:

- X API Pricing;
- X Developer Console;
- X Getting Access;
- X Apps and App Permissions;
- X Authenticated User Quickstart.

## Required Owner-Controlled Console Sequence

### Step 1 — Developer Account

The owner signs into the official X Developer Console using `@DiscrepancyDesk` and:

- reviews and accepts the current Developer Agreement;
- records whether the account is approved immediately or requires review;
- does not paste credentials, recovery codes, payment details, or private console screenshots into project chat or either repository.

### Step 2 — App Creation

Recommended app identity:

```text
App name: Discrepancy Desk Read Probe
Purpose: Owner-operated, read-only inspection of the account's own X API payloads for local data-model research. No posting, replying, liking, following, reposting, direct messaging, recurring polling, or third-party account management.
```

Use a distinct probe app rather than treating a future production app as already designed.

### Step 3 — Permissions

Required app permission:

```text
Read only
```

Do not select:

```text
Read and write
Read, write, and Direct Messages
```

If the console cannot preserve a read-only permission boundary, stop and record the blocker.

### Step 4 — Authentication Choice

Recommended for the bounded M01 probe:

```text
OAuth 1.0a User Context
Read-only Access Token and Access Token Secret for @DiscrepancyDesk
```

Reason:

- `/2/users/me` requires user-context authentication;
- owned mentions and private/owner-only fields may require user context;
- OAuth 1.0a is supported by the admitted endpoints;
- for a single owner-operated probe, console-generated account tokens avoid implementing an OAuth 2.0 callback and PKCE authorization flow before application implementation is admitted;
- the app-level permission remains read-only.

This is an M01 probe choice, not approval of the future application authentication architecture.

OAuth 2.0 Authorization Code with PKCE remains the preferred candidate for a later user-facing application because it supports fine-grained scopes. That decision belongs to a later implementation milestone.

Do not use only an app-only Bearer Token for the full probe because `/2/users/me` requires user-context authentication.

### Step 5 — Credential Handling

Credentials expected for the recommended probe method:

```text
API Key
API Key Secret
Access Token
Access Token Secret
```

Rules:

- never save credentials in either Git repository;
- never save credentials in Markdown, screenshots, manifests, raw responses, command history, or evidence files;
- never paste credentials into project chat;
- save the one-time displayed values directly into an owner-approved password manager or secure vault;
- inject credentials into the probe process through session-scoped environment variables;
- do not create a committed `.env` file;
- if a temporary local secret file is later required, it must live outside both repositories, have a documented owner-only path, and be deleted or securely retained according to an approved procedure;
- rotate/regenerate immediately after suspected exposure;
- evidence records only authentication class and redaction status, never secret values or secret fingerprints.

Recommended environment-variable names for the future controlled procedure:

```text
DD_X_API_KEY
DD_X_API_KEY_SECRET
DD_X_ACCESS_TOKEN
DD_X_ACCESS_TOKEN_SECRET
```

No environment variables are to be created during Work Package B planning.

### Step 6 — Billing Controls

Before purchasing credits:

- inspect and record the live Developer Console unit prices without exposing payment details;
- inspect the minimum purchasable credit amount;
- keep auto-recharge disabled;
- set the billing-cycle spending limit to no more than the owner-approved initial purchase amount;
- record the approved purchase amount and spending limit in the redacted configuration record;
- stop if the console requires broader billing permissions or an unexpectedly large minimum purchase.

Recommended owner decision rule:

```text
Purchase the smallest amount the Developer Console permits, provided it is no more than $10.
```

If the minimum exceeds `$10`, stop for a new owner decision.

No purchase is authorized until the owner explicitly approves the exact amount after seeing the live console minimum and prices.

### Step 7 — Redacted Configuration Evidence

After setup, create a redacted configuration record containing only:

- UTC setup timestamp;
- developer account status;
- app name and app ID if the owner approves recording the non-secret ID;
- selected app permission level;
- selected authentication method;
- credential types generated, without values;
- secret-storage method and owner-controlled location category, without vault entry contents;
- auto-recharge state;
- spending limit;
- credit purchase amount, when separately approved and completed;
- live pricing recheck date;
- any console warnings, review requirements, or deviations;
- confirmation that no write or DM permission is enabled.

The evidence must not contain payment information, account recovery material, tokens, keys, secrets, authorization headers, or unredacted console screenshots.

## Stop Conditions

Stop Work Package B execution when:

- the console requires write or DM permissions;
- the app cannot be configured read-only;
- a credential appears in chat, Git, Markdown, screenshot, or evidence;
- auto-recharge cannot be disabled;
- a spending limit cannot be set as planned;
- the minimum credit purchase exceeds the owner-approved ceiling;
- current prices materially differ from the recorded official rates;
- the developer account or app enters an unexpected review or suspension state;
- the Developer Agreement conflicts with the admitted project use;
- any step would make an API request before Work Package C approval.

## Required Owner Decisions

Before console execution, the owner must confirm:

1. use `Discrepancy Desk Read Probe` as the app name, or provide a replacement;
2. approve OAuth 1.0a User Context with read-only app permission for M01;
3. identify the approved password manager or secure vault for one-time credential storage;
4. approve the purchase rule: smallest permitted amount up to `$10`, with a new stop if the minimum is higher;
5. confirm auto-recharge must remain disabled;
6. confirm the spending limit must not exceed the approved initial purchase.

## Completion Conditions

Work Package B is complete only when:

- the developer account and probe app exist or a documented blocker is recorded;
- read-only permission is verified;
- authentication method is recorded;
- secrets are stored outside repositories using the approved mechanism;
- no secrets appear in evidence;
- live console pricing is rechecked;
- exact credit amount is separately owner-approved before purchase;
- auto-recharge is disabled;
- spending limit is verified;
- redacted configuration evidence is complete;
- no API request has occurred;
- milestone and status documents are updated.

## Explicit Non-Authorization

This package does not authorize:

- purchasing credits before the exact amount is approved;
- calling any X endpoint;
- enabling write or DM permissions;
- creating application code;
- creating probe scripts;
- initializing the main app directory as a Git repository;
- creating a database, schema, migration, or persistence layer;
- recurring collection or polling.
