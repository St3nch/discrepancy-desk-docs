# R-M06-04 — Document Normalization and Parser Provenance

## Research status

Research report completed on 2026-07-20.

This report is source material for M06 architecture synthesis. It does not authorize implementation, parser selection, OCR selection, SaaS admission, schema adoption, Vault structure, Qdrant indexing, or any production ingestion behavior.

All parser, format, schema, and normalization options remain subject to owner review during architecture synthesis.

## Question and project boundary

How should The Discrepancy Desk transform heterogeneous inputs—HTML, PDFs, transcripts, images, screenshots, JSON, feeds, Markdown, plain text, and future document formats—into governed normalized records without losing original bytes, layout, source location, parser provenance, uncertainty, or the ability to reprocess later?

The report covers:

- immutable originals and exact supplied inputs;
- media-type detection and mismatch handling;
- parser selection as a governed activity;
- normalized document elements;
- page, region, block, line, word, and timestamp locators;
- OCR and speech-derived text;
- parser warnings, confidence, and failure states;
- schema validation;
- human-readable projections;
- parser replacement and reprocessing;
- security and hostile-document boundaries;
- future retrieval compatibility.

It does not design database tables or implementation code.

---

# 1. Executive findings

## 1.1 The original must remain canonical evidence

**PROJECT RECOMMENDATION**

Every ingestion should preserve the exact original input or exact human-supplied artifact before any parsing, cleaning, conversion, OCR, transcription, summarization, or chunking.

A normalized document is a derivative observation produced by a parser. It is not a replacement for the source artifact.

Recommended conceptual flow:

```text
original artifact
    ↓
media-type and integrity inspection
    ↓
parser execution record
    ↓
normalized element package
    ↓
human-readable projection
    ↓
knowledge candidates
    ↓
retrieval chunks later
```

## 1.2 Normalization should be element-based, not text-only

**PROJECT RECOMMENDATION**

The common interchange model should preserve document elements such as:

```text
title
heading
paragraph
list item
table
caption
quotation
code block
link
image region
page break
transcript segment
feed entry
metadata field
unknown region
```

Flattening immediately to plain text or Markdown loses information needed for:

- precise provenance;
- reading-order correction;
- table reconstruction;
- page citations;
- visual-region review;
- transcript timestamps;
- parser comparison;
- later deterministic chunking.

## 1.3 Parser provenance is mandatory

**PROJECT RECOMMENDATION**

Every normalized derivative should record at least:

```text
parser name
parser version
parser configuration
execution time
input artifact identity and hash
output package identity and hash
media type observed and detected
warnings and errors
OCR/transcription method where applicable
language settings
reading-order strategy
human corrections
```

The system should be able to answer:

> Which parser, version, and settings produced this exact text from which exact artifact?

## 1.4 A parser output is an observation, not truth

**PROJECT RECOMMENDATION**

OCR, layout analysis, table extraction, transcript segmentation, and metadata extraction must be treated as fallible observations.

Examples:

```text
parser extracted author = Jane Doe
OCR read year = 1988
layout parser classified block = table
speaker diarizer labeled segment = Speaker B
```

None becomes accepted knowledge without later review and governance.

## 1.5 Retrieval chunking must remain downstream

**PROJECT RECOMMENDATION**

The canonical normalized package should preserve semantic and structural elements. Retrieval chunks should be generated later from those elements for a particular retrieval model and can be destroyed and rebuilt.

Do not force the canonical representation to match one embedding model's token limit.

---

# 2. Verified standards and current tool capabilities

## 2.1 HTTP media types

**VERIFIED FACT**

RFC 9110 defines `Content-Type` as the media type of an HTTP representation and notes that absent or incorrect media types may require cautious examination of the content.

**PROJECT IMPLICATION**

The system should preserve separately:

```text
server_declared_media_type
filename_extension
locally_detected_media_type
detection_method
mismatch_state
```

A server declaring `text/plain` does not prove the bytes are safe plain text. A `.pdf` extension does not prove the file is a valid PDF.

## 2.2 JSON Schema

**VERIFIED FACT**

JSON Schema 2020-12 provides a standard vocabulary for describing and validating JSON structure and constraints.

**PROJECT IMPLICATION**

Versioned JSON Schema is a strong candidate for validating normalized packages and manifests, but schema validation only proves structural conformance. It does not prove extracted content is accurate.

## 2.3 W3C PROV-O

**VERIFIED FACT**

W3C PROV-O provides a stable model for provenance involving entities, activities, and agents.

