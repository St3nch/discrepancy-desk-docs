# M06-A Adversarial Closure Matrix — Owner-Accepted Planning Baseline

## Status

```text
Status: owner-accepted planning baseline
Phase 1 rows: implemented and clean-evidence-bound to application commit 8fe3be4
Independent Phase 1 audit: deferred under D032; required before Phase 2
Later-phase rows: planning only
```

This is the executable-plan matrix. At D028 acceptance no listed test was claimed to exist or have executed. D030 authorized implementation and execution of the exact Phase 1 row set, now completed with clean evidence bound to application commit `8fe3be4`. D032 defers the independent Phase 1 audit without claiming it passed. Every later row must be implemented, collected, executed, and bound to commit-matched evidence before its corresponding phase can close.

Governing references:

- `05-implementation-planning/hammer-test-strategy.md`;
- `05-implementation-planning/m06a-local-manual-vault-canonical-plan.md`;
- `05-implementation-planning/m06a-parser-admission-plan.md`;
- `08-audits/m06a-local-manual-vault-planning-package-independent-review.md`;
- D030 through D032 in `99-decisions/decision-log.md`.

---

# 1. Matrix Contract

Each row contains:

```text
invariant ID
requirement source
implementation phase
valid fixture
deliberate violation fixture
planned pytest node or exact test identifier
real-engine requirement
expected fail-closed result
evidence output
closure requirement
```

Fixture roots are planned under:

```text
tests/fixtures/m06a/
```

Per-invariant evidence is planned under:

```text
runtime/test-evidence/hammer/m06a/<invariant-id>.json
```

The aggregate runner output is planned under:

```text
runtime/ht-evidence/latest-m06a-ht-evidence.json
```

---

# 2. Vault Identity, Account Isolation, and Paths

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-001 | D03; D027; core §§3–6 | 1 | two registered brand Vaults with distinct roots and SQLite files | Vault A request using Vault B opaque ID, database, or content | `tests/test_m06a_vault_identity.py::test_m06a_ht_001_vault_isolation` | two SQLite DBs + FS | no cross-Vault row/file visible; no audit mutation in either Vault | `M06A-HT-001.json` | Phase 1 and final |
| M06A-HT-002 | core §5 | 1 | registry, marker, and DB singleton agree | registered root contains another Vault's DB | `tests/test_m06a_vault_identity.py::test_m06a_ht_002_wrong_database_rejected` | SQLite + FS | open rejected before migration/write | `M06A-HT-002.json` | Phase 1 |
| M06A-HT-003 | core §5 | 1 | valid marker fingerprint | copied/edited `VAULT_IDENTITY.json` | `tests/test_m06a_vault_identity.py::test_m06a_ht_003_marker_tamper_rejected` | SQLite + FS | identity mismatch; Vault unavailable | `M06A-HT-003.json` | Phase 1 |
| M06A-HT-004 | core §5; F-09 | 1 | existing registered DB opens read/write | typo/missing DB path passed to open | `tests/test_m06a_vault_identity.py::test_m06a_ht_004_open_never_creates_database` | SQLite + FS | error; no directory or SQLite file created | `M06A-HT-004.json` | Phase 1 |
| M06A-HT-005 | core §§4–5 | 1 | normalized safe relative Vault root | `..`, absolute path, drive-relative, UNC path | `tests/test_m06a_vault_identity.py::test_m06a_ht_005_path_traversal_rejected` | Windows FS | rejected before filesystem access | `M06A-HT-005.json` | Phase 1 |
| M06A-HT-006 | core §5 | 1 | safe Windows directory name | `CON`, `NUL`, trailing dot/space, ADS colon, case collision | `tests/test_m06a_vault_identity.py::test_m06a_ht_006_windows_name_and_case_rules` | Windows FS | creation/registration rejected deterministically | `M06A-HT-006.json` | Phase 1 + packaged |
| M06A-HT-007 | core §5 | 1 | ordinary directories/files | symlink, junction, or reparse point in root/database/objects/packages/temp/backups | `tests/test_m06a_vault_identity.py::test_m06a_ht_007_reparse_points_rejected` | Windows FS | open/intake/backup rejected; no target traversal | `M06A-HT-007.json` | Phase 1 + final |
| M06A-HT-008 | D027; core §§3–4 | 1 | platform account actively bound to The Discrepancy Desk Vault | unbound, fabricated, removed, or wrong-brand account binding | `tests/test_m06a_routing.py::test_m06a_ht_008_platform_binding_required` | central + Vault SQLite | combined workflow rejected; no cross-database or publication authority inferred | `M06A-HT-008.json` | Phase 1 |

---

