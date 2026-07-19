# Truth Social Automation Boundaries

Distilled from `06-research/truth-social-platform-research.md` (researched July 2026). Companion to `automation-boundaries.md` (X). Re-verify periodically.

## Allowed

- drafting
- summarizing
- source extraction from manually captured content
- claim classification
- context retrieval
- manual checklist generation
- metrics analysis (on manually recorded metrics)
- preparing manual-ready post text for the human to paste or the share composer to prefill

## Not Allowed

- any automated read of Truth Social pages, endpoints, or search results
- any automated post, reply, like, follow, repost, or DM
- calling Truth Social's internal/undocumented API endpoints directly, even read-only
- session-cookie reuse or reuse of embedded first-party OAuth client credentials
- browser automation or headless collection against Truth Social
- keyword-triggered replies
- engagement-pod behavior
- coordinated multi-account amplification (including coordination with the X account)
- treating a reachable endpoint as permission to use it

## Manual Posting First — Truth Social Specifics

Approved content becomes manual-ready.

The extension may prefill Truth Social's official share composer with human-approved text and a public URL. The human still reviews and presses post inside Truth Social's own UI.

The user then records the published URL, platform post ID, and timestamp.

## No General-Purpose Developer API Available to This Project

Unlike X, there is currently no paid or free general-purpose developer read API available to this project. Truth API exists but is an institutional B2B licensing product, not something this project can access. "Manual capture first" is not a temporary phase for Truth Social — it is the only sanctioned path until TMTG publishes a general-purpose developer program or grants written permission.

## Future Write or Read API

Any future Truth Social API integration requires:

1. A published, official developer program or an explicit written grant from TMTG.
2. A separate explicit approval decision.
3. A new safety design reviewed against this doctrine.

It is not part of early MVP and no bounded timeline currently exists for it, since no program to admit to exists yet.
