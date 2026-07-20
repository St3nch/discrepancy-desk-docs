# R-M06-02 — YouTube and Audiovisual Ingestion

## Status

Research report completed on 2026-07-20.

This report is source material for M06 architecture synthesis. It is not implementation authority, transcript-provider admission, permission to download audiovisual media, permission to scrape YouTube, approval to retain third-party content indefinitely, or approval to automate channel monitoring.

## Research Question

How should The Discrepancy Desk manually ingest a YouTube video and its transcript now, while preserving a safe path toward official metadata APIs, transcript SaaS providers, locally generated speech-to-text, channel monitoring, and later LLM retrieval?

## Executive Finding

A YouTube item is not one blob called a “video transcript.” The system must distinguish at least:

```text
YouTube video identity
metadata observation
caption/transcript artifact
transcript acquisition method
transcript segments and optional words
audio or video artifact, when lawfully supplied
visual observations not present in speech
human corrections
extracted claims and entities
later retrieval chunks
```

The recommended immediate workflow is manual-first:

```text
human supplies YouTube URL
        ↓
video identity is normalized
        ↓
metadata is entered manually or obtained through an admitted official lookup
        ↓
human pastes transcript or uploads TXT/SRT/VTT/SBV/JSON
        ↓
original supplied transcript is preserved exactly
        ↓
parser produces normalized segments without replacing the original
        ↓
human reviews provenance, language, timing, omissions, and likely errors
        ↓
source is admitted to governed research memory
```

Future APIs and automation must produce the same downstream package. The acquisition method may change; the evidence and knowledge boundaries must not.

The official YouTube Data API is suitable for video and channel metadata. It is not a general official API for downloading arbitrary public transcripts. Third-party URL-to-transcript services exist and can be automated, but their acquisition methods, retention, reliability, source provenance, and platform-policy posture require individual admission review. Local speech-to-text is viable only when the project possesses an audiovisual file it is permitted to process.

---

# 1. Research Method and Evidence Labels

This report uses:

- **VERIFIED FACT** — directly supported by an official platform, standard, project, or service document.
- **SOURCE/PROVIDER CLAIM** — a provider’s statement about its own feature or policy.
- **INFERENCE** — a reasoned implication from verified material.
- **PROJECT RECOMMENDATION** — a proposed project rule requiring architecture synthesis and owner approval.
- **OPEN QUESTION** — unresolved and unsafe to treat as settled.

Official YouTube documentation was treated as authoritative for official API capabilities. Provider documentation was treated as evidence of advertised capability, not independent proof of accuracy, legality, uptime, or suitability.

---

# 2. What Constitutes a YouTube Source

## 2.1 Stable video identity

**VERIFIED FACT**

The YouTube Data API `videos.list` method returns video resources selected by request parameters. The `snippet` part can include fields such as channel ID, title, description, tags, and category ID. A request has a documented quota cost of one unit.

**PROJECT RECOMMENDATION**

The canonical external identity should be the YouTube video ID, not the full URL. The system should preserve every supplied URL occurrence while normalizing recognized forms to one video identity.

Examples of occurrences that may identify the same item:

```text
https://www.youtube.com/watch?v=<video_id>
https://youtu.be/<video_id>
https://www.youtube.com/shorts/<video_id>
embedded or playlist-bearing URLs
```

The system should preserve:

```yaml
source_platform: youtube
video_id:
canonical_watch_url:
supplied_url:
normalized_at:
normalizer_version:
```

URL normalization must not prove that the item exists, is public, or remains available.

## 2.2 Video identity is not a metadata snapshot

**PROJECT RECOMMENDATION**

Separate:

```text
video identity
metadata observation
transcript observation
availability observation
statistics observation
```

Title, description, tags, statistics, caption availability, and public status can change. Therefore, a later lookup should append an observation rather than silently mutate historical evidence.

A current projection may point to the most recent accepted observation, but earlier observations must remain auditable.

## 2.3 A transcript is not the video

**PROJECT RECOMMENDATION**

A transcript captures speech or captions. It may omit:

- on-screen text;
- charts and documents shown in-frame;
- visual demonstrations;
- facial reactions and gestures;
- silent scene changes;
- music, sound effects, or nonverbal events;
- comments, description links, pinned comments, and external references.

The system must never treat transcript completeness as video completeness.

A future audiovisual package may include human-authored visual observations, creator-provided slides, or lawfully derived keyframe observations. These must remain separate derivatives with their own provenance.

---