# 3. Actor Authority, Audit, and Idempotency

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-009 | D09; core §9; F-06 | 1 | active human ActorContext resolved server-side | request body supplies `actor_id=owner` | `tests/test_m06a_actor_authority.py::test_m06a_ht_009_request_actor_impersonation_rejected` | SQLite + service | supplied actor ignored/rejected; no mutation | `M06A-HT-009.json` | Phase 1 |
| M06A-HT-010 | core §9 | 1 | active human performs human-only ruling | system actor attempts admission/editorial-use/merge | `tests/test_m06a_actor_authority.py::test_m06a_ht_010_system_human_authority_rejected` | SQLite | transaction rolled back; audit refusal preserved | `M06A-HT-010.json` | Phase 1 and Phase 5 |
| M06A-HT-011 | core §9 | 1 | candidate-only model actor creates proposal | model actor attempts accepted conclusion or policy ruling | `tests/test_m06a_actor_authority.py::test_m06a_ht_011_model_authority_rejected` | SQLite | candidate may persist; authority record absent | `M06A-HT-011.json` | Phase 5 |
| M06A-HT-012 | core §9 | 1 | active actor in selected Vault | unknown, disabled, revoked, or other-Vault actor | `tests/test_m06a_actor_authority.py::test_m06a_ht_012_actor_status_and_scope_enforced` | SQLite | operation rejected atomically | `M06A-HT-012.json` | Phase 1 |
| M06A-HT-013 | core §9 | 1 | desktop token plus server-resolved owner context | valid desktop token with fabricated human identity | `tests/test_m06a_desktop_contract.py::test_m06a_ht_013_token_is_not_human_identity` | service + SQLite | token authenticates session only; decision refused | `M06A-HT-013.json` | Phase 1 + packaged |
| M06A-HT-014 | D01; core §13 | 1 | append-only valid audit chain | direct update/delete or chain-byte mutation | `tests/test_m06a_actor_authority.py::test_m06a_ht_014_audit_tamper_detected` | SQLite | engine blocks normal mutation; verifier detects out-of-band tamper | `M06A-HT-014.json` | Phase 1 + restore |
| M06A-HT-015 | core §13 | 1 | exact operation replay with same request hash | reused key with changed actor/Vault/content | `tests/test_m06a_actor_authority.py::test_m06a_ht_015_idempotency_scope_and_conflict` | SQLite | exact replay returns same result; conflict rejected | `M06A-HT-015.json` | Phase 1 |
| M06A-HT-016 | D027; core §13; F-11 | 1 | central and Vault receipts share correlation ID | Vault commit succeeds, central completion receipt fails | `tests/test_m06a_routing.py::test_m06a_ht_016_cross_database_partial_failure_reconciles` | central + Vault SQLite | no atomic-success claim; explicit reconciliation-required state | `M06A-HT-016.json` | Phase 1 + final |
| M06A-HT-017 | core §13 | 1 | both receipts and hashes agree | fabricated/mismatched correlation or result hash | `tests/test_m06a_routing.py::test_m06a_ht_017_cross_database_receipt_fabrication_rejected` | two SQLite DBs | reconciliation refuses closure | `M06A-HT-017.json` | Phase 1 |

---

# 4. Observation, Acquisition, Artifact Storage, and Provenance

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-018 | D02; core §§6–7; F-02 | 2 | source→item→occurrence→observation chain | acquisition created without required observation/manual intake event | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_018_observation_chain_required` | SQLite | FK/semantic validation rejects transaction | `M06A-HT-018.json` | Phase 2 |
| M06A-HT-019 | core §7; F-04 | 2 | acquisition starts `started`, outcome null, then finalizes | acquisition inserted initially as failed or success without artifact | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_019_truthful_acquisition_lifecycle` | SQLite | invalid state/outcome rejected | `M06A-HT-019.json` | Phase 2 |
| M06A-HT-020 | core §7 | 2 | YouTube URL creates locator/observation only | URL-only intake marked successful remote acquisition | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_020_locator_is_not_acquisition` | SQLite | no artifact/document version; false success rejected | `M06A-HT-020.json` | Phase 2 |
| M06A-HT-021 | core §7; F-03 | 2 | same bytes acquired twice from distinct encounters | uniqueness rule collapses acquisition receipts | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_021_repeated_identical_bytes_preserve_encounters` | SQLite + FS | one artifact object, two acquisitions/links/receipts | `M06A-HT-021.json` | Phase 2 |
| M06A-HT-022 | core §7 | 2 | existing object bytes/hash/size agree | content-addressed path exists with altered bytes | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_022_artifact_overwrite_or_hash_mismatch_rejected` | SQLite + FS | no overwrite/reuse; integrity incident recorded | `M06A-HT-022.json` | Phase 2 |
| M06A-HT-023 | core §7 | 2 | bounded temp file finalizes and commits | crash after temp write, before object move | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_023_temp_partial_write_reconciles` | SQLite + FS | no canonical object/link; pending receipt reconciled | `M06A-HT-023.json` | Phase 2 |
| M06A-HT-024 | core §§4,7 | 2 | object move succeeds and DB commit succeeds | crash after object move, before DB link commit | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_024_orphan_object_reconciliation` | SQLite + FS | unlinked object remains non-authoritative and blocked by reconciliation state; relink only with exact pending receipt | `M06A-HT-024.json` | Phase 2 |
| M06A-HT-025 | core §§6–7; F-08 | 2 | all parents share Vault and provenance chain | valid IDs from unrelated source/acquisition branches combined | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_025_cross_provenance_composition_rejected` | SQLite | composite FK/service chain check rejects | `M06A-HT-025.json` | Phase 2 |
| M06A-HT-026 | core §10; F-07 | 2/5 | typed evidence/member target exists | fabricated generic `ref_type/ref_id` attempts authority | `tests/test_m06a_assertions_entities.py::test_m06a_ht_026_polymorphic_authority_rejected` | SQLite | no authority-bearing generic reference persists | `M06A-HT-026.json` | Phase 5 |

---

