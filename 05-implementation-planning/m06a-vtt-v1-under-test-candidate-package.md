# M06-A `m06a.vtt.v1` Under-Test Candidate — Exact Implementation Package

## Status

```text
Package status: implemented at a96928f482f7b6c308da81061f0e8f64c6ef2966; Rust 3/3 passed; D045 self-review requires C1 correction
Preparation authority: D044
Implementation authority: D045 — exact package and exact 28-invariant profile only
Application baseline: 6a8082253a52a601291efaf3ed85ee411b04be20
Implementation commit: a96928f482f7b6c308da81061f0e8f64c6ef2966
Implementation return: 08-audits/m06a-vtt-v1-implementation-return.md
Documentation baseline: 344a3e6
Parser state if later implemented: under_test only
VTT owner admission authority: none
VTT canonical execution authority: none
SRT authority change: none
JSON/Phase 3B authority: none
Review claim: Project-Steward self-review only; D045 REQUIRES CORRECTION
```

This package defined the stdlib-first VTT candidate implemented through D045. D045-SR-01 through D045-SR-03 now block closure. The package still does not authorize admission, canonical parsing, existing-Vault retrofit, JSON, or Phase 3B.

---

# 1. Purpose

Prepare one exact, independently hashed and packaged `m06a.vtt.v1` candidate for later implementation in
`under_test` state only.

The package must:

- preserve every closed D039/D040/D041 plain-text and SRT tuple input;
- apply D041's full packaged-resource verification from the first VTT implementation;
- avoid extending the SRT-owned combined-status coupling;
- preserve WebVTT source bytes and locators without rendering or interpreting CSS/DOM semantics;
- install no canonical or owner-admitted VTT authority.

---

# 2. Normative and Project Basis

The strict subset is based on the current W3C WebVTT syntax verified during package preparation on
2026-07-23. The governing project sources remain:

```text
05-implementation-planning/m06a-parser-admission-plan.md §9
05-implementation-planning/m06a-local-manual-vault-canonical-plan.md Phase 3A
05-implementation-planning/m06a-adversarial-closure-matrix.md
05-implementation-planning/m06a-srt-v1-under-test-candidate-package.md
05-implementation-planning/m06a-srt-v1-c1-self-review-correction-package.md
08-audits/m06a-d039-d040-owner-closure.md
99-decisions/decision-log.md D039–D044
```

Where the full WebVTT format permits behavior outside this package's strict subset, this package fails
closed rather than silently accepting broader semantics.

---

# 3. Exact VTT Identity

```text
parser_id: m06a.vtt.v1
format_id: text/vtt
implementation_kind: internal
implementation_entrypoint: discrepancy_desk.parsers.vtt_v1:parse_bytes
implementation_version: 0.1.0
license_id: project-code
package_schema_version: m06a.normalized-package.vtt.v1
deterministic_contract_version: m06a.vtt-determinism.v1
warning_policy_version: m06a.vtt.warnings.v1
worker_protocol_version: m06a.vtt-worker.v1
security_profile_id: m06a.parser-worker.windows.v2
state: under_test
```

The implementation, parser-scoped resource manifest, configuration, schema, dependency lock, package
schema, deterministic contract, warning policy, and security profile form one immutable tuple. Any change
creates a new candidate identity and cannot reuse the original definition/configuration/admission IDs.

---

# 4. Exact Encoding and File Framing

## 4.1 Encoding

Admitted encodings:

```text
UTF-8
UTF-8 with U+FEFF BOM
```

Terminal failures:

- UTF-16 LE/BE, with or without BOM;
- locale or legacy encodings;
- invalid UTF-8;
- replacement-character decoding;
- NUL-bearing input;
- binary/compressed/archive content.

The BOM, when present, is preserved by an exact `encoding_preamble` region and produces the admitted
`encoding_bom_removed` warning for normalized derivative text only.

No Unicode normalization is performed.

## 4.2 Required file signature

The decoded file must begin with:

```text
WEBVTT
```

Optionally, the signature line may contain one SPACE or TAB followed by inert header text. The exact header
text is preserved and never interpreted as instructions or metadata authority.