**PROJECT IMPLICATION**

The project does not need to adopt RDF or PROV-O directly to benefit from its core distinction:

```text
entity   = source artifact or derived package
activity = parsing, OCR, normalization, correction
agent    = human, parser, provider, or system component
```

A lightweight internal model should preserve equivalent relationships.

## 2.4 Element-based partitioning

**SOURCE/PROVIDER CLAIM**

Unstructured documents that its partitioning process can produce semantic elements such as titles, narrative text, and list items, and that chunking can occur after partitioning using whole elements where possible.

**PROJECT IMPLICATION**

This supports the architecture pattern of:

```text
partition first
chunk later
```

It does not approve Unstructured as the project parser.

## 2.5 Broad file-type extraction

**SOURCE/PROVIDER CLAIM**

Apache Tika describes itself as a toolkit for detecting and extracting metadata and structured text from more than a thousand file types through underlying parser libraries.

**PROJECT IMPLICATION**

A broad fallback detector/extractor may be useful for file identification or unsupported formats, but broad format coverage does not ensure high-quality layout, table, image, or provenance extraction.

## 2.6 PDF text and geometry

**VERIFIED FACT / SOURCE CAPABILITY**

PyMuPDF exposes text extraction at multiple levels including plain text, blocks, words, structured dictionaries, JSON, HTML, and character-level structures. Block and word outputs can include bounding boxes and ordering metadata.

**PROJECT IMPLICATION**

PDF normalization should preserve geometry when available instead of keeping only flattened page text.

## 2.7 Tagged PDF limitations

**VERIFIED FACT**

The PDF Association notes that many PDFs do not follow tagged-PDF best practices.

**PROJECT IMPLICATION**

The system cannot assume logical reading order, headings, tables, or accessibility structure are present even when a PDF visually appears well formed.

---

# 3. Required conceptual objects

## 3.1 Original artifact

The exact bytes or exact supplied text before transformation.

Candidate attributes:

```text
artifact_id
account_id
acquisition_id
original_filename
source_url
observed_media_type
declared_media_type
detected_media_type
byte_size
sha256
created_or_observed_at
storage_locator
retention_class
```

For pasted text, the exact UTF-8 representation and line endings used at admission should be preserved.

## 3.2 Parser execution

A record of one attempt to transform an artifact.

Candidate attributes:

```text
parser_execution_id
artifact_id
parser_name
parser_version
parser_build_or_model
configuration_hash
started_at
completed_at
status
warnings
error_class
resource_profile
output_package_id
```

Multiple parser executions may exist for the same artifact.

## 3.3 Normalized document package

A versioned derivative containing metadata, elements, locators, relationships, and warnings.

Candidate top-level shape:

```text
schema_version
document_id
document_version_id
source_artifact_id
parser_execution_id
document_kind
language_observations
metadata_observations
elements
relationships
warnings
package_sha256
```

## 3.4 Normalized element

A unit of source-derived structure.

Candidate attributes:

```text
element_id
element_type
ordinal
parent_element_id
text_raw
text_normalized
source_locator
bounding_region
time_range
page_number
language
confidence
parser_labels
warnings
content_sha256
```

`text_raw` and `text_normalized` should not be conflated.

## 3.5 Human correction

A separate governed record that changes the preferred interpretation without erasing the parser output.

Candidate attributes:

```text
correction_id
target_element_id
original_value
corrected_value
field_name
reason
evidence_reference
actor_id
created_at
superseded_by
```

---

# 4. Common normalization envelope

**PROJECT RECOMMENDATION**

All supported formats should eventually produce a common package envelope while retaining source-specific extensions.

Candidate envelope:

```json
{
  "schema_version": "candidate-1",
  "document_id": "doc_...",
  "document_version_id": "docver_...",
  "account_id": "acct_...",
  "source_artifact_id": "artifact_...",
  "parser_execution_id": "parse_...",
  "document_kind": "pdf",
  "media_type": {
    "declared": "application/pdf",
    "detected": "application/pdf",
    "mismatch": false
  },
  "metadata_observations": [],
  "elements": [],
  "relationships": [],
  "warnings": [],
  "status": "parsed_with_warnings"
}
```

This is a research candidate, not an approved schema.

## 4.1 Required status vocabulary candidate

```text
pending
inspection_failed
unsupported
quarantined
parse_failed
parsed
parsed_with_warnings
human_review_required
accepted_for_knowledge_review
superseded
```