# 3. Official YouTube Metadata

## 3.1 Video metadata available through the Data API

**VERIFIED FACT**

`videos.list` is the official method for retrieving video resources. Depending on requested parts and access, video resources can expose structured metadata including snippet fields, content details, status, statistics, live-streaming details, and caption-availability indicators.

**PROJECT RECOMMENDATION**

A future admitted official metadata connector should request only fields needed by the product. The initial candidate observation includes:

```yaml
video_id:
channel_id:
channel_title:
title:
description:
published_at:
default_language:
default_audio_language:
duration_iso8601:
category_id:
tags:
caption_available:
live_broadcast_content:
privacy_status_if_visible:
statistics_if_admitted:
api_etag:
observed_at:
request_parts:
quota_cost:
```

Raw API JSON should be preserved as the original acquisition artifact. The normalized record must point to that artifact and record the API/parser version.

## 3.2 API data is governed by changing policies

**VERIFIED FACT**

The YouTube API Services Terms and Developer Policies are binding components of API use. YouTube may change them, monitor API clients, and suspend access for violations. The official revision history records policy changes, including 2026 changes concerning derived metrics and data storage.

**PROJECT RECOMMENDATION**

YouTube API admission requires a policy record containing:

```yaml
policy_reviewed_at:
terms_revision_observed:
developer_policy_revision_observed:
credential_class:
requested_data_classes:
retention_rule:
refresh_or_deletion_obligation:
privacy_notice_requirement:
owner_approval_id:
```

No research report should hard-code a permanent retention period for all YouTube API data. The connector contract must be reviewed against the policies current at implementation and operation time.

## 3.3 Public metadata does not grant transcript access

**VERIFIED FACT**

The official captions API requires OAuth authorization. `captions.list` returns caption-track metadata, not caption text. Actual caption retrieval uses `captions.download`, which belongs to the authenticated caption-management surface.

**INFERENCE**

The captions API is designed around authorized management of caption tracks, not unrestricted transcript retrieval for arbitrary public videos.

**PROJECT RECOMMENDATION**

The system must never label a third-party transcript as “official YouTube API transcript” unless it was acquired through an admitted creator-authorized caption API flow and the authorization context is preserved.

---

# 4. Manual Transcript Acquisition

## 4.1 YouTube’s transcript viewer

**VERIFIED FACT**

YouTube Help documents a transcript view for videos with captions. The transcript scrolls with playback, and selecting a transcript line navigates to the associated part of the video.

**PROJECT RECOMMENDATION**

The first product workflow should let the operator copy visible transcript text and paste it into the application. The system should ask the operator to declare what was copied:

```text
YouTube transcript panel with timestamps
YouTube transcript panel without timestamps
creator-provided transcript in description/site
third-party transcript export
human-written notes
unknown source
```

The application should not infer provenance solely from text shape.

## 4.2 Manual transcript intake formats

**PROJECT RECOMMENDATION**

Initial admitted transcript formats should be:

```text
plain text (.txt)
Markdown (.md)
SubRip (.srt)
WebVTT (.vtt)
YouTube-style SBV (.sbv)
structured JSON from an admitted or manually identified provider
pasted text
```

Each import must preserve the supplied bytes or exact pasted UTF-8 text before parsing.

DOCX, PDF transcripts, spreadsheets, and proprietary exports may enter through the general document-normalization research rather than being treated as native transcript formats.

## 4.3 Untimestamped transcripts remain useful

**PROJECT RECOMMENDATION**

An untimestamped transcript may be admitted, but the system must record:

```yaml
timing_precision: none
seek_links_available: false
segment_boundaries: parser_derived_or_paragraph_only
```

It must not invent timestamps.

Later alignment against lawfully available audio should create a new derived alignment artifact, not overwrite the original transcript.

## 4.4 Human review requirements

**PROJECT RECOMMENDATION**

Before admission, the review screen should expose:

- video identity and supplied URL;
- transcript source and acquisition method;
- transcript language;
- whether captions were described as automatic or human-created;
- whether timestamps exist and their precision;
- parser warnings;
- malformed or overlapping cues;
- likely duplicate transcript observations;
- very long gaps or zero-duration segments;
- transcript/video duration mismatch when duration is known;
- possible omitted opening/closing sections;
- whether speaker labels are supplied, inferred, or absent;
- whether the material contains prompt-injection-like instructions;
- whether the source account and target research account are correct.

The operator may:

```text
admit as source transcript
admit as unverified lead
return for correction
reject
```