Strict requirements:

- exact uppercase `WEBVTT` magic;
- no leading whitespace before the magic;
- optional header text must remain on the signature line;
- optional header text may not contain `-->`;
- the signature line must be followed by at least one blank logical line;
- CRLF, LF, and CR source line endings are accepted and recorded;
- a header-only file with the required blank separation is valid and emits zero cue elements.

Header extensions such as `X-TIMESTAMP-MAP` are outside this subset and fail terminally. The parser does
not infer external media timing, HLS mapping, track kind, language, rendering mode, or playback context.

---

# 5. Data Blocks

After the header separation, the strict subset permits only:

```text
NOTE blocks
cue blocks
blank separators
```

One or more blank logical lines must separate data blocks.

## 5.1 NOTE blocks

A NOTE block begins with exact `NOTE` followed by SPACE, TAB, line ending, or end of the NOTE marker line.
Its content:

- is preserved as an inert `note_block` region;
- may occur before, between, or after cues;
- may contain multiple nonblank logical lines;
- may not contain `-->`;
- is never converted into a cue, assertion, instruction, or executable content;
- is never silently omitted.

The exact initial configuration is:

```text
note_policy: preserve_inert_region
```

## 5.2 STYLE and REGION blocks

The exact initial configuration is:

```text
style_block_policy: reject
region_block_policy: reject
region_cue_setting_policy: reject
```

Any exact STYLE block, REGION block, or cue `region:` setting fails the entire candidate with no partial
output.

Rationale:

- STYLE carries CSS/rendering semantics outside the archive parser's purpose;
- REGION carries viewport/rendering semantics and cross-block references;
- preserving these as if understood would overstate parser support;
- no browser, CSS parser, DOM construction, remote resource, or rendering path is admitted.

Strings such as `STYLE` or `REGION` inside cue payload or NOTE content remain inert text and are not block
markers.

---

# 6. Cue Contract

## 6.1 Cue structure

Each cue consists of:

```text
optional cue identifier line
required timing/settings line
one or more payload lines
```

Cue identifiers:

- are optional;
- must be nonempty when present;
- may not contain `-->`, CR, or LF;
- are case-sensitive;
- must be unique within the file;
- are bounded by the configured identifier-byte limit.

A duplicate cue identifier is a terminal failure.

## 6.2 Timestamp grammar

Accepted timestamps are exactly:

```text
MM:SS.mmm
HH:MM:SS.mmm
HHH...:MM:SS.mmm
```

Rules:

- the hours field may be omitted only when the hour value is zero; zero hours may also be written explicitly as `00`;
- an hours field, when present, contains two or more ASCII digits;
- minutes and seconds contain exactly two ASCII digits and are within `00..59`;
- milliseconds contain exactly three ASCII digits;
- one or more SPACE or TAB characters must surround `-->`;
- end time must be strictly greater than start time;
- cue start times must be nondecreasing in source order;
- exact source order is preserved;
- overlapping cues are allowed and produce the exact warning `overlapping_cues`;
- timestamps beyond the configured maximum fail terminally.

The parser never silently sorts cues.

## 6.3 Cue settings

The strict parser accepts these recognized setting names:

```text
vertical
line
position
size
align
```

Recognized values must satisfy the closed grammar:

```text
vertical: rl | lr
line: integer with optional leading `-` (no `+`) or 0..100 percentage, optionally followed by ,start | ,center | ,end
position: 0..100 percentage, optionally followed by ,line-left | ,center | ,line-right
size: 0..100 percentage
align: start | center | end | left | right
```

Rules:

- each setting token is `name:value` with nonempty name and value;
- tokens are separated by one or more SPACE or TAB characters;
- a setting name may occur only once per cue;
- setting count and token bytes are bounded;
- `region:` is terminally rejected;
- malformed recognized settings fail terminally;
- an unknown but syntactically bounded setting is preserved as inert structured metadata with
  `recognized: false` and produces `unsupported_cue_setting_preserved`;
- raw setting text and source order are preserved;
- no setting is rendered or applied.

## 6.4 Cue payload

The cue payload:

