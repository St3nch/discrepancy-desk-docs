<!--
  Frozen historical artifact — do not edit.
  Preserved from the v1 documentation archive on 2026-08-09.
  Original path: vedaops-legacy-2026-08-05/discrepancy-desk-docs/09-operations/vela-incident-first-live-pipeline-pass.md
  Companion disposition: reference/vela-v1-findings-disposition.md
  Purpose: durable Vela benchmark source-of-record for ticket 17 / F-68.
-->

# B1-L7 First Live Pipeline Pass — Vela Incident

## Status

```text
Authorized by owner: 2026-07-26
Original decision: D080
Closed by owner: 2026-07-27
Closure decision: D081
Execution mode: manual, human-governed
Original application baseline: 921f938de85944613512efe1a44d41c7c1db8d1b
Current preserved application baseline: 8f300e1ab462a1f0692aeb5a61b4421c98db0717
Gate-3: owner-closed through D079
Gate-4 and Gate-5: frozen through D082
Outcome: intentionally incomplete; technically informative but editorially unsuccessful
Draft disposition: five Project Steward GPT drafts preserved as diagnostic evidence and retired from publication
Approvals/publications/metrics: none
Platform automation: forbidden
Application changes: not authorized by this operational record
```

The original completion criteria below are preserved as the live-pass contract. They were not met. Current findings and successor direction are governed by `08-audits/vela-findings-disposition.md`, the two preserved Claude reviews, and D081 through D087.

---

# 1. Purpose

Run the first real research-to-publication pass through B1-L7 using the 22 September 1979 Vela Incident.

This pass must answer three questions:

1. Does the governed editorial loop work end to end?
2. Do refusal paths reject invalid operations?
3. Does the system help produce publishable Discrepancy Desk content at a sustainable human cost?

A technically correct refusal is not enough. The pass must measure whether the resulting work is interesting, usable, and worth repeating.

---

# 2. Hard Boundaries

The agent may:

```text
research public sources
assemble the source manifest and claim ledger
prepare draft text
prepare operator commands and paste packets
prepare timing and metrics logs
identify workflow friction and product findings
```

The agent may not:

```text
interact with X or another platform
approve its own text
post, reply, repost, like, follow, or send direct messages
directly edit the production database
alter approved text after human approval
present disputed attribution as confirmed fact
impersonate an agency or imply real classified access
make an application change without a new explicit owner decision
```

The owner performs every governed transition and all platform interaction.

---

# 3. Case Identity

```text
Case: The Vela Incident
Date: 22 September 1979
Detected by: U.S. Vela 6911 satellite
Core discrepancy: a nuclear-test-like double flash followed by conflicting official and scientific interpretations
Public resolution: unresolved
Attribution: unverified
```

---

# 4. Source Manifest

## S-01 — National Security Archive 2016 briefing book

```text
Title: The Vela Incident: South Atlantic Mystery Flash in September 1979 Raised Questions about Nuclear Test
Publisher: National Security Archive, George Washington University
Type: archival briefing book with declassified documents
URL: https://nsarchive.gwu.edu/briefing-book/nuclear-vault/2016-12-06/vela-incident-south-atlantic-mystery-flash-september-1979-raised-questions-about-nuclear-test
Use: primary source index; CIA panel, Ruina panel, Carter administration, DIA/NRL dispute, attribution limits
```

Supported statements:

```text
Vela 6911 recorded a flash on 22 September 1979.
A CIA-sponsored panel said the signals were consistent with an atmospheric nuclear explosion.
The later White House/Ruina panel concluded the signal was probably not nuclear.
The Ruina panel considered a meteoroid or space-debris reflection.
DIA official Jack Varona argued the weight of evidence pointed toward a nuclear event.
NRL-analyzed hydroacoustic signals were described as characteristic of maritime nuclear shots.
No direct public evidence definitively attributed the event to Israel, South Africa, or another state.
```

## S-02 — National Security Archive Document 39

