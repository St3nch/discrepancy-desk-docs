# AC-02 — M06 Research Review Correction Disposition

## Status

Corrections implemented in the synthesis package on 2026-07-20; closure pending independent synthesis review and explicit owner acceptance.

Independent review verdict:

```text
RESEARCH PACKAGE READY WITH CORRECTIONS
```

No critical or high findings were reported. Owner option review may proceed while the bounded documentation corrections are applied. The owner resolved all before-synthesis decisions. Architecture synthesis is prepared and now awaits independent review. AC-02 remains open until that review is dispositioned and the owner accepts the synthesis.

## Source review

```text
08-audits/claude-m06-research-program-independent-review.md
```

## Accepted correction set

### AC02-01 — Admission stage and permitted methods

Disposition: accepted.

Required result:

```text
admission_stage
permitted_methods[]
```

Automation maturity and permitted acquisition methods must be separate dimensions. `retired`, `revoked`, and `prohibited` require distinct meanings.

### AC02-02 — Canonical object terminology

Disposition: accepted.

Required result: one terminology map for source, source item, occurrence, observation, acquisition, original artifact, document version, element/region, assertion, accepted conclusion, context run, evidence packet, and chunk.

### AC02-03 — Layered assertion vocabulary

Disposition: accepted.

Required result:

```text
internal epistemic type
→ review state
→ LLM output status
→ account/public claim label
```

### AC02-04 — Physical Vault wording

Disposition: accepted.

Required result: D024 is an accepted isolation direction favoring separate physical Vaults. The complete M06 Vault architecture remains subject to owner decision and synthesis.

### AC02-05 — Missing adversarial rules

Disposition: accepted.

Required result:

- fail closed when a known contradiction would be truncated away from its claim;
- verify every output citation against an exact supplied document version and locator;
- define minimum import contents and restrictions when the original artifact cannot transfer.

### AC02-06 — Editorial-use and publication approval relationship

Disposition: accepted as a required synthesis question.

The synthesis must prevent editorial-use rulings from becoming an ungoverned second approval system.

### AC02-07 — Account-scoped observability

Disposition: accepted.

Logs, caches, traces, temporary files, provider receipts, and diagnostic output must preserve account isolation and may not leak one account's source universe to another.

### AC02-08 — STATUS reconciliation

Disposition: accepted.

STATUS must identify completed research and the current audit/correction gate accurately.

### AC02-09 — Unified evidence packet model

Disposition: accepted.

YouTube/video-specific evidence fields become source-specific extensions inside the general context-run/evidence-packet contract, not a parallel authority model.

## Owner decision recorded now

### OD-M06-01 — Split network retrieval from the local Vault foundation

Owner ruling: accepted 2026-07-20.

```text
M06-A — Local Manual Vault
M06-B — Bounded Static Webpage Retrieval
```

M06-A must remain local-only. It may define source locators and future connector contracts but must not perform network retrieval.

M06-B may be planned only after M06-A proves its authority model, artifact preservation, normalization, lexical retrieval, backup/restore, and adversarial boundaries. M06-B introduces the first new network/SSRF surface and therefore requires a separate plan, entry gate, hammer matrix, owner approval, and implementation authorization.

This ruling does not approve either package for implementation.

## Closure conditions

AC-02 may close only when:

1. the correction specification is committed;
2. affected research/status wording is reconciled;
3. the owner resolves the before-synthesis decision set;
4. the M06 architecture synthesis incorporates the accepted corrections;
5. an independent review confirms the synthesis is coherent;
6. the owner explicitly accepts the synthesis.

## Implementation record

The accepted corrections are incorporated into:

- `05-implementation-planning/m06-architecture-synthesis.md`;
- `05-implementation-planning/milestone-06-records-dossiers-anomaly-vault.md`;
- `05-implementation-planning/implementation-roadmap.md`;
- `LLM_MAP.md`;
- `STATUS.md`.

## Current block

AC-02 is not closed until independent synthesis review and explicit owner acceptance. No M06-A or M06-B implementation authority exists.
