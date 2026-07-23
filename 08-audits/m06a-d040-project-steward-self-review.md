# D040 Project-Steward Adversarial Self-Review

## Status

```text
Review type: Project-Steward adversarial self-review
Independent review claim: none
Application reviewed: 529165c19da30185dd9833fab608d6dc28dfed88
Verdict: D040 REQUIRES CORRECTION
Findings: two Medium, closure-blocking
SRT admission/canonical authority: none
```

## Limitation

Claude AI repeatedly crashed during review. At the owner's direction, the Project Steward conducted the review against live repositories. Because the steward implemented D040, this record is not described as independent.

## Verified Areas

The exact D040 path surface contains no migration or dependency changes and preserves every exact D039 tuple input. The SRT parser implements the admitted strict subset, deterministic warnings, source-order preservation, explicit encodings, exact timestamp checks, fail-closed limits, full byte/character/line reconciliation, fixed worker dispatch, under-test-only installation in fresh V0004 Vaults, no existing-Vault retrofit, and no SRT mutation route.

The immutable evidence files and hashes are valid. The D040 aggregate contains 24 unique required invariants, 24/24 passes, 35/35 mapped tests, no skipped/xfailed/xpassed/errored tests, clean commit state, exact fixture corpus hash, and the recorded parser tuple.

## Finding D040-SR-01 — Packaged worker does not verify the exact full D040 resource tuple

```text
Severity: Medium
Closure blocking: yes
Primary files:
- src/discrepancy_desk/srt_worker.py:25-39, 95-117, 120-159
- src/discrepancy_desk/srt_service.py:106-152
Governing requirements:
- parser-scoped immutable resources
- packaged sidecar uses exact SRT resources
- exact implementation/config/schema/dependency tuple
- M06A-SRT-019
```

The worker locates a directory merely by the presence of `manifest.sha256`. It validates the request's implementation hash against a hard-coded constant and validates the request's config hash against the current config file, but it does not validate the exact manifest hash, schema hash, dependency-lock hash, packaged implementation bytes, or the canonical D040 config tuple.

The config hash is request-supplied, so a modified packaged config can self-authorize by supplying its own new hash.

### Reproduction

A disposable copy of the packaged sidecar was modified outside the repository.

1. Replacing `schema.json` with `{"tampered":true}` still returned exit `0`, a succeeded receipt, and a candidate package.
2. Changing `input_size_limit_bytes` and sending the modified config's own hash also returned exit `0`, a succeeded receipt, and a candidate package.

The tests proving packaged execution therefore prove execution and denial controls, but not the claimed exact full parser-scoped resource tuple.

### Required correction

- pin the exact D040 manifest, config, schema, implementation, and dependency-lock hashes;
- validate the complete packaged resource set after security controls are installed and before request acceptance/parsing;
- compare request tuple fields only to the independently validated resource tuple, never to self-authorizing request material;
- add real packaged tamper tests for schema, config-with-matching-request-hash, manifest, dependency lock, and implementation bytes;
- emit no candidate on every mismatch.

## Finding D040-SR-02 — SRT resource failure hides unaffected D039 parser status

```text
Severity: Medium
Closure blocking: yes
Primary files:
- src/discrepancy_desk/srt_service.py:330-381
- src/discrepancy_desk/web.py:281-309
Governing requirements:
- parser-scoped resource isolation
- exact D039 preservation
- read-only SRT status
- M06A-SRT-022 and M06A-SRT-023
```

`list_all_parser_status()` appends `list_srt_status()` without isolating SRT resource-loading failures. A missing or tampered SRT resource raises through the combined function, causing the shared `/parsers` endpoint to return `409` and hiding the otherwise healthy D039 plain-text status and admission information.

### Reproduction

A disposable V0004 Vault returned `200` with both parser statuses. After changing only the SRT schema in a disposable project root, the same `/desktop-api/v1/vaults/{vault_id}/parsers` request returned `409` with `Vault parser status is unavailable.`

### Required correction

- SRT status must fail closed as one safe `unavailable` status row;
- the plain-text status must remain available when only SRT resources are missing or invalid;
- no absolute path, manifest content, or exception detail may leak;
- add an endpoint-level regression proving `200`, healthy plain-text status, and unavailable SRT status after SRT-only tamper.

## Verdict

```text
D040 REQUIRES CORRECTION
```

The findings grant no SRT admission or canonical authority. D039 remains unaffected.