```text
Title: Naval Research Laboratory Analysis of Data Relevant to 22 September 1979 Possible Nuclear Event
Date: 17 June 1980
Type: declassified State Department memorandum recounting DIA/NRL findings
URL: https://nsarchive.gwu.edu/document/22351-39-robert-martin-inrpma-political-military
OCR: https://nsarchive.gwu.edu/media/22351/ocr
Use: hydroacoustic dispute; Varona criticism; Prince Edward/Marion Islands location claim
```

Qualification:

```text
This memorandum reports what Varona said about NRL analysis.
It is not the still-unpublished full NRL report.
```

## S-03 — National Security Archive 2019 briefing book

```text
Title: The Vela Flash: Forty Years Ago
Publisher: National Security Archive
URL: https://nsarchive.gwu.edu/briefing-book/nuclear-vault/2019-09-22/vela-flash-forty-years-ago
Use: iodine-131 record, Arecibo disturbance, agency disagreement, NRL report history
```

Supported statements:

```text
Australian sheep thyroid samples showed unusually high iodine-131 levels.
The Arecibo Observatory detected an ionospheric disturbance that some officials viewed as potentially corroborative.
The NRL report was once described as unlocatable.
```

## S-04 — Wilson Center critical oral-history report

```text
Title: Revisiting the 1979 VELA Mystery: A Report on a Critical Oral History Conference
Publisher: Wilson Center
URL: https://www.wilsoncenter.org/blog-post/revisiting-1979-vela-mystery-report-critical-oral-history-conference
Use: later testimony, NRL report characterization, continuing disagreement, report-location update
```

Supported statements:

```text
Former NRL director Alan Berman described a large classified NRL study favoring a nuclear-test hypothesis.
The report was presented as a most-probable hypothesis rather than conclusive proof.
The Navy later reportedly located the report and initiated declassification review.
Conference participants still could not conclusively settle the event.
```

## S-05 — De Geer and Wright, 2018

```text
Title: The 22 September 1979 Vela Incident: Radionuclide and Hydroacoustic Evidence for a Nuclear Explosion
Journal: Science & Global Security 26(1), 20–54
DOI: 10.1080/08929882.2018.1451050
Archive page: https://scienceandglobalsecurity.org/archive/2018/05/the_22_september_1979_vela_inc_1.html
Use: scholarly reanalysis of iodine-131 and hydroacoustic evidence
```

Qualification:

```text
The paper argues that the combined evidence strongly indicates a nuclear explosion.
That is a scholarly conclusion, not a definitive public government determination.
```

## S-06 — Nuclear Weapon Archive overview

```text
Title: The Vela Incident
Author: Carey Sublette
URL: https://nuclearweaponarchive.org/Safrica/Vela.html
Use: technical and historical orientation; not the controlling source for disputed claims
```

## S-07 — Carter diary statement

```text
Source: Jimmy Carter, White House Diary, entry for 27 February 1980
Publicly reproduced and discussed by the National Security Archive
Use: the president's contemporaneous belief, not proof of attribution
```

## S-08 — Case-owner notes

```text
Type: manual note
Use: editorial angle selection, voice, owner judgments, workflow observations
Authority: no factual claim authority unless separately sourced
```

---

# 5. Claim Ledger

## CONFIRMED

```text
C-01  Vela 6911 recorded a double-flash-type optical signal on 22 September 1979.
C-02  U.S. officials investigated whether the signal represented an atmospheric nuclear explosion.
C-03  A CIA-sponsored scientific panel found the signals consistent with a nuclear explosion.
C-04  A later White House scientific panel concluded the signal was probably not nuclear.
C-05  The White House panel considered a meteoroid or space-debris reflection as a possible explanation.
C-06  U.S. agencies and laboratories disagreed materially about the event.
C-07  No definitive public U.S. determination resolved the event or its attribution.
```

## OBSERVED OR REPORTED EVIDENCE

```text
O-01  Hydroacoustic signals were analyzed and reported as potentially consistent with a maritime nuclear event.
O-02  Australian sheep thyroid samples showed unusually high iodine-131 levels.
O-03  The Arecibo Observatory recorded an ionospheric disturbance near the relevant time.
O-04  Declassified memoranda preserve claims about NRL findings even though the full NRL study is not public.
```

