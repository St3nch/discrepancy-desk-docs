# Editorial Control Room and Full Product Roadmap Ruling

## Status

Accepted by the owner on 2026-07-20.

## Source Material

- `06-research/The Discrepancy Desk Editorial Control Room - Product Definition.md`
- independent Hermes architecture review supplied by the owner on 2026-07-20
- live application and documentation repository truth

The research is retained as research. This ruling extracts the decisions recommended for promotion into canonical planning and deliberately rejects wholesale adoption of every researched feature into M04.

## Accepted Product Direction

### Records-first editorial operation

```text
The Desk does not chase general news.
It maintains a planned records-driven editorial pipeline.
When an approved authoritative source releases relevant material,
the Desk can enter a rapid human-cleared filing workflow.
```

An authoritative source is authoritative for what that institution published, not automatically for the ultimate truth of every claim it contains.

### Editorial lanes

- **The Archive** — evergreen and historical filings.
- **The Docket** — active tracked subjects and ongoing work.
- **The Flash Release** — time-sensitive work triggered by a relevant official release.

A lane is a work-item property/view. It does not replace lifecycle state.

### Scheduling doctrine

- Active scheduling uses a rolling maximum horizon of 90 days.
- Ideas, dossiers, and evergreen material may remain indefinitely in the Unscheduled Reserve.
- The operator can schedule, move earlier/later within the horizon, and return work to Reserve.
- Schedule metadata is separate from approved content bytes.
- A schedule-only change does not invalidate exact text approval.
- Editing approved content creates/supersedes a revision and requires renewed approval.
- Empty schedule slots are acceptable; the system must not manufacture weak filler.

### Command Center doctrine

The home surface must answer within five seconds:

- what needs attention;
- what is scheduled now;
- what awaits review;
- what is approved but unscheduled;
- what is manual-ready;
- what needs reconciliation or correction;
- what is blocked;
- where meaningful schedule gaps exist.

### Product surfaces

- Tauri: sole supported human operator interface and product-grade daily workspace.
- FastAPI loopback backend: governed local API/service host required by Tauri and the packaged sidecar.
- Legacy FastAPI/Jinja pages: frozen historical regression scaffolding only; no new product features or browser/Tauri feature-parity requirement.
- Connected LLMs/agents: candidate-producing assistants behind governed business operations.
- SQLite/service layer: durable operational truth and authority enforcement.
- Hermes: possible replaceable sibling runtime, never trusted memory or control plane.

D031 records this clarified product-surface boundary. Existing M03/M04 browser routes may remain for inherited regression coverage, but all new operator capability is implemented through Tauri consuming governed loopback API operations.

### LLM operating model

A connected LLM receives a bounded task/context packet and may propose candidate research, drafts, comparisons, classifications, schedule suggestions, or metric hypotheses. It cannot approve, publish, alter accepted truth, access SQLite directly, or rely on hidden memory as canonical truth.

### Asset/evidence distinction

Assets are reusable/versioned creative material. Evidence is immutable, provenance-bound proof. A file may play both roles, but an asset transformation may never overwrite historical evidence or publication bindings.

## Full Feature Preservation

The roadmap assigns explicit destinations and gates for:

- Command Center;
- scheduler and Reserve;
- WIP, Ready-to-Post, and Need-a-Post;
- Tauri;
- dossiers and Anomaly Vault;
- capture helper;
- governed agent API/MCP;
- Hermes Release Watch;
- Truth Social;
- metrics and monetization learning;
- Reply Desk;
- Article Room;
- Asset Library;
- advanced calendar/pipeline;
- No Coincidences;
- Qdrant;
- hardening and recovery.

A later milestone means dependency sequencing, not feature abandonment.

## M04 Scope Ruling

M04 is substantial. It delivers the complete functional editorial planning and X operating loop, including:

- Command Center;
- editorial lanes, topics, tags, and operator organization;
- rolling 90-day schedule contract;
- Unscheduled Reserve;
- useful Pipeline views;
- deterministic Ready-to-Post;
- bounded Need-a-Post with anti-filler behavior;
- exact review and approval behavior;
- manual X publication and reconciliation;
- manual metrics;
- state-aware actions and trustworthy errors.

M04 does not include automated Release Watch, a provider/API feed, Hermes, agent API, dossiers-as-knowledge-system, Reply Desk, Article Room, Asset Library, Qdrant, Truth Social, or major web polish.

## Execution Cadence

Implementation should use a small number of substantial coherent packages. Ordinary adjacent files/screens do not require repeated owner approval. Stop only for a meaningful doctrine/authority/provider/platform/schema decision, destructive action, failed validation, or audit finding.

## Owner Ruling

Accepted by the owner on 2026-07-20. This acceptance authorizes the coordinated roadmap and planning direction. M04 code implementation remains blocked until the exact M04 work package and added adversarial matrix are prepared and owner-reviewed.