# 5. Rights, Retention, and Policy

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-027 | D027; core §15; F-14 | 2 | explicit preservation-compatible policy allows bounded private use | absent, unknown, expired, or contradictory retention policy | `tests/test_m06a_policy_context.py::test_m06a_ht_027_unknown_rights_fail_closed` | Vault SQLite + FS | intake rejected before canonical byte copy; content-free receipt only | `M06A-HT-027.json` | Phase 2/5 |
| M06A-HT-028 | core §15 | 2/4 | internal retrieval allowed, public projection denied | public/quotation eligibility inferred from retention | `tests/test_m06a_policy_context.py::test_m06a_ht_028_rights_dimensions_independent` | SQLite + search | denied capability stays denied | `M06A-HT-028.json` | Phase 4/5 |
| M06A-HT-029 | D06; core §15 | 2 | new policy binding supersedes old immutably | artifact row or prior binding updated in place | `tests/test_m06a_policy_context.py::test_m06a_ht_029_policy_binding_is_versioned` | SQLite | mutation blocked/detected; lineage preserved | `M06A-HT-029.json` | Phase 2 |
| M06A-HT-030 | D027; core §§7,15 | 2 | preservation-compatible material | material declared to require deadline-based deletion or future-delete promise | `tests/test_m06a_policy_context.py::test_m06a_ht_030_timed_deletion_material_rejected` | Vault SQLite + FS | admission refused before byte copy; no artifact or purge promise | `M06A-HT-030.json` | Phase 2 and final |
| M06A-HT-031 | D05; core §16 | 5 | policy change creates complete impact work | affected editorial-use/publication candidate silently remains current | `tests/test_m06a_policy_context.py::test_m06a_ht_031_policy_impact_invalidates_use` | central + Vault SQLite | use blocked pending review; no publication inference | `M06A-HT-031.json` | Phase 5 |

---

# 6. Parser Security, Failure, and Determinism

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-032 | D11; D027; parser §§1–2; F-15 | 3A | exact owner-admitted parser/config/hash tuple and preservation-compatible same-Vault artifact link | candidate, unadmitted, mismatched, wrong-Vault, or retention-ineligible input invoked | `tests/test_m06a_parser_framework.py::test_m06a_ht_032_runtime_admission_manifest_enforced` | Vault SQLite + worker | invocation rejected; no execution or package | `M06A-HT-032.json` | each parser admission |
| M06A-HT-033 | parser §4; F-16 | 3A | clean parser uses no network | malicious parser calls `socket.socket`/connect | `tests/test_m06a_parser_framework.py::test_m06a_ht_033_socket_egress_denied` | worker | security violation; no outbound connection/package | `M06A-HT-033.json` | Phase 3A + packaged |
| M06A-HT-034 | parser §4 | 3A | clean parser performs local work | malicious parser calls DNS/getaddrinfo/urllib/HTTP client | `tests/test_m06a_parser_framework.py::test_m06a_ht_034_dns_and_http_denied` | worker | security violation and terminal failure | `M06A-HT-034.json` | Phase 3A + packaged |
| M06A-HT-035 | parser §4 | 3A | fixed worker process only | parser attempts subprocess, shell, or arbitrary executable | `tests/test_m06a_parser_framework.py::test_m06a_ht_035_subprocess_denied` | worker | execution denied; receipt preserved | `M06A-HT-035.json` | Phase 3A + packaged |
| M06A-HT-036 | parser §§4,12 | 3B | guarded local RSS/Atom parses | XML DOCTYPE/entity/XInclude/external resolver | `tests/test_m06a_parser_external.py::test_m06a_ht_036_xxe_and_resolvers_denied` | worker | terminal security failure; no entity expansion/network | `M06A-HT-036.json` | RSS admission |
| M06A-HT-037 | parser §13 | 3B | inert basic HTML extraction | script/iframe/event handler/external resource/active SVG | `tests/test_m06a_parser_external.py::test_m06a_ht_037_active_html_never_executes` | worker | active content excluded with exact warning or terminal failure; no fetch | `M06A-HT-037.json` | HTML admission |
| M06A-HT-038 | parser §14 | 3B | text-bearing unencrypted PDF | encrypted, image-only, malformed xref, attachment/action PDF | `tests/test_m06a_parser_external.py::test_m06a_ht_038_hostile_pdf_rejected` | worker | unsupported/security/terminal failure; no package | `M06A-HT-038.json` | PDF admission |
| M06A-HT-039 | D07; core §§4,8; F-05 | 3A | identical input/config in two processes | timestamps/random IDs/host info alter package bytes | `tests/test_m06a_parser_framework.py::test_m06a_ht_039_package_is_deterministic` | worker + FS | byte-identical hash required; mismatch receives governed database quarantine state and no document version | `M06A-HT-039.json` | Phase 3A |
| M06A-HT-040 | core §8 | 3A | run receipt differs, package remains same | execution receipt fields inserted into package hash | `tests/test_m06a_parser_framework.py::test_m06a_ht_040_execution_receipt_separate_from_package` | SQLite + FS | schema/hash validation rejects package | `M06A-HT-040.json` | Phase 3A |
| M06A-HT-041 | D07; parser §3; F-18 | 3A | explicit warning with complete coverage | parser silently omits final cue/page/node | `tests/test_m06a_parser_framework.py::test_m06a_ht_041_silent_partial_output_fails` | worker | `partial_output_failure`; no document version | `M06A-HT-041.json` | every parser admission |
| M06A-HT-042 | parser §§7–10 | 3A | boundary-sized text/subtitle/JSON | oversized, deep, node-bomb, giant cue/string | `tests/test_m06a_parser_stdlib.py::test_m06a_ht_042_limits_fail_closed` | worker | deterministic `limit_exceeded`; no partial package | `M06A-HT-042.json` | Phase 3A |
| M06A-HT-043 | parser §§3,5 | 3A | valid UTF-8/BOM handling | invalid encoding or replacement-character decoding | `tests/test_m06a_parser_stdlib.py::test_m06a_ht_043_encoding_is_explicit` | worker | terminal encoding failure; original preserved | `M06A-HT-043.json` | Phase 3A |
| M06A-HT-044 | parser §§17–18 | 3A/3B | source and packaged parser hashes match | sidecar missing resource/dependency or admission hash differs | `tests/test_m06a_packaging.py::test_m06a_ht_044_packaged_parser_authority_matches` | packaged sidecar | parser unavailable, not silently degraded | `M06A-HT-044.json` | each parser + Phase 6 |

