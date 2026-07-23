# M06-A Parser Admission Plan — Owner-Accepted Planning Baseline

## Status

```text
Status: owner-accepted planning baseline
Implementation authority: none
```

Owner acceptance approves this admission contract only. No parser is admitted by this document.

Every parser remains a candidate until its exact implementation, configuration, dependency lock, fixture corpus, packaging proof, deterministic-output proof, no-network proof, independent findings, and explicit owner admission are recorded in a machine-enforceable admission version.

Governing references:

- `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`;
- M06-D07, M06-D11, M06-D14, and M06-D16 in `99-decisions/m06-owner-architecture-rulings.md`;
- `05-implementation-planning/m06a-adversarial-closure-matrix.md`;
- `08-audits/m06a-planning-correction-disposition.md`.

---

# 1. Admission Doctrine

## 1.1 Prose is not runtime authority

This plan describes candidates. Runtime invocation is permitted only when the selected Vault database contains a current `parser_admission_versions` row whose hashes match the installed parser definition, implementation, configuration, fixture/evidence record, and packaged resource manifest.

A parser name in architecture, source code, a dependency lock, or this document does not authorize use.

## 1.2 Candidate lifecycle

```text
candidate
under_test
owner_admitted
suspended
revoked
retired
prohibited
```

Only `owner_admitted` may execute on canonical intake. `under_test` executes only in disposable synthetic test workspaces. A changed implementation version, configuration hash, package schema, dependency lock, or worker security profile returns the candidate to `under_test`.

## 1.3 Sequencing

Recommended first admission group:

```text
plain text
SRT
VTT
JSON
```

These are stdlib-first and should be admitted individually after the common worker boundary is proven.

Later individual admissions:

```text
Markdown
local RSS/Atom
local basic HTML
born-digital PDF
```

YouTube identity plus a human-supplied transcript is an intake profile, not a remote parser or provider connector.

## 1.4 Retention eligibility precedes parser eligibility

Parser admission and intake admission are separate gates, and both must pass. Before parser selection or worker launch, the trusted service must prove that:

- the selected physical Vault and its SQLite database identity are verified;
- the input belongs to that same Vault;
- the intake was classified `preservation_compatible`;
- no timed-deletion requirement is present;
- retention eligibility is not missing, contradictory, expired, or unknown;
- an immutable admitted acquisition-artifact link exists.

Timed-deletion-required or unknown-retention material is rejected before canonical byte admission. It cannot enter `parser_executions`, worker input, normalized packages, document versions, search, chunks, projections, dossiers, context runs, or backup content. A safe rejection receipt may record content-free reason and policy metadata, but cannot retain supplied bytes, pasted content, extracted text, filename text that discloses content, parser diagnostics derived from the rejected material, or a cryptographic hash derived from any rejected content. A digest can confirm possession of known content and is not automatically content-free.

A parser cannot cure an inadmissible retention state, and a promise to delete parser output later cannot bypass rejection.

---

# 2. Machine-Enforceable Admission Records

## 2.1 Parser definition

```text
parser_definitions
  id TEXT PRIMARY KEY
  vault_account_id TEXT NOT NULL
  format_id TEXT NOT NULL
  implementation_kind TEXT NOT NULL CHECK(stdlib, internal, external_python, native)
  implementation_entrypoint TEXT NOT NULL
  implementation_version TEXT NOT NULL
  implementation_sha256 TEXT NOT NULL
  dependency_lock_sha256 TEXT NOT NULL
  license_id TEXT NOT NULL
  package_schema_version TEXT NOT NULL
  deterministic_contract_version TEXT NOT NULL
  security_profile_id TEXT NOT NULL
  created_at TEXT NOT NULL
```

## 2.2 Configuration version

```text
parser_configuration_versions
  id TEXT PRIMARY KEY
  vault_account_id TEXT NOT NULL
  parser_definition_id TEXT NOT NULL REFERENCES parser_definitions(id)
  canonical_config_json BLOB NOT NULL
  config_sha256 TEXT NOT NULL UNIQUE
  size_limit_bytes INTEGER NOT NULL
  depth_limit INTEGER
  element_limit INTEGER
  page_or_cue_limit INTEGER
  warning_policy_version TEXT NOT NULL
  created_at TEXT NOT NULL
```