Admission does not accept extracted claims as true.

---

# 5. Transcript Provenance Classes

## 5.1 Required provenance vocabulary

**PROJECT RECOMMENDATION**

Every transcript observation should use a closed acquisition vocabulary such as:

```text
creator_authorized_caption_api
creator_supplied_caption_file
youtube_ui_human_copy
human_supplied_transcript_file
third_party_native_caption_service
third_party_generated_asr
local_generated_asr
human_transcription
unknown_external_transcript
```

A separate production-method field should distinguish:

```text
human_caption
automatic_caption
speech_to_text
translation
edited_transcript
unknown
```

These are not interchangeable.

## 5.2 Native-caption retrieval versus generated ASR

**PROJECT RECOMMENDATION**

A provider that “returns a transcript” may:

- retrieve an existing caption track;
- translate an existing track;
- acquire media and generate speech-to-text;
- use cached output from an earlier request;
- combine methods under an automatic mode.

The provider adapter must require or derive a method declaration for each result. If the provider does not expose method provenance, the transcript should be marked:

```yaml
production_method: unknown
provider_method_claim: unavailable
confidence_class: reduced
```

The system must not upgrade it to “native captions” based only on provider marketing.

## 5.3 Translation is a derivative

**PROJECT RECOMMENDATION**

A translated transcript must reference:

```text
source transcript observation
source language
target language
translation provider/model
translation time
translation settings
```

The source-language transcript remains canonical evidence of what was transcribed. Translation is a derivative and may introduce semantic errors.

---

# 6. Candidate Transcript SaaS Capability

## 6.1 URL-to-transcript services exist

**SOURCE/PROVIDER CLAIM**

Supadata documents an authenticated transcript endpoint accepting YouTube and other supported video URLs. Its API advertises three modes:

```text
native    retrieve an existing transcript only
auto      try native, then generate with AI
generate  force AI transcription
```

It returns plain text or timestamped segments and may return an asynchronous job for longer processing. Its documentation advertises millisecond offsets, language metadata, and native-caption or AI fallback behavior.

**INFERENCE**

This proves that a future URL-based connector can be technically simple from The Discrepancy Desk’s perspective. It does not prove the provider’s upstream acquisition method is acceptable, that results are accurate, or that retention and platform-policy questions are resolved.

## 6.2 Generic speech-to-text APIs exist

**SOURCE/PROVIDER CLAIM**

AssemblyAI documents asynchronous transcription APIs with temporally ordered word objects, utterances, speaker diarization, speaker labels, and configurable retention/deletion behavior. Amazon Transcribe documents batch and streaming transcription, word timing, channel identification, and speaker partitioning. OpenAI documents audio transcription and translation endpoints but requires an uploaded supported audio file rather than accepting a YouTube URL directly.

**INFERENCE**

Generic speech-to-text providers are downstream processors. The project must first possess a media file or accessible media URL it is permitted to submit. They do not solve YouTube media-acquisition authority.

## 6.3 No provider is admitted by this report

**PROJECT RECOMMENDATION**

A transcript provider evaluation must compare at least:

```text
input types
native-caption retrieval versus generated ASR
method provenance returned per job
timestamp granularity
word-level output
speaker diarization
language detection
translation separation
maximum duration/file size
synchronous/asynchronous behavior
webhook or polling support
idempotency and request identifiers
raw result export
error vocabulary
rate limits
pricing and cost caps
data retention and deletion APIs
training opt-out
regional processing
subprocessors
security documentation
platform-policy statement
provider continuity and exportability
```

Vendor selection belongs to a later bounded connector-admission package.

---

# 7. Provider Data Retention

## 7.1 Retention varies materially

**SOURCE/PROVIDER CLAIM**

AssemblyAI states that customer-uploaded audio deletion generally begins after 24 hours and may take up to 48 hours. It documents configurable transcript TTL for some accounts, with asynchronous transcript artifacts otherwise potentially retained indefinitely until customer deletion when no TTL or BAA applies. It also documents training opt-out and metadata retained for logging or billing.

**VERIFIED FACT**

OpenAI states that API business data is not used for model training by default unless the organization opts in. Its Audio API requires file upload rather than accepting remote media links directly; model-specific input limits apply.

**PROJECT RECOMMENDATION**

Every provider admission must explicitly choose and verify:

```yaml
provider_training_opt_out:
requested_ttl:
delete_after_success:
delete_endpoint_verified:
provider_job_id_preserved:
deletion_receipt_preserved:
metadata_retained_by_provider:
region:
```

