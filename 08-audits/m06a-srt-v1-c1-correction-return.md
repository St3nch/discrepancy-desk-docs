# M06-A `m06a.srt.v1` C1 — Correction Implementation Return

## Status

```text
Correction authority: D041
Application baseline: 529165c19da30185dd9833fab608d6dc28dfed88
Correction commit: 6a8082253a52a601291efaf3ed85ee411b04be20
Commit subject: Correct SRT packaged verification
Review type: Project-Steward adversarial self-review
Independent review claim: none
D040-SR-01: corrected
D040-SR-02: corrected
Owner closure: granted through D042
SRT owner admission: not authorized
SRT canonical execution: not authorized
```

## Implemented Corrections

### Exact packaged resource tuple

The SRT worker now pins and verifies the exact D040:

```text
resource manifest SHA-256  04d9f9780e13bc3658194d3f0d6cc8f6ce9426e154b8d229e4d5c80b2e20dd41
configuration SHA-256      88db85d7b93ca55cf2f1bc3104941cf3076943ed388c4e803a480b75e5bbf309
schema SHA-256             99ec97748389d61ead4d06b91416c64163b3f40269a473b9f1786ba20b0ba551
implementation SHA-256     e8bc536aac12e60ebfa0962177af34fa4d05a6d564d08fcbf694dee1a88ccb2a
dependency-lock SHA-256    feb1aea2f45166a25c6b1618798790f65656db9490dc63d77481c519c8765351
```

The fixed dependency-lock bytes are staged into the isolated operation directory before the inherited filesystem controls are installed, then reverified inside the boundary with the manifest, config, schema, and packaged implementation bytes. Request-supplied hashes are compared only against this independently established tuple.

Real packaged regressions prove that modified schema, self-hashed modified config, manifest, dependency lock, and packaged implementation bytes all preserve a failed `packaging_mismatch` receipt and emit no candidate.

### Per-parser status isolation

An SRT-only resource absence or mismatch now returns one content-free SRT status:

```text
state: unavailable
canonical_available: false
admission_ready: false
admission_manifest: null
reason_code: packaged_tuple_mismatch
```

The shared parser endpoint remains successful and continues to expose the unaffected D039 plain-text status and admission material. Paths, exception details, and tampered content are not returned.

## Exact Change Surface

The correction changed exactly six D041-authorized application paths:

```text
scripts/run_ht_evidence.py
src/discrepancy_desk/srt_contract.py
src/discrepancy_desk/srt_service.py
src/discrepancy_desk/srt_worker.py
tests/test_m06a_desktop_workflow.py
tests/test_m06a_srt_packaging.py
```

It changed none of:

```text
pyproject.toml
uv.lock
migrations/**
vault_migrations/**
parser_resources/**
src/discrepancy_desk/parser_service.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parsers/plain_text_v1.py
src/discrepancy_desk/parsers/srt_v1.py
desktop/**
```

## Clean Commit-Bound Validation

```text
Python full suite                      286/286 passed
Focused corrected SRT/desktop suite     49/49 passed
D041 C1 profile                           8/8 invariants passed
D041 C1 mapped tests                      9/9 passed
D040 SRT profile                         24/24 passed
D040 mapped tests                        35/35 passed
D039 plain-text profile                  28/28 passed
D039 mapped tests                        30/30 passed
Phase 3A                                35/35 passed
Phase 2                                 33/33 passed
Phase 1                                 31/31 passed
Legacy                                  31/31 executed and passed
Legacy deferral                         HT-14 only, previously approved
Tauri/Vitest                              6/6 passed
Frontend production build                passed
Rust                                      3/3 passed
Central/Vault migration heads            0005 / V0004
Working tree dirty                        false
```

Fresh packaged sidecar SHA-256:

```text
c736a6d1d0d83957ae06b4a5ebe5bedd66db0168678baba7f726e4cc2a4575bb
```

## Immutable Evidence

All records are bound to:

```text
6a8082253a52a601291efaf3ed85ee411b04be20
```

```text
Full suite     86b6356b74e2d264d116aa4b82aa05836ae8d666ceb5b23a3c7a2f854dc2387c
D041 C1        7ca8ab732832c469b71ea87054b354af32107ca7c18ea88f97fb3c7e0d138f29
D040 SRT       0b572da7b7a4145c5de536ac42b8687648450a007bdcb0084f0b5faa2283a34c
D039 text      f72f7c9c11ba0e2db7723014688731dad2d9f0644e286eb250b295f280fc0a73
Phase 3A       38b5a29644c3543d154affd2f68dbc67c5c00e6d3b3c7a5397b7201b7168a024
Phase 2        a7e8b797c147b161d514b0045590b16d3713f8d1718e194f622c4f3adb7cf2d6
Phase 1        24833a5af283ce5041fe8e64df896a126fbe579a40f289771f7b3318a4d9609b
Legacy         a2bcd628fcde90cbcb7c1e5257f39d2088a4638c116334d89777936dea918eb1
```

The C1 aggregate records `independent_review_claim: false`, exact resource hashes, all five packaged tamper refusals, valid packaged execution, per-parser status isolation, no SRT admission authority, no canonical SRT authority, and no later-capability leakage.

## Preserved Boundary

This correction does not authorize or implement:

- SRT owner admission;
- canonical SRT execution;
- existing-Vault SRT retrofit;
- automatic, bulk, background, scheduled, watcher, agent, or provider parsing;
- VTT, JSON, Markdown, RSS/Atom, HTML, PDF, OCR, or Phase 3B;
- Phase 4 through 6 or M06-B;
- providers, monitoring, live LLM integration, Qdrant, graph work, purge, or publication automation.