## 2.3 Admission version

```text
parser_admission_versions
  id TEXT PRIMARY KEY
  vault_account_id TEXT NOT NULL
  parser_definition_id TEXT NOT NULL REFERENCES parser_definitions(id)
  parser_configuration_version_id TEXT NOT NULL REFERENCES parser_configuration_versions(id)
  state TEXT NOT NULL CHECK(candidate, under_test, owner_admitted, suspended, revoked, retired, prohibited)
  fixture_manifest_sha256 TEXT NOT NULL
  focused_test_evidence_sha256 TEXT NOT NULL
  no_egress_evidence_sha256 TEXT NOT NULL
  packaged_sidecar_evidence_sha256 TEXT NOT NULL
  dependency_lock_sha256 TEXT NOT NULL
  admitted_by_actor_id TEXT
  admitted_at TEXT
  supersedes_admission_id TEXT REFERENCES parser_admission_versions(id)
  reason TEXT NOT NULL
```

Runtime performs an exact match over all applicable hashes and state. It rejects ambiguous multiple-current admissions.

## 2.4 Runtime invocation receipt

Every `parser_executions` row is stored in the selected physical Vault database and binds:

```text
vault account ID and Vault instance ID
parser definition ID
configuration version ID
admission version ID
worker security profile ID
input acquisition-artifact links
input hashes
execution outcome
warning codes
run-specific timestamps and diagnostics
resulting deterministic package hash, if any
```

All parser definitions, configurations, admissions, executions, packages, and document versions carry same-Vault identity and composite foreign-key/service validation. No parser record or package output may be linked into another Vault, even when bytes or parser hashes are identical.

## 2.5 Rejected-intake receipt boundary

A retention rejection is not a parser execution. It creates no parser row and launches no worker. The content-free intake receipt may preserve only the selected Vault ID, trusted actor, operation/correlation ID, safe source-kind classification, sanitized non-content descriptor, retention classification, policy-basis reference, client-generated operation nonce, reason code, and timestamp.

No rejection receipt, parser-adjacent audit event, operation key, log, or retained diagnostic may contain an input hash or any value derived from rejected bytes or pasted text. Rejection-path idempotency is based only on the content-free fields above. The service must not compute a parser input hash merely to reject retention-ineligible material.

## 2.6 Parser quarantine contract

Parser quarantine is a governed database state over an existing immutable artifact, parser execution, or normalized-package candidate. It does not create a separate canonical filesystem root. A quarantined record is ineligible for document-version creation, search, chunks, projections, dossiers, context use, export, or backup until a governed resolution supersedes or clears the state.

Worker output that has not passed admission, determinism, coverage, and security validation may exist only in `temp/<operation-id>/temporary-quarantine/`. That directory is non-canonical, operation-scoped, excluded from backup, and inaccessible to ordinary Vault reads. Before operation closure it must be admitted into the ordinary content-addressed package path or destroyed with content-free reconciliation. Unresolved temporary bytes keep the operation blocked and never become authority.

---

# 3. Common Warning and Failure Vocabulary

## 3.1 Warnings

Closed warning codes include:

```text
encoding_bom_removed
encoding_replacement_disallowed
line_ending_normalized
trailing_blank_region_ignored
unsupported_metadata_ignored
unsupported_markup_preserved_as_text
cue_overlap_observed
cue_order_normalized
json_number_precision_risk
html_hidden_content_excluded
html_active_content_removed
pdf_font_mapping_uncertain
pdf_reading_order_uncertain
limit_truncation_prohibited
```

`limit_truncation_prohibited` is never a successful warning; it converts the run to terminal failure. Warning definitions state whether search, assertion evidence, quotation, and context-run use remain eligible.

## 3.2 Terminal outcomes

```text
succeeded
succeeded_with_warnings
unsupported_format
malformed_input
encoding_failure
limit_exceeded
partial_output_failure
security_boundary_violation
determinism_failure
dependency_failure
packaging_mismatch
internal_error
```