A provider’s default should never silently become the project retention policy.

## 7.2 Delete-after-retrieval candidate rule

**PROJECT RECOMMENDATION**

For third-party ASR, the default candidate posture should be:

```text
submit admitted media
poll or receive completion
preserve provider response locally
validate local artifact
request provider deletion where supported
preserve deletion response/receipt
```

Exceptions require an explicit reason and owner-approved retention rule.

---

# 8. Local Speech-to-Text

## 8.1 OpenAI Whisper

**VERIFIED FACT**

The open-source Whisper project supports multilingual speech recognition, speech translation, and language identification. Its command-line interface can emit TXT, VTT, SRT, TSV, and JSON outputs. It supports segment timestamps and optional word-level timestamps.

**PROJECT RECOMMENDATION**

Whisper should be evaluated as a local, provider-independent transcription engine when the operator supplies an audiovisual file that the project may process.

The model name, model hash/version, decoding settings, language choice, device, software version, and output formats must be recorded.

## 8.2 faster-whisper

**SOURCE/PROJECT CLAIM**

The faster-whisper project implements Whisper inference through CTranslate2, supports segment and optional word timestamps, and documents CPU/GPU execution choices and performance considerations.

**PROJECT RECOMMENDATION**

It may be a practical local-engine candidate, but it should not be accepted only because it is faster. Evaluation must include:

- transcription accuracy on project-like material;
- proper nouns and unusual names;
- timestamp accuracy;
- hallucination behavior during silence/music;
- model size and hardware needs;
- deterministic-enough operational settings;
- upgrade/reprocessing behavior.

## 8.3 Local does not eliminate provenance

**PROJECT RECOMMENDATION**

A locally generated transcript should record:

```yaml
production_method: speech_to_text
processor_location: local
engine:
engine_version:
model:
model_revision_or_hash:
language_requested:
language_detected:
decoding_options:
word_timestamps_enabled:
vad_enabled:
started_at:
completed_at:
source_media_artifact_id:
```

Local processing reduces third-party disclosure but does not make the transcript authoritative or error-free.

## 8.4 Media acquisition remains separate

**VERIFIED FACT**

YouTube’s Terms restrict downloading or using content except as specifically permitted by the service, with permission, or as permitted by applicable law.

**PROJECT RECOMMENDATION**

The project must not add a generic “download arbitrary YouTube video/audio” capability as an incidental step in local transcription.

Local ASR may process:

```text
operator-owned media
creator-provided downloadable media
media supplied with documented permission
lawfully retained project evidence
other explicitly admitted audiovisual files
```

The operator may still paste a transcript from YouTube’s visible UI without the system acquiring the audiovisual file.

---

# 9. Transcript Normalization Model

## 9.1 Preserve three levels

**PROJECT RECOMMENDATION**

The normalized package should preserve:

```text
transcript observation
ordered segments
optional ordered words
```

Candidate structure:

```yaml
transcript_observation:
  transcript_id:
  video_id:
  language:
  production_method:
  acquisition_method:
  provider:
  provider_job_id:
  timing_precision:
  speaker_precision:
  source_artifact_id:
  observed_at:
  content_sha256:

segments:
  - segment_id:
    ordinal:
    start_ms:
    end_ms:
    raw_text:
    normalized_text:
    speaker_label:
    speaker_label_method:
    confidence:
    source_locator:

words:
  - word_id:
    segment_id:
    ordinal:
    start_ms:
    end_ms:
    raw_token:
    normalized_token:
    speaker_label:
    confidence:
```

Words are optional. Their absence must not prevent transcript admission.

## 9.2 Raw and normalized text

**PROJECT RECOMMENDATION**

Every segment should preserve:

```text
raw_text          exactly parsed from supplied/provider artifact
normalized_text   a deterministic convenience projection
corrected_text    optional human correction in a separate correction record
```

Normalization may standardize line endings or cue wrappers but must not silently repair names, punctuation, grammar, or factual statements.

## 9.3 Stable segment identity

**PROJECT RECOMMENDATION**

Segment identity should not rely only on ordinal position because inserting one cue can shift all later ordinals.

The synthesis should evaluate a deterministic identity derived from:

```text
transcript observation ID
source cue identifier if supplied
start/end timing if supplied
raw text hash
local disambiguator
```

Moved or resegmented content may need new segment IDs linked through derivation lineage rather than pretending to be unchanged.

