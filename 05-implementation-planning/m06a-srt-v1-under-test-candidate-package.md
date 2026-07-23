# M06-A `m06a.srt.v1` Under-Test Candidate — Exact Implementation Package

## Status

```text
Package status: implemented and clean-evidence-bound; independent review deferred, not waived
Implementation authority: exact package and exact 24-invariant profile only
Application baseline: 7980b1e7ab3fff51a705d61a93ac9e26b4c26ca9
Implementation commit: 529165c19da30185dd9833fab608d6dc28dfed88
Implementation return: 08-audits/m06a-srt-v1-implementation-return.md
Documentation baseline: 9f43e1a
D039 independent review: deferred by timing, not waived
SRT owner admission authority: none
SRT canonical execution authority: none
VTT/JSON/Phase 3B authority: none
```

This package continues the accepted stdlib-first parser sequence after plain text while preserving the unresolved D039 independent-review obligation. It does not close D039, admit SRT, or authorize any higher-risk parser.

---

# 1. Purpose

Implement one independently hashed and packaged `m06a.srt.v1` candidate in `under_test` state only.

The package must also correct a newly identified expansion hazard without changing the admitted plain-text tuple:

```text
The current `m06a.text.v1` tuple binds the root parser-resource manifest.
Adding SRT entries to that root manifest would change the exact D039 tuple.
```

Therefore SRT must use a parser-scoped immutable resource manifest and a separate fixed worker entrypoint. The root plain-text manifest and every exact D039 tuple input remain byte-identical.

---

# 2. Deferred Review Ruling

Claude usage limits currently prevent the planned D039 independent review.

D040 permits continued bounded implementation but does not waive review. The debt remains explicit:

- D039 remains implemented, clean-evidence-bound, and unclosed;
- its independent review must be attempted when review capacity returns;
- any later finding must be reproduced and dispositioned against live repository truth;
- D039 may not be retroactively described as independently verified;
- Phase 6 final closure cannot declare the review ledger complete while this debt remains open.

This package receives its own implementation return and review debt. Review debt may be batched later, but findings may not be ignored.

---

# 3. Exact SRT Contract

## 3.1 Identity

```text
parser_id: m06a.srt.v1
format_id: application/x-subrip
implementation_kind: internal
implementation_entrypoint: discrepancy_desk.parsers.srt_v1:parse_bytes
implementation_version: 0.1.0
license_id: project-code
state: under_test
```

The implementation/config/schema/security/dependency tuple is content-addressed. Any changed tuple creates a new immutable candidate identity.

## 3.2 Supported subset

- UTF-8, UTF-8 BOM, or explicitly BOM-marked UTF-16 LE/BE;
- numeric cue index optional;
- exact timestamp form `HH:MM:SS,mmm --> HH:MM:SS,mmm`;
- multiline cue text;
- one or more blank logical lines separating cues;
- cue order preserved exactly as source order;
- markup retained as inert untrusted text; no HTML interpretation or execution.

## 3.3 Excluded and terminal failures

Fail the whole candidate with no partial output for:

- undecodable, replacement-character, NUL-bearing, or UTF-16-like input without BOM;
- malformed timestamp or arrow;
- invalid minute/second/millisecond fields;
- timestamp beyond the configured maximum;
- end time before start time;
- missing cue text;
- missing cue separator that makes boundaries ambiguous;
- size, cue-count, cue-byte, line, or element limit breach;
- locator/coverage mismatch;
- determinism failure;
- security-boundary violation;
- packaged resource/hash mismatch.

## 3.4 Limits

```text
input size:          10 MiB
cue count:           100,000
maximum cue bytes:   1 MiB
logical lines:       300,000
maximum timestamp:   86,400,000 ms
partial output:      prohibited
```

## 3.5 Warning vocabulary

Only these warnings are admitted:

```text
encoding_bom_removed
line_ending_normalized
nonsequential_cue_index
overlapping_cues
```

Warnings are sorted, deduplicated, deterministic, and repeated on each cue element exactly as required by the SRT contract.

## 3.6 Elements and regions

