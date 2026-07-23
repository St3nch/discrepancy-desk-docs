# M13 Governed Discrepancy Detection — Planning Direction

## Status

```text
Document type: owner-accepted planning direction
Owner decision: D043
Application authority: NONE
Migration authority: NONE
Schema authority: NONE
Phase 4/5 implementation authority: NONE
M13 entry authority: NONE
Detector admission authority: NONE
Documentation baseline before this package: 3279bfa
Application baseline: 6a8082253a52a601291efaf3ed85ee411b04be20
Latest completed decision before this package: D042
Review history: multi-model planning critique
Independent review claim: none
```

This document preserves the future governed discrepancy-detection direction for **No Coincidences**.
It does not authorize implementation. It exists so active M06 development can continue without losing
requirements that Phase 4, Phase 5, M12, and M13 will later need.

No application repository work begins from this document. No migration, dependency, schema, route,
worker, detector, background task, or UI change is authorized.

---

# 1. Required Reading

Before preparing any implementation package derived from this direction, read:

```text
LLM_MAP.md
STATUS.md
00-doctrine/editorial-anomaly-archive-direction.md
00-doctrine/human-approval-policy.md
02-product/no-coincidences.md
05-implementation-planning/implementation-roadmap.md
05-implementation-planning/m06a-local-manual-vault-canonical-plan.md
05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md
05-implementation-planning/milestone-12-rich-editorial-workspaces.md
05-implementation-planning/milestone-13-no-coincidences.md
05-implementation-planning/hammer-test-strategy.md
99-decisions/decision-log.md
```

If any live governing document conflicts with this planning direction, stop and return to owner review.
Repository truth controls over summaries, chat memory, and copied proposal text.

---

# 2. Product Inversion

The useful idea adapted from the external “LLM Wiki” lint pattern is narrow:

- in a normal wiki, contradictions, stale claims, missing links, and gaps are maintenance defects;
- in The Discrepancy Desk, the same governed comparisons may be editorial leads;
- therefore lint-like analysis is candidate generation, not archive repair;
- the archive remains authoritative and unchanged;
- a detector drafts observations and candidates only;
- a human disposes them;
- no detector declares conspiracy, causation, intent, deception, or accepted truth.

The controlling line remains:

> Crumbs open drawers. They do not close cases.

---

# 3. Placement and Sequencing

This capability does **not** create a new milestone.

```text
M06 Phase 4  supplies governed searchable document/element coverage
M06 Phase 5  supplies stable assertions, entities, evidence, and comparison dimensions
M12          may later supply a rich competing-explanations investigation workspace
M13          owns governed pattern candidates and detector review
M14          may later supply semantic similarity only after its separate gate
```

Two tiers are reserved:

## Tier 1 — deterministic shadow pilot

A separately authorized Phase 4 companion may run over governed document versions, elements, regions,
search coverage, and exact metadata. It produces **evaluation-only observations and candidates**.
It has no assertion-level contradiction authority and no publication or dossier authority.

Its purpose is to measure whether the feature is worth further investment.

## Tier 2 — assertion-level M13

After Phase 5 assertion types **and comparison dimensions** are stable, M13 may compare typed assertions,
source lineage, entities, expectations, and human review history.

Full M13 remains post-M06 and separately gated.

---

# 4. Required Separation of Layers

The design must preserve six distinct layers:

```text
1. Corpus manifest
   What exact material was eligible, searched, excluded, or missing?

2. Deterministic observation
   What exact values, phrases, dates, links, or absences were observed?

3. Pattern candidate
   Why might the observation deserve human attention?

4. Human epistemic disposition
   What does the evidence support?

5. Human editorial disposition
   Is the item useful for research or content?

6. Detector calibration
   How useful or noisy has this detector historically been?
```

The following equivalences are prohibited:

```text
observation != interpretation
interpretation != accepted truth
truth/support assessment != editorial value
editorial value != publication approval
detector precision != evidence for one candidate
```

---

# 5. Governed Corpus and Coverage Manifest

Every run and candidate must bind a governed corpus snapshot. A count without a denominator is not an
adequate finding.

The future contract must include at least:

```text
vault identity
corpus snapshot ID and hash
ruleset/detector version
time of corpus cutoff outside deterministic identity where appropriate
eligible artifact/document/element counts
searched artifact/document/element counts
rights/policy exclusions
current-version and withdrawal filtering
source families and publishers represented
date range represented
known acquisition gaps
known expected-but-missing collections
failed or unavailable inputs
```

A candidate must be able to say both:

```text
observed count: 6
eligible searched denominator: 74
```

Negative-space findings require stronger coverage proof than presence findings.

The corpus manifest is not a truth claim. It is the exact boundary of what the detector inspected.