---

# 7. Search, Chunks, and Projections

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-045 | D12; core §17; F-17 | 4 | SQLite and packaged sidecar both report FTS5 | packaged executable lacks FTS5 | `tests/test_m06a_packaging.py::test_m06a_ht_045_fts5_required_in_source_and_sidecar` | SQLite + packaged | Phase/startup blocked; no LIKE fallback | `M06A-HT-045.json` | Phase 4 + packaged |
| M06A-HT-046 | core §17 | 4 | bounded parameterized FTS query | malformed operators, injection-like string, huge query | `tests/test_m06a_search_chunks.py::test_m06a_ht_046_fts_query_is_bounded` | SQLite FTS5 | clean validation/query error; no SQL injection/write | `M06A-HT-046.json` | Phase 4 |
| M06A-HT-047 | core §17 | 4 | current allowed element appears | superseded/withdrawn/rights-denied element remains indexed | `tests/test_m06a_search_chunks.py::test_m06a_ht_047_stale_or_ineligible_search_results_removed` | SQLite FTS5 | stale result absent; invalidation work closed | `M06A-HT-047.json` | Phase 4 |
| M06A-HT-048 | D12; core §18 | 4 | same version/strategy yields same chunk IDs | content/policy/version changes but old chunk remains current | `tests/test_m06a_search_chunks.py::test_m06a_ht_048_chunk_identity_and_invalidation` | SQLite | old chunk invalid; deterministic replacement only | `M06A-HT-048.json` | Phase 4 |
| M06A-HT-049 | AC02 contradiction rule; core §18 | 4/5 | claim and known contradiction fit together | chunk/context drops known contradiction without bound warning | `tests/test_m06a_policy_context.py::test_m06a_ht_049_contradiction_truncation_fails_closed` | SQLite | claim omitted or warning-bound; no clean support | `M06A-HT-049.json` | Phase 5 |
| M06A-HT-050 | D04; core §§4,19 | 4 | projection hash matches manifest | manual projection edit | `tests/test_m06a_projections.py::test_m06a_ht_050_projection_tamper_detected` | SQLite + FS | tamper receipt and governed manifest state; no physical quarantine authority and regeneration does not ingest edit | `M06A-HT-050.json` | Phase 4 |
| M06A-HT-051 | D01/D04; core §19 | 4 | generated projection is display-only | edited projection submitted as canonical correction/assertion | `tests/test_m06a_projections.py::test_m06a_ht_051_projection_cannot_become_authority` | SQLite + service | authority operation rejects projection source | `M06A-HT-051.json` | Phase 4/5 |
| M06A-HT-052 | core §§17–19 | 4 | two Vaults rebuild separate derivatives | Vault B content injected into Vault A FTS/chunks/projections | `tests/test_m06a_search_chunks.py::test_m06a_ht_052_derived_cross_vault_contamination_detected` | two SQLite + FS | rebuild verification fails; contaminated derivative cleared | `M06A-HT-052.json` | Phase 4 + restore |

---

# 8. Assertions, Questions, Entities, Dossiers, Context, and Publication Boundary

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-053 | D05; core §16 | 5 | source-content assertion distinct from human interpretation | parser/model candidate promoted as accepted conclusion | `tests/test_m06a_assertions_entities.py::test_m06a_ht_053_candidate_cannot_self_promote` | SQLite | promotion rejected without verified human context | `M06A-HT-053.json` | Phase 5 |
| M06A-HT-054 | D09; core §16 | 5 | merge/split proposal plus evidence accepted by human | merge/split missing evidence or using system actor | `tests/test_m06a_assertions_entities.py::test_m06a_ht_054_merge_split_requires_human_and_evidence` | SQLite | acceptance absent; identities unchanged | `M06A-HT-054.json` | Phase 5 |
| M06A-HT-055 | D06; core §16 | 5 | correction creates successor lineage | prior assertion/document/entity overwritten | `tests/test_m06a_assertions_entities.py::test_m06a_ht_055_corrections_are_append_only` | SQLite | update blocked/detected; prior remains visible | `M06A-HT-055.json` | Phase 5 |
| M06A-HT-056 | D05; core §16 | 5 | editorial-use ruling is human/policy/evidence bound | `editorial_use=allowed` treated as publication approval | `tests/test_m06a_policy_context.py::test_m06a_ht_056_publication_authority_not_inferred` | central + Vault SQLite | publication gate still requires live exact central approval | `M06A-HT-056.json` | Phase 5 |
| M06A-HT-057 | core §11; F-19 | 5 | current central approval receipt verified live | fabricated/stale/wrong-account external approval ID | `tests/test_m06a_policy_context.py::test_m06a_ht_057_cross_database_reference_is_observation_only` | central + Vault SQLite | operation rejected; Vault receipt grants no authority | `M06A-HT-057.json` | Phase 5 |
| M06A-HT-058 | core §16 | 5 | audience metric remains central observation | metric value changes support status/editorial ruling | `tests/test_m06a_policy_context.py::test_m06a_ht_058_metrics_firewall` | central + Vault SQLite | no research-authority mutation | `M06A-HT-058.json` | Phase 5 |
| M06A-HT-059 | AC02 citation rule; core §16 | 5 | citation resolves to supplied version/locator | fabricated version/element/region or omitted evidence packet | `tests/test_m06a_policy_context.py::test_m06a_ht_059_citation_locator_validation` | SQLite | citation marked invalid and cannot display as verified | `M06A-HT-059.json` | Phase 5 |
| M06A-HT-060 | core §16; F-02 | 5 | question and dossier typed memberships resolve | fabricated question ID or cross-Vault dossier membership | `tests/test_m06a_assertions_entities.py::test_m06a_ht_060_questions_and_dossiers_are_typed` | SQLite | FK/scope rejection; no generic dangling member | `M06A-HT-060.json` | Phase 5 |
| M06A-HT-061 | hammer doctrine | 5 | detector returns candidate/non-detected/errored as advisory | detector error/non-detection promotes assertion or merge | `tests/test_m06a_assertions_entities.py::test_m06a_ht_061_detector_outcomes_are_non_authoritative` | SQLite | no authority change; explicit result retained | `M06A-HT-061.json` | Phase 5 |