Each cue is one element containing:

- source ordinal;
- optional numeric cue index;
- start and end milliseconds;
- full raw cue block;
- cue-text raw and normalized text;
- exact full-block locator;
- exact cue-text locator;
- content SHA-256;
- admitted warning list.

Regions may contain only:

- encoding preamble;
- blank separators.

Together, element and region locators must partition every input byte and every decoded character exactly once.

---

# 4. Resource Isolation

SRT resources live under:

```text
parser_resources/m06a.srt.v1/
```

with a parser-scoped manifest containing exactly:

```text
config
schema
implementation
uv.lock dependency binding
```

The following D039 files must remain byte-identical:

```text
parser_resources/manifest.sha256
parser_resources/configs/m06a.text.v1.json
parser_resources/schemas/m06a.normalized-package.v1.json
src/discrepancy_desk/parsers/plain_text_v1.py
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parser_worker.py
uv.lock
pyproject.toml
```

The build may package the new SRT resource subtree, but plain-text resource loading must continue to require its original exact root manifest and tuple.

---

# 5. Worker Boundary

SRT uses a separate fixed packaged/source entrypoint:

```text
--m06a-srt-parser-worker
```

It may accept only the fixed canonical request fields already proven for the parser worker:

- protocol version;
- parser ID;
- implementation SHA-256;
- config SHA-256;
- security profile ID;
- fixed verified-input filename;
- input SHA-256 and size;
- fixed candidate-output filename.

The caller cannot select a module, Python path, entrypoint, URL, command, arbitrary filename, resource root, schema, or security profile.

The SRT worker must reuse the proven denial layer without modifying the plain-text worker module. Source and packaged execution must prove:

- network/DNS denial;
- subprocess/shell/exec denial;
- dynamic-library denial;
- credential/proxy environment clearing;
- low-level write and filesystem-mutation denial outside the operation directory;
- fixed resource and operation roots;
- receipt preservation on success and failure.

---

# 6. Vault and Product Behavior

Fresh V0004 Vault provisioning may install the exact SRT candidate as an immutable `under_test` parser definition/config/admission row alongside plain text.

Required behavior:

- zero SRT `owner_admitted` rows;
- no SRT canonical parser selection;
- no SRT admission manifest exposed;
- no SRT parse, admit, lifecycle, configuration, bulk, or background route;
- read-only parser status may show `SubRip (SRT) — under test — canonical unavailable`;
- existing D039 plain-text admission and canonical routes remain unchanged;
- no migration is required;
- no canonical package/document/element/region is persisted by candidate installation or synthetic tests.

Existing Vault retrofit or SRT owner admission is not authorized by this package.

---

# 7. Exact Changed-Path Surface

Allowed application paths:

```text
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
src/discrepancy_desk/srt_contract.py
src/discrepancy_desk/srt_service.py
src/discrepancy_desk/srt_worker.py
src/discrepancy_desk/parsers/srt_v1.py
src/discrepancy_desk/vault_service.py
src/discrepancy_desk/web.py
parser_resources/m06a.srt.v1/config.json
parser_resources/m06a.srt.v1/manifest.sha256
parser_resources/m06a.srt.v1/schema.json
tests/fixtures/m06a/parsers/srt/**
tests/test_m06a_srt_parser.py
tests/test_m06a_srt_packaging.py
tests/test_m06a_desktop_workflow.py
desktop/src/App.tsx
desktop/src/api/types.ts
```

A listed path need not change. No unlisted application path is authorized.