---

# 6. Assertion Comparison Signature

Phase 5 planning must not stop at an optional date field. Assertions intended for governed comparison
must have stable comparison dimensions.

The future comparison signature must be able to represent, as applicable:

```text
subject/entity
predicate or measure
value and value type
unit
asserted period start/end or point in time
geographic scope
population or included class
operational status
counting method or definition
qualifiers and uncertainty
source certainty/attribution
```

The detector must distinguish at least:

```text
hard contradiction
potential contradiction
scope mismatch
definition mismatch
temporal mismatch
unit mismatch
insufficient comparison dimensions
```

`insufficient comparison dimensions` is a successful fail-closed outcome. The detector must not force
incomparable statements into a contradiction class.

The future M13 entry gate therefore requires:

```text
assertion types stable
assertion comparison dimensions stable
```

---

# 7. Deterministic Tier 1 Detection Classes

The earliest separately authorized pilot should remain small and deterministic.

Initial candidate classes:

```text
numeric_divergence
same_publisher_drift
orphan_or_unmined_material
locator_or_current-version_integrity
common_origin where exact lineage proves it
negative_space_annual_continuation
negative_space_page_or_document_continuity
```

Tier 1 must not require:

- embeddings;
- paraphrase detection;
- LLM judgment;
- graph infrastructure;
- background scheduling;
- network retrieval;
- first-class event objects;
- automatic threshold tuning.

Tier 1 answers:

> What is the useful-findings-per-hundred-artifacts rate, and is further governance worth the cost?

---

# 8. Governed Expectation Contracts

Absence is meaningful only when an explicit expectation and adequate coverage exist.

The first permitted expectation types should be limited to:

```text
annual_or_periodic_continuation
page_or_document_continuity
```

A future expectation record must bind:

```text
expectation ID and version
expectation type
subject or series identity
basis records and exact locators
expected interval or sequence
expected presence condition
coverage requirements
known exceptions
human approval where required
state and successor lineage
```

Example:

```text
reports exist annually from FY2012 through FY2019
FY2020 is absent
FY2021 and FY2022 are present
all known collection locations for the series were searched
no closure or publication-gap notice is recorded
```

The output is not “the agency concealed FY2020.” It is:

> A governed continuation expectation predicted a record for FY2020; the covered corpus did not contain it.

Later expectation types require separate planning and authorization.

---

# 9. Source Independence and Claim Genealogy

Source independence remains a hard unsolved problem.

The early implementation vocabulary may reserve:

```text
common_origin
claim_mutation
numeric_drift
certainty_inflation
citation_laundering
attribution_loss
recursive_citation
```

Only `common_origin` is proposed for early deterministic implementation, and only where exact source or
syndication lineage supports it.

A common origin is not merely a reason to lower another candidate’s score. It may emit its own candidate:

> Six apparently independent stories trace to one wire report.

The remaining mutation-chain classes require a separate later gate because paraphrase alignment and
semantic similarity can pull in M14-class infrastructure and increase false positives.

---

# 10. One Generic Candidate Lifecycle

Tier 1 and M13 must not create parallel lifecycle systems.

A future generic candidate lifecycle must support:

```text
under_test detector output
undispositioned candidate
human epistemic disposition
human editorial disposition
parked or rejected state
promotion into a separately governed research work item
immutable successors when evidence or rules change
suspension or invalidation after detector defects
```

A candidate does not auto-promote into:

- accepted truth;
- a dossier assertion;
- an editorial-use ruling;
- a draft;
- a publication work item;
- approved post bytes.

Every transition that creates authority remains human-controlled and separately governed.

---

# 11. Candidate Fingerprint and Successor Delta

A durable candidate fingerprint should bind:

```text
detector ID and version
ruleset/configuration hash
corpus snapshot ID/hash
normalized subject/comparison signature
supporting canonical locator IDs
expectation contract ID/version when applicable
```

Identical evidence under the same detector and corpus must not generate endless duplicates.

New evidence, a new ruleset, corrected source material, or a changed expectation creates an explainable
successor rather than mutating the predecessor.

Every successor must record:

```text
predecessor candidate
new evidence added
removed, corrected, or withdrawn evidence
detector/ruleset change
comparison-signature change
score-component change
previous dispositions
why the previous disposition no longer controls
human reopening or disposition actor when applicable
```

This produces a governed biography of a lead.

---

# 12. Two-Axis Human Disposition

The original review labels mix epistemic judgment and content value. The future schema must separate them.

## Epistemic disposition

Indicative vocabulary:

```text
unreviewed
explained
likely_coincidence
source_dependence
scope_mismatch
definition_mismatch
unresolved_discrepancy
corroborated_anomaly
insufficient_evidence
invalid_detector_output
```

