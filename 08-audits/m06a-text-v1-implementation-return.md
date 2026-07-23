# M06-A `m06a.text.v1` Owner Admission and Canonical Execution — Implementation Return

## Status

```text
Implementation status: complete and pushed
Authorization: D039
Application commit: 7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9
Clean commit-bound evidence: complete
Review type: Project-Steward adversarial self-review
Independent review claim: none
Owner closure: granted through D042
Automatic or bulk admission: not authorized and not implemented
Production owner-admitted rows created by implementation: 0
Phase 3B or later authority: none
M06-B authority: none
```

This record returned the exact D039-authorized implementation for review. D041 Project-Steward self-review found no material findings, and D042 closes the package without claiming independent review. Closure does not admit `m06a.text.v1` automatically or across every physical Vault.

## Implemented Boundary

Application commit `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9` implements:

- the supported Tauri Vault gate at the live `V0004` head;
- one explicit confirmation-based, active-human-only, per-Vault owner-admission ceremony;
- one immutable `owner_admitted` successor to the exact current `under_test` admission row;
- exact tuple and D039 evidence-manifest verification;
- human-triggered canonical parsing of one already admitted, retention-eligible, same-Vault artifact lineage at a time;
- exact artifact path, size, hash, and reparse verification before worker launch;
- two isolated worker runs with byte-identical deterministic candidate enforcement;
- source-worker and real PyInstaller-packaged-sidecar canonical execution;
- started/succeeded/failed parser execution receipts with parent-generated content-free failure receipts where classification is possible;
- content-addressed canonical package storage and exact same-input/same-tuple package reuse;
- initial document version ordinal `1`, exact package/execution/artifact lineage, elements, regions, audit, and idempotency;
- no document-version noise for an identical rerun;
- package-before-database interruption state that remains blocked for reconciliation;
- safe document summaries through Tauri/loopback without raw text or absolute paths;
- backup, disposable restore, package tamper, missing/extra, and cross-Vault proof;
- the exact D039 28-invariant evidence suite.

## Exact Change Surface

The implementation changed exactly 13 D039-authorized application paths:

```text
desktop/src/App.tsx
desktop/src/api/client.test.ts
desktop/src/api/client.ts
desktop/src/api/types.ts
scripts/run_ht_evidence.py
src/discrepancy_desk/parser_service.py
src/discrepancy_desk/vault_filesystem.py
src/discrepancy_desk/web.py
tests/test_m06a_desktop_workflow.py
tests/test_m06a_parser_framework.py
tests/test_m06a_parser_packaging.py
tests/test_m06a_text_admission.py
tests/test_m06a_text_canonical_execution.py
```

The implementation changed none of:

```text
pyproject.toml
uv.lock
central migrations
Vault migrations
parser resources or manifests
plain_text_v1.py
parser_contract.py
Jinja templates
other parser modules
provider, network, agent, LLM, search, dossier, projection, Qdrant, graph, purge, or publication modules
```

No dependency or migration was added.

## Admission Boundary

The exact admission ceremony requires:

- the selected physical Vault at verified `V0004` state;
- the active human Vault owner with `vault_admin` authority;
- exact confirmation text `ADMIT m06a.text.v1 FOR THIS VAULT`;
- exact D039 tuple, definition, configuration, predecessor admission, evidence hashes, dependency lock, and material hash;
- exactly one current `under_test` row;
- an immutable successor insert rather than update or deletion;
- exact operation-key idempotency and conflict refusal.

Fresh or newly migrated Vaults remain `under_test`. Startup, migration, candidate installation, package installation, and application launch create no `owner_admitted` row. Admission in one Vault does not admit another Vault.

## Canonical Execution Boundary

Canonical parsing requires a separate human action for one acquisition-artifact link. The trusted service verifies:

- the selected Vault and input link have the same Vault identity;
- retention is `allow` and acquisition outcome is `succeeded`;
- the current admission is the exact D039 `owner_admitted` successor;
- the request names that exact admission version;
- artifact relative path, content address, byte size, SHA-256, regular-file state, and reparse chain;
- source and packaged resources match the admitted tuple.

The request cannot select a parser module, entrypoint, command, path, URL, actor, configuration, resource manifest, security profile, or output location.

A successful first run creates one current document version with ordinal `1`. An identical later operation may append an execution and reuse the exact package/document authority without creating another version. A different result that would require supersession fails closed because D039 grants no document-transition authority.

## API and Tauri Surface

Added loopback routes:

```text
POST /desktop-api/v1/vaults/{vault_id}/parsers/m06a.text.v1/admit
POST /desktop-api/v1/vaults/{vault_id}/artifacts/{acquisition_artifact_link_id}/parse-text
GET  /desktop-api/v1/vaults/{vault_id}/documents
```

