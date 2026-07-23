# Deferred Humalike Evaluation for Quinton X Replies

## Status

```text
Classification: deferred post-launch product research candidate
Roadmap destination: M12 Reply Desk
Implementation authority: none
Provider admission authority: none
Hermes plugin installation authority: none
Autonomous reply authority: none
Current milestone impact: none
Re-evaluation trigger: project launch plus demonstrated daily-use maturity
```

This note preserves a future experiment idea without adding work to the active roadmap.

The proposed use is narrow:

> Evaluate whether Humalike can improve the conversational naturalness and Quinton Clearance voice fidelity of candidate replies to public comments on the owned X account.

Humalike would be an optional advisory behavior layer inside the future Reply Desk. It would not define facts, own the Quinton persona, approve text, or publish replies.

---

## 1. Why Preserve This Idea

Replies are a distinct editorial surface. A reply may be factually correct yet still feel generic, overlong, overly eager, repetitive, condescending, or recognizably machine-written.

The future Reply Desk already belongs in M12 and is expected to provide contextual candidate replies, exact human approval, manual posting, reconciliation, and metrics. Humalike may eventually be useful for a narrower question:

```text
Given an already-grounded candidate reply,
how should Quinton say it naturally in this particular conversation?
```

Initial reconnaissance on 2026-07-23 found that Humalike presents behavioral APIs around turn-taking, persona, theory of mind, social norms, social observability, social memory, and social signals. It also advertises a Hermes integration. These are potentially relevant to conversational replies, but the product, terms, privacy posture, prices, APIs, and plugin behavior must all be re-verified at evaluation time.

Official starting points:

- `https://humalike.ai/`
- `https://humalike.ai/start`
- `https://docs.humalike.com/`
- `https://github.com/Humalike/hermes-humalike-plugin`

This document is not provider approval and does not claim that Humalike will ultimately outperform a well-designed project-owned prompt and evaluation stack.

---

## 2. Product-Owned Quinton Voice Contract

Before any provider experiment, the project must create a provider-neutral **Quinton Reply Voice Contract** derived from:

- `01-brand/quinton-clearance-persona.md`;
- `01-brand/voice-and-style-guide.md`;
- approved published posts and replies;
- owner-approved positive and negative reply examples;
- the fictional/parody and non-impersonation boundaries.

The contract must remain canonical project material. Humalike may be tested against it but may not redefine it.

The future contract should specify at least:

- concise deadpan bureaucratic voice;
- calm treatment of disagreement;
- distinction between documented material, Desk inference, speculation, and unresolved questions;
- selective use of humor rather than repetitive catchphrases;
- avoidance of generic engagement bait, excessive enthusiasm, hashtags, emojis, and canned assistant phrasing;
- willingness to ask for an original source;
- willingness to acknowledge a useful correction;
- willingness to remain silent when a reply would be unhelpful;
- no claim of real government employment, classified access, or nonexistent records;
- no invented quotations, evidence, corroboration, or certainty.

A reply that feels more human but weakens these boundaries is a failed result.

---

## 3. Intended Future Architecture

The preferred future sequence is:

```text
public comment on an owned X post
→ governed thread and account context
→ factual/context grounding
→ base candidate reply
→ project-owned Quinton Reply Voice Contract
→ optional Humalike advisory assessment or rewrite
→ factual-preservation, repetition, policy, and risk checks
→ human review and edit
→ exact human approval
→ manual X reply
→ posted URL reconciliation and later metrics
```

Humalike may advise on:

- reply versus no-reply;
- conversational length and pacing;
- whether the draft sounds natural in the thread;
- Quinton voice adherence;
- likely reception or unintended condescension;
- excessive repetition across recent replies;
- an optional revised candidate.

Humalike must not decide:

- what the source record establishes;
- whether a claim is true;
- whether speculation may be presented as fact;
- whether a correction is accepted;
- whether a reply is approved;
- whether or when a reply is published.

---

## 4. Post-Launch Maturity Gate

No experiment should begin merely because the provider is available.

The project should reach all of the following first:

1. The Discrepancy Desk has launched and the X workflow is stable in real use.
2. The owner has enough experience with actual comments and manual replies to identify recurring reply problems.
3. The M12 Reply Desk or a separately approved bounded reply-evaluation surface exists.
4. A provider-neutral Quinton Reply Voice Contract and approved example corpus exist.
5. A baseline model or prompt can produce replies without Humalike.
6. The project has a representative, minimized evaluation corpus from public comments on owned posts plus synthetic adversarial examples.
7. X rules, automation boundaries, monetization rules, and API terms have been freshly re-verified.
8. Humalike's API behavior, terms, privacy policy, retention, security posture, pricing, and failure modes have been freshly re-verified.
9. A separate exact provider-admission and spending package has been owner-approved.