## 9.4 Cue validation

**PROJECT RECOMMENDATION**

The parser should flag rather than silently discard:

- invalid timestamps;
- end before start;
- overlapping cues;
- duplicate cues;
- empty text;
- unrecognized encoding;
- huge cue duration;
- impossible duration relative to known video duration;
- non-monotonic order;
- malformed speaker tags;
- provider JSON fields not recognized by the admitted schema.

---

# 10. Speaker Diarization and Identification

## 10.1 Diarization is not identity

**VERIFIED FACT**

Speech-to-text services document speaker diarization or partitioning as grouping speech by distinct speakers and labeling them with generated identifiers. AssemblyAI returns speaker fields at utterance and word level; Amazon Transcribe can partition speakers or separate known channels.

**PROJECT RECOMMENDATION**

The system must distinguish:

```text
speaker cluster: Speaker A
speaker role candidate: Host
identified person candidate: Jane Doe
human-confirmed identity: Jane Doe
```

An ASR provider’s `Speaker A` must never automatically become a named entity.

## 10.2 Identification requires evidence

**PROJECT RECOMMENDATION**

A human may map a speaker cluster to a person or role only through a correction/annotation record containing evidence such as:

- self-introduction;
- on-screen label;
- description/show notes;
- known host identity;
- channel or episode metadata;
- external authoritative source.

The transcript text remains unchanged; the identity annotation attaches separately.

## 10.3 Multichannel audio

**PROJECT RECOMMENDATION**

When a supplied file has separate audio channels, channel identification should be preserved distinctly from diarization. A channel may contain one or more speakers, and one speaker may appear across channels after editing.

---

# 11. Accuracy, Corrections, and Competing Transcripts

## 11.1 Automatic captions can be wrong

**VERIFIED FACT**

YouTube warns that automatic captions can misrepresent spoken content because of accents, dialects, pronunciation, background noise, or overlapping speakers.

**PROJECT RECOMMENDATION**

Automatic transcripts should carry a visible warning and should not be quoted publicly without human verification against the source when the wording matters.

## 11.2 Multiple transcript observations may coexist

**PROJECT RECOMMENDATION**

The same video may have:

```text
YouTube automatic captions
creator-edited captions
third-party native-caption copy
third-party generated ASR
local Whisper transcript
human correction transcript
translation
```

Do not overwrite one with another. Store separate observations and relations:

```text
same_video
supersedes
translation_of
alignment_of
human_correction_of
provider_retrieval_of_claimed_track
```

## 11.3 Correction model

**PROJECT RECOMMENDATION**

A human correction should record:

```yaml
correction_id:
transcript_id:
segment_id_or_time_range:
original_text:
corrected_text:
reason:
evidence_reference:
actor_id:
created_at:
```

The original provider or parser output remains immutable.

## 11.4 Quote verification state

**PROJECT RECOMMENDATION**

Candidate quote states:

```text
unreviewed_transcript_text
reviewed_against_transcript_artifact
reviewed_against_video_playback
creator_supplied_exact_text
disputed_or_unclear
```

Only a human may advance quote verification.

---

# 12. Livestreams, Premieres, Shorts, and Edited Videos

## 12.1 Livestream state

**PROJECT RECOMMENDATION**

A livestream should be modeled as one video identity with time-varying observations:

```text
scheduled
live
ended_processing
archive_available
unavailable
```

A transcript captured while live may differ from final captions. Preserve each observation time and never silently replace the live transcript.

## 12.2 Shorts

**PROJECT RECOMMENDATION**

Shorts use the same video-ID identity model but may have different URL forms and short duration. Do not create a separate source class unless later API behavior requires it.

## 12.3 Description/title edits

**VERIFIED FACT**

YouTube’s push-notification guidance describes notifications for new uploads and title or description updates through an Atom/WebSub mechanism.

**PROJECT RECOMMENDATION**

A future monitor should treat a notification as a prompt to acquire a new metadata observation. The notification itself is not proof of the complete current resource and should be preserved as an event artifact.

## 12.4 Deleted, private, or unavailable videos

**PROJECT RECOMMENDATION**

Later unavailability must append an availability observation:

```yaml
availability_state:
observed_at:
observation_method:
response_status_or_api_result:
```

Previously admitted evidence must not be rewritten as though the video never existed. Retention and deletion obligations still apply independently.

---

# 13. Channel Monitoring

## 13.1 Official push candidate

**VERIFIED FACT**

