# M06-A `m06a.vtt.v1` C1 — Project-Steward Review Correction Package

## Status

```text
Package status: implemented and clean-evidence-bound at 0f0717eac27442c8706c9031aa8cff5031761493; final Rust validation and owner closure pending
Review source: 08-audits/m06a-d045-project-steward-self-review.md
Application baseline: a96928f482f7b6c308da81061f0e8f64c6ef2966
Correction commit: 0f0717eac27442c8706c9031aa8cff5031761493
Correction return: 08-audits/m06a-vtt-v1-c1-correction-return.md
Findings: D045-SR-01 through D045-SR-03 only
Correction implementation authority: D047 — exact package and exact nine-invariant profile only
VTT admission authority: none
VTT canonical authority: none
JSON/Phase 3B authority: none
```

## Purpose

Correct the three reproduced D045 self-review findings without changing dependencies, migrations, desktop
source, plain-text/SRT tuples, product mutation authority, or the strict under-test-only VTT boundary.

## Correction 1 — ASCII-only numeric grammar

Requirements:

- timestamps use explicit ASCII `[0-9]` digits only;
- numeric `line`, `position`, and `size` values use explicit ASCII digits only;
- Arabic-Indic, extended Arabic-Indic, full-width, and other Unicode decimal digits fail as malformed input;
- ordinary Unicode remains allowed in inert header, identifier, NOTE, unknown-setting value, and payload text;
- source and real packaged workers prove the corrected behavior.

## Correction 2 — Independent reconciliation

`vtt_contract.py` must independently derive and validate:

```text
decoded character count
source logical-line count
line-ending profile
BOM warning fact
line-ending warning fact
overlap warning fact
markup warning fact
unknown-setting warning fact
full element/region line locators
timing-line locator
payload locator
```

The validator may retain deterministic whole-candidate comparison as an additional check, but it may not use
that comparison as the only proof of line/count/warning correctness.

Tests must prove rejection when a candidate and a substituted parser both repeat fabricated line locators,
counts, or warnings.

## Correction 3 — Exact evidence/corpus coverage

The deterministic ZIP and exact mappings must add direct cases for:

```text
header-only LF, CRLF, and CR
multiline cue payload
mixed line endings
NOTE before, between, and after cues
active-looking markup and URL payload
non-ASCII numeric grammar refusal
source manifest/config/schema/implementation/lock tamper
line/count/warning fabrication
```

The real packaged full-tuple tamper matrix remains required.

## Exact Changed-Path Surface

Allowed application paths:

```text
src/discrepancy_desk/parsers/vtt_v1.py
src/discrepancy_desk/vtt_contract.py
parser_resources/m06a.vtt.v1/manifest.sha256
tests/fixtures/m06a/parsers/vtt/corpus.zip
tests/fixtures/m06a/parsers/vtt/manifest.sha256
tests/test_m06a_vtt_parser.py
tests/test_m06a_vtt_packaging.py
scripts/run_ht_evidence.py
```

A listed path may be omitted. No other application path may change.

Explicitly forbidden:

```text
pyproject.toml
uv.lock
migrations/**
vault_migrations/**
desktop/**
parser_resources/m06a.vtt.v1/config.json
parser_resources/m06a.vtt.v1/schema.json
all plain-text resources and implementation
all SRT resources and implementation
src/discrepancy_desk/parser_service.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/srt_service.py
src/discrepancy_desk/vtt_service.py
src/discrepancy_desk/vtt_worker.py
src/discrepancy_desk/vault_service.py
src/discrepancy_desk/web.py
```

The corrected parser implementation hash and parser-scoped manifest hash must be repinned in
`vtt_contract.py`; this does not authorize changing config, schema, dependency lock, parser identity,
package schema, deterministic contract, security profile, or admission state.

## Exact C1 Invariants

```text
M06A-VTT-C1-001 corrected implementation and manifest constants match live bytes
M06A-VTT-C1-002 timestamps and recognized numeric settings reject every non-ASCII decimal family
M06A-VTT-C1-003 independent line locators reject fabricated parser-repeated line metadata
M06A-VTT-C1-004 independent counts and warnings reject fabricated parser-repeated facts
M06A-VTT-C1-005 source full-tuple resource tamper fails before worker launch
M06A-VTT-C1-006 real packaged full-tuple tamper refusal remains green
M06A-VTT-C1-007 exact expanded deterministic corpus and direct mappings are complete
M06A-VTT-C1-008 valid source and packaged execution remain deterministic and denial-controlled
M06A-VTT-C1-009 no admission, canonical, retrofit, JSON, Phase 3B, or later authority enters
```

## Validation

Required on the clean correction commit:

```text
uv sync --frozen --extra dev
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
focused VTT C1 tests
m06a-vtt-v1-c1 exact profile
m06a-vtt-v1 profile
m06a-srt-v1-c1 profile
m06a-srt-v1 profile
m06a-text-v1 profile
m06a-phase3a profile
m06a-phase2 profile
m06a-phase1 profile
legacy profile
central/Vault heads 0005/V0004
Tauri tests and production build
cargo test --manifest-path desktop/src-tauri/Cargo.toml
fresh packaged sidecar build and full-tuple tamper matrix
exact changed-path audit
git diff --check
clean commit-bound evidence
```

## Stop Conditions

Stop and return to owner review if correction requires:

- a dependency, migration, config, schema, desktop, route, service, worker, or Vault-provisioning change;
- any plain-text or SRT tuple change;
- VTT admission, canonical execution, existing-Vault retrofit, automatic/background/bulk parsing;
- JSON, Phase 3B, later M06 work, providers, agents, or publication automation;
- any unlisted path.