There is no fixed calendar date. "Post-launch" means demonstrated operational maturity, not simply the first public post.

---

## 5. Evaluation Design

The first evaluation should be offline and shadow-only. It should publish nothing.

### Candidate conditions

Compare at least:

```text
A — baseline model + Quinton Reply Voice Contract
B — baseline candidate + bounded Humalike advisory pass
```

A third provider or prompt variant may be added only if it does not blur the primary comparison.

### Input boundary

Use only:

- public comments on owned X posts;
- the minimum public thread context needed to understand the exchange;
- synthetic comments designed to test disagreement, jokes, bait, corrections, source requests, speculation, hostility, and ambiguity;
- project-owned persona instructions and approved examples.

Do not send during the initial evaluation:

- private messages;
- unpublished Vault material;
- credentials, cookies, access tokens, or X API secrets;
- private personal profiles;
- unnecessary commenter history;
- internal evidence locations or absolute paths;
- direct database access;
- the full project repository.

Persistent provider social memory should be disabled or excluded from the initial experiment unless separately reviewed and authorized.

### Blind owner scoring

The owner should compare candidates without being told which condition produced them and score:

- Quinton fidelity;
- conversational naturalness;
- factual preservation;
- appropriateness to the specific comment;
- concision;
- non-repetition;
- absence of canned AI language;
- absence of needless hostility or condescension;
- correct reply/no-reply judgment;
- amount of editing required;
- confidence in approving the exact text.

### Operational measurements

Track separately:

- candidate acceptance rate;
- edit distance from candidate to approved reply;
- time to approval;
- repeated-language warnings;
- factual or policy corrections required;
- cost and latency per evaluated reply;
- provider failures and fail-closed behavior;
- later engagement metrics for manually approved and manually posted replies.

Engagement must not be the sole optimization target. A reply that drives reactions by becoming inflammatory, misleading, repetitive, or out of character fails the project even if its raw metrics rise.

---

## 6. Provider and Hermes Boundary

The preferred initial experiment is a narrow, directly governed API integration rather than installation of the broad Hermes plugin.

Humalike must receive no:

- X credentials;
- posting authority;
- approval authority;
- direct SQLite or Vault access;
- unrestricted conversation monitoring;
- permission to rewrite canonical Quinton doctrine;
- permission to modify Hermes-wide settings;
- authority to create persistent social memory by default.

Any later consideration of the Hermes plugin requires a separate source review, configuration-diff review, data-flow review, fail-open/fail-closed analysis, memory review, uninstall/recovery proof, and explicit owner authorization. This deferred idea does not authorize plugin installation.

Provider outage or refusal must fail closed to the ordinary baseline Reply Desk workflow. The Desk must remain capable of drafting and approving replies without Humalike.

---

## 7. Possible Admission Standard

Humalike should be considered for bounded production use only if the evaluation demonstrates a meaningful improvement over the baseline in naturalness and Quinton fidelity while all of the following remain true:

- no material factual mutation;
- no weakening of parody, provenance, or non-fabrication rules;
- no increase in repetitive engagement behavior;
- human approval remains exact and mandatory;
- public-comment data is minimized and governed;
- provider terms and privacy boundaries are acceptable;
- cost and latency fit a hard owner-approved budget;
- requests and responses can be recorded as advisory provider receipts;
- failures are safe and do not create partial publication authority;
- the integration remains replaceable and provider-neutral at the business-operation layer.

Reject or defer the provider if improvement is marginal, inconsistent, expensive, difficult to audit, dependent on persistent third-party memory, or achieved by making Quinton louder, more provocative, or less accurate.

---

## 8. Explicit Non-Authority

This note does not authorize:

```text
Humalike account creation or spending
Humalike API calls
Hermes plugin installation
new credentials or secret storage
automatic comment monitoring
automatic reply generation after every comment
autonomous X replies
bulk replies
private-message analysis
persistent social memory
Quinton doctrine changes
M12 implementation
roadmap resequencing
```

The idea remains a deferred M12 Reply Desk evaluation candidate until a later exact owner-approved research and provider-admission package exists.