Only the first two may produce a document version, and only when the warning policy explicitly permits it. A crash, incomplete coverage, unresolved external entity, encrypted PDF, scanned PDF, or silent parser exception never produces a canonical version.

## 3.3 Encoding

- Exact original bytes remain canonical.
- Parser input decoding is deterministic and recorded.
- UTF-8 is preferred.
- UTF-8 BOM may be removed with a warning.
- For plain text and subtitle formats, UTF-16 LE/BE may be accepted only with an explicit BOM and exact recorded decoding.
- Locale-dependent decoding is prohibited.
- Replacement-character decoding is prohibited unless the original already contains that scalar value and byte mapping is proven.
- Output text uses Unicode scalar strings and canonical JSON serialization; no hidden normalization is applied unless a configuration explicitly records it.

---

# 4. Parser Worker and No-Network Boundary

## 4.1 Process boundary

Parsers execute in a short-lived, fixed-entrypoint worker process started by the trusted service only after both retention admission and parser admission succeed. The worker receives:

- a verified read-only input file or bounded input bytes from an admitted acquisition-artifact link in the selected Vault;
- an operation-scoped output directory under `temp/<operation-id>/temporary-quarantine/` until all validation passes;
- an exact parser/config/admission tuple;
- no arbitrary module, command, URL, or filesystem path from the request.

The worker output directory is inside the selected Vault's bounded operation workspace. The parent validates Vault identity, outputs, hashes, coverage, warning vocabulary, and deterministic package bytes before any canonical link is created. Output cannot be moved or linked into another Vault.


## 4.2 Python-level denial controls

Before parser code loads, the worker:

- clears proxy and credential environment variables;
- replaces `socket.socket`, `socket.create_connection`, and `socket.getaddrinfo` with denying implementations;
- installs a `sys.addaudithook` policy that rejects socket, DNS, subprocess, shell, dynamic-library, and unapproved filesystem events;
- rejects `subprocess.Popen`, `os.system`, shell execution, and arbitrary executable launch;
- blocks `urllib`, HTTP-client, FTP, SMTP, and similar client use through import and audit policy;
- permits reads only from the exact input and packaged parser resources;
- permits writes only inside the operation output directory;
- sets deterministic locale/timezone/hash-seed behavior where applicable;
- applies wall-clock, memory, output-size, recursion, node, page, cue, and element limits.

This is defense in depth, not a claim that monkeypatching alone is an OS sandbox.

## 4.3 XML/HTML/PDF resolver controls

- XML candidates reject `DOCTYPE`, entity declarations, XInclude, processing instructions that imply external resources, and unresolved external references before parse.
- HTML parsing does not fetch images, stylesheets, scripts, frames, links, or metadata URLs.
- External libraries must expose and use no-network/no-external-resolver configuration.
- A native dependency is not admitted until focused tests prove it does not spawn helpers, load unapproved plugins, or resolve external resources.

## 4.4 Canary proof

The adversarial fixture corpus includes a malicious parser/test double attempting:

```text
socket.socket
socket.create_connection
socket.getaddrinfo
urllib request
HTTP client import/use
subprocess launch
os.system
external XML entity
HTML external resource
native resolver callback where applicable
```

Every attempt must yield `security_boundary_violation`, no outbound connection, no canonical package, a preserved worker receipt, and no partial canonical write.

If the packaged Windows environment cannot prove the no-egress boundary for a candidate, that parser remains unadmitted.

---

# 5. Determinism and Coverage Contract

For each candidate:

- parse the same fixture at least twice in separate worker processes;
- require byte-identical canonical package output;
- require identical element order, locators, raw/normalized distinctions, warnings, and coverage declaration;
- exclude execution timestamps, host details, process IDs, random IDs, and absolute paths from package bytes;
- preserve run-specific data only in `parser_executions`;
- fail if the parser reports fewer elements/pages/cues/nodes than its declared coverage without a terminal outcome;
- fail if warnings differ across identical reruns;
- fail if an input or output hash is absent.

---

# 6. Candidate Summary