A successful parser process must not automatically mean `accepted_for_knowledge_review`.

## 4.2 Source locators

Every element should carry the most precise available locator.

Examples:

```text
PDF: page, bounding box, block, line, word indexes
HTML: canonical URL, DOM/XPath-like locator, heading path
Transcript: start/end time, segment ordinal
Image: pixel bounding box or whole image
Feed: feed ID, entry ID, content field
JSON: JSON Pointer
Markdown: heading path and line range
Plain text: line and character range
```

Locators must be described as parser-generated unless they are native stable identifiers.

---

# 5. Format-specific research findings

## 5.1 HTML and webpages

A normalized HTML package should distinguish:

```text
raw response bytes
response headers
DOM representation
main-content extraction
metadata extraction
links
embedded-media references
structured-data blocks
human-readable projection
```

Candidate element types:

```text
page_title
heading
paragraph
list_item
quotation
code_block
table
image_reference
caption
navigation
boilerplate
unknown
```

**PROJECT RECOMMENDATION**

Boilerplate removal should never delete the preserved raw response. The parser should mark excluded regions and rationale where feasible.

**OPEN QUESTION**

Whether DOM locators should use XPath, CSS selectors, structural hashes, heading paths, or a combination requires later testing because web layouts change.

## 5.2 PDF

PDF normalization should preserve:

```text
page count
page dimensions
embedded metadata
text blocks
word geometry where needed
images and image regions
tables or table candidates
reading-order observations
OCR state
page render references
parser warnings
```

A PDF may be:

```text
born-digital text
scanned image
mixed text and image
malformed
encrypted
form-based
portfolio or attachment container
```

The parser must not silently treat an empty text layer as an empty document.

### Candidate PDF parser strategy options

#### P1 — Text-first local parser

Use a local PDF library for metadata, pages, blocks, words, and images. OCR only when text is missing or clearly unusable.

Advantages:

- local processing;
- direct geometry;
- lower cost;
- deterministic configuration.

Risks:

- reading order can be poor;
- tables can be difficult;
- complex PDFs vary widely.

#### P2 — Layout-aware local parser

Use a layout model or document-partitioning framework.

Advantages:

- semantic element candidates;
- potentially better tables and headings.

Risks:

- model dependencies;
- heavier runtime;
- confidence and version drift;
- harder reproducibility.

#### P3 — Provider-assisted document parsing

Send documents to a SaaS extraction provider.

Advantages:

- broad format support;
- managed OCR/layout models.

Risks:

- retention and privacy;
- cost;
- vendor dependency;
- provider output drift;
- uploading third-party documents.

No PDF parser option is approved.

## 5.3 Images and screenshots

Image normalization should preserve:

```text
original pixels
format and dimensions
orientation metadata
color profile where relevant
OCR regions
OCR text
region coordinates
visual-description candidates
manual annotations
```

OCR text should include:

```text
ocr_engine
ocr_engine_version
language/model
region
raw OCR result
confidence if meaningful
human correction
```

A screenshot is not merely an image. It may also need:

```text
source page or application
capture time
viewport dimensions
scroll position
operator notes
capture method
whether content was cropped
```

Automated image captioning or visual interpretation must remain a candidate observation, never accepted fact.

## 5.4 Transcripts and subtitles

R-M06-02 already establishes transcript provenance classes.

Normalization should preserve:

```text
original transcript artifact
format
segment order
start/end time
speaker label as supplied
raw text
normalized text
language
caption or ASR provenance
confidence where supplied
human corrections
```

Converting SRT or VTT to plain paragraphs must not destroy timing or cue identity.

## 5.5 JSON and structured API responses

Structured responses should preserve:

```text
exact raw JSON bytes
request context
response headers
schema or provider version
JSON Pointer locators
normalization mapping version
unknown fields
```

**PROJECT RECOMMENDATION**

Unknown provider fields should be retained in the original response even when ignored by the normalized package.

A normalization mapping should not silently coerce invalid values. For example, an invalid date should become a warning and retained raw value, not a guessed timestamp.

## 5.6 RSS and Atom feeds

Feed normalization should preserve:

```text
raw feed artifact
feed identity
entry identity
entry updated/published fields
content and summary fields
links and relation types
author fields
feed parser warnings
```

Feed content should not automatically replace a separately retrieved webpage observation. They are different source representations.

## 5.7 Markdown and plain text

Markdown normalization should preserve:

```text
original source
frontmatter or metadata block
heading hierarchy
paragraphs
lists
code fences
links
quotes
tables where parseable
line ranges
```

