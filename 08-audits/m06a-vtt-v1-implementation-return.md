# M06-A `m06a.vtt.v1` Under-Test Candidate — Implementation Return

## Status

```text
Implementation decision: D045
Application commit: a96928f482f7b6c308da81061f0e8f64c6ef2966
Application branch: main
Application origin: pushed and synchronized
Package: 05-implementation-planning/m06a-vtt-v1-under-test-candidate-package.md
Implementation state: complete inside exact package surface
Commit-bound Python/parser/packaged evidence: complete
Frontend tests/build: complete
Rust validation: owner-executed, 3/3 passed, 0 failed
Adversarial implementation review: complete — D045 REQUIRES CORRECTION
Review: 08-audits/m06a-d045-project-steward-self-review.md
Correction package: 05-implementation-planning/m06a-vtt-v1-c1-self-review-correction-package.md
Owner closure: blocked pending correction
VTT owner admission authority: none
VTT canonical execution authority: none
Existing-Vault retrofit authority: none
JSON/Phase 3B authority: none
```

This record returns the exact D045 implementation for completion of the remaining Rust validation,
adversarial review, finding disposition, and explicit owner closure. It does not admit VTT and does not
authorize canonical VTT parsing.

---

## Implemented Capability

Application commit `a96928f482f7b6c308da81061f0e8f64c6ef2966` adds:

- one strict internal `m06a.vtt.v1` parser candidate;
- UTF-8 and UTF-8-BOM input only;
- exact `WEBVTT` header and required blank separation;
- optional inert header text;
- optional unique cue identifiers;
- short and hours-bearing WebVTT timestamps;
- positive duration and nondecreasing source-order cue starts;
- deterministic overlap warnings;
- closed validation for `vertical`, `line`, `position`, `size`, and `align` settings;
- inert preservation of bounded unknown settings;
- inert NOTE regions;
- terminal refusal of STYLE, REGION, region references, and timeline mapping;
- inert cue markup with no DOM, CSS, rendering, or resource retrieval;
- exact byte, character, line, cue, timing, payload, header, NOTE, separator, and BOM locators;
- parser-scoped config, schema, manifest, implementation, and dependency-lock verification;
- a fixed source and packaged VTT worker;
- neutral read-only status aggregation for plain text, SRT, and VTT;
- fresh-V0004 immutable `under_test` installation only;
- deterministic binary ZIP fixtures and exact 28-invariant evidence.

No VTT admission, canonical parsing, existing-Vault retrofit, automatic parsing, bulk operation, background
operation, or desktop mutation control was added.

---

## Exact Changed Surface

```text
parser_resources/m06a.vtt.v1/config.json
parser_resources/m06a.vtt.v1/manifest.sha256
parser_resources/m06a.vtt.v1/schema.json
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
src/discrepancy_desk/parser_status_service.py
src/discrepancy_desk/parsers/vtt_v1.py
src/discrepancy_desk/vault_service.py
src/discrepancy_desk/vtt_contract.py
src/discrepancy_desk/vtt_service.py
src/discrepancy_desk/vtt_worker.py
src/discrepancy_desk/web.py
tests/fixtures/m06a/parsers/vtt/corpus.zip
tests/fixtures/m06a/parsers/vtt/manifest.sha256
tests/test_m06a_desktop_workflow.py
tests/test_m06a_vtt_packaging.py
tests/test_m06a_vtt_parser.py
```

Changed files: `17`.

Unexpected paths: `0`.

Forbidden or frozen changes: `0`.

The following remained byte-identical to application baseline `6a808225…`:

```text
pyproject.toml
uv.lock
migrations/**
vault_migrations/**
parser_resources/manifest.sha256
plain-text resources, contract, worker, and parser
SRT resources, contract, service, worker, and parser
desktop/**
```

---

## Exact Resource Tuple

```text
parser_id: m06a.vtt.v1
implementation_version: 0.1.0
implementation_sha256: 8e98ac8552ba6048af37adb1f0c2cc1946bf5153537bc019dcd0056ae145b350
resource_manifest_sha256: 9eb1be7efda9707d9eaeebb5f69e68f32fce6da816b11c8183fdea6b1368dbb7
config_sha256: 2caf5a54bbf13c470c09428d191921203cbe23443168211dc76c9068a89afce5
schema_sha256: 1121baef45ab0f8582ee7d2005b81f235beb720520cab36791eb3d52fd595aa9
dependency_lock_sha256: feb1aea2f45166a25c6b1618798790f65656db9490dc63d77481c519c8765351
fixture_corpus_sha256: 8e1491425ca9399802952d18c939ff49eed8def66eeb2c3e6d44fdd0e60735c7
security_profile_id: m06a.parser-worker.windows.v2
```

Packaged tamper proof rejects independently modified manifest, schema, dependency lock, implementation, and
a modified config even when the request supplies the modified config's own hash.