YouTube documents push notifications through WebSub-compatible feeds for channel uploads and changes to video title or description.

**PROJECT RECOMMENDATION**

This is the preferred future discovery mechanism for explicitly admitted channels because it avoids blind high-frequency polling.

Candidate flow:

```text
owner admits channel
        ↓
subscribe through official feed/hub mechanism
        ↓
receive notification
        ↓
preserve notification receipt
        ↓
lookup current metadata through admitted API
        ↓
create pending ingestion candidate
        ↓
human decides whether transcript acquisition is warranted
```

## 13.2 Monitoring is not transcript automation

**PROJECT RECOMMENDATION**

A new-upload notification should not automatically trigger paid transcript generation unless the source admission record explicitly authorizes:

- transcription method;
- provider;
- per-item and daily cost cap;
- duration cap;
- language handling;
- retention/deletion behavior;
- human-review queue;
- duplicate suppression.

Discovery, metadata observation, and transcript acquisition are separate operations.

## 13.3 Channel admission record

Candidate fields:

```yaml
channel_id:
channel_title_observed:
account_id:
admission_status:
monitoring_method:
metadata_lookup_allowed:
transcript_method_allowed:
max_video_duration_seconds:
daily_item_limit:
daily_cost_limit:
language_allowlist:
content_sensitivity:
last_notification_at:
last_successful_lookup_at:
owner_approval_id:
```

---

# 14. Visual and Non-Speech Information

## 14.1 Transcript-only blind spots

**PROJECT RECOMMENDATION**

The review UI should include a visible question:

```text
Does the video communicate material information visually that is absent from the transcript?
```

The human may add timestamped visual notes:

```yaml
visual_observation_id:
video_id:
start_ms:
end_ms:
observation_text:
observation_actor: human
source_locator:
```

These are human observations, not extracted transcript segments.

## 14.2 Future visual analysis

**OPEN QUESTION**

Future research may evaluate creator-provided slides, scene sampling, OCR, object/text detection, and multimodal models. This report does not authorize automated frame extraction from arbitrary YouTube media.

Any future visual analysis must start from an admitted media artifact or platform-provided representation and preserve derivation provenance.

---

# 15. LLM Context Assembly for Video Sources

## 15.1 The LLM should receive a bounded evidence packet

**PROJECT RECOMMENDATION**

A content-generation or research task should receive only relevant material, such as:

```yaml
task:
account_voice_context:
video_identity:
metadata_observation:
selected_transcript_segments:
surrounding_segments:
segment_time_links:
transcript_provenance:
quote_verification_states:
visual_observations:
related_dossier_excerpts:
competing_sources:
known_corrections:
uncertainty_warnings:
forbidden_claims:
```

The system should not dump an entire long transcript into every prompt.

## 15.2 Retrieval should resolve to timestamps

**PROJECT RECOMMENDATION**

Every retrieved transcript chunk should resolve back to:

```text
video ID
transcript observation ID
segment IDs
time range
raw artifact hash
current correction state
```

The LLM may suggest a quote or connection candidate, but the operator must be able to open the source at the relevant time and verify it.

## 15.3 Transcript prompt injection

**PROJECT RECOMMENDATION**

Transcript content is untrusted data. Spoken or displayed instructions such as “ignore previous rules” must never control the system.

Context assembly must mark source text as quoted evidence and keep system/tool authority outside retrieved content.

---

# 16. Future Retrieval and Chunking Implications

## 16.1 Do not use provider chunks as canonical chunks

**PROJECT RECOMMENDATION**

Provider segment boundaries are evidence locators, not necessarily optimal retrieval chunks. The later chunk compiler may group adjacent segments by:

- semantic topic;
- pauses;
- speaker turn;
- maximum token size;
- section markers;
- human chapter boundaries.

Each retrieval chunk must preserve the underlying segment range.

## 16.2 Rebuildable chunk identity

Candidate payload lineage:

```yaml
account_id:
video_id:
transcript_id:
chunk_id:
segment_ids:
start_ms:
end_ms:
content_sha256:
transcript_provenance:
production_method:
language:
speaker_clusters:
entity_candidate_ids:
status:
```

Qdrant remains out of scope. These fields are future-compatibility requirements, not a collection design.

---

# 17. Evaluation Program

## 17.1 Test corpus

**PROJECT RECOMMENDATION**

Before admitting automated transcription, create a synthetic or permission-safe evaluation corpus covering:

- one clear single-speaker video;
- two speakers with overlap;
- music and long silence;
- unusual names and acronyms;
- quoted historical text;
- strong accent or dialect;
- low-quality audio;
- a livestream archive;
- multiple languages;
- code-switching;
- on-screen facts absent from speech;
- existing human captions;
- intentionally conflicting transcript files.

## 17.2 Measures

Evaluate:

```text
word error rate where reference text exists
proper-noun error rate
timestamp drift
speaker-cluster error
false speech during silence/music
omitted speech
language-detection error
cost per media hour
processing time
provider failure/retry behavior
deletion completion
```

## 17.3 Product-oriented retrieval tests

Example questions:

```text
Find the segment where the speaker alleges X.
Find the exact surrounding context before and after quote Y.
Find videos mentioning organization Z.
Do not return another account's transcript.
Do not return a superseded transcript as current without warning.
Show competing transcript readings for the same time range.
Return the source timestamp for every result.
```

---

# 18. Manual-First Product Workflow

## 18.1 Add YouTube Video

**PROJECT RECOMMENDATION**

Initial UI:

```text
Add Source
  └── YouTube Video
```

Required:

```text
owning account
YouTube URL
```

Optional:

```text
title
channel
published date
description
transcript paste
transcript upload
human notes
```

## 18.2 Transcript import choices

```text
Paste transcript
Upload transcript file
Continue without transcript
```

The application should not promise automatic transcript retrieval in the initial M06 implementation.

## 18.3 Review screen

Show:

```text
normalized video ID
supplied URL
metadata origin
transcript origin
language
caption/ASR method
segments and timestamps
parser warnings
duplicate candidates
source-account boundary
retention warning
```

## 18.4 Admission result

The admitted package should create:

```text
video source identity
source occurrence
metadata observation if supplied
transcript observation if supplied
original transcript artifact
normalized segments
human notes as separate records
candidate dossier/entity/assertion links
```

Candidate links remain unaccepted until human review.

---

# 19. Future Automation Ladder

**PROJECT RECOMMENDATION**

Automation should proceed through explicit stages:

```text
Stage 0 — Manual URL + manual transcript paste/upload
Stage 1 — Manual URL + official metadata lookup
Stage 2 — Manual URL + owner-selected transcript provider request
Stage 3 — Admitted channel notifications create pending candidates
Stage 4 — Admitted rules optionally request transcript automatically
Stage 5 — Periodic metadata/availability observations
```

No stage inherits authorization from the previous stage automatically.

Each stage needs its own cost, policy, retention, and hammer-test gate.

---

# 20. Failure and Stop Conditions

Stop and require human resolution when:

- the supplied URL does not resolve to one recognized video ID;
- official metadata and supplied identity disagree;
- the transcript provider does not disclose whether output was native or generated and policy requires that distinction;
- transcript language is unknown when language is required;
- the transcript is empty or malformed;
- timestamps are impossible or materially exceed known duration;
- a provider request exceeds cost or duration limits;
- a provider returns a transcript for a different video;
- the source becomes unavailable during acquisition;
- media acquisition would require an unadmitted downloader or bypass;
- a transcript contains likely private or restricted information outside the admitted scope;
- deletion from a third-party provider fails when deletion is required;
- the item belongs to another account’s source boundary;
- an automated monitor produces duplicates or replay conflicts;
- retrieved source content attempts to influence system/tool instructions.

The system should preserve failed acquisition receipts without admitting defective transcript content as usable knowledge.

---

# 21. Required Architecture Decisions

M06 synthesis must decide:

1. exact video/source/observation identifiers;
2. exact transcript provenance vocabulary;
3. exact admitted manual formats;
4. transcript artifact storage and hashing rules;
5. parser schemas for SRT, VTT, SBV, text, and provider JSON;
6. segment and word identity;
7. correction and competing-transcript behavior;
8. quote-verification states;
9. official metadata retention and refresh policy;
10. whether M06 includes a manual official metadata lookup;
11. whether any transcript SaaS evaluation belongs in M06 or a later connector package;
12. local ASR admission prerequisites;
13. lawful media-artifact acquisition boundary;
14. livestream and unavailable-video state vocabulary;
15. future channel-monitoring admission record;
16. future provider deletion receipts;
17. visual-observation format;
18. transcript prompt-injection handling;
19. evaluation corpus and acceptance thresholds;
20. which transcript fields are eligible for later semantic indexing.

---

# 22. Recommendations for M06

## 22.1 Include now

The initial M06 architecture should support:

