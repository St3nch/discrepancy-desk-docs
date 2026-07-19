# Milestone 01 — X Identity, Policy, and Read-Only API Probe

## Status

Active.

## Goal

Finish X-facing identity and policy readiness, then inspect a bounded set of real X API payloads before persistence design.

## Required Reading

- `LLM_MAP.md`
- `STATUS.md`
- `05-implementation-planning/implementation-roadmap.md`
- `01-brand/brand-identity.md`
- `01-brand/quinton-clearance-persona.md`
- `01-brand/voice-and-style-guide.md`
- `01-brand/profile-bio-options.md`
- `01-brand/avatar-banner-direction.md`
- `04-platform-rules/x-account-rules.md`
- `04-platform-rules/x-api-probe-plan.md`
- `04-platform-rules/automation-boundaries.md`
- `04-platform-rules/platform-risk-register.md`
- `06-research/x-policy-api-verification-2026-07-19.md`
- M00 completion record

## Entry Gate

- M00 complete.
- Current X policy, pricing, limits, and billing model verified from official sources.
- Owner has approved M01 as the active milestone.

Credit purchase and API execution remain separately gated by explicit owner approval.

## Work Package A — Account and Identity Readiness

- apply the conservative PCF/commentary posture;
- set or confirm `Commentary | The Discrepancy Desk` as the display name;
- enable the Commentary PCF label;
- finalize bio and non-affiliation language;
- finalize avatar and banner against anti-impersonation rules;
- prepare and approve a pinned-post draft;
- record final profile settings and policy recheck date.

No public profile change should be claimed complete without owner confirmation or evidence.

## Work Package B — Developer and Cost Boundary

- create or verify the X developer account, Project, and App;
- use minimum read permissions;
- define local secret handling;
- verify live Developer Console prices immediately before purchase;
- owner approves the credit amount;
- disable auto-recharge and set a low spending limit;
- record the bounded endpoint and cost plan.

Do not store credentials in either repository.

## Work Package C — Controlled Read-Only Probe

Execute the packages admitted by `04-platform-rules/x-api-probe-plan.md`:

- identity;
- owned posts;
- mentions;
- direct post lookup;
- conversation and quote context only when justified.

Preserve raw evidence, request context, manifest entries, hashes, rate-limit headers, costs, empty responses, and errors.

## Work Package D — Analysis and M02 Handoff

Produce:

- payload-field inventory;
- relationship inventory;
- access and cost observations;
- retention and privacy notes;
- missing-field and unknowns report;
- schema implications;
- candidate M02 questions;
- explicit non-conclusions.

## Scope

- X profile compliance and identity readiness;
- read-only developer setup;
- bounded endpoint list;
- raw JSON evidence;
- probe manifest and hashes;
- payload analysis;
- M02 handoff.

## Out of Scope

- X write API;
- API posting or replying;
- automated platform actions;
- recurring collection;
- database schema or migrations;
- dashboard implementation;
- Chrome extension;
- Truth Social probe;
- production analytics.

## Required Evidence

- official-policy verification record;
- final profile checklist and owner confirmation;
- redacted developer-app configuration record;
- owner-approved spend ceiling;
- immutable raw responses;
- request context with secrets redacted;
- `probe_manifest.json`;
- timestamps, endpoint/version, rate-limit, and cost information;
- SHA-256 hashes;
- payload-field inventory;
- schema-implications document;
- exact controlled procedure or commands used.

## Exit Gate

- account posture is documented and owner-confirmed;
- the official pricing/policy recheck is current;
- raw samples and manifest exist;
- evidence contains no exposed secrets;
- payload fields, relationships, and missing fields are inventoried;
- actual probe cost is recorded;
- schema implications and unknowns are documented;
- no write capability was exercised;
- M02 input is bounded;
- milestone and `STATUS.md` are updated.

## Completion Record

Not yet complete.

On completion, record date, commit, profile evidence, API evidence paths, costs, official-doc recheck date, deviations, defects, owner decisions, and next milestone.