## DISPUTED INTERPRETATIONS

```text
D-01  The event was a low-yield nuclear explosion.
D-02  The Ruina panel's non-nuclear conclusion was politically influenced.
D-03  The hydroacoustic and radionuclide observations originated from the same event.
D-04  The apparent source was near the Prince Edward and Marion Islands.
D-05  The optical signal's anomalies can be explained while retaining a nuclear interpretation.
```

## UNVERIFIED ATTRIBUTION

```text
U-01  Israel conducted the test.
U-02  South Africa conducted the test.
U-03  Israel and South Africa conducted a joint test.
U-04  A ship or another state was responsible.
```

## PATTERN CANDIDATE — NOT PROOF

```text
P-01  Prior Vela double-flash detections were associated with known nuclear tests.
```

Rule:

```text
P-01 may justify continued attention.
P-01 may not be used to claim that the 22 September event was necessarily nuclear.
```

---

# 6. Work Items

## WI-1 — Case File: The Double Flash

```text
Lane: docket
Priority: 1
Topic: Vela Incident
Tags: vela, nuclear-history, unresolved, declassified, case-file
Required path: capture → organize → sources → draft → human review → approval → manual-ready → manual publication → metrics
```

### Draft thread

**1/5**

```text
CASE FILE — 22 SEP 1979

A U.S. Vela satellite recorded a double flash over the southern oceans.

The signal resembled an atmospheric nuclear detonation.

What followed was not a finding. It was an argument between instruments, agencies, and panels.
```

**2/5**

```text
CONFIRMED

A CIA-sponsored scientific panel said the signal was consistent with a nuclear explosion.

A later White House panel said it was probably not nuclear and proposed a possible meteoroid or space-debris reflection.
```

**3/5**

```text
OBSERVED

Other records pointed back toward a test: disputed hydroacoustic signals, iodine-131 in Australian sheep thyroids, and an ionospheric disturbance recorded at Arecibo.

None of those observations, alone, closed the case.
```

**4/5**

```text
DISPUTED

DIA officials rejected the White House conclusion. President Carter later recorded a growing belief among scientists that Israel had conducted a test.

Public attribution to Israel, South Africa, or a joint operation remains unverified.
```

**5/5**

```text
STATUS

No definitive public U.S. determination settled the event.

The classified NRL hydroacoustic report was once reported unlocatable and later reportedly found for declassification review.

The drawer remains open.
```

## WI-2 — Requisition Denied

```text
Lane: docket
Priority: 2
Tags: requisition-denied, vela, missing-records, nrl
```

Draft:

```text
REQUISITION DENIED

Requested: the Naval Research Laboratory's hydroacoustic report on the 22 September 1979 event.

Status: still not public. Once reported unlocatable; later reportedly found for declassification review.

The file exists more confidently than the conclusion.
```

## WI-3 — Chain of Custody

```text
Lane: archive
Priority: 3
Purpose: deliberately exercise publication_mismatch without publishing a false factual claim
```

Approved text:

```text
CHAIN OF CUSTODY

The official file says “probably not nuclear.”
The internal argument says the matter never closed.
```

Public text for the mismatch exercise:

```text
CHAIN OF CUSTODY

The official file says “probably not nuclear.”
The internal argument says the matter never closed — formally.
```

The additional word is harmless but must produce an exact-text mismatch when the publication is recorded against the approved revision.

## WI-4 — Pattern Candidate

```text
Lane: archive
Priority: 3
Purpose: reject an overclaim and preserve the rationale
```

Deliberately unacceptable draft:

```text
Forty-one earlier Vela double flashes were nuclear tests. The forty-second was too.
```

Required rejection rationale:

```text
The prior detection history is a base-rate argument, not proof of the disputed 22 September event. The draft converts a pattern candidate into a false finding.
```

Corrected candidate:

```text
A base rate is not a finding. It is a reason to keep the drawer open.
```