- contains one or more nonblank logical lines;
- may contain ordinary Unicode, WebVTT-looking tags, entities, timestamps, or URLs as inert text;
- may not contain the substring `-->`;
- is preserved exactly as raw text;
- receives derivative normalized text with line-ending normalization only;
- is never parsed into a DOM;
- is never decoded as HTML;
- is never rendered;
- never fetches a URL or external resource.

If payload contains `<` or `&`, it produces the deterministic warning `cue_markup_preserved_inert`.
Malformed or active-looking markup remains inert text; it does not execute. The parser does not claim full
caption-component conformance.

---

# 7. Exact Initial Limits

```text
input_size_limit_bytes:            10,485,760
cue_limit:                          100,000
element_limit:                      100,000
logical_line_limit:                 300,000
region_limit:                       300,000
maximum_cue_bytes:                  1,048,576
maximum_note_bytes:                 1,048,576
maximum_header_bytes:               16,384
maximum_cue_identifier_bytes:       16,384
maximum_setting_token_bytes:        16,384
maximum_settings_per_cue:           16
maximum_timestamp_ms:               86,400,000
partial_output:                     prohibited
```

Any limit breach fails the whole candidate. No candidate, package, document, element, or region authority is
created from partial work.

---

# 8. Warning Vocabulary

Only these warnings are admitted:

```text
encoding_bom_removed
line_ending_normalized
overlapping_cues
unsupported_cue_setting_preserved
cue_markup_preserved_inert
```

Warnings are deterministic, sorted, deduplicated, and validated against source facts. No warning is emitted
merely because a NOTE block or optional header text exists.

---

# 9. Elements, Regions, and Coverage

## 9.1 Cue elements

Each cue produces one element containing at least:

```text
source ordinal
optional cue identifier
start_ms
end_ms
raw full cue block
raw timing/settings line
raw cue payload
normalized cue payload
ordered structured settings
exact full-block byte/character/line locator
exact timing-line byte/character/line locator
exact payload byte/character/line locator
content SHA-256
admitted warning list
```

## 9.2 Regions

Regions may contain only:

```text
encoding_preamble
file_header
note_block
blank_separator
```

Each region contains exact raw text/bytes, source ordinal where applicable, locator, content hash, and
region kind.

## 9.3 Reconciliation

Top-level cue-element and region locators must partition every input byte and every decoded character
exactly once.

The validator independently verifies:

- source bytes decode exactly under the declared encoding;
- byte and character locators reproduce the same source slice;
- line locators reproduce exact logical lines;
- nested timing and payload locators remain inside the cue element;
- raw cue, timing, payload, header, NOTE, separator, and BOM fields match source bytes;
- no gap, overlap, duplicated coverage, fabricated range, or omitted trailing line ending exists;
- total counts match emitted records;
- warning facts match the source.

Coverage mismatch is terminal.

---

# 10. Parser-Scoped Resources and Full Tuple Verification

VTT resources live under:

```text
parser_resources/m06a.vtt.v1/
```

The subtree contains exactly:

```text
config.json
schema.json
manifest.sha256
```

The parser-scoped manifest binds exactly:

```text
config.json
schema.json
discrepancy_desk.parsers.vtt_v1 implementation bytes
uv.lock dependency-lock bytes
```

The source and packaged VTT worker must independently establish and verify the full exact tuple before
parsing. Request-supplied hashes never authorize modified packaged resources.

Real packaged tests must prove terminal refusal with no candidate for independently modified:

```text
manifest
config, including a request carrying the modified config's own hash
schema
implementation bytes
dependency lock
```

The following closed inputs must remain byte-identical:

```text
pyproject.toml
uv.lock
migrations/**
vault_migrations/**
parser_resources/manifest.sha256
parser_resources/configs/m06a.text.v1.json
parser_resources/schemas/m06a.normalized-package.v1.json
parser_resources/m06a.srt.v1/config.json
parser_resources/m06a.srt.v1/schema.json
parser_resources/m06a.srt.v1/manifest.sha256
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parsers/plain_text_v1.py
src/discrepancy_desk/parsers/srt_v1.py
src/discrepancy_desk/srt_contract.py
src/discrepancy_desk/srt_service.py
src/discrepancy_desk/srt_worker.py
```

