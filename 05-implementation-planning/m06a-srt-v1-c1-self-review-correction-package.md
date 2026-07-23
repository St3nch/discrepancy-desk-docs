# M06-A `m06a.srt.v1` C1 — Project-Steward Review Correction Package

## Status

```text
Package status: implemented and clean-evidence-bound; owner closure pending
Application baseline: 529165c19da30185dd9833fab608d6dc28dfed88
Correction commit: 6a8082253a52a601291efaf3ed85ee411b04be20
Correction return: 08-audits/m06a-srt-v1-c1-correction-return.md
Review disposition: 08-audits/m06a-d040-project-steward-self-review-disposition.md
Review source: 08-audits/m06a-d040-project-steward-self-review.md
Findings: D040-SR-01 and D040-SR-02 only
SRT admission authority: none
SRT canonical authority: none
```

## Purpose

Correct the two reproduced D040 self-review findings without changing the SRT parser implementation, parser resources, D039 tuple, migrations, dependencies, or product authority.

## Correction 1 — Exact packaged resource tuple

The packaged SRT worker must validate the exact D040 resource set before parsing:

```text
resource manifest SHA-256: 04d9f9780e13bc3658194d3f0d6cc8f6ce9426e154b8d229e4d5c80b2e20dd41
config SHA-256:            88db85d7b93ca55cf2f1bc3104941cf3076943ed388c4e803a480b75e5bbf309
schema SHA-256:            99ec97748389d61ead4d06b91416c64163b3f40269a473b9f1786ba20b0ba551
implementation SHA-256:    e8bc536aac12e60ebfa0962177af34fa4d05a6d564d08fcbf694dee1a88ccb2a
dependency lock SHA-256:   feb1aea2f45166a25c6b1618798790f65656db9490dc63d77481c519c8765351
```

Requirements:

- locate the fixed SRT resource root;
- install inherited security controls;
- validate manifest syntax, exact manifest hash, exact entries, exact config bytes/hash, exact schema hash/identity, exact packaged implementation bytes/hash, and exact dependency-lock bytes/hash;
- validate the request against that independently established tuple;
- never accept a request-supplied hash as authority for modified packaged bytes;
- every mismatch preserves a failed receipt and no candidate.

## Correction 2 — Per-parser status isolation

Requirements:

- SRT resource absence or mismatch returns one safe SRT `unavailable` status row;
- `canonical_available=false`, `admission_ready=false`, and `admission_manifest=null`;
- reason code is safe and content-free;
- D039 plain-text status remains available;
- the shared parser endpoint remains `200` when only SRT is unavailable;
- no path or exception detail leaks.

## Exact Changed-Path Surface

Allowed application paths:

```text
src/discrepancy_desk/srt_contract.py
src/discrepancy_desk/srt_service.py
src/discrepancy_desk/srt_worker.py
tests/test_m06a_srt_packaging.py
tests/test_m06a_desktop_workflow.py
scripts/run_ht_evidence.py
```

A listed path may be omitted. No other application path may change.

Explicitly forbidden:

```text
pyproject.toml
uv.lock
migrations/**
vault_migrations/**
parser_resources/**
src/discrepancy_desk/parser_service.py
src/discrepancy_desk/parser_worker.py
src/discrepancy_desk/parser_contract.py
src/discrepancy_desk/parsers/plain_text_v1.py
src/discrepancy_desk/parsers/srt_v1.py
desktop/**
```

## Exact C1 Invariants

```text
M06A-SRT-C1-001 exact D040 resource constants match live bytes
M06A-SRT-C1-002 packaged schema tamper fails with no candidate
M06A-SRT-C1-003 packaged config tamper fails even with matching request hash
M06A-SRT-C1-004 packaged manifest and dependency-lock tamper fail
M06A-SRT-C1-005 packaged implementation-byte tamper fails
M06A-SRT-C1-006 valid packaged execution remains green
M06A-SRT-C1-007 SRT-only resource failure returns safe unavailable status while D039 remains visible
M06A-SRT-C1-008 no SRT mutation or canonical authority enters
```

## Validation

Required on the clean correction commit:

```text
uv sync --frozen --extra dev
uv run ruff check .
uv run pytest -o addopts= --disable-warnings -q
focused correction tests
m06a-srt-v1-c1 exact profile
m06a-srt-v1 profile
m06a-text-v1 profile
m06a-phase3a profile
m06a-phase2 profile
m06a-phase1 profile
legacy profile
central/Vault heads 0005/V0004
Tauri tests and production build
Rust tests
fresh packaged sidecar build
exact changed-path audit
git diff --check
clean commit-bound evidence
```

## Stop Conditions

Stop and return to owner review if the correction requires:

- changing any parser resource or parser implementation bytes;
- changing the D039 tuple or route semantics;
- a migration or dependency;
- an SRT admission or canonical route;
- an unlisted path;
- weakening security controls or status fail-closed behavior.
