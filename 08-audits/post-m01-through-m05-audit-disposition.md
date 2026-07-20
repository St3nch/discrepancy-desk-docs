# Post-M01 Through M05 Independent Audit — Finding Disposition

## Audit Verdict

```text
ACCEPT WITH CORRECTIONS
```

The audit found no critical or high issue and no defect compromising approval authority, account isolation, loopback/token boundaries, audit integrity, recovery, installer data preservation, or repository secret hygiene.

## Finding Dispositions

### M-1 — Lifecycle doctrine and implementation divergence

**Disposition:** accepted.

The restrictive implemented transition set is the correction baseline. The dead `publication_mismatch → published` table entry must be removed. `captured → withdrawn` is accepted explicitly. Doctrine and exact-contract tests must be updated together.

### L-1 — Full-suite evidence overwritten by hammer sessions

**Disposition:** accepted.

Full-suite, focused, and per-invariant hammer sessions must write distinct evidence paths. AC-01 must retain a new full-suite record and hammer manifest bound to the correction commit.

### L-2 — External `age` prerequisite undeclared

**Disposition:** accepted.

The external `age` and `age-keygen` tools must be documented. Missing tooling must fail with a clear prerequisite error or produce an explicit test skip. Packaged backup/key-management remains M15 scope.

### L-3 — Legacy milestone and directory numbering ambiguity

**Disposition:** accepted with clarification.

The legacy files were already supersession stubs, but their active-directory placement remained ambiguous. They move under `05-implementation-planning/legacy/`. `08-operations/` is renamed `09-operations/`; all references must follow the move.

### L-4 — STATUS foregrounds M03 validation

**Disposition:** accepted.

STATUS must foreground the current accepted M05 baseline and later the AC-01 correction binding.

### L-5 — npm/pnpm ambiguity and stale sidecar artifact

**Disposition:** accepted.

`desktop/package-lock.json` is the JavaScript authority and canonical documentation uses npm. The rejected one-file sidecar is ignored local residue and should be deleted when Windows releases its handle. The on-directory sidecar remains the admitted packaging model.

## Observation Dispositions

- Constant-time token comparison: no required change for an ephemeral random loopback token.
- `Cargo.lock`: promote to committed reproducibility authority for the Tauri application.
- Generated-output test: test Git tracking rather than absence from a developer's working directory.
- Capability contract: update to `core:default` plus bounded `dialog:allow-open`.

## Closure

AC-01 is closed:

```text
Application correction commit: 17f40e3d18a58ac47b48933551a4044586d940aa
Application evidence binding: 6cbd036
Full-suite evidence: 104 passed, SHA-256 78fc6073c087eb35f404b57030137d8a2c9e51b28f8da4c903812f2a64b40796
Hammer evidence: 31 executed, 31 passed, 0 failed, SHA-256 baaba75a25125e9dde53bbf8255e13d1c4a6e4df66c20985b3857bad1c898dbf
```

R-M06-01 may begin. M06 implementation remains blocked.