Adding the VTT subtree may change packaged sidecar bytes but must not change the closed text or SRT tuples.

---

# 11. Worker Boundary

VTT uses one separate fixed source/packaged entrypoint:

```text
--m06a-vtt-parser-worker
```

The fixed canonical request may contain only:

```text
protocol version
parser ID
implementation SHA-256
config SHA-256
security profile ID
fixed verified-input filename
input SHA-256
input size
fixed candidate-output filename
```

The caller cannot select:

- module, import path, or entrypoint;
- executable, command, shell, or subprocess;
- arbitrary input/output filename or directory;
- parser resource root;
- schema or dependency lock;
- URL, network destination, or browser behavior;
- actor or Vault identity;
- security profile.

The VTT worker reuses the proven Windows denial layer and must prove in source and real packaged execution:

- network/socket/DNS/HTTP denial;
- subprocess/shell/exec denial;
- dynamic-library loading denial;
- credential and proxy environment clearing;
- low-level write and filesystem mutation denial outside the operation directory;
- fixed operation and resource roots;
- content-free failure receipts;
- no candidate on failure.

The controls are application-enforced denials and are not described as an operating-system sandbox.

---

# 12. Neutral Parser-Status Aggregation

The current combined status aggregator lives in `srt_service.py`. VTT must not add another parser to an
SRT-owned aggregation function.

A new neutral service module must aggregate independently obtained status rows for:

```text
m06a.text.v1
m06a.srt.v1
m06a.vtt.v1
```

Required behavior:

- the desktop API imports the neutral aggregator;
- healthy text and SRT status behavior remains unchanged;
- a plain-text resource/manifest failure produces one safe plain-text `unavailable` row without editing `parser_service.py`;
- VTT resource absence or mismatch produces one safe VTT `unavailable` row;
- a broken text tuple cannot hide healthy SRT or VTT status;
- a broken VTT tuple cannot hide healthy text or SRT status;
- a broken SRT tuple cannot hide healthy text or VTT status;
- status contains no paths, exception text, tampered bytes, or resource locations;
- no status row creates admission or canonical authority.

The historical `srt_service.list_all_parser_status` may remain unused for compatibility in this package;
removing or refactoring it is not required and does not authorize edits to `srt_service.py`.

---

# 13. Vault and Product Behavior

Fresh V0004 Vault creation may install the exact VTT candidate as immutable `under_test` definition,
configuration, and admission rows.

Required behavior:

- zero VTT `owner_admitted` rows;
- zero VTT canonical parser selections;
- no automatic admission at migration, startup, open, package install, or upgrade;
- no existing-Vault VTT retrofit;
- historical V0003 behavior remains unchanged;
- candidate installation creates no execution, package, document, element, or region authority;
- read-only status may display `WebVTT — under test — canonical unavailable`;
- no VTT admission manifest is exposed;
- no admit, parse, lifecycle, configuration, bulk, background, watcher, provider, or agent route exists;
- no Tauri VTT mutation control exists;
- existing plain-text admission/canonical routes remain unchanged;
- SRT remains under_test with no canonical or admission authority;
- no migration is required.

Synthetic worker/package proof operates outside canonical Vault authority.

---

# 14. Deterministic Fixture Corpus

Use one deterministic binary ZIP corpus plus an exact manifest:

```text
tests/fixtures/m06a/parsers/vtt/corpus.zip
tests/fixtures/m06a/parsers/vtt/manifest.sha256
```

The ZIP must preserve exact bytes across Windows checkout and include at least:

```text
valid header-only LF/CRLF/CR
UTF-8 BOM
optional header text
single and multiline cues
short and hours-bearing timestamps
large-hours boundary and configured maximum
cue identifiers and duplicate identifiers
recognized cue settings and every valid value family
unknown inert cue setting
NOTE before, between, and after cues
cue overlap
nondecreasing equal start times
out-of-order start time
negative/zero duration
malformed magic, arrow, timestamp, spacing, and blank separation
UTF-16 and invalid UTF-8
embedded NUL and replacement character
STYLE and REGION blocks
region cue setting
X-TIMESTAMP-MAP or other unsupported header extension
empty/missing payload
payload containing `-->`
crafted tags, entities, URLs, and active-looking markup
mixed line endings
header, identifier, setting, NOTE, cue, line, element, region, and input limits
```