## WI-5 — Filed Under

```text
Lane: flash_release
Priority: 2
Purpose: measure ceremony cost for a micro-post
```

Draft:

```text
Filed under: conclusions that became less certain as the evidence accumulated.
```

---

# 7. Refusal Tests

Record each result as `PASS`, `FAIL`, or `NOT EXECUTED`.

```text
R-01 Exact-text approval binding
Create an approved revision, create a successor, and verify the old approval does not authorize the successor.

R-02 Evidence path traversal
Attempt to register an evidence path containing ../ and confirm refusal before mutation.

R-03 Duplicate source
Attempt to add the exact same source locator twice and record the behavior.

R-04 Empty source
Submit a source without a usable locator or bounded note and confirm refusal.

R-05 Evidence hash drift
Register a valid evidence file, change the file outside the system, and confirm later verification detects disagreement. Preserve the original test artifact separately.

R-06 Duplicate evidence
Attempt to register the same verified evidence twice and record the behavior.

R-07 Schedule supersession
Schedule one work item, reschedule it, and verify the previous schedule is superseded rather than overwritten.

R-08 Publication mismatch
Publish the harmless WI-3 variant and record it against the approved text so the item enters publication_mismatch.

R-09 Pattern overclaim
Reject WI-4's deliberately unacceptable draft with the required rationale.

R-10 Scanned-PDF reality check
Attempt no fake parser success. Record that scanned primary-source PDFs remain unreadable by the admitted text parser unless a governed human transcription or text extraction is supplied.
```

Safety constraint:

```text
Do not damage a real source artifact for R-05.
Use a copied test artifact created specifically for the drift exercise.
```

---

# 8. Editorial Viability Scorecard

For every work item record:

```text
time to first usable draft
number of meaningful owner rewrites
whether claim labels improved or flattened the writing
whether the final item still sounds like The Discrepancy Desk
whether the owner would voluntarily post it today
number of sources consulted
number of sources actually needed
number of viable angles discovered
number of angles abandoned because the system could not represent them
phone or desktop used for each transition
whether any step required leaving the control room
wall-clock minutes from capture to manual-ready
wall-clock minutes from manual-ready to publication record
```

Owner ratings, 1–5:

```text
publishability
brand voice
factual confidence
workflow tolerability
repeatability
```

---

# 9. Metrics

For WI-1, WI-2, and WI-5 record at:

```text
publication
+24 hours
+72 hours
```

Metrics:

```text
impressions
likes
reposts
replies
bookmarks
profile visits
new follows attributable if available
```

Comparison of interest:

```text
Requisition Denied versus Case File
Filed Under ceremony cost versus long-thread ceremony cost
```

---

# 10. Predicted Findings

```text
PF-01 Workflow ceremony may be nearly flat across long and short posts.
PF-02 The scanned-PDF gap will appear immediately.
PF-03 Confirmed/observed/disputed/unverified labels may lack a direct schema home.
PF-04 Approval-to-post handoff may require too much copying and context switching.
PF-05 Strong governance may improve trust while excessive ceremony reduces sustainable output.
```

---

# 11. Completion Criteria

The pass completes when:

```text
WI-1 Case File is published and measured.
WI-2 Requisition Denied is published and measured.
WI-5 Filed Under is published and measured.
WI-3 produces publication_mismatch and a governed resolution.
WI-4 is rejected with the stated rationale; the corrected candidate is preserved separately.
All refusal tests have recorded outcomes.
Every transition has timing data.
All five predicted findings are scored supported, unsupported, or mixed.
The owner records whether the system is worth using again.
A Vela findings disposition is recorded before any Gate-4 or Gate-5 work.
```

---

# 12. Stop and Escalate

Stop the affected operation and record a product finding if:

```text
a required source or claim cannot be represented honestly
a workflow step requires direct database editing
a refusal path mutates state before refusing
a human-only decision can be executed by a model/system actor
approved text can be silently altered
publication can be recorded without an exact approval relationship
the app requires a code or schema change
```

No application change is pre-authorized by this live pass.