| Parser ID | Format | Proposed implementation | License | Candidate state | Sequence |
|---|---|---|---|---|---|
| `m06a.text.v1` | plain text | internal stdlib decoder/line-region parser | project code | candidate | Phase 3A |
| `m06a.srt.v1` | SRT | internal strict state-machine parser | project code | candidate | Phase 3A |
| `m06a.vtt.v1` | WebVTT | internal strict subset parser | project code | candidate | Phase 3A |
| `m06a.json.v1` | JSON | Python stdlib `json` with duplicate-key/depth controls | PSF | candidate | Phase 3A |
| `m06a.markdown.v1` | Markdown | candidate — implementation choice unresolved | unresolved | candidate | Phase 3B |
| `m06a.feed.v1` | local RSS/Atom | internal guarded `xml.etree.ElementTree` subset | PSF/project | candidate | Phase 3B |
| `m06a.html.v1` | local basic HTML | internal `html.parser.HTMLParser` extractor | PSF/project | candidate | Phase 3B |
| `m06a.pdf.v1` | born-digital PDF | candidate — implementation choice unresolved | unresolved | candidate | Phase 3B |
| `m06a.youtube-transcript.v1` | YouTube locator + supplied transcript | identity profile plus admitted text/SRT/VTT parser | n/a | candidate | after underlying parser admission |

No row in this table is an admission.

---

# 7. Plain Text Candidate — `m06a.text.v1`

## Exact proposal

- Implementation: internal Python module using stdlib byte decoding and deterministic line/paragraph segmentation.
- Version policy: implementation SHA-256 plus semantic parser version; any code/config change creates a new definition.
- License: project code.
- Supported subset: UTF-8, UTF-8 BOM, and explicitly BOM-marked UTF-16 LE/BE; CRLF/LF/CR recorded and normalized only in the derivative.
- Excluded subset: binary/NUL-heavy files, locale encodings, undecodable input, compressed/archive content.
- Limits: 10 MiB input; 1,000,000 characters; 100,000 lines; 50,000 elements; maximum line 1 MiB.
- Element model: paragraph elements with exact byte and character ranges; blank separators recorded as regions when needed for locator fidelity.
- Warnings: BOM removal and line-ending normalization only.
- Partial failure: prohibited.
- Terminal failure: decoding, limit, NUL/binary heuristic, coverage, or determinism failure.
- Fixture corpus: UTF-8 ASCII/Unicode, BOM variants, CRLF/LF/CR, empty file, huge line, embedded NUL, invalid UTF-8, zero-width characters, combining marks.
- Packaging proof: source and package hashes included in sidecar evidence.
- Runtime admission: candidate.
- Owner admission: required after focused and no-egress proof.

---

# 8. SRT Candidate — `m06a.srt.v1`

## Exact proposal

- Implementation: internal strict state-machine parser; no external library.
- Version policy: implementation/config/package-schema hash bound.
- License: project code.
- Supported subset: numeric cue index optional for tolerance; `HH:MM:SS,mmm --> HH:MM:SS,mmm`; multiline cue text; blank-line separation.
- Excluded subset: embedded media, remote resources, arbitrary HTML execution, malformed timestamps, negative durations.
- Limits: 10 MiB; 100,000 cues; 1 MiB per cue; 24-hour maximum individual timestamp unless configuration explicitly raises it.
- Encoding: same as plain text candidate.
- Warning behavior: non-sequential numeric indexes and overlapping cues may be explicit warnings; cue order in output is deterministic by source ordinal, not silently reordered unless an admitted configuration says so.
- Partial failure: prohibited; one malformed cue fails the document.
- Terminal failure: malformed timestamp, missing separator, limit, decoding, coverage, determinism, or security failure.
- Elements: one cue per element with source ordinal, exact text range, start/end milliseconds, raw text, normalized text.
- Fixture corpus: valid multiline, overlap, duplicate index, missing index, malformed arrow, out-of-order times, giant cue, mixed newline, adversarial markup.
- Packaging proof and owner admission: required.

---

# 9. WebVTT Candidate — `m06a.vtt.v1`