- manually supplied YouTube URLs;
- normalized video identity;
- exact pasted transcript preservation;
- TXT, Markdown, SRT, VTT, and SBV transcript imports;
- optional provider JSON import as an explicitly identified external format;
- transcript provenance and language declarations;
- timestamped normalized segments;
- human corrections without overwriting originals;
- human visual notes tied to timestamps;
- review/admission states;
- future-provider-compatible acquisition receipts;
- retrieval-ready lineage without Qdrant.

## 22.2 Consider but do not automatically include

- manual official `videos.list` lookup;
- local Whisper/faster-whisper transcription of operator-supplied media;
- one bounded transcript-SaaS probe using synthetic or permission-safe media;
- creator-authorized caption API support for owned channels.

Each requires an exact work package and owner approval.

## 22.3 Exclude from initial M06 implementation

- arbitrary YouTube media downloading;
- generic caption scraping;
- undocumented YouTube endpoints;
- automatic transcript generation for every discovered video;
- unrestricted channel monitoring;
- bulk playlist/channel ingestion;
- automated speaker identity assignment;
- automated visual-frame extraction;
- Qdrant indexing;
- autonomous acceptance of claims or correlations.

---

# 23. Research Conclusion

The immediate product does not need to solve automatic YouTube transcription to become useful.

The strongest first capability is:

```text
paste YouTube URL
paste or upload transcript
preserve exact input
normalize timestamps and segments
review provenance and errors
admit to governed research memory
```

This workflow can accept outputs from YouTube’s transcript viewer, a downloaded creator-provided caption file, a transcript SaaS export, or a local ASR engine without changing the downstream Vault model.

Official YouTube metadata can later enrich the package through a narrow, policy-reviewed API connector. Channel notifications can later create pending candidates. Transcript SaaS and local ASR can later become alternative acquisition instruments. None should become a separate truth authority.

The governing rule is:

> A transcript records what a particular human, platform, provider, or model represented as speech from a video. It is never the video itself, and it never establishes that the statements inside it are true.

---

# 24. Primary and Provider Sources Consulted

## Official YouTube and Google

- YouTube Data API — Videos: list
  https://developers.google.com/youtube/v3/docs/videos/list
- YouTube Data API — Captions
  https://developers.google.com/youtube/v3/docs/captions
- YouTube Data API — Captions: list
  https://developers.google.com/youtube/v3/docs/captions/list
- YouTube Data API — Captions implementation guide
  https://developers.google.com/youtube/v3/guides/implementation/captions
- YouTube Help — View video transcripts
  https://support.google.com/youtube/answer/15930243
- YouTube API Services Terms of Service
  https://developers.google.com/youtube/terms/api-services-terms-of-service
- YouTube API Services Developer Policies
  https://developers.google.com/youtube/terms/developer-policies
- YouTube API Services Terms revision history
  https://developers.google.com/youtube/terms/revision-history
- YouTube Terms of Service
  https://www.youtube.com/t/terms
- YouTube push notifications guide
  https://developers.google.com/youtube/v3/guides/push_notifications

## Local speech-to-text projects

- OpenAI Whisper repository and README
  https://github.com/openai/whisper
- SYSTRAN faster-whisper repository and README
  https://github.com/SYSTRAN/faster-whisper

## Speech-to-text provider documentation

- Supadata transcript endpoint
  https://docs.supadata.ai/get-transcript
- AssemblyAI API overview and transcript submission
  https://www.assemblyai.com/docs/api-reference/overview
  https://www.assemblyai.com/docs/api-reference/transcripts/submit
- AssemblyAI speaker diarization
  https://www.assemblyai.com/docs/pre-recorded-audio/label-speakers
- AssemblyAI production retention
  https://support.assemblyai.com/articles/2240096256-does-assemblyai-offer-zero-data-retention
- Amazon Transcribe documentation
  https://docs.aws.amazon.com/transcribe/
- Amazon Transcribe speaker diarization
  https://docs.aws.amazon.com/transcribe/latest/dg/diarization.html
- OpenAI Audio API FAQ
  https://help.openai.com/en/articles/7031512-whisper-api-faq
- OpenAI API data usage policies
  https://help.openai.com/en/articles/5722486-api-data-usage-policies

## Evidence limitations

Provider documentation establishes advertised interfaces and policies, not independent performance or compliance. No transcript provider was called, no media was downloaded, no YouTube account was authenticated, no caption track was retrieved, and no transcription benchmark was executed during this research.