The supported Tauri screen now:

- renders healthy `V0004` Vault records, parser status, artifacts, and document summaries;
- requires exact typed confirmation before admission;
- enables one per-artifact parse action only after admission;
- exposes no parser configuration editor, lifecycle controls, bulk admission, bulk parsing, background parsing, or later parser choices.

## Clean Commit-Bound Validation

All required validation was rerun against clean application commit `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9`.

```text
uv sync --frozen                                      passed
uv sync --frozen --extra dev                          passed; uv.lock unchanged
uv run ruff check .                                   passed
full Python suite                                      244 passed
focused D039/parser/migration/recovery suite           81 passed
D039 exact hammer profile                              28/28 invariants; 30/30 mapped tests; 0 deferred
M06-A Phase 3A hammer                                  35/35 passed; 0 deferred
M06-A Phase 2 hammer                                   33/33 passed; 0 deferred
M06-A Phase 1 hammer                                   31/31 passed; 0 deferred
legacy hammer                                          31/31 executed/passed; HT-14 approved deferral only
central/Vault migration heads                         0005 / V0004
Tauri Vitest                                           6/6 passed
Tauri production build                                passed
Rust tests                                             3/3 passed
real packaged canonical execution                     passed
fresh PyInstaller packaged sidecar build              passed
packaged sidecar SHA-256                              76a1b6db0b40069c9b63ab9ccae955007a79049d9118e96aacf93494c4874469
git diff --check                                       passed
post-validation working tree                          clean
remote                                                 synchronized with origin/main
```

## Immutable Evidence

```text
Full suite:
runtime/test-evidence/by-commit/7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9.json
SHA-256: 6f565f7f7a149a82624cfa4100db5b6796610ad302bd1354855c88806ff082a4

D039 exact profile:
runtime/test-evidence/m06a-text-v1/by-commit/7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9.json
SHA-256: cc961f43d3c65605164f46059957382e08fc027875efd5f7cf02953fa311aa47

Phase 3A inherited profile SHA-256:
4af7464bc91dc232da0e1ddab281ba1cb31db006a7507abc035f46d2130073c5

Phase 2 inherited profile SHA-256:
43ca231926dd0dc31625b195e31b90dbf72bad9a274d06ad05a814d4f2757800

Phase 1 inherited profile SHA-256:
668f706ff8f215c4a4a9a076565c2c7c101c64db60fd40041a6d480a932c7f96

Legacy profile SHA-256:
be9a55400b9e3cc660461c7560b41a8357c4523ba3aabc6e5a477fd9c7d79e7d
```

The D039 aggregate records:

- the exact parser tuple and admission manifest;
- `automatic_owner_admission: false`;
- `admission_is_per_vault: true`;
- `canonical_execution_requires_human_action: true`;
- source-worker and packaged-sidecar execution;
- package reuse and no-version-noise proof;
- backup/restore proof;
- exact API/UI surface proof;
- absence of later-capability leakage;
- 28 required/executed/passed invariants;
- 30 collected/executed/passed mapped tests;
- zero failed, skipped, xfailed, xpassed, or errored mapped tests;
- `working_tree_dirty: false`.

## Preserved Blocks

This implementation does not authorize or implement:

- automatic, startup, migration, package-install, bulk, cross-Vault, or every-Vault admission;
- parser configuration editing or lifecycle controls beyond the exact successor insert;
- automatic, background, scheduled, watcher, provider, or agent parsing;
- SRT, VTT, JSON, Markdown, RSS/Atom, HTML, PDF, OCR, or additional parsers;
- document correction or supersession workflows;
- Phase 3B, Phases 4 through 6, or M06-B;
- FTS, chunks, projections, assertions, dossiers, context runs, providers, network retrieval, monitoring, live LLM integration, Qdrant, graph work, purge, or publication automation.

## Historical Review Gate and D042 Disposition

The original D039 package required independent inspection before owner closure. D041 replaced the unstable Claude attempt with a disclosed Project-Steward adversarial self-review covering:

- exact D039/package and changed-path compliance;
- exact admission tuple/evidence/predecessor binding and per-Vault human authority;
- absence of automatic or bulk admission;
- artifact verification and pre-worker gating;
- started/finalized/failed execution lifecycle and safe receipts;
- two-worker determinism, source and packaged execution;
- package storage/reuse and execution-package links;
- initial document, element, region, locator, and version-noise behavior;
- interruption/reconciliation and backup/restore behavior;
- loopback/Tauri authority and leakage boundaries;
- exact 28-invariant evidence and inherited regressions;
- absence of other parsers and later capability.

D041 found no material D039 findings. D042 explicitly accepted that self-review basis and granted owner closure without claiming independent review. Closure still does not admit the parser automatically in any live Vault and does not authorize Phase 3B or another parser.
