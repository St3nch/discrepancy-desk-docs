# M03 Work Package A — Repository Governance and Bootstrap

## Status

Prepared for owner decision. Physical execution blocked until the governance choices below are approved.

## Purpose

Satisfy the M03 repository-initialization entry gate without exposing credentials, publishing third-party evidence, or mixing immutable raw evidence into ordinary source-control history.

## Live Boundary

`C:\dev\x\discrepancy-desk` currently contains approved brand assets and M01 evidence and is intentionally not a Git repository. D017 prohibits casual initialization.

## Recommended Governance Decisions

### RG-01 — Third-party data and PII

- Raw API evidence and third-party content remain outside Git.
- Operational code and synthetic test fixtures may enter Git.
- Fixtures must use fictional, synthetic, owned, or public-domain material.
- No credentials, tokens, private messages, private account data, or unnecessary personal data may enter Git.

### RG-02 — Raw-evidence publication

- Raw evidence is never committed to the public repository.
- Evidence remains under the governed local evidence root and is referenced by relative locator and SHA-256.
- Publication of any raw evidence requires a later explicit owner decision and review.

### RG-03 — Repository layout

Initialize Git at `C:\dev\x\discrepancy-desk` only after approval. Track application code, migrations, tests, synthetic fixtures, non-secret configuration templates, documentation, and approved production brand assets. Keep live evidence and local runtime state untracked.

### RG-04 — `.gitignore` minimum

Ignore at least:

```text
.env
.env.*
!.env.example
local-secrets/
secrets/
*.key
*.pem
*.p12
*.sqlite
*.sqlite3
*.db
*.db-wal
*.db-shm
*.db-journal
runtime/
backups/
restore/
evidence/
__pycache__/
.pytest_cache/
.ruff_cache/
.mypy_cache/
.venv/
dist/
build/
*.egg-info/
.DS_Store
Thumbs.db
```

The existing `assets/brand/` tree remains tracked only for owner-approved production masters and exports.

### RG-05 — Binary and size policy

- Approved brand images may be committed.
- Raw evidence, databases, backup archives, generated test databases, and runtime logs may not be committed.
- Default maximum ordinary tracked file size: 10 MiB. Larger files require explicit owner approval and a documented storage choice.
- Git LFS is not admitted yet.

### RG-06 — Secret controls

Before every commit:

- inspect staged paths;
- run secret-pattern review;
- reject `.env`, credential, token, key, cookie, authorization-header, and secret-bearing log files;
- keep Bitwarden as credential authority;
- keep the live X credential file outside `C:\dev` and outside all MCP-readable roots.

### RG-07 — Remote policy

Use a GitHub repository only for source, tests, planning-linked implementation records, and approved brand assets. No raw evidence or runtime database is pushed. Repository visibility must be explicitly confirmed by the owner before the first remote push.

## Bounded Bootstrap After Approval

1. create the approved `.gitignore`;
2. create the application skeleton and non-secret configuration template;
3. verify ignored evidence/runtime paths;
4. initialize Git;
5. inspect staged scope and secret review;
6. make the initial application-baseline commit;
7. add/configure remote only after owner confirms repository URL and visibility;
8. proceed into the approved physical-design and persistence package.

## Non-Authorization

This document does not itself approve the governance choices or authorize Git initialization. Owner approval is required because D017 reserved these decisions.
