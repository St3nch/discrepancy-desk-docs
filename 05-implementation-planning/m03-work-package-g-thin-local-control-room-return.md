# M03 Work Package G — Thin Local Control Room Return

## Status

Implemented, validated, committed, and pushed. M03 remains active.

## Application Commit

```text
be5b42c985c72b8e1ae85a2a8a656f8be654f5f6
be5b42c — Add thin local control room interface
```

Repository:

```text
https://github.com/St3nch/discrepancy-desk
```

## Implemented Interface Boundary

- FastAPI application factory and local startup entry point.
- Guarded Alembic upgrade during application lifespan startup.
- Startup refusal when a durable migration dirty marker exists.
- Jinja server-rendered control-room pages.
- Health page showing SQLite integrity and Alembic version.
- Owned-account registration form.
- Work-item capture form and work-item list.
- Bounded source URL/note form.
- Existing governed evidence-file registration form.
- Draft revision form bound to an owned account.
- Explicit human approve and reject controls.
- Manual-ready control bound to an accepted approval.
- Matched or mismatched manual-publication recording form.
- Metric-observation form.
- Work-item readback containing sources, evidence, revisions, approvals, publication, and metrics.
- Local startup command bound to `127.0.0.1`.

## Route Authority Boundary

Mutating routes do not issue direct lifecycle-update SQL. They call:

- governed operator-service operations;
- dedicated exact-revision approval;
- dedicated manual-ready operation;
- dedicated matched/mismatched publication operation;
- dedicated metric-snapshot operation;
- governed evidence registration;
- governed lifecycle transitions only where generic transitions remain authorized.

The interface therefore does not introduce an alternate persistence or approval path.

## Validation

Commands:

```text
uv sync --extra dev
uv run ruff check .
uv run pytest
```

Results:

```text
ruff: All checks passed
pytest: 47 passed
warnings: 0
```

Post-commit test evidence:

```text
commit_sha: be5b42c985c72b8e1ae85a2a8a656f8be654f5f6
passed: 47
failed: 0
errors: 0
skipped: 0
python: 3.11.15
sqlite: 3.50.4
```

## HTTP Hammer Coverage

The route suite proves:

- startup creates or upgrades through the guarded migration runner;
- health reports SQLite integrity and migration version `0002`;
- empty control-room rendering works;
- complete HTTP capture-to-publication-to-metrics execution works;
- source and evidence registration survive readback;
- exact approval and manual-ready state gates remain enforced;
- matched publication reaches `published`;
- mismatch publication reaches `publication_mismatch` and preserves `verified_mismatch`;
- fabricated revision approval is rejected with no state mutation;
- malformed metric JSON is rejected with no snapshot insertion;
- dirty migration state prevents application startup.

## Dependencies

Added runtime dependencies:

- FastAPI
- Jinja2
- python-multipart
- Uvicorn

Added development dependency:

- `httpx2`, required by the installed FastAPI/Starlette test client.

The initial `httpx` test dependency emitted a framework deprecation warning and was replaced rather than suppressed. The final suite runs without warnings.

## Deliberate Interface Decision

This first interface uses ordinary server-rendered POST/redirect forms.

HTMX progressive enhancement is deferred. No CDN dependency, vendored JavaScript, or untested partial-render path was added merely to claim HTMX usage. A later bounded interface-refinement package may add local HTMX behavior only after preserving the same governed route and service contracts.

## Local Startup

```powershell
uv run uvicorn discrepancy_desk.web:app --host 127.0.0.1 --port 8000
```

The application is local-only under the current boundary. No external deployment, authentication system, or public listener is admitted.

## Remaining M03 Work

- successor-revision approval invalidation;
- corrected/replacement publication workflow;
- more complete named HT fixture evidence;
- HT-01 through HT-20 closure review;
- thin-interface manual walkthrough on the actual local runtime database;
- optional local HTMX progressive enhancement after route-contract review;
- M03 exit-gate review.

## Next Gate

The next bounded package should strengthen successor revision and corrected-publication semantics, then perform a real local operator walkthrough before M03 closure review.