```text
Exact package: 05-implementation-planning/m06a-vtt-v1-under-test-candidate-package.md
Package preparation: accepted through D044
Implementation authority: none
```

## Exact proposal

- Implementation: internal strict WebVTT subset parser.
- License: project code.
- Supported subset: required `WEBVTT` signature; cue identifiers; standard cue timestamps; cue settings preserved as structured untrusted metadata; NOTE blocks recorded or ignored according to exact config.
- Excluded subset: STYLE and REGION semantics beyond inert preservation; scripting; remote resources; browser rendering.
- Limits: 10 MiB; 100,000 cues; 1 MiB per cue; bounded header/metadata length.
- Encoding: UTF-8 or UTF-8 BOM only, matching WebVTT expectations.
- Warnings: unsupported inert settings preserved; cue overlap; NOTE omission only when config declares it.
- Partial failure: prohibited.
- Terminal failure: missing signature, malformed timestamp, unsupported active construct, limit, coverage, determinism, or security failure.
- Elements: cue ID, ordinal, start/end milliseconds, settings as closed key/value data, raw/normalized text, exact ranges.
- Fixture corpus: valid cues, identifiers, NOTE, STYLE/REGION rejection, malformed signature, Unicode, overlap, huge cue, crafted tags.
- Packaging proof and owner admission: required.

---

# 10. JSON Candidate — `m06a.json.v1`

## Exact proposal

- Implementation: Python stdlib `json` with `object_pairs_hook` rejecting duplicate keys and custom pre/post traversal limits.
- License: Python Software Foundation.
- Supported subset: one RFC-style JSON value encoded as UTF-8; objects, arrays, strings, booleans, null, and finite numbers.
- Excluded subset: JSON Lines, comments, trailing commas, NaN, Infinity, duplicate keys, arbitrary schema inference, embedded executable content.
- Limits: 10 MiB; depth 100; 1,000,000 aggregate scalar/container nodes; key length 16 KiB; string length 1 MiB; output elements 100,000.
- Number behavior: original numeric token text is retained where exact lexical representation matters; parsed numeric value is derivative. Non-finite values fail.
- Warning: `json_number_precision_risk` only when a consumer-facing numeric interpretation would exceed the admitted exact range; package still retains raw token.
- Partial failure: prohibited.
- Terminal failure: duplicate key, non-finite number, malformed JSON, depth/node/string/size limit, coverage, determinism, or security failure.
- Elements: deterministic JSON Pointer locators with escaping; object member order follows source order in elements while canonical package object keys remain sorted.
- Fixture corpus: nested objects/arrays, duplicate keys, huge integer, exponent, Unicode keys, escaped pointer chars, deep nesting, node bomb, NaN/Infinity, trailing data.
- Packaging proof and owner admission: required.

---

# 11. Markdown Candidate — `m06a.markdown.v1`

```text
candidate — implementation choice unresolved
```

The final implementation requires focused dependency and behavior admission research. The candidate must choose exactly one implementation before owner review; alternatives cannot remain runtime options.

Required choice criteria:

- deterministic block/token tree with source-position support sufficient for exact locators;
- CommonMark subset declared explicitly;
- raw HTML either preserved as inert text/element or rejected according to configuration;
- no link/image fetch;
- no plugins, includes, executable directives, diagram renderers, syntax highlighter subprocesses, or extension auto-discovery;
- dependency version and license locked in `uv.lock`;
- packaged-sidecar inclusion and no-egress proof;
- stable source maps across Windows/source execution.

Provisional limits:

- 10 MiB input;
- depth 100;
- 100,000 tokens/elements;
- 1 MiB code block;
- 100,000-character link destination cap far below input maximum, refined before admission.

Terminal failures include unsupported extension/directive, source-position loss, silent raw-HTML execution, limit, packaging, determinism, or egress failure.

Until the exact choice is approved, Markdown remains unadmitted even if a library happens to be installed.

---

# 12. Local RSS/Atom Candidate — `m06a.feed.v1`

## Exact proposal