## Editorial disposition

Indicative vocabulary:

```text
no_editorial_use
park
needs_research
reply_candidate
short_post_candidate
thread_candidate
article_candidate
brain_soup_potential
```

A candidate may honestly be both:

```text
epistemic: likely_coincidence
editorial: excellent folklore explainer
```

The first epistemic review should hide content-potential scores, engagement predictions, audience metrics,
and editorial labels. Editorial review follows the epistemic decision.

This is a mechanical extension of the metrics firewall.

---

# 13. Detector Admission

M13 authority must not automatically authorize every detector.

Each detector/version needs a lighter analogue of parser admission:

```text
detector ID and version
candidate class
required canonical inputs
required comparison dimensions
required corpus coverage
ruleset/configuration hash
maximum candidate volume
known false-positive classes
non-detection expectations
failure behavior
state: under_test | admitted | suspended | retired
```

A changed ruleset or threshold returns that detector version to `under_test` unless an exact successor
contract says otherwise.

A noisy phrase detector may be suspended without disabling numeric divergence or common-origin detection.

Detector admission grants candidate-generation authority only. It grants no truth, editorial, or publication
authority.

---

# 14. Structured Detector Calibration

Human dispositions create a valuable detector-performance dataset. Capture it from the first pilot, but keep
it advisory-only.

Useful metrics include:

```text
precision by detector and candidate class
explained/likely-coincidence/source-dependence rates
promotion-to-research rate
invalid-output rate
duplicate/noise rate
reopen rate after new evidence
median review time
reviewer-hours per useful candidate
findings per hundred artifacts
useful findings per hundred artifacts
false-positive reason distribution
```

Calibration data must not silently change:

- evidence strength;
- truth/support assessment;
- candidate disposition;
- detector thresholds;
- publication priority.

Automated threshold tuning requires a separate future gate.

---

# 15. Bounded Tier 1 Pilot and Kill Criteria

A Tier 1 implementation package must define a bounded experiment before code is authorized.

The pilot contract must specify:

```text
fixed corpus snapshot and maximum corpus size
exact admitted detector versions
maximum candidates emitted
maximum reviewer-hours
candidate deduplication rules
noise/duplicate ceiling
minimum useful-candidate yield
maximum detector-error rate
stop conditions
retire/suspend criteria
```

The experiment succeeds if it produces trustworthy information about detector value. It does not need to
justify continued implementation.

A valid result may be:

> The detector generated 170 candidates, 164 were trivial reuse, and review cost exceeded editorial value.
> Suspend the detector.

No sunk-cost rule is permitted.

---

# 16. Competing Explanations

Candidate records may reserve a future relation to competing explanations, supporting evidence,
contradicting evidence, unresolved questions, and next-evidence-needed records.

The rich workbench UI belongs to M12 — Rich Editorial Workspaces — and does not enter M13 automatically.

M13 may later expose a minimal evidence-and-disposition view. A full investigation-management workbench
requires demonstrated operator need and a separately admitted M12 subpackage.

---

# 17. Public Dismissal Ledger

The complete internal finding and disposition history should remain immutable and governed.

Public disclosure, if later accepted, should begin with:

```text
aggregate statistics
selected public dismissal case studies
explicitly cleared records only
```

A raw public ledger is deferred because it may expose unpublished research, private material, policy-limited
sources, or sensitive locators.

The public dismissal ledger is an editorial decision, not an automatic detector output.

---

# 18. Explicitly Deferred Features

The following require separately named future gates:

```text
permutation or null-model rarity estimates
paraphrase-based mutation-chain detection
semantic claim genealogy
competing-explanations workbench UI
automated detector threshold tuning
broad public dismissal-ledger publication
embeddings, Qdrant, graph, or GraphRAG assistance
background, scheduled, or watcher execution
cross-Vault detection or corpus pooling
```

## Why null models are deferred

A small, hand-curated, deliberately unusual corpus does not provide a trustworthy null distribution.
A statistical rarity number can create false confidence while depending on unaudited assumptions.
Any later null-model package requires a minimum-corpus-size rule, a defensible sampling model, and an
explicit statement that rarity is not causation or conspiracy probability.

---

# 19. Indicative Data Shape — Not a Schema Authorization

```text
corpus_snapshots
  immutable governed coverage boundary

detector_versions
  immutable detector contract and admission state

expectation_versions
  immutable governed expectation contracts

candidate_runs
  immutable run, corpus, detector, commit/ruleset binding

pattern_candidates
  immutable candidate fingerprint, class, observations, locators, coverage

candidate_successors
  append-only predecessor/successor and exact delta

epistemic_dispositions
  human-only append-only decisions with reason

editorial_dispositions
  human-only append-only editorial fate

detector_calibration_snapshots
  derived/advisory performance measurements
```

