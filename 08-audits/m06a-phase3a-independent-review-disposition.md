# M06-A Phase 3A Independent Review Disposition

## Status

```text
Independent review verdict: M06-A PHASE 3A REQUIRES CORRECTION
Original implementation: 251b3ca841af46e63485b9ab5bf292cbae55a418
Correction authority: D037
Corrected application commit: 1337b5ac450ae82664aa1ad9667a85af41c4351e
Correction evidence: complete and clean commit-bound
Parser admission: not granted
Canonical parsing: unavailable
Owner closure: accepted through D038
```

The independent report is preserved at `08-audits/claude-m06a-phase3a-independent-static-review.md`. The Project Steward reproduced every material finding before implementation and applied the exact D037 correction package without widening Phase 3A.

## Finding Dispositions

### C1-F01 — Current-head inherited regression proof gap

**Reproduced:** yes.

The V0002 fixture and hard-coded backup head caused schema-dependent inherited Phase 3A nodes, especially HT-066 through HT-070, to execute against the historical schema.

**Correction:**

- the default M06-A Vault fixture now uses the live V0004 head;
- historical V0002 and V0003 fixtures are explicitly named and used only for historical compatibility/migration proofs;
- generic backup generation derives the actual database migration head;
- current-head backup, recovery, migration, actor, policy, ingestion, and isolation nodes execute against V0004;
- direct governed V0001/V0002/V0003-to-V0004 migration source heads are recognized without bypassing identity or dirty-migration checks.

**Disposition:** corrected; blocking finding cleared.

### C1-F02 — Worker filesystem/process denial gaps

**Reproduced:** yes, executable.

A disposable child process successfully used low-level `os.open` to write outside the operation root and used `os.remove` outside the operation root under the original controls.

**Correction:**

- low-level open flags now classify `O_WRONLY`, `O_RDWR`, `O_CREAT`, `O_APPEND`, and `O_TRUNC` as writes;
- filesystem mutation events for remove, rename, replace, truncate, directory, link, permission, ownership, and timestamp operations are operation-root constrained;
- `os.exec*`, `os.spawn*`, `os.startfile*`, shell, subprocess, socket, DNS, HTTP, and dynamic-library paths fail closed;
- deterministic environment variables and credential/proxy removal are applied to the child environment before interpreter startup;
- source and packaged workers self-test low-level write, mutation, exec, and Windows startfile denial before parsing;
- the exact Phase 3A HT-035 and HT-044 nodes execute the corrected proofs.

**Disposition:** corrected; blocking finding cleared.

### C1-F03 — Emitted-locator coverage not reconciled

**Reproduced:** yes, executable.

The original validator accepted a package claiming complete `[0,N]` coverage while its only emitted locator covered a strict subrange.

**Correction:**

- candidate validation now receives the original input bytes;
- encoding and BOM are reproduced from source bytes;
- every element and region locator is structurally validated;
- byte and character slices must reproduce exact raw text and hashes;
- line locators must match touched logical lines;
- emitted byte and character spans must form exact contiguous non-overlapping coverage with no terminal omission;
- BOM bytes are represented explicitly as an `encoding_preamble` region;
- omitted-prefix, gap, overlap, malformed locator, and BOM coverage cases fail closed.

**Disposition:** corrected; blocking finding cleared.

### C1-F04 — Immutable parser identity and package/document lineage

**Reproduced:** yes, with broader impact than the original report described.

The original fixed parser definition ID could not represent a corrected immutable tuple. Package reuse and document versioning also lacked an exact execution/package/artifact chain and artifact-lineage ordinal scope.

**Correction:**

- V0004 was added; V0001 through V0003 remain byte-unchanged;
- parser definition IDs are derived from the immutable implementation/resource/dependency/package/determinism/security tuple;
- configuration and admission IDs remain tuple-bound;
- V0004 adds append-only `parser_execution_package_links`;
- package reuse requires the same successful input and complete parser/config/admission/security tuple;
- document versions require an exact execution/package/source-artifact chain;
- `version_ordinal` uniqueness is enforced across the acquisition-artifact lineage;
- destructive V0004 downgrade refuses governed corrected authority.

**Disposition:** corrected; blocking finding cleared.

### C1-F05 — Packaged identity verification conditional

**Reproduced:** yes.

**Correction:** implementation-source bytes and dependency-lock bytes are now mandatory in source and packaged modes. Absence fails closed; present bytes must hash to the manifest. Synthetic tests prove both missing-resource failures.

**Disposition:** corrected.

### C1-F06 — Read-boundary and deterministic-environment accuracy

**Reproduced:** yes.

**Correction:** governing wording now distinguishes immutable interpreter/import/resource reads from operation-root-only writes and mutations. Enforceable deterministic environment settings are applied at child launch rather than represented as post-start interpreter controls.

**Disposition:** corrected.

## Corrected Implementation Boundary

Application commit `1337b5ac450ae82664aa1ad9667a85af41c4351e` changed exactly 19 D037-authorized paths:

- one new V0004 migration;
- parser contract/service/worker/plain-text implementation;
- migration routing/specification and backup-head behavior;
- resource and migration manifests;
- evidence registry;
- current/historical test fixtures and focused regression tests.

It did not change `pyproject.toml`, `uv.lock`, central migrations, V0001–V0003, Tauri product code, Jinja templates, or later-phase modules. It added no dependency and no new parser format.

## Clean Commit-Bound Validation

```text
Application commit                                      1337b5ac450ae82664aa1ad9667a85af41c4351e
ruff                                                    passed
full Python suite                                        215 passed
focused parser/migration/backup suite                    55 passed
Phase 3A hammer                                          35/35 invariants; 53/53 mapped tests
Phase 2 hammer                                           33/33 passed
Phase 1 hammer                                           31/31 passed
legacy hammer                                            31/31 executed/passed; HT-14 approved deferral
central/Vault heads                                      0005 / V0004
Tauri tests                                              4/4 passed
frontend production build                               passed
Rust tests                                               3/3 passed
packaged sidecar build                                   passed
packaged sidecar SHA-256                                 04052260311a87eae072a0bfb8987c6f0435d53aa63cf784a86298590aefa166
working tree                                             clean
remote                                                   synchronized with origin/main
```

Immutable evidence:

```text
full suite SHA-256       13780c36c695ba441f9c4ed1146c3a903709b48b467c5d5586927bde5ccb60ad
Phase 3A SHA-256         dab23159b696574ee972aa5ecec3037fcef546a72f6f6a7ca8fedf81df54185e
Phase 2 SHA-256          66f7bd0639899f4c253bdbe0d29764c737c8a2db03955137ce1804c3e9a785f2
Phase 1 SHA-256          c5f89dbacb94386851297c9832e8d10e17eef26fc4c2cd9d167de0e8d4773810
legacy SHA-256           f4cbb4c628112be990d53eba611101d396b8ebc4208f5c7a4e6795bb325f33e1
```

The Phase 3A aggregate records V0004, security profile `m06a.parser-worker.windows.v2`, implementation `1.1.0`, source and packaged execution, self-tested denials, deterministic package bytes, exact package recovery/isolation, `under_test`, zero production `owner_admitted` rows, canonical unavailable, and `working_tree_dirty: false`.

## Closure Recommendation

All independently reported and Project-Steward-reproduced findings are corrected and clean-evidence-bound. D038 accepts the disposition and closes Phase 3A.

D038 closure does not admit `m06a.text.v1`, authorize canonical parsing, authorize Phase 3B or later M06-A phases, or authorize M06-B.