---

# 9. Migration, Backup, Restore, and Rebuild

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-062 | core §12; F-10 | 1 | exact config/root/manifest/head/version table | mismatched config, manifest, head, extra/missing revision | `tests/test_m06a_migrations.py::test_m06a_ht_062_exact_migration_environment_enforced` | SQLite + Alembic | upgrade refused before schema change | `M06A-HT-062.json` | Phase 1 |
| M06A-HT-063 | core §12 | 1 | clean migration completes and marker clears | interruption/exception after marker creation | `tests/test_m06a_migrations.py::test_m06a_ht_063_partial_migration_remains_dirty` | SQLite + Alembic | dirty marker retained; open/mutation refused | `M06A-HT-063.json` | Phase 1 |
| M06A-HT-064 | core §12 | 1 | recovery proves matching identity/head/manifest | wrong operation, wrong Vault, missing version, leftover temp table | `tests/test_m06a_migrations.py::test_m06a_ht_064_migration_recovery_is_exact` | SQLite | marker not cleared; restore/review required | `M06A-HT-064.json` | Phase 1 |
| M06A-HT-065 | core §12; F-20 | 1/6 | non-destructive compatibility or verified restore | destructive downgrade drops governed rows | `tests/test_m06a_migrations.py::test_m06a_ht_065_destructive_downgrade_refused` | SQLite + Alembic | downgrade refused; data preserved | `M06A-HT-065.json` | each migration |
| M06A-HT-066 | D13; D027; core §20 | 2/6 | complete generation for one selected Vault SQLite file/root | referenced original omitted or another Vault included | `tests/test_m06a_backup_restore.py::test_m06a_ht_066_missing_original_fails_backup_or_restore` | Vault SQLite + FS | generation not complete or restore rejected | `M06A-HT-066.json` | Phase 2 + final |
| M06A-HT-067 | core §20 | 2/6 | manifest and files agree | manifest or artifact/package bytes modified | `tests/test_m06a_backup_restore.py::test_m06a_ht_067_manifest_and_artifact_tamper_detected` | SQLite + FS | verification/restore rejected | `M06A-HT-067.json` | Phase 2 + final |
| M06A-HT-068 | D13; core §20 | 2/6 | generation restored to matching disposable identity | Vault A generation restored as Vault B | `tests/test_m06a_backup_restore.py::test_m06a_ht_068_wrong_account_restore_rejected` | SQLite + FS | restore rejected before registry/use | `M06A-HT-068.json` | Phase 2 + final |
| M06A-HT-069 | core §20 | 2/6 | empty disposable restore target | nonempty target, dirty marker, or existing DB | `tests/test_m06a_backup_restore.py::test_m06a_ht_069_dirty_restore_target_rejected` | FS + SQLite | no overwrite/mix; target unchanged | `M06A-HT-069.json` | Phase 2 + final |
| M06A-HT-070 | core §20; F-13 | 2/6 | complete marker plus final audit receipt reconciles | crash before COMPLETE or after COMPLETE before DB receipt | `tests/test_m06a_backup_restore.py::test_m06a_ht_070_partial_backup_reconciliation` | SQLite + FS | incomplete not restorable; completed FS state reconciled by correlation | `M06A-HT-070.json` | Phase 2 + final |
| M06A-HT-071 | core §20 | 4/6 | derived rows cleared and rebuilt | FTS/chunk/projection rebuild errors or leaves stale rows | `tests/test_m06a_backup_restore.py::test_m06a_ht_071_failed_rebuild_blocks_restore` | SQLite FTS5 + FS | restore remains disposable/blocked | `M06A-HT-071.json` | Phase 4 + final |
| M06A-HT-072 | D13; core §20 | 6 | restored derivatives contain only selected Vault | repaired manifest hides cross-account derivative contamination | `tests/test_m06a_backup_restore.py::test_m06a_ht_072_cross_account_derivatives_detected` | two SQLite + FS | contamination detected after deterministic rebuild | `M06A-HT-072.json` | final |
| M06A-HT-073 | core §20; F-13 | 2/6 | full SQLite snapshot classified honestly | plan/verifier assumes derived tables absent | `tests/test_m06a_backup_restore.py::test_m06a_ht_073_derived_snapshot_rows_are_non_authoritative` | SQLite | restore clears/invalidates derived rows before use | `M06A-HT-073.json` | Phase 2 + final |
| M06A-HT-074 | core §§13,20 | 6 | backup/restore preserves valid audit and operation receipts | audit chain or correlation receipt altered | `tests/test_m06a_backup_restore.py::test_m06a_ht_074_recovery_audit_tamper_fails` | SQLite + FS | restore rejected | `M06A-HT-074.json` | final |

---