This is a reasoning aid only. It does not authorize these names, tables, columns, migrations, or physical
storage choices.

---

# 20. Hard Rules

1. Candidates only. No conspiracy, causation, intent, deception, or accepted-truth declaration.
2. No auto-promotion into assertions, dossiers, work items, drafts, or publication.
3. Every observation and candidate traces to exact canonical records and locators.
4. Source statements and Desk inference remain distinct.
5. Detector failure emits no candidate and records a safe failure receipt.
6. Deliberate non-detection must be testable.
7. Fabricated IDs, missing locators, stale document versions, or cross-Vault material fail closed.
8. Tier 1 remains deterministic with no LLM in the path.
9. M06-A remains human-triggered; no background or scheduled execution is implied.
10. Detector calibration is advisory and cannot mutate truth or editorial authority.
11. Metrics and content-potential information are hidden during the first epistemic review.
12. Every detector/version is separately admitted, suspended, or retired.
13. Corpus coverage and known gaps are visible with every candidate.
14. Negative-space findings require explicit expectation and coverage contracts.
15. Prefer under-detection to over-detection.

---

# 21. Required Adversarial Planning Areas

Any future exact implementation package must plan tests for at least:

```text
wrong Vault or cross-Vault candidate contamination
fabricated or stale locator IDs
withdrawn/corrected document versions
corpus manifest tamper
coverage denominator mismatch
policy/rights exclusion leakage
comparison of incompatible units/scopes/periods
forced contradiction despite insufficient dimensions
expectation without adequate coverage
negative-space false positive from missing ingestion
common-origin false independence
candidate fingerprint collision or duplicate storm
successor without exact delta
reopening from identical evidence
human epistemic/editorial disposition bypass
content-potential leakage into blind epistemic review
detector error producing a candidate
non-detection regression
candidate-volume budget overrun
detector suspension bypass
automated threshold mutation
candidate auto-promotion into downstream authority
```

---

# 22. M06 Phase 4 and Phase 5 Dependencies

This direction creates two planning dependencies, not implementation authority.

## Before the Phase 4 exact package

The package must decide whether to reserve a governed corpus/coverage snapshot contract sufficient for
search, pilot evaluation, and later M13 use. It must not implement Tier 1 unless that exact pilot is separately
authorized.

## Before the Phase 5 exact package

The package must define stable assertion comparison dimensions, including time, unit, scope, population,
definition/counting method, and uncertainty where applicable.

M06-D10 still defers first-class event and chronology objects. Comparison dimensions do not create a general
event model.

Neither dependency opens M13 or authorizes detector tables.

---

# 23. Future M13 Entry Gate

M13 may not begin until all applicable conditions are satisfied:

```text
sufficient governed corpus exists
M06 completion and applicable owner closure exist
assertion types are stable
assertion comparison dimensions are stable
governed corpus/coverage manifest contract is stable
single generic candidate/successor lifecycle is approved
two-axis human disposition taxonomy is approved
detector admission contract is approved
structured advisory calibration contract is approved
source-independence/common-origin treatment is approved
bounded Tier 1 results justify further work where applicable
adversarial detector plan is approved
required architecture review is complete
```

M13 implementation authority remains a separate owner decision.

---

# 24. Work Required Now

The work required now is documentation only:

```text
preserve this direction in the repository
update No Coincidences product and milestone documents
update the implementation roadmap and feature map references
record M06 Phase 4/5 planning dependencies
record the M12 investigation-workbench handoff
update STATUS and LLM_MAP
record D043
```

No current application change is necessary or authorized.

After this documentation package, active development returns to the M06-A parser sequence. The next bounded
action is preparation of the exact `m06a.vtt.v1` under-test candidate package for owner review. VTT implementation is not authorized.

---

# 25. Review History and Claims

```text
source proposal: external Karpathy lint pattern adapted by Claude discussion
planning critique: Claude and Project Steward
owner direction: preserve and integrate into roadmap
review description: multi-model planning critique
independent review claim: none
```

No model critique creates authority. D043 accepts only this documentation and roadmap direction.

---

# 26. Authority Boundary

D043 authorizes this planning direction and synchronized documentation updates only.

D043 does not authorize:

- application changes;
- migrations, dependencies, tables, or schema;
- Tier 1 implementation;
- detector admission;
- Phase 4 or Phase 5 implementation;
- M13 entry or implementation;
- M12 workbench implementation;
- VTT or JSON implementation;
- Phase 3B;
- providers, agents, live LLM integration, Qdrant, graph work, purge, or publication automation.

Every future implementation package requires a separate exact owner decision.