- Implementation: internal guarded subset using stdlib `xml.etree.ElementTree` after byte-level preflight.
- License: Python Software Foundation/project code.
- Input is a local file only. No URL dereference occurs.
- Supported subset: RSS 2.0 channel/item basics and Atom 1.0 feed/entry basics; title, ID/GUID, link values as inert locators, author text, publication/update text, summary/content as inert text/markup payload.
- Excluded subset: `DOCTYPE`, entity declarations, XInclude, external schemas, processing instructions requiring external behavior, arbitrary extension semantics, polling.
- Limits: 10 MiB; depth 64; 100,000 XML nodes; 25,000 entries; 1 MiB text node; bounded attributes.
- Namespace behavior: exact admitted RSS/Atom namespaces; unknown extensions preserved as inert namespaced metadata only if under limits, otherwise warning/failure policy.
- Warning behavior: unsupported metadata ignored with explicit path codes; missing optional fields are explicit; malformed required entry identity fails that entry unless a later admission version explicitly permits a governed database-state exclusion with complete coverage reporting—otherwise terminal failure.
- Partial failure default: prohibited for initial admission. A later admission version may add explicit entry-level governed exclusion after separate tests; it must not create an uncontrolled filesystem quarantine or silently produce a complete package.
- Elements: feed/channel metadata plus entry elements and child regions with XML path/source-order locators.
- Fixture corpus: RSS, Atom, mixed namespaces, empty feed, duplicate GUID/ID, entity/DOCTYPE/XXE attempts, depth bomb, node bomb, external URL values, malformed date text.
- Packaging and owner admission: required.

---

# 13. Local Basic HTML Candidate — `m06a.html.v1`

## Exact proposal

- Implementation: internal extractor based on stdlib `html.parser.HTMLParser`; no browser, DOM engine, CSS engine, or JavaScript runtime.
- License: Python Software Foundation/project code.
- Supported subset: title, headings, paragraphs, list items, table cells, blockquotes, pre/code text, link text and inert href locator, selected metadata; deterministic source order.
- Excluded/removed: script, style, iframe, object, embed, canvas behavior, event handlers, forms, active content, external resources, CSS layout, rendered visibility claims.
- Limits: 10 MiB; nesting depth 256; 200,000 start/end/data events; 100,000 emitted elements; 1 MiB text event; bounded attribute count/value length.
- Encoding: UTF-8 preferred; a declared BOM or validated HTML charset may be used only under a deterministic admitted algorithm; conflicting declarations fail.
- Warning behavior: active content removal and unsupported markup are explicit; the parser may not claim rendered-page completeness.
- Partial failure: prohibited. Error-tolerant HTML parsing must still declare complete source-event consumption.
- Terminal failure: resource/resolver attempt, limit, encoding conflict, unclosed structure beyond admitted tolerance, coverage, determinism, or security failure.
- Elements: structural source-order units with exact character/byte range when available; raw and text-only representations remain distinct.
- Fixture corpus: simple documents, malformed-but-admitted HTML, scripts/styles/iframes, event handlers, external images/CSS, entity text, deep nesting, tag soup, huge attribute, active SVG/MathML cases.
- Packaging and owner admission: required.

The stdlib parser is a source extractor, not a standards-complete browser parser. If tests show source-location or tolerance behavior is insufficient, the implementation choice returns to unresolved rather than silently adding `lxml`, `html5lib`, or a renderer.

---

# 14. Born-Digital PDF Candidate — `m06a.pdf.v1`

```text
candidate — implementation choice unresolved
```

PDF requires focused admission research because external/native behavior, encryption, object streams, font mapping, reading order, attachments, actions, and packaged dependencies materially affect security and determinism.

Required pre-admission result:

- exactly one locked library/version/license;
- no OCR path;
- reject encrypted/password-protected PDFs;
- reject PDFs with no extractable text as scanned/unsupported;
- disable or ignore JavaScript, launch actions, embedded files, external streams, forms, multimedia, and remote references;
- no helper subprocess or external resolver;
- deterministic page-by-page extraction with page and region/character locators;
- explicit font-mapping and reading-order warnings;
- page count, object count, decompressed-size, text-size, recursion, and time limits;
- sidecar packaging and native-library inventory;
- malicious corpus covering malformed xref, object bombs, attachments, actions, encryption, font confusion, and image-only pages.