# 10. Observability, Secrets, and Packaged Parity

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-075 | AC02 observability; core §§1,14 | all | redacted Vault-scoped error/log/temp record | another Vault's locator, filename, text, or actor leaked | `tests/test_m06a_routing.py::test_m06a_ht_075_logs_cache_temp_are_vault_scoped` | FS + service | test fails; leaked output never accepted as closure evidence | `M06A-HT-075.json` | every phase + final |
| M06A-HT-076 | project hard boundary | all | synthetic non-secret fixtures | token/password/private key/API key in docs/log/manifest/evidence | `tests/test_m06a_packaging.py::test_m06a_ht_076_secret_leakage_detected` | repo + FS | validation/closure fails; secret not committed/preserved | `M06A-HT-076.json` | every phase + final |
| M06A-HT-077 | core §§14,22 | 6 | source and packaged desktop expose same Vault health/routes | sidecar missing migration/parser/FTS resource or selects arbitrary path | `tests/test_m06a_packaging.py::test_m06a_ht_077_packaged_desktop_parity` | packaged sidecar + Tauri | build/smoke proof fails; package not releasable | `M06A-HT-077.json` | Phase 6 |

---

# 11. Hammer Runner and Evidence Integrity

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-078 | F-18; runner contract | 6 | all required invariants execute | required invariant set empty/filtered to zero | `tests/test_m06a_hammer_runner.py::test_m06a_ht_078_zero_execution_fails` | runner | nonzero exit; evidence says zero-execution defect | `M06A-HT-078.json` | final |
| M06A-HT-079 | F-18 | 6 | every matrix ID has exact test mapping | required row has empty/missing test tuple | `tests/test_m06a_hammer_runner.py::test_m06a_ht_079_missing_test_mapping_fails` | runner | nonzero exit before claiming execution | `M06A-HT-079.json` | final |
| M06A-HT-080 | F-18 | 6 | runner IDs equal matrix IDs exactly | extra runner ID, missing matrix ID, duplicate ID | `tests/test_m06a_hammer_runner.py::test_m06a_ht_080_matrix_runner_divergence_fails` | runner | nonzero exit and divergence list | `M06A-HT-080.json` | final |
| M06A-HT-081 | evidence rule | 6 | each invariant produces expected evidence file | evidence file absent, malformed, duplicated, or fabricated | `tests/test_m06a_hammer_runner.py::test_m06a_ht_081_missing_or_fabricated_evidence_fails` | runner + FS | nonzero exit; closure blocked | `M06A-HT-081.json` | final |
| M06A-HT-082 | evidence rule | 6 | evidence commit SHA equals tested HEAD | evidence copied from another commit | `tests/test_m06a_hammer_runner.py::test_m06a_ht_082_wrong_commit_evidence_fails` | runner + Git | nonzero exit; mismatch preserved | `M06A-HT-082.json` | final |
| M06A-HT-083 | hammer doctrine | 6 | detector returns explicit result | detector raises exception | `tests/test_m06a_hammer_runner.py::test_m06a_ht_083_detector_error_fails` | runner | invariant fails; no clean/non-detected substitution | `M06A-HT-083.json` | final |
| M06A-HT-084 | hammer doctrine | 6 | known negative produces explicit non-detection evidence | detector does not run but result marked non-detected | `tests/test_m06a_hammer_runner.py::test_m06a_ht_084_non_detection_must_be_executed` | runner | fabricated non-detection rejected | `M06A-HT-084.json` | final |
| M06A-HT-085 | evidence rule | 6 | all required tests pass or have accepted disposition | pytest skip/xfailed without owner-approved disposition | `tests/test_m06a_hammer_runner.py::test_m06a_ht_085_unapproved_skip_fails` | runner | nonzero exit; skipped node listed | `M06A-HT-085.json` | final |
| M06A-HT-086 | evidence rule | 6 | matrix/runner/fixture manifests hashed | matrix or evidence output edited after run | `tests/test_m06a_hammer_runner.py::test_m06a_ht_086_modified_evidence_detected` | runner + FS | verification fails; no closure | `M06A-HT-086.json` | final |
| M06A-HT-087 | test collection rule | 6 | every mapped node is collected | typo/nonexistent node yields empty collection | `tests/test_m06a_hammer_runner.py::test_m06a_ht_087_empty_pytest_collection_fails` | pytest runner | invariant fails even if subprocess exit handling is wrong | `M06A-HT-087.json` | final |

---

