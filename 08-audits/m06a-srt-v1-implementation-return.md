# M06-A `m06a.srt.v1` Under-Test Candidate — Implementation Return

## Verdict

```text
D040 IMPLEMENTATION COMPLETE AND CLEAN-EVIDENCE-BOUND
Independent review: deferred, not waived
Owner closure: not requested
SRT owner admission: not authorized
SRT canonical execution: not authorized
```

## Live Application Truth

```text
Repository: C:\dev\x\discrepancy-desk
Branch: main
Implementation commit: 529165c19da30185dd9833fab608d6dc28dfed88
Commit subject: Add SRT under-test parser candidate
Remote: origin/main synchronized
Working tree: clean
```

## Governing Authority

D040 authorized only `05-implementation-planning/m06a-srt-v1-under-test-candidate-package.md` and its exact 24-invariant profile.

D040 also records that the D039 independent review was deferred because Claude usage limits prevented execution. That review obligation remains open and was neither waived nor represented as satisfied.

## Implemented Boundary

The application now contains one strict internal `m06a.srt.v1` candidate with:

- parser-scoped immutable resources under `parser_resources/m06a.srt.v1/`;
- a separate fixed `--m06a-srt-parser-worker` source/packaged entrypoint;
- exact UTF-8, UTF-8 BOM, and BOM-declared UTF-16 LE/BE handling;
- optional numeric cue indexes;
- exact `HH:MM:SS,mmm --> HH:MM:SS,mmm` timestamps;
- multiline cue text;
- exact source-order preservation;
- deterministic warning vocabulary for BOM removal, line-ending normalization, nonsequential indexes, and overlapping cues;
- independently reconciled byte, character, line, full-cue, and cue-text locators;
- exact input, cue-count, cue-byte, line, and timestamp limits;
- no partial output;
- inherited network, process, shell, exec, dynamic-library, credential/proxy, and filesystem-mutation denials;
- deterministic source and packaged candidate/package proof;
- immutable `under_test` installation in fresh V0004 Vaults only;
- read-only `SubRip (SRT)` parser status;
- zero SRT owner-admitted rows and no SRT canonical execution path.

Historical V0003 provisioning remains unchanged.

## D039 Preservation

The implementation intentionally did not modify:

```text
pyproject.toml
uv.lock
migrations/**
vault_migrations/**
parser_resources/manifest.sha256
parser_resources/configs/m06a.text.v1.json
parser_resources/schemas/m06a.normalized-package.v1.json
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parsers/plain_text_v1.py
```

The exact D039 tuple inputs were compared byte-for-byte with application commit `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9` and passed.

The root plain-text resource manifest remains plain-text-only. SRT uses a separate parser-scoped resource manifest, preventing a silent change to the admitted plain-text tuple.

## Fixture Reproducibility

The adversarial SRT corpus is stored as one deterministic binary ZIP:

```text
tests/fixtures/m06a/parsers/srt/corpus.zip
```

This avoids Windows line-ending conversion and reserved-filename behavior while preserving the exact malformed and encoded input bytes. Its parser fixture manifest is verified during the D040 suite.

## Clean Validation

```text
Python full suite                     277/277 passed
Focused SRT/parser/recovery suite      81/81 passed
D040 SRT invariants                    24/24 passed
D040 mapped tests                      35/35 passed
D039 text invariants                   28/28 passed
Phase 3A invariants                    35/35 passed
Phase 2 invariants                     33/33 passed
Phase 1 invariants                     31/31 passed
Legacy                                 31/31 executed and passed
Legacy deferral                        HT-14 only, previously approved
Tauri/Vitest                            6/6 passed
Frontend production build              passed
Rust                                    3/3 passed
Central migration head                 0005
Vault migration head                   V0004
Packaged SRT worker                     passed
Working tree dirty                      false
```

Packaged sidecar SHA-256:

```text
af174e1d4252e274eb12152cf4a4804316070e7228dbbf49568b239b537cd6e9
```

## Immutable Evidence

All records are bound to:

```text
529165c19da30185dd9833fab608d6dc28dfed88
```

Evidence SHA-256 values:

```text
Full suite     1419e3507de8017d2e30e33bf572dfd367b67b544be0f1042212b946749cb705
D040 SRT       3e0b425305e40f615d4f5103498aad0cc893ffcfbaa2120e5803f441a29b73af
D039 text      b3053ccd43476b3fb85271658a0eff5c22f0329cfabe5e61e4c2f95f23b62086
Phase 3A       02482cb8f3f57b85c4619c63def9ee13d88edd1dc4e8be1a76f8bd243a2002f5
Phase 2        8b65ecf1ec48b29b0b829fbf3cff9bbc2a5217aaa27ca94b5b58edd42ef0b264
Phase 1        c99ca11a84924cad472306cf91c640e040eae46ebd6b34b342b65b9bbe0f45c4
Legacy         6b587507e655b153ca3323d208644039f1aabc7be55ed21e0bb45308a1dd9359
```

The D040 contract metadata records:

```text
plain_text_tuple_inputs_preserved: true
automatic_owner_admission: false
canonical_execution_available: false
fresh_vault_state: under_test
source_denial_proof: true
packaged_execution_proof: true
read_only_status_proof: true
inherited_regression_proof: true
later_capability_leakage_absent: true
d039_independent_review_deferred_not_waived: true
```

## Explicit Non-Authority

This implementation does not authorize:

- SRT owner admission;
- canonical SRT parsing;
- existing-Vault SRT retrofit;
- automatic, background, bulk, scheduled, watcher, agent, or provider parsing;
- VTT, JSON, Markdown, RSS/Atom, HTML, PDF, OCR, or Phase 3B;
- Phase 4 through 6 or M06-B;
- providers, network retrieval, monitoring, live LLM integration, Qdrant, graph work, purge, or publication automation;
- closure or waiver of D039;
- closure or waiver of the D040/SRT independent-review obligation.

## Review Ledger

Open independent-review obligations:

1. D039 plain-text owner admission and canonical execution at `7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9`.
2. D040 SRT under-test candidate at `529165c19da30185dd9833fab608d6dc28dfed88`.

These may be reviewed together later if the independent reviewer can clearly separate findings by governing package and commit. Any finding must be reproduced against live repository truth and corrected or explicitly dispositioned before the affected package receives owner closure.