Provisional limits pending exact library research:

```text
input size             50 MiB
pages                  2,000
extracted text         20 MiB
objects                1,000,000
single page text       2 MiB
```

These are ceilings for review, not admitted values. PDF remains unadmitted until exact limits and implementation are owner-approved.

---

# 15. YouTube Identity Plus Human-Supplied Transcript

Parser ID/profile:

```text
m06a.youtube-transcript.v1
```

This is not a network connector and performs no YouTube call.

The governed intake records:

- inert YouTube URL as source-item/occurrence identity;
- human observation that the locator was supplied;
- human-supplied transcript as pasted text or a local TXT/SRT/VTT artifact;
- transcript provenance class such as owner_supplied, manually_copied, or owner_export;
- no claim that title, channel, video availability, caption origin, or transcript accuracy was remotely verified;
- timing precision and speaker labels only when present in the supplied artifact.

The transcript is parsed only through an independently owner-admitted underlying parser. A YouTube URL alone produces no acquisition artifact or document version.

---

# 16. Fixture and Evidence Contract

Each parser admission has a versioned fixture manifest containing:

```text
fixture ID
license/provenance
input SHA-256
format/classification
expected terminal outcome
expected warning codes
expected element count/coverage
expected locator samples
expected package SHA-256
security purpose
```

Fixture classes:

- clean known-valid;
- edge but admitted;
- malformed;
- hostile/security;
- limit boundary;
- deterministic rerun;
- packaging-only;
- egress canary;
- silent partial-output detector.

Evidence outputs include focused pytest evidence, worker receipt hashes, no-egress canary evidence, package determinism evidence, dependency/lock hash, license record, and packaged sidecar proof.

Real third-party/private material is not required for parser admission. Use synthetic, owned, or public-domain fixtures.

---

# 17. Packaging and Dependency Controls

## 17.1 Existing environment

The current application lock contains no dedicated Markdown or PDF parser. Adding an external parser requires deliberate changes to:

```text
pyproject.toml
uv.lock
scripts/build_desktop_sidecar.py
packaging tests
license inventory
```

## 17.2 Sidecar proof

The packaged executable must prove:

- parser module/resource exists;
- exact implementation and dependency hashes match admission;
- the Vault migration resources are present;
- FTS5 is available;
- the worker launches without network or arbitrary subprocess authority;
- a clean fixture parses to the expected package hash;
- a malicious egress fixture fails closed;
- no unapproved plugin, parser, resolver, or executable is discoverable.

## 17.3 Version policy

No floating runtime selection is allowed. Dependency upgrades create new parser definitions and require focused re-admission. Historical packages retain the exact old parser/config/admission identity.

---

# 18. Admission Checklist

A parser may reach `owner_admitted` only when all are true:

1. exact implementation and entrypoint selected;
2. exact dependency/version/license recorded;
3. supported and excluded subsets fixed;
4. exact limits fixed;
5. encoding behavior fixed;
6. warning/failure vocabulary fixed;
7. deterministic package hashes proven;
8. silent partial output detector proven;
9. hostile fixtures pass fail-closed expectations;
10. no-network controls and canary proof pass in source and packaged sidecar;
11. package/resource hashes match;
12. fixture and evidence manifests are complete;
13. parser admission row is created by a verified human actor;
14. no open Critical/High parser finding remains;
15. owner explicitly authorizes that parser/version/config;
16. runtime proves selected-Vault identity and same-Vault output binding;
17. intake retention eligibility is `preservation_compatible` before worker launch;
18. timed-deletion-required and unknown-retention fixtures prove no parser execution or package is created.

Bulk admission by milestone acceptance is prohibited.

---

# 19. Explicit Authority Block

```text
Status: owner-accepted planning baseline
Implementation authority: none

No parser is admitted.

This plan does not authorize parser code, dependency installation,
worker execution, fixture execution, network access, provider use,
application changes, migrations, databases, Vault creation, staging,
commit, push, M06-B work, or implementation.
```