# 12. Resolved Owner-Decision Invariants

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-088 | D027 OD-1; core §3 | 1 | The Discrepancy Desk Vault bound centrally to synthetic X and Truth Social owned accounts | service creates a second physical Vault solely because the second platform account exists | `tests/test_m06a_routing.py::test_m06a_ht_088_brand_vault_binds_multiple_platform_accounts` | central + Vault SQLite + FS | both accounts resolve to one brand Vault; no second root/database created | `M06A-HT-088.json` | Phase 1 and final |
| M06A-HT-089 | D027 OD-1; core §§3,14 | 1 | opaque Vault ID selected independently from platform account | platform account ID or handle used to create/select a Vault implicitly | `tests/test_m06a_routing.py::test_m06a_ht_089_platform_account_cannot_select_or_create_vault` | central SQLite + service + FS | request rejected; registry and filesystem unchanged | `M06A-HT-089.json` | Phase 1 |
| M06A-HT-090 | D027 OD-1; core §§3,5,14 | 1 | owned account bound to the selected brand Vault | account binding belongs to a different editorial/public brand Vault | `tests/test_m06a_routing.py::test_m06a_ht_090_wrong_brand_binding_fails` | central + two Vault SQLite DBs | routing and combined workflow rejected before Vault mutation | `M06A-HT-090.json` | Phase 1 and final |
| M06A-HT-091 | D027 OD-1; core §§3,11,16 | 5 | active Vault/account binding plus exact live central approval | binding and editorial-use ruling exist but exact approval is absent/stale | `tests/test_m06a_policy_context.py::test_m06a_ht_091_vault_binding_is_not_publication_authority` | central + Vault SQLite | publication remains blocked; binding grants no authority | `M06A-HT-091.json` | Phase 5 and final |
| M06A-HT-092 | D027 OD-1; core §§3,6,14 | 2/5 | X and Truth Social source observations stored under one selected brand Vault ID | platform label silently redirects content to another Vault or hidden platform partition | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_092_cross_platform_research_stays_in_brand_vault` | central + Vault SQLite | all canonical research remains in selected brand Vault; unauthorized redirect rejected | `M06A-HT-092.json` | Phase 2/5 |
| M06A-HT-093 | D027 OD-2; core §§3–5,12 | 1 | two physical Vaults each have distinct `database/vault.sqlite3` and singleton identity | registry maps two Vaults to one SQLite file | `tests/test_m06a_vault_identity.py::test_m06a_ht_093_each_physical_vault_has_own_sqlite_file` | central + two Vault SQLite DBs + FS | registration/open rejected; no shared-file authority | `M06A-HT-093.json` | Phase 1 and final |
| M06A-HT-094 | D027 OD-2; core §§5,14 | 1 | registry root, marker, and database identity agree | registry mapping is changed to another Vault's root/database or request attempts another DB | `tests/test_m06a_routing.py::test_m06a_ht_094_wrong_registry_database_mapping_fails` | central + two Vault SQLite DBs + FS | open rejected before migration/write; incident receipt remains central | `M06A-HT-094.json` | Phase 1 and final |
| M06A-HT-095 | D027 OD-2; core §12 | 1 | Vault A is dirty/outdated while Vault B is clean/current | dirty marker or Alembic head from A is read as B's state | `tests/test_m06a_migrations.py::test_m06a_ht_095_migration_state_is_per_vault` | two Vault SQLite DBs + Alembic + FS | A blocked; B remains independently usable; no marker/head bleed | `M06A-HT-095.json` | Phase 1 and each migration |
| M06A-HT-096 | D027 OD-2; core §20 | 2/6 | backup and disposable restore contain exactly one selected Vault DB/root | generation includes another Vault DB, objects, packages, or backup history | `tests/test_m06a_backup_restore.py::test_m06a_ht_096_backup_restore_is_per_vault` | two Vault SQLite DBs + FS | generation/restore rejected; no cross-Vault content accepted | `M06A-HT-096.json` | Phase 2 and final |
| M06A-HT-097 | D027 OD-2; core §13 | 1/6 | separate central and Vault commits reconcile through correlation receipts | service reports atomic success after one database commits and the other fails | `tests/test_m06a_routing.py::test_m06a_ht_097_cross_database_atomicity_is_never_claimed` | central + Vault SQLite | operation reports reconciliation required, never atomic success | `M06A-HT-097.json` | Phase 1 and final |
| M06A-HT-098 | D027 OD-3; core §§3,7,15 | 2 | preservation-compatible declaration passes to acquisition | timed-deletion declaration supplied with readable local file | `tests/test_m06a_policy_context.py::test_m06a_ht_098_timed_deletion_rejected_before_byte_admission` | Vault SQLite + instrumented FS | file is not copied/opened for canonical intake; content-free rejection receipt only | `M06A-HT-098.json` | Phase 2 and final |
| M06A-HT-099 | D027 OD-3; core §15; parser §§1–2 | 2/3A | complete safe rejection metadata | unknown/missing retention plus filename, pasted text, or diagnostic content in rejection receipt | `tests/test_m06a_policy_context.py::test_m06a_ht_099_unknown_retention_and_rejection_receipt_fail_closed` | Vault SQLite + FS | admission and parser launch rejected; receipt contains no prohibited content | `M06A-HT-099.json` | Phase 2/3A and final |
| M06A-HT-100 | D027 OD-3; core §§7,15,17–20; parser §1 | 2–6 | rejected operation ID has no downstream records/files | rejected content appears in artifact object, parser execution, package, search, chunk, projection, dossier, context, or backup | `tests/test_m06a_policy_context.py::test_m06a_ht_100_rejected_material_has_no_downstream_presence` | Vault SQLite + FTS5 + FS | detector fails closure and marks the synthetic workspace blocked/noncanonical; no clean result | `M06A-HT-100.json` | every affected phase and final |
| M06A-HT-101 | D027 OD-3; core §§15,23 | 2/6 | no purge operation exists in service/API/runner contracts | future-delete flag, deadline scheduler, tombstone purge, or delete-later promise bypasses admission | `tests/test_m06a_policy_context.py::test_m06a_ht_101_no_hidden_purge_or_delete_later_bypass` | repo + service + Vault SQLite | operation/closure rejected; new owner-authorized package required | `M06A-HT-101.json` | Phase 2 and final |

---

# 13. Claude Resolved-Review Correction Invariants

| ID | Requirement source | Phase | Valid fixture | Deliberate violation fixture | Planned pytest node | Real engine | Expected fail-closed result | Evidence output | Closure |
|---|---|---:|---|---|---|---|---|---|---|
| M06A-HT-102 | R-01; core §12.2 and §20 | 2 | Vault upgraded through `V0002_ingestion_artifacts_policy_and_backup.py` exposes backup-generation tables and receipt structures | Phase 2 backup starts while schema is at `V0001`, or tables first appear only in `V0006` | `tests/test_m06a_migrations.py::test_m06a_ht_102_foundational_backup_schema_exists_by_phase_2` | Vault SQLite + Alembic | backup operation refused before any generation directory/receipt; phase mapping defect fails closure | `M06A-HT-102.json` | Phase 2 and final |
| M06A-HT-103 | R-03; core §§3,13,15; parser §§1–2 | 2/3A | rejection receipt and operation key use only content-free fields and client nonce | rejected bytes/text hash appears in receipt, audit, operation key, log, diagnostic, or retained metadata | `tests/test_m06a_policy_context.py::test_m06a_ht_103_rejected_content_hashes_never_persist` | Vault SQLite + instrumented service/FS | rejection fails validation; prohibited digest absent from all durable outputs and evidence | `M06A-HT-103.json` | Phase 2/3A and final |
| M06A-HT-104 | R-05; core §7.2 | 2 | locator-only governed operation finalizes `no_artifact` without expected bytes | failed acquisition, missing expected content, parser failure, retention rejection, interruption, or silent loss uses `no_artifact` | `tests/test_m06a_ingestion_artifacts.py::test_m06a_ht_104_no_artifact_is_locator_only_and_cannot_mask_failure` | Vault SQLite + service | illegal outcome rejected; truthful failure/interruption/rejection state remains | `M06A-HT-104.json` | Phase 2 and final |
| M06A-HT-105 | R-02; core §4.1; parser §§2.6,4 | 2/3A | unadmitted worker/intake bytes remain operation-scoped under temporary quarantine and are admitted or destroyed before closure | temporary bytes are searchable, backed up, linked as canonical, survive clean closure, or escape the operation root | `tests/test_m06a_parser_framework.py::test_m06a_ht_105_temporary_quarantine_is_noncanonical_and_reconciled` | Vault SQLite + FS + worker | operation remains blocked; no canonical link/backup/search result; unsafe path rejected | `M06A-HT-105.json` | Phase 2/3A and final |
| M06A-HT-106 | R-02; core §§4.1,8,19 | 3A/4 | governed `quarantined` state references ordinary immutable object/package/projection paths | service creates or trusts a second canonical `quarantine/` store or moves bytes there as authority | `tests/test_m06a_projections.py::test_m06a_ht_106_database_quarantine_creates_no_second_truth_store` | Vault SQLite + FS | second-store operation rejected; ordinary path plus database state remains sole contract | `M06A-HT-106.json` | Phase 3A/4 and final |
| M06A-HT-107 | R-04; core §§22–23 | 6 | runner invariant, pytest node, `tests/conftest.py` evidence destination, result ID, counts, and commit SHA agree | plugin writes wrong path/ID/SHA or runner summary diverges from plugin evidence | `tests/test_m06a_hammer_runner.py::test_m06a_ht_107_pytest_plugin_and_runner_evidence_agree` | pytest runner + Git + FS | nonzero exit; mismatched evidence cannot close invariant | `M06A-HT-107.json` | Phase 6 |
| M06A-HT-108 | R-04; D031; core §§14.7,22.5 | 1–6 | Tauri client invokes the token-gated desktop API, which delegates to the governed Vault service with server-resolved actor/Vault authority | Tauri bypasses the API, frontend reaches persistence directly, desktop API trusts request-supplied human identity, or a new browser mutation route duplicates authority | `tests/test_m06a_desktop_workflow.py::test_m06a_ht_108_tauri_api_uses_governed_service` | Tauri client source + FastAPI desktop API + governed service + Vault SQLite | bypassing/duplicated authority fails; no mutation outside the governed service path | `M06A-HT-108.json` | each desktop UI phase and final |

---

# 14. Required Runner Changes

The current `scripts/run_ht_evidence.py` must be strengthened before M06-A closure. The M06-A implementation plan must:

1. load a versioned machine-readable M06-A invariant registry corresponding exactly to this matrix;
2. reject duplicate IDs;
3. reject any required invariant with no tests;
4. compare matrix ID/hash, runner ID/hash, and fixture manifest hash;
5. prove each mapped pytest node exists and is collected;
6. fail when zero required invariants execute;
7. fail on any pytest failure, error, unexpected skip, unexpected xfail/xpass, empty collection, timeout, or detector exception;
8. require per-invariant evidence to exist and match the tested commit SHA;
9. distinguish owner-approved deferral from ordinary skip and require an exact decision/disposition reference;
10. reject extra/missing result IDs, malformed results, or fabricated summaries;
11. include exact commands, Python/SQLite/platform versions, migration heads, packaged/source mode, and matrix hash;
12. return nonzero when any closure requirement is unmet.

A summary with `failed == 0` is insufficient if required work did not execute.

---

# 15. Closure Accounting

The owner-accepted planning matrix contains:

```text
108 M06-A invariants
108 required exact test mappings
0 admitted deferrals
0 executed tests
```

After owner acceptance, the matrix may be strengthened only through an explicit reviewed amendment. It may not be silently weakened. Any removed, remapped, or deferred invariant requires an exact disposition and renewed owner acceptance before the affected phase can proceed.

Before Phase 6 closure:

- every invariant must map to an existing collected node;
- every required invariant must execute;
- every required invariant must pass;
- every evidence file must match the tested commit and matrix;
- no unapproved skip/deferral may remain;
- the aggregate evidence must reconcile exactly with per-invariant evidence.

---

# 16. Explicit Authority Block

```text
Status: owner-accepted planning baseline
Phase 1 rows: implemented and clean-evidence-bound through 8fe3be4 and 5f0f9ae
Independent implementation audit: deferred under D032; not claimed complete
Later-phase implementation authority: none

The 31 required Phase 1 invariants executed and passed against the clean
implementation commit with no Phase 1 deferral. D032 preserves the later
independent-audit requirement and blocks Phase 2 until findings, if any,
are corrected and evidence-bound.

This matrix does not authorize Phase 2 through 6 application work, parser
implementation or admission, new dependencies, M06-B work, or any other
deferred capability. Those actions require their separate governing gates.
```