Plain text should retain line numbers and exact supplied text before whitespace normalization.

---

# 6. Text normalization rules

## 6.1 Preserve raw and normalized forms

Recommended distinction:

```text
text_raw        parser output or exact supplied segment
text_normalized conservative machine-friendly form
text_corrected  optional human-governed correction
```

## 6.2 Conservative normalization candidate

Possible safe operations:

```text
normalize line-ending representation
record but do not erase Unicode normalization differences
remove parser-inserted control characters when explicitly identified
preserve paragraph and element boundaries
preserve meaningful whitespace in code and tables
```

Unsafe automatic operations include:

```text
rewriting spelling
fixing dates
changing quotes
merging speakers
removing repeated text without provenance
summarizing
translating
resolving ambiguous entities
```

## 6.3 Unicode and encoding

The system should record:

```text
source encoding if known
encoding detector and confidence if inferred
decode errors
replacement-character counts
normalization form used for derivative search fields
```

The exact original bytes remain the authority for forensic review.

---

# 7. Parser provenance model

## 7.1 Provenance relationship candidate

```text
artifact_123
  wasInputTo → parse_456
parse_456
  used → parser PyMuPDF 1.28.0
  usedConfig → config hash abc...
  generated → package_789
package_789
  wasReviewedBy → human operator
  generatedProjection → readable.md
```

## 7.2 Configuration must be identifiable

Parser configuration may include:

```text
OCR enabled/disabled
OCR language
page range
reading-order sorting
HTML main-content algorithm
table extraction mode
image extraction mode
maximum size
sandbox limits
network disabled/enabled
```

Recording only the parser version is insufficient.

## 7.3 Reprocessing behavior

A new parser run should create a new derivative package.

It should not overwrite the previous package.

The system should compare:

```text
same artifact hash?
same parser and version?
same configuration hash?
same normalized output hash?
new warnings?
element identity continuity?
```

The preferred package may change after review, but historical packages remain traceable.

---

# 8. Deterministic identity candidates

## 8.1 Artifact identity

An artifact occurrence needs a durable record ID even if the bytes match a previous artifact.

The SHA-256 hash supports deduplication and integrity but should not be the sole business identity.

## 8.2 Element identity options

### E1 — Random durable IDs

Assign a stable generated ID when the parser package is created.

Advantages:

- simple;
- survives text duplicates within a document.

Risks:

- reprocessing creates unrelated IDs unless reconciliation is performed.

### E2 — Deterministic content/location IDs

Derive IDs from document version, type, locator, and content hash.

Advantages:

- supports reproducible reprocessing.

Risks:

- locator changes can cause churn;
- duplicate text requires careful ordinal handling.

### E3 — Hybrid IDs

Use durable record IDs plus a deterministic fingerprint for comparison.

Advantages:

- separates identity from similarity;
- supports moved/changed element analysis.

**PROJECT RECOMMENDATION**

E3 appears strongest for later synthesis, but no element-identity method is approved yet.

---

# 9. Human-readable projections

A Markdown or HTML projection may be generated for review.

It should be labeled clearly as a derivative and include:

```text
document ID
source artifact ID
parser execution ID
parser/version
warnings
source URL or filename
page/timestamp locators
projection generation time
```

The projection should not become the only retained normalized form.

Markdown is useful for human readability but insufficient as the sole canonical interchange format because it may lose:

- geometry;
- confidence;
- parser warnings;
- multiple competing metadata observations;
- precise word and region locators;
- structured provenance.

---

# 10. Schema validation and evolution

## 10.1 Candidate rule

Every normalized package should declare a schema version and validate before review admission.

Validation failures should produce:

```text
invalid_package
validation_errors
parser execution retained
original artifact preserved
no knowledge promotion
```

## 10.2 Forward compatibility

The schema should allow controlled extension without silently accepting arbitrary authority-bearing fields.

Possible approach:

```text
strict core fields
source-specific extension namespace
unknown fields preserved only in raw provider output
explicit schema migration tools later
```

## 10.3 Schema migration

Normalized packages should be either:

- retained in their original schema version; or
- migrated to a new derivative with a recorded migration activity.

Do not rewrite historical packages without lineage.

---

# 11. Security and failure modes

## 11.1 Hostile or malformed documents

Potential threats include:

```text
archive bombs
oversized dimensions
malformed PDFs
embedded files
parser crashes
excessive memory or CPU use
crafted fonts
malicious macros
external resource references
path traversal names
script-bearing HTML
prompt injection text
```