Test ZIP reading/extraction must be bounded and must not trust archive paths. Fixtures are test evidence only
and never canonical Vault input.

---

# 15. Exact Changed-Path Surface

Allowed application paths if the package is later accepted for implementation:

```text
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
src/discrepancy_desk/parser_status_service.py
src/discrepancy_desk/vtt_contract.py
src/discrepancy_desk/vtt_service.py
src/discrepancy_desk/vtt_worker.py
src/discrepancy_desk/parsers/vtt_v1.py
src/discrepancy_desk/vault_service.py
src/discrepancy_desk/web.py
parser_resources/m06a.vtt.v1/config.json
parser_resources/m06a.vtt.v1/manifest.sha256
parser_resources/m06a.vtt.v1/schema.json
tests/fixtures/m06a/parsers/vtt/**
tests/test_m06a_vtt_parser.py
tests/test_m06a_vtt_packaging.py
tests/test_m06a_desktop_workflow.py
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
parser_resources/m06a.srt.v1/**
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parsers/plain_text_v1.py
src/discrepancy_desk/parsers/srt_v1.py
src/discrepancy_desk/srt_contract.py
src/discrepancy_desk/srt_service.py
src/discrepancy_desk/srt_worker.py
plain-text admission material constants
plain-text canonical route semantics
SRT packaged tuple constants and route semantics
desktop/**
```

Desktop source remains unchanged because the existing parser-status UI is generic and VTT receives no
mutation control.

If an unlisted or forbidden path is required, stop and return to owner review.

---

# 16. Exact 28-Invariant Profile

```text
M06A-VTT-001  exact closed text and SRT tuple inputs remain byte-identical
M06A-VTT-002  parser-scoped VTT manifest binds config, schema, implementation, and lock exactly
M06A-VTT-003  source and packaged workers reject every full-tuple tamper class
M06A-VTT-004  fresh V0004 Vault installs only immutable under_test VTT authority
M06A-VTT-005  no VTT admission, canonical, bulk, background, or lifecycle mutation surface exists
M06A-VTT-006  exact WEBVTT signature, optional header text, and required blank separation parse
M06A-VTT-007  header-only file is valid and produces zero cue elements with complete coverage
M06A-VTT-008  UTF-8 and UTF-8 BOM succeed; UTF-16, invalid UTF-8, replacement, and NUL fail
M06A-VTT-009  valid short and hours-bearing timestamp forms parse exactly
M06A-VTT-010  end-after-start and nondecreasing start order are enforced without silent sorting
M06A-VTT-011  overlapping cues produce only the exact deterministic warning
M06A-VTT-012  cue identifiers are optional, exact, bounded, and unique
M06A-VTT-013  recognized cue settings use the exact closed value grammar
M06A-VTT-014  duplicate, malformed, region, or excessive settings fail closed
M06A-VTT-015  unknown syntactically valid settings are preserved inert with the exact warning
M06A-VTT-016  NOTE blocks are preserved as inert regions before, between, and after cues
M06A-VTT-017  STYLE, REGION, region references, and unsupported header extensions fail closed
M06A-VTT-018  cue payload is preserved inert; markup is never interpreted, rendered, or fetched
M06A-VTT-019  malformed magic, timing, arrow, separation, empty payload, or payload arrow fails
M06A-VTT-020  all input, line, cue, NOTE, header, identifier, setting, element, and region limits fail closed
M06A-VTT-021  byte, character, line, cue, timing, payload, header, NOTE, and separator coverage reconciles
M06A-VTT-022  identical isolated source workers produce byte-identical candidates and packages
M06A-VTT-023  source worker denial controls preserve failed receipts and emit no candidate
M06A-VTT-024  real packaged sidecar verifies exact resources, denial controls, and valid execution
M06A-VTT-025  run-specific receipt data never enters deterministic candidate or package bytes
M06A-VTT-026  under-test execution creates no Vault package, document, element, or region authority
M06A-VTT-027  neutral read-only status isolates text, SRT, and VTT failures from healthy parser rows
M06A-VTT-028  inherited D039/D040/D041, backup, desktop, and no-JSON/Phase3B regressions remain green
```