---

## Clean Commit-Bound Validation

All figures below were rerun on clean commit:

```text
a96928f482f7b6c308da81061f0e8f64c6ef2966
```

```text
Full Python suite                         319/319 passed
D045 VTT invariants                        28/28 passed
D045 VTT mapped tests                      33/33 passed
D041 SRT-C1 invariants                       8/8 passed
D041 SRT-C1 mapped tests                     9/9 passed
D040 SRT invariants                         24/24 passed
D040 SRT mapped tests                       35/35 passed
D039 plain-text invariants                  28/28 passed
D039 plain-text mapped tests                30/30 passed
Phase 3A invariants                         35/35 passed
Phase 3A mapped tests                       53/53 passed
Phase 2 invariants                          33/33 passed
Phase 2 mapped tests                        33/33 passed
Phase 1 invariants                          31/31 passed
Phase 1 mapped tests                        35/35 passed
Legacy                                      31/31 executed and passed
Approved legacy deferral                    HT-14 only
Legacy mapped tests                        151/151 passed
Ruff                                         passed
uv sync --frozen --extra dev                 passed
Central migration head                      0005
Vault migration head                        V0004
Tauri/Vitest                                 6/6 passed
Frontend production build                    passed
Real PyInstaller sidecar build               passed
Real packaged VTT tamper matrix              passed
Rust cargo test                              3/3 passed — owner-executed
```

No skipped, failed, errored, xfailed, or xpassed mapped test appears in the D045 profile or inherited
executed profiles.

---

## Packaged Artifact

Final clean-commit packaged sidecar:

```text
artifact: desktop/src-tauri/binaries/discrepancy-desk-backend/discrepancy-desk-backend.exe
sha256: 87e9c5fc04dee78367761fae3b50c5e414c54a0f4d16c6397a72a58d9e3cee7a
executable_size_bytes: 12,417,384
package_size_bytes: 41,003,477
```

The final packaged VTT tests passed `4/4` against this build.

---

## Immutable Evidence SHA-256

```text
Full suite:
1f4bfe05e1822dc726d5a55d5e68d71d7475c61ac122e1640cffa6741a678471

D045 VTT:
ef629fe6a3f2d1b0d638df50e9a713993240a47b20bcde758510ef3bd495adb9

D041 SRT-C1:
dedd65e9149054c15624c87b3ff0acda9a8ff8e317b1c3ceae5e1f18a66359f8

D040 SRT:
7635c6ebdf15e4f0542df7868bcb394e4e7041d5be7f623050d8ebe6bc0ee14d

D039 plain text:
91abb1c526bf7dc27b0df4cb126edb87fa1900b58e56dd22bbcc14a6172b7b2e

Phase 3A:
20199261b934580cc4f8743b7b306516800844a8a7e4922a5751fd916d8361d1

Phase 2:
d35aae1fc300de5774758a0f55874da19d3656f63ab98f751636135e93cca886

Phase 1:
22f007534c517831007d16e476b80d741296fc45b6603762dcbd17280507dd4c

Legacy:
01c8cd24b135fedeb5c5000bbb6fd793e7af3025efb53523b7864f09f5c0805a
```

Every listed record binds commit `a96928f482f7b6c308da81061f0e8f64c6ef2966` and records
`working_tree_dirty: false`.

---

## Product and Authority Result

Fresh V0004 Vaults receive:

```text
m06a.text.v1   existing governed state
m06a.srt.v1    under_test
m06a.vtt.v1    under_test
```

VTT status is read-only:

```text
state: under_test
canonical_available: false
admission_ready: false
admission_manifest: null
```

A broken text, SRT, or VTT packaged tuple degrades only that parser's status row. Healthy parser rows remain
visible. Status failures expose no path, exception text, tampered bytes, or resource location.

---

## Preserved Blocks

This implementation does not authorize or implement:

- VTT owner admission;
- canonical VTT parsing;
- existing-Vault VTT retrofit;
- automatic, background, watcher, scheduled, or bulk parsing;
- SRT owner admission or canonical SRT parsing;
- JSON or Phase 3B;
- Phases 4 through 6 or M06-B;
- providers, agents, live LLM integration, Qdrant, graph work, purge, or publication automation.

---

## Remaining Gate

Rust validation is complete: the owner executed `cargo test --manifest-path desktop/src-tauri/Cargo.toml` and all 3 tests passed.

Project-Steward adversarial self-review returned `D045 REQUIRES CORRECTION` with three Medium, closure-blocking findings. Before owner closure:

1. obtain explicit owner acceptance of the exact VTT C1 correction package;
2. implement only its exact changed-path surface;
3. rerun the complete clean commit-bound closure stack, including Rust;
4. disposition all three findings;
5. obtain explicit owner closure.

No independent-review claim is made.