## 11.2 Candidate safety boundary

A future implementation should consider:

```text
size limits
format allowlists
quarantine states
parser subprocess isolation
network-disabled parsing where possible
timeouts
memory limits
attachment-count limits
embedded-file policy
sanitized filenames
no macro execution
no script execution
```

No sandbox design is approved by this report.

## 11.3 Prompt injection

Source text can contain instructions addressed to an LLM.

Normalization must preserve that text as source content but must not treat it as system or operator instructions.

Later context assembly should label retrieved material as untrusted evidence.

## 11.4 Partial extraction

A parser may succeed partially.

The package should distinguish:

```text
complete
partial
text_only
metadata_only
ocr_partial
table_failed
images_not_processed
unsupported_feature
```

A process exit code of zero does not prove complete extraction.

---

# 12. Parser comparison and acceptance testing

Before selecting any parser, the project should create a synthetic and representative corpus containing:

```text
simple HTML article
JavaScript-rendered article snapshot
clean born-digital PDF
scanned PDF
mixed PDF
multi-column PDF
table-heavy PDF
malformed PDF
SRT and VTT transcripts
screenshot with text
JSON API response
RSS and Atom feeds
Markdown with frontmatter
Unicode and encoding edge cases
```

Candidate evaluation dimensions:

```text
text accuracy
reading order
heading recognition
table preservation
geometry preservation
metadata accuracy
warnings and failure honesty
reproducibility
runtime and memory
security isolation
license
maintenance health
local versus provider processing
cost
```

The evaluation should include deliberately bad inputs and prove that failures are visible.

---

# 13. Candidate architecture options

## N1 — Markdown-first normalization

Convert all inputs quickly to Markdown and preserve originals separately.

Advantages:

- easy human review;
- simple tooling;
- useful for early prototypes.

Risks:

- loses geometry and rich provenance;
- encourages Markdown to become accidental truth;
- weak for tables, OCR regions, timestamps, and parser comparison.

## N2 — Common JSON element package plus projections

Normalize to versioned JSON elements with source-specific locators and generate Markdown/HTML projections.

Advantages:

- preserves structure;
- schema validation;
- strong parser lineage;
- compatible with later chunking.

Risks:

- more design work;
- requires schema governance;
- projections need tooling.

## N3 — Parser-native outputs only

Keep each parser's native JSON/XML output and adapt downstream consumers.

Advantages:

- minimal transformation;
- preserves tool-specific detail.

Risks:

- vendor/tool lock-in;
- inconsistent semantics;
- difficult cross-format retrieval;
- downstream complexity.

## N4 — Database-only normalized records

Store every parsed element directly in SQLite without a portable package.

Advantages:

- queryable;
- transactional;
- centralized integrity.

Risks:

- harder reconstruction and interchange;
- schema churn;
- encourages operational DB to own parser-specific content;
- weaker file-level evidence packaging.

## N5 — Hybrid governed package

Preserve originals in governed storage, store manifests and authority state in SQLite, keep normalized versioned JSON packages, and generate human-readable projections.

Advantages:

- separation of authority and artifacts;
- rebuildability;
- portable derivatives;
- structured provenance;
- future retrieval compatibility.

Risks:

- most moving parts;
- requires precise boundaries and hammer tests.

**PROJECT RECOMMENDATION**

N5 appears strongest for later synthesis, with N2 as the normalization core. This is not owner-approved architecture.

---

# 14. Decisions reserved for owner approval

The following must be presented as options during synthesis:

1. Whether the canonical normalized package is JSON, JSONL, SQLite rows, or a hybrid.
2. Whether Markdown is only a projection or also an editable human layer.
3. Which initial formats M06 supports.
4. Which parser class is used for HTML.
5. Which parser class is used for PDF.
6. Whether OCR is local, provider-assisted, deferred, or selectable.
7. Whether broad fallback parsing such as Tika is admitted.
8. Whether geometry is retained for all PDFs or only selected cases.
9. How parser executions and packages receive stable IDs.
10. How human corrections affect preferred text.
11. Whether parser-native outputs are retained in addition to normalized packages.
12. What security isolation is required before implementation.
13. What corpus and acceptance thresholds must be met.

No decision above is made by this report.

---

# 15. Recommended direction for synthesis

Subject to owner review, the strongest research direction is:

```text
immutable original artifact
+ SQLite authority/manifests
+ versioned JSON element package
+ explicit parser execution provenance
+ source-specific locators
+ human-readable Markdown/HTML projection
+ human correction lineage
+ downstream, rebuildable retrieval chunks
```

