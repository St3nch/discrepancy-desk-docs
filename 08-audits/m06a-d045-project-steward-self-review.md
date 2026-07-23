# D045 Project-Steward Adversarial Self-Review

## Status

```text
Review type: Project-Steward adversarial self-review
Independent review claim: none
Application reviewed: a96928f482f7b6c308da81061f0e8f64c6ef2966
Rust validation: owner-executed, 3/3 passed, 0 failed
Verdict: D045 REQUIRES CORRECTION
Findings: three Medium, closure-blocking
VTT admission/canonical authority: none
```

## Review Basis

The Project Steward reviewed the exact D045 package, implementation diff from
`6a8082253a52a601291efaf3ed85ee411b04be20` through
`a96928f482f7b6c308da81061f0e8f64c6ef2966`, live source, fixtures, evidence mappings,
packaged-worker tests, fresh-Vault behavior, and immutable evidence.

Because the same steward implemented D045, this is not an independent-review claim.

The owner separately executed:

```text
cargo test --manifest-path desktop/src-tauri/Cargo.toml
```

Result:

```text
3 passed; 0 failed; 0 ignored
```

## Verified Areas

The exact changed-path surface is authorized and contains no dependency, migration, desktop-source,
plain-text tuple, or SRT tuple change. Fresh V0004 Vaults install VTT only as `under_test`; no VTT
admission, canonical execution, retrofit route, automatic parsing, bulk parsing, JSON, or Phase 3B
surface exists. Full Python, focused parser/recovery, inherited evidence, packaged sidecar, frontend,
migration-head, and owner-executed Rust checks are green.

## Finding D045-SR-01 — Unicode decimal digits bypass the strict ASCII grammar

```text
Severity: Medium
Closure blocking: yes
Primary file: src/discrepancy_desk/parsers/vtt_v1.py
Governing requirements: package §§6.2, 6.3; M06A-VTT-009, -013, -014, -019
```

The timestamp and numeric setting regular expressions use Python `\\d`. In Unicode mode, `\\d`
accepts non-ASCII decimal digits. The strict package requires ASCII timestamp digits and a closed numeric
setting grammar.

Reproduced accepted inputs include:

```text
Arabic-Indic timestamp digits
full-width percentage digits in size:
Arabic-Indic integer digits in line:
```

All three were accepted by the live parser.

Required correction:

- replace every numeric grammar token with explicit ASCII `[0-9]` ranges;
- add source and packaged rejection tests for non-ASCII decimal digits in timestamps and recognized
  numeric settings;
- preserve ordinary Unicode in cue identifiers, NOTE text, header text, and payload.

## Finding D045-SR-02 — Parent validation is not independent of parser-produced line/count/warning facts

```text
Severity: Medium
Closure blocking: yes
Primary file: src/discrepancy_desk/vtt_contract.py
Governing requirements: package §9.3; M06A-VTT-021
```

The parent validator independently checks byte/character partitioning, but it does not independently derive
exact line locators, source-line count, line-ending facts, or warning facts. Instead, it reruns the same
`parse_bytes` implementation and compares canonical output.

Adversarial reproduction showed that when the parser repeats fabricated facts, the validator accepts:

```text
source_line_start/source_line_end = 999
source_line_count = 999
fabricated overlapping_cues warning on a non-overlapping file
```

Required correction:

- independently scan logical lines in `vtt_contract.py`;
- independently verify every top-level and nested line locator;
- independently verify decoded-character and source-line counts;
- independently derive line-ending, BOM, overlap, markup, and unknown-setting warning facts;
- add direct detector-error and fabricated-metadata tests that do not rely on the parser repeating the
  expected answer.

## Finding D045-SR-03 — The exact corpus and evidence mapping overclaim required coverage

```text
Severity: Medium
Closure blocking: yes
Primary files:
- tests/fixtures/m06a/parsers/vtt/corpus.zip
- tests/test_m06a_vtt_parser.py
- tests/test_m06a_vtt_packaging.py
- scripts/run_ht_evidence.py
Governing requirements: package §§10, 14, 16; M06A-VTT-003, -006, -016, -018, -020, -021
```

The deterministic ZIP contains 19 entries but omits several cases expressly required by the package,
including CR-only header framing, mixed line endings, multiline cue payload, NOTE blocks before/between/after
cues in one corpus case, and an active-looking URL payload case.

Invariant 016 claims NOTE preservation before, between, and after cues, while its mapped test proves only a
single NOTE before a cue. Invariant 003 in the accepted package requires source and packaged full-tuple tamper
proof, while the registered test proves packaged tamper only.

Direct probes confirm the parser currently handles NOTE placement, CR-only framing, and mixed multiline
input, so this finding is primarily an evidence-integrity failure rather than a demonstrated semantic parser
failure. It remains closure-blocking because the exact evidence profile overstates what was executed.

Required correction:

- expand the deterministic ZIP to the accepted minimum corpus;
- map NOTE before/between/after, CR-only, mixed-ending, multiline, and active-looking payload cases directly;
- add source-resource full-tuple tamper tests as well as the retained real packaged tamper matrix;
- strengthen M06A-VTT-021 with direct line/count/warning fabrication tests;
- update the runner mappings and metadata so every claim is supported by executed tests.

## Verdict

```text
D045 REQUIRES CORRECTION
```

The findings grant no VTT admission or canonical authority and do not weaken the closed D039/D040/D041
surfaces.
