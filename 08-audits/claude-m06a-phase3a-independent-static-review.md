# Claude M06-A Phase 3A Independent Static Review

## Review Identity

```text
Review type: independent static implementation review
Reviewed application commit: 251b3ca841af46e63485b9ab5bf292cbae55a418
Reviewed documentation commit: 85ea7a752a16b179e79df45733965eba22a641e0
Verdict: M06-A PHASE 3A REQUIRES CORRECTION
```

This record preserves the substantive independent report returned to the owner. The reviewer inspected the live repositories, governing package, implementation diff, critical authority/security files, packaged resources, and immutable evidence. The reviewer did not edit, commit, push, admit a parser, or authorize later phases.

## Verified Strengths

The review confirmed:

- both repositories matched the expected clean synchronized baselines;
- all 43 Phase 3A changed paths were inside the D036 allowlist;
- `pyproject.toml`, `uv.lock`, central migrations, prior Vault migrations, and later-phase modules were unchanged;
- V0003 same-Vault references, append-only controls, parser admission vocabulary, and destructive downgrade refusal were generally sound;
- no production `owner_admitted` parser, canonical execution path, parser mutation endpoint, or parser mutation UI existed;
- deterministic package construction, content-addressed package storage, package-aware backup/restore, and Vault isolation were materially implemented;
- the Tauri/API parser surface was read-only;
- all asserted evidence and resource hashes recomputed correctly against the reviewed tree;
- no Phase 3B, provider, network, live LLM, Qdrant, graph, purge, or publication authority leaked into Phase 3A.

## Findings Returned

### F-01 — High — Current-head inherited regression proof gap

The shared M06-A test fixture was repinned to V0002, the generic backup helper hard-coded V0002, and several inherited Phase 3A evidence nodes therefore ran against the prior schema instead of the shipping V0003 schema. The reviewer specifically identified the backup/recovery invariants HT-066 through HT-070 as lacking current-head proof.

**Review disposition:** blocks Phase 3A closure.

### F-02 — Medium — Worker filesystem and process denial coverage incomplete

The audit hook did not correctly classify low-level `os.open` write flags and did not cover several filesystem mutation and process-launch events, including `os.exec*` and Windows `os.startfile` paths.

**Review disposition:** correction required before parser admission; the Project Steward later elevated it to a Phase 3A closure blocker after executable reproduction.

### F-03 — Medium — Coverage completeness self-asserted

The candidate declared complete byte coverage without independently reconciling emitted element/region locators. BOM bytes were included in the declared consumed range but not represented by an emitted locator.

**Review disposition:** correction required before parser admission; the Project Steward later elevated it to a Phase 3A closure blocker after executable reproduction.

### F-04 — Low — Reparse/package reuse and document-version lineage inconsistent

The deterministic package storage contract allowed byte reuse, but the database uniqueness model did not provide a coherent second-execution linkage. `version_ordinal` was scoped to parser execution rather than acquisition-artifact lineage.

**Review disposition:** resolve before Phase 3B; the Project Steward later identified additional immutable tuple-ID and exact package/execution/document-chain defects and treated the group as a correction-package blocker.

### F-05 — Low — Packaged implementation and dependency verification conditional

Implementation-source and dependency-lock checks were skipped if the expected files were absent, rather than failing closed.

### F-06 — Low — Worker read-boundary wording overstated implementation

The accepted package said reads were limited to exact input and packaged parser resources, while the actual importer necessarily allowed interpreter, standard-library, package, frozen-bundle, resource, and operation roots. Timezone and hash-seed settings were also assigned after interpreter startup.

## Closure Recommendation Returned

The reviewer recommended that Phase 3A not close on application commit `251b3ca`. The report stated that the authority model and much of the migration/package design were sound, but current-head regression proof required correction and clean evidence rebinding.

The verdict did not admit `m06a.text.v1`, authorize canonical parsing, authorize Phase 3B or later M06-A phases, or authorize M06-B.