Initial implementation scope should remain narrow and should not claim broad document intelligence.

A possible first-format set for owner consideration is:

```text
plain text
Markdown
SRT/VTT
basic HTML
born-digital PDF
JSON
RSS/Atom
```

Scanned PDFs, OCR-heavy documents, office formats, browser-rendered pages, and complex tables could remain deferred until parser evaluation proves reliable behavior.

This is a recommendation for later review, not an approved scope.

---

# 16. Rejected or unsafe shortcuts

The report rejects these shortcuts as architecture recommendations:

- converting every source to Markdown and discarding structural detail;
- treating extracted text as equivalent to original evidence;
- overwriting old parser output after upgrades;
- recording parser name without version and configuration;
- silently guessing reading order;
- inventing missing timestamps or page locators;
- automatically accepting OCR corrections;
- treating metadata extraction as authoritative;
- feeding hostile source instructions to the LLM as trusted instructions;
- chunking directly from originals without a governed normalized layer;
- choosing one SaaS because it supports many formats;
- allowing parser success to bypass human admission review.

---

# 17. Open questions

1. Which formats are essential for the first M06 package versus later connectors?
2. How much PDF geometry should be preserved by default?
3. Should every page receive a rendered reference image for review?
4. What OCR languages are required initially?
5. How should corrected text coexist with raw and normalized text in retrieval?
6. Should parser-native output be retained permanently or only for selected tools?
7. How should duplicate elements across headers/footers be represented?
8. What confidence fields are comparable across different OCR and parser tools?
9. Should main-content extraction decisions be human-reviewable at region level?
10. What is the minimal schema that supports future Qdrant without overdesigning M06?
11. How will package migrations be tested and restored?
12. What licensing constraints apply to candidate parser libraries and models?
13. What process isolation is feasible in the Windows desktop environment?

---

# 18. Inputs required by later reports

R-M06-05 should use this report to define:

- which normalized objects become durable Vault records;
- which remain parser derivatives;
- how dossiers reference source elements;
- how assertions point to exact locators;
- how human corrections and accepted conclusions remain distinct.

R-M06-06 should define:

- artifact, document, version, execution, package, and element identity;
- deduplication;
- parser rerun lineage;
- correction and supersession.

R-M06-07 should define:

- how untrusted source elements enter bounded LLM context;
- how parser warnings and provenance are shown;
- how source instructions are isolated from system instructions.

R-M06-08 should define:

- how normalized elements become deterministic retrieval chunks;
- how changed packages invalidate chunks;
- how page/timestamp locators survive retrieval.

R-M06-09 should define:

- parser sandboxing;
- backup and restore;
- rebuild and reprocessing;
- failure recovery;
- malicious-document handling.

---

# 19. Primary sources and access dates

Accessed 2026-07-20.

- RFC 9110 — HTTP Semantics, Content-Type and media types
  https://www.rfc-editor.org/rfc/rfc9110

- JSON Schema specification, Draft 2020-12
  https://json-schema.org/specification

- W3C PROV-O Recommendation
  https://www.w3.org/TR/prov-o/

- Unstructured partitioning documentation
  https://docs.unstructured.io/open-source/core-functionality/partitioning

- Unstructured chunking documentation
  https://docs.unstructured.io/open-source/core-functionality/chunking

- Apache Tika project documentation/repository
  https://tika.apache.org/
  https://github.com/apache/tika

- PyMuPDF documentation — text extraction, blocks, words, JSON, geometry
  https://pymupdf.readthedocs.io/en/latest/
  https://pymupdf.readthedocs.io/en/latest/app1.html
  https://pymupdf.readthedocs.io/en/latest/textpage.html

- PDF Association — Tagged PDF Best Practice Guide
  https://www.pdfa-inc.org/product/tagged-pdf-best-practice-guide-syntax/

---

# 20. Research conclusion

The Discrepancy Desk should not normalize by erasing differences between formats.

The strongest research direction is to preserve exact originals, record every parsing activity, produce a versioned common element package with source-specific locators, retain warnings and uncertainty, allow human corrections without overwriting parser output, and generate human-readable projections separately.

Markdown may be useful for review, but it should not automatically become the sole canonical representation. Retrieval chunking should remain downstream and rebuildable.

No parser, OCR system, schema, format set, or normalization architecture is approved by this report. Those choices remain reserved for owner review during synthesis.