Explicitly forbidden:

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
plain-text admission material constants
plain-text canonical route semantics
```

If an unlisted or forbidden path is required, stop and return to owner review.

---

# 8. Exact 24-Invariant Profile

```text
M06A-SRT-001  exact D039 plain-text tuple inputs remain byte-identical
M06A-SRT-002  parser-scoped SRT manifest is complete and exact
M06A-SRT-003  fresh V0004 Vault installs only under_test SRT authority
M06A-SRT-004  no SRT admission or canonical mutation surface exists
M06A-SRT-005  valid indexed single-line cue parses
M06A-SRT-006  optional cue index parses
M06A-SRT-007  multiline cue text and blank separators preserve locators
M06A-SRT-008  nonsequential numeric indexes produce the exact warning
M06A-SRT-009  overlapping cues produce the exact warning
M06A-SRT-010  source cue order is never silently reordered
M06A-SRT-011  malformed timestamp and arrow fail closed
M06A-SRT-012  invalid fields, excessive timestamp, and negative duration fail closed
M06A-SRT-013  missing cue text or ambiguous separation fails closed
M06A-SRT-014  input, cue-count, cue-byte, line, and element limits fail closed
M06A-SRT-015  encoding contract matches the admitted subset
M06A-SRT-016  byte/character/line and cue-text coverage is independently reconciled
M06A-SRT-017  identical source workers produce byte-identical candidates/packages
M06A-SRT-018  source worker denial controls pass with no candidate on violation
M06A-SRT-019  packaged sidecar uses exact SRT resources and denial controls
M06A-SRT-020  run-specific receipt data never enters deterministic candidate/package bytes
M06A-SRT-021  under-test execution creates no Vault package/document authority
M06A-SRT-022  Tauri/parser API exposes read-only under-test status only
M06A-SRT-023  V0004 backup/restore and plain-text regressions remain green
M06A-SRT-024  no VTT, JSON, Phase 3B, provider, agent, or publication capability leaks in
```

The evidence runner must fail on missing, empty, skipped, errored, xfailed, xpassed, duplicated, or remapped required nodes.

---

# 9. Validation Contract

Required before commit:

```text
uv sync --frozen --extra dev
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
uv run pytest -o addopts= --disable-warnings -q tests/test_m06a_srt_parser.py tests/test_m06a_srt_packaging.py tests/test_m06a_parser_framework.py tests/test_m06a_parser_packaging.py tests/test_m06a_backup_restore.py tests/test_m06a_desktop_workflow.py
uv run python scripts/run_ht_evidence.py --suite m06a-srt-v1
uv run python scripts/run_ht_evidence.py --suite m06a-text-v1
uv run python scripts/run_ht_evidence.py --suite m06a-phase3a
uv run python scripts/run_ht_evidence.py --suite m06a-phase2
uv run python scripts/run_ht_evidence.py --suite m06a-phase1
uv run python scripts/run_ht_evidence.py --suite legacy
uv run alembic -c alembic.ini heads
uv run alembic -c vault_migrations/alembic.ini heads
npm --prefix desktop test
npm --prefix desktop run build
cargo test --manifest-path desktop/src-tauri/Cargo.toml
uv run python scripts/build_desktop_sidecar.py
git diff --check
```

After the implementation commit, rerun the complete closure stack on the clean commit and preserve immutable commit-bound evidence.

---

# 10. Stop Conditions

Stop and return to owner review if:

- any exact D039 tuple input changes;
- SRT requires a dependency change;
- SRT requires a migration;
- the existing normalized-package schema must be edited;
- the proven plain-text worker or parser contract must change;
- safe SRT structure cannot be represented by a parser-specific schema without changing plain text;
- candidate installation requires automatic owner admission;
- canonical SRT execution, document supersession, or existing-Vault retrofit becomes necessary;
- an unlisted path must change;
- any required validation fails and cannot be corrected inside this exact boundary.

---

# 11. Explicit Authority Block

```text
Authorized now:
- parser-scoped resource isolation for SRT
- strict internal SRT parser candidate
- separate fixed SRT worker
- synthetic source and packaged proof
- immutable under_test candidate installation in fresh V0004 Vaults
- read-only SRT status
- exact 24-invariant evidence

Not authorized:
- SRT owner admission
- canonical SRT parsing
- existing-Vault SRT retrofit
- automatic/bulk admission or parsing
- VTT or JSON
- Markdown, RSS/Atom, HTML, PDF, or Phase 3B
- Phase 4 through 6 or M06-B
- providers, network retrieval, monitoring, agents, live LLM, Qdrant, graph, purge, or publication automation
- closure of D039
- waiver of any independent review debt
```