The evidence runner must fail on missing, empty, skipped, errored, xfailed, xpassed, duplicated, remapped,
uncollected, or unexecuted required nodes.

---

# 17. Validation Contract

Required before an implementation commit:

```text
uv sync --frozen --extra dev
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
uv run pytest -o addopts= --disable-warnings -q tests/test_m06a_vtt_parser.py tests/test_m06a_vtt_packaging.py tests/test_m06a_srt_parser.py tests/test_m06a_srt_packaging.py tests/test_m06a_text_admission.py tests/test_m06a_text_canonical_execution.py tests/test_m06a_parser_framework.py tests/test_m06a_parser_packaging.py tests/test_m06a_backup_restore.py tests/test_m06a_desktop_workflow.py
uv run python scripts/run_ht_evidence.py --suite m06a-vtt-v1
uv run python scripts/run_ht_evidence.py --suite m06a-srt-v1-c1
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

Required exact audits:

- changed paths are a subset of Section 15;
- every forbidden path is byte-identical to baseline;
- `pyproject.toml` and `uv.lock` remain byte-identical;
- migration heads remain `0005` and `V0004`;
- packaged VTT full-tuple tamper matrix passes;
- text and SRT status/authority remain unchanged;
- no production `owner_admitted` VTT row exists;
- no VTT canonical route or UI exists;
- no JSON or later capability enters.

After the implementation commit, rerun the complete closure stack on the clean commit and preserve immutable
commit-bound evidence with `working_tree_dirty: false`.

---

# 18. Review and Closure Gate

Successful implementation and green evidence do not close the package.

Before closure:

1. perform an adversarial implementation review against the exact package and commit;
2. label the review truthfully as independent or Project-Steward self-review;
3. do not substitute self-review without an explicit owner decision if the accepted implementation decision
   requires independence;
4. reproduce and disposition every material finding;
5. rerun affected evidence after correction;
6. obtain explicit owner closure.

Closure does not admit VTT or authorize canonical VTT parsing.

---

# 19. Stop Conditions

Stop and return to owner review if:

- any D039/D040/D041 closed tuple input must change;
- a dependency or migration is required;
- the normalized plain-text or SRT package schema must change;
- the proven plain-text or SRT worker/contract/service must change;
- safe status aggregation cannot be added without changing `srt_service.py`;
- STYLE, REGION, CSS, DOM, browser rendering, remote resources, or timeline mapping become necessary;
- safe VTT structure cannot be represented by a parser-scoped schema;
- candidate installation requires automatic owner admission or existing-Vault retrofit;
- canonical VTT execution, document creation, supersession, or correction authority becomes necessary;
- desktop mutation controls become necessary;
- an unlisted or forbidden path must change;
- any required validation fails and cannot be corrected inside the exact boundary.

---

# 20. Owner Review Block

This package is prepared for owner review only.

If accepted later, the owner may authorize implementation using language equivalent to:

```text
I accept the exact M06-A m06a.vtt.v1 Under-Test Candidate package and its exact 28-invariant profile.

I authorize implementation only within the package's exact changed-path surface at application baseline
6a8082253a52a601291efaf3ed85ee411b04be20.

This authorizes a strict internal VTT candidate, parser-scoped resources, a fixed source/packaged worker,
synthetic proof, fresh-V0004 under_test installation, neutral read-only status, and exact evidence only.

This does not admit VTT, authorize canonical VTT parsing, authorize existing-Vault retrofit, automatic or
bulk parsing, JSON, Phase 3B, Phase 4 through 6, M06-B, providers, agents, live LLM integration, Qdrant,
graph work, purge, or publication automation.
```

D045 grants implementation authority only inside this exact package. Admission, canonical execution, existing-Vault retrofit, and every later capability remain unauthorized.
