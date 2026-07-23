# M06-A `m06a.vtt.v1` C1 — Correction Return

## Status

```text
Correction authority: D047
Application baseline: a96928f482f7b6c308da81061f0e8f64c6ef2966
Correction commit: 0f0717eac27442c8706c9031aa8cff5031761493
Application branch/origin: main, pushed and synchronized
Review source: 08-audits/m06a-d045-project-steward-self-review.md
Findings corrected: D045-SR-01 through D045-SR-03
Independent review claim: none
Final Rust validation on correction commit: pending owner execution
Owner closure: pending
VTT admission authority: none
VTT canonical authority: none
```

## Corrected Findings

### D045-SR-01 — ASCII numeric grammar

Corrected:

- every timestamp digit class now uses explicit ASCII `[0-9]`;
- recognized numeric `line`, `position`, and `size` settings use explicit ASCII digits;
- Arabic-Indic, extended Arabic-Indic, full-width, and Devanagari numeric forms fail in source and real packaged execution;
- ordinary Unicode remains valid in inert header, identifier, NOTE, unknown-setting, and payload text.

Disposition: `CORRECTED`.

### D045-SR-02 — Independent reconciliation

Corrected parent validation independently derives and verifies:

```text
decoded-character count
source logical-line count
line-ending profile
BOM and line-ending warning facts
overlap warning facts
markup warning facts
unknown-setting warning facts
top-level element and region line locators
timing-line and payload line locators
timestamp values from exact source timing lines
setting metadata from exact source timing tokens
normalized cue payload
```

Adversarial tests substitute a parser that repeats fabricated values. Fabricated line locators, counts,
line-ending profiles, and warnings still fail before deterministic whole-candidate comparison.

Disposition: `CORRECTED`.

### D045-SR-03 — Exact corpus and evidence coverage

The deterministic ZIP now contains 31 entries and directly covers:

```text
header-only LF, CRLF, and CR
mixed line endings
multiline cue payload
NOTE before, between, and after cues
active-looking markup and URL payload
Arabic-Indic, extended Arabic-Indic, full-width, and Devanagari numeric refusal
source full-tuple tamper
real packaged full-tuple tamper
parser-repeated line/count/warning fabrication
```

The original D045 mappings were strengthened so M06A-VTT-003 proves source and packaged tuple tamper, and
M06A-VTT-021 proves independent line/count/warning reconciliation.

Disposition: `CORRECTED`.

## Exact Changed Surface

```text
parser_resources/m06a.vtt.v1/manifest.sha256
scripts/run_ht_evidence.py
src/discrepancy_desk/parsers/vtt_v1.py
src/discrepancy_desk/vtt_contract.py
tests/fixtures/m06a/parsers/vtt/corpus.zip
tests/fixtures/m06a/parsers/vtt/manifest.sha256
tests/test_m06a_vtt_packaging.py
tests/test_m06a_vtt_parser.py
```

Exactly `8/8` authorized paths changed.

Unexpected paths: `0`.

Forbidden changes: `0`.

Dependencies, migrations, desktop source, routes, services, workers, Vault provisioning, config, schema,
plain-text resources, and SRT resources remained unchanged.

## Corrected Resource and Fixture Hashes

```text
implementation_sha256:
291c6389a39683a7dab2a471a58773ecc90de711a667e3afb6f0a652d3da850a

resource_manifest_sha256:
2723624c58b5bdbfeffd8404b49261ee96de820f0d4a7244e4ae338cce8bc18b

config_sha256:
2caf5a54bbf13c470c09428d191921203cbe23443168211dc76c9068a89afce5

schema_sha256:
1121baef45ab0f8582ee7d2005b81f235beb720520cab36791eb3d52fd595aa9

dependency_lock_sha256:
feb1aea2f45166a25c6b1618798790f65656db9490dc63d77481c519c8765351

fixture_corpus_sha256:
a0c256a84550452c69cf624649ba639f52fea4fc69ba127ca350ee80c2caa97c
```

## Clean Commit-Bound Validation

```text
Full Python suite                         342/342 passed
D047 VTT-C1 invariants                       9/9 passed
D047 VTT-C1 mapped tests                   27/27 passed
Strengthened D045 VTT invariants           28/28 passed
Strengthened D045 VTT mapped tests         38/38 passed
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
Final real packaged VTT tests               14/14 passed
Final Rust validation                        pending owner execution
```

All immutable evidence records bind correction commit
`0f0717eac27442c8706c9031aa8cff5031761493` and record `working_tree_dirty: false`.

## Final Packaged Sidecar

```text
sha256: 4efd02bcda3ebeb1e292fb090e8930f1873b6be3ade4fa5917ba7ee6f71c215f
executable_size_bytes: 12,421,548
package_size_bytes: 41,014,766
```

## Immutable Evidence SHA-256

```text
Full suite:
721268a06d608ef651ed6c1b673e912a598379dab09692eebdb6419debd7547b

D047 VTT-C1:
635e7bb5cc42b9c1aa3914f11ae3159a8acfc11fac8b15d39ce19b1f8cb62162

Strengthened D045 VTT:
f027fae59ed95d7885d983ddaf67d23e782ff28d782012eb8d900d8f962d7d4c

D041 SRT-C1:
4526d9ddbb56e945f0f4ad154b4da46a7a5cd3e4f20c7b091223354020a0fd5a

D040 SRT:
08d037e56e7322ce8055a7b8e15400fd5aff559e6346cb68152953b1f1aa35ff

D039 plain text:
274c46c150d3dad36912b8544cebb77f40d8d93e856f5fd5a7ec90057623b04e

Phase 3A:
667c5d78f656d1b36b8f3f99da301d5d70b1e3ba66b648620c20cfd206d2d26f

Phase 2:
c7409c9eee05deb50a934eb36e67408615e79c787da16643901a117d5646a3d2

Phase 1:
1d7bfeeecd594da24f2a71485618feedc6e001d5ae9b52cc70c88934d6c06e3e

Legacy:
0bdcab42e974c586a0e47e1c3674805594e8ff031b44618123a317e38aa23a9f
```

## Remaining Gate

Before final disposition and owner closure:

1. the owner must execute `cargo test --manifest-path desktop/src-tauri/Cargo.toml` on application commit
   `0f0717eac27442c8706c9031aa8cff5031761493`;
2. the result must be recorded without overstatement;
3. the Project-Steward re-review disposition must be finalized;
4. the owner must explicitly close corrected D045/D047.

No VTT admission or canonical authority is granted by this return.
