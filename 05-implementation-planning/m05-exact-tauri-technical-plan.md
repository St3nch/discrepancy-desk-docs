# M05 Exact Tauri Technical Plan — Desktop Foundation and Product Parity

## Status

Owner-accepted on 2026-07-20. This exact plan and `05-implementation-planning/m05-adversarial-test-matrix.md` authorize M05 implementation only within the fixed scope and stop conditions below.

## Governing Baseline

- application repository: `C:\dev\x\discrepancy-desk`
- application baseline commit: `770a6bb2b596e8df917119af24be5538ae61e6af`
- documentation repository baseline commit: `cfc5d47`
- M04: owner-accepted and closed
- M05: implementation authorized under this accepted exact plan
- M04 service contract: `docs/m04-service-api-contract.md`
- M04 final exit-gate record: `docs/m04-exit-gate-review.md`

## Objective

Build the first product-grade Windows desktop shell around the already-proven M04 local business operations without moving authority into the frontend, duplicating persistence, exposing direct database access, or adding new platform/provider behavior.

The desktop must operate the complete M04 workflow while the existing FastAPI/Jinja web surface remains an executable contract and regression harness.

## Current Tauri Baseline

Planning is based on current Tauri 2 documentation reviewed on 2026-07-20:

- capabilities and permissions are explicit per window/webview;
- capability overlap merges permissions and therefore must be minimized;
- bundled code receives API access by default, while remote API access requires explicit configuration;
- external binaries may be bundled as sidecars and should use exact command/argument allowlists;
- Windows installers are produced as MSI or NSIS packages;
- Windows 10/11 normally provide WebView2 through the operating system;
- notifications require explicit permission handling and only work normally for installed Windows apps;
- updater support exists but distribution, signing, endpoint, and rollback policy must be separately admitted.

## Architectural Ruling

### Desktop shape

Create a new top-level application directory:

```text
desktop/
```

Use:

- Tauri 2;
- React;
- TypeScript;
- Vite;
- Rust Tauri host;
- Windows-first target using the MSVC toolchain;
- NSIS current-user installation as the first packaging proof;
- system WebView2 rather than bundling a fixed runtime;
- no Microsoft Store or auto-update release in the first M05 package.

### Backend connection model

The desktop must not open SQLite or import the Python persistence layer directly.

The accepted initial model is:

```text
Tauri shell
→ Rust-owned local backend supervisor
→ bundled Discrepancy Desk Python sidecar
→ loopback-only FastAPI service
→ existing governed Python operations
→ SQLite
```

Rules:

- bind only to `127.0.0.1`;
- choose an ephemeral or installation-scoped non-public port;
- generate an ephemeral launch token for each backend process;
- pass the token through process environment or inherited pipe, never command-line arguments or logs;
- require the token on every desktop API request;
- refuse requests from missing or incorrect tokens;
- do not enable CORS for arbitrary origins;
- do not expose the local service on LAN interfaces;
- the Rust host owns start, health check, restart, and shutdown;
- only one governed backend instance may own the active application database;
- web-harness development startup remains separate and cannot silently share the production desktop database.

### Sidecar ruling

The Python backend may be packaged as a sidecar only after a reproducible build process proves:

- exact source commit binding;
- migration manifest inclusion;
- templates and required resources included;
- no credentials embedded;
- deterministic version reporting;
- startup failure surfaces a trustworthy desktop error;
- child process terminates when the desktop exits;
- no arbitrary shell command interface is exposed.

PyInstaller is the leading packaging mechanism for the first proof, but implementation may use another bounded Python bundler if the exact change is recorded and independently validated within M05.

### Frontend authority ruling

The React frontend is presentation and operator-intent collection only.

It may:

- request account-scoped views;
- submit candidate user-entered values to governed operations;
- display returned state, errors, health, and evidence references;
- store non-authoritative UI preferences.

It may not:

- calculate or persist approval authority;
- mark content Ready-to-Post by frontend logic alone;
- open SQLite;
- read arbitrary files;
- execute arbitrary processes;
- create platform actions;
- bypass account scope;
- silently retry non-idempotent requests with a new operation key.

## Exact Application Change Surface

### New top-level desktop project

```text
desktop/package.json
desktop/package-lock.json
desktop/tsconfig.json
desktop/tsconfig.node.json
desktop/vite.config.ts
desktop/index.html
desktop/src/main.tsx
desktop/src/App.tsx
desktop/src/styles.css
desktop/src/api/client.ts
desktop/src/api/types.ts
desktop/src/state/account.ts
desktop/src/components/AppShell.tsx
desktop/src/components/CommandPalette.tsx
desktop/src/components/HealthBanner.tsx
desktop/src/components/InspectorPane.tsx
desktop/src/pages/CommandCenterPage.tsx
desktop/src/pages/CalendarPage.tsx
desktop/src/pages/WorkPage.tsx
desktop/src/pages/RecordsPage.tsx
desktop/src/pages/ReleaseWatchPage.tsx
desktop/src/pages/LibraryPage.tsx
desktop/src/pages/MetricsPage.tsx
desktop/src/pages/SystemPage.tsx
desktop/src-tauri/Cargo.toml
desktop/src-tauri/build.rs
desktop/src-tauri/tauri.conf.json
desktop/src-tauri/capabilities/main.json
desktop/src-tauri/src/lib.rs
desktop/src-tauri/src/main.rs
desktop/src-tauri/src/backend.rs
desktop/src-tauri/src/commands.rs
desktop/src-tauri/src/security.rs
desktop/src-tauri/icons/*
```

### Existing application files admitted for modification

```text
pyproject.toml
uv.lock
README.md
src/discrepancy_desk/web.py
src/discrepancy_desk/operator_service.py
src/discrepancy_desk/editorial_queries.py
scripts/build_desktop_sidecar.py
scripts/run_ht_evidence.py
docs/ht-coverage-ledger.md
docs/m05-desktop-security-contract.md
docs/m05-desktop-exit-gate-review.md
```

`scripts/build_desktop_sidecar.py`, `docs/m05-desktop-security-contract.md`, and `docs/m05-desktop-exit-gate-review.md` are new admitted files.

### Tests admitted

```text
tests/test_m05_desktop_api_contract.py
tests/test_m05_sidecar_lifecycle.py
tests/test_m05_desktop_security.py
tests/test_m05_packaging_contract.py
desktop/src/**/*.test.tsx
desktop/src-tauri/src/**/*_test.rs
```

No other path is admitted without an explicit package amendment.

## Package A — Desktop Security and Backend Boundary

Deliver together:

- loopback-only desktop API mode;
- ephemeral launch-token authentication;
- explicit desktop API version;
- Rust backend supervisor;
- single-instance backend ownership;
- controlled startup/health/shutdown;
- exact Tauri capabilities file;
- deny-by-default shell, filesystem, process, window, notification, and updater permissions;
- no remote webview/API origins;
- typed desktop errors;
- Python and Rust adversarial tests.

### Tauri capability minimum

The main window receives only permissions required for:

- core app/window/event operations needed by the shell;
- explicitly admitted native file selection;
- explicit notifications after permission grant;
- narrowly defined custom Rust commands for backend health and lifecycle.

Do not grant broad shell execution, unrestricted filesystem access, remote URL API access, updater permissions, global shortcuts, or arbitrary window creation in Package A.

## Package B — Product Shell and M04 Parity

Deliver together:

- B1-L7 application shell;
- left navigation and active-account selector;
- Command Center;
- Calendar;
- Work detail and inspector;
- Records placeholder backed only by admitted M04 source/evidence records;
- Release Watch placeholder explicitly marked unavailable until M09;
- Library placeholder for later M12 assets/articles/replies;
- Metrics view using current manual observations;
- System health and diagnostics;
- command palette;
- keyboard navigation;
- resizable panes;
- window-size and pane-layout restoration using non-authoritative local preferences;
- complete M04 mutation parity through governed API operations;
- offline, backend-starting, unhealthy, refused, and recovery states;
- frontend component and end-to-end tests.

Placeholders must not fabricate later-milestone capability.

## Package C — Native Integration, Packaging, and Closure

Deliver together:

- governed native file selection for evidence paths;
- drag/drop limited to explicit user action and admitted file handling;
- local notifications limited to operator reminders derived from existing local schedule data;
- NSIS current-user installer proof on Windows;
- application version and backend version compatibility check;
- clean install, launch, restart, shutdown, and uninstall proof;
- no updater activation;
- no code-signing claim unless a real signing certificate and process are admitted;
- no production release claim;
- full desktop workflow scenario;
- M05 hammer evidence and closure record.

## Credential and Secret Boundary

M05 does not require X credentials for its admitted manual workflow.

Rules:

- do not import the existing X probe environment into the desktop;
- do not store secrets in Tauri Store, browser storage, source, config, logs, screenshots, crash output, or installer resources;
- UI preferences may use a non-secret local store;
- future credentials require a separately approved Windows Credential Manager/keyring design;
- updater signing keys, code-signing certificates, and release tokens are out of scope.

## Packaging and Update Boundary

### First admitted installer

- Windows x64;
- NSIS;
- current-user install;
- system WebView2/default bootstrap behavior;
- local/manual installation only;
- unsigned development artifact clearly labeled non-production if no certificate exists.

### Explicitly blocked

- automatic update checks;
- download/install updater commands;
- downgrade support;
- Microsoft Store distribution;
- public download hosting;
- release-channel infrastructure;
- signing-key creation or storage;
- unattended production installation.

A later package may admit updater/signing only after endpoint ownership, signature verification, rollback, compromised-key response, and release approval are separately reviewed.

## Admitted Product Navigation

- Command Center — fully functional M04 parity;
- Calendar — fully functional M04 schedule/Reserve parity;
- Work — fully functional M04 work-item parity;
- Records — bounded M04 sources/evidence view only;
- Release Watch — disabled explanatory placeholder;
- Library — disabled explanatory placeholder;
- Metrics — functional M04 manual metrics view;
- System — backend health, versions, database location classification, evidence root classification, and recovery guidance.

## Validation Commands

At minimum:

```text
uv run ruff check .
uv run pytest
npm --prefix desktop test
npm --prefix desktop run build
cargo test --manifest-path desktop/src-tauri/Cargo.toml
npm --prefix desktop run tauri build -- --bundles nsis
uv run python scripts/run_ht_evidence.py
```

Installer execution tests may use a disposable local directory or VM. Do not install over an operator's active data without an explicit isolated-test plan.

## Stop Conditions

Stop and return for owner ruling if implementation discovers:

- direct SQLite access from Rust or JavaScript is required;
- broad shell/filesystem permissions are required;
- a stable network port or LAN binding is proposed;
- secrets must be placed in frontend-accessible storage;
- updater, signing, public hosting, or Store distribution is needed;
- a new business authority or lifecycle state is required;
- M04 operations cannot support parity without weakening authority;
- packaging requires destructive changes to active user data;
- the exact change surface must expand materially;
- any admitted security or parity invariant fails.

## Owner Review Gate

Owner acceptance of this document and `05-implementation-planning/m05-adversarial-test-matrix.md` authorizes Packages A through C only within this exact boundary. It does not authorize M06 or later milestones, autonomous platform behavior, updater release, code-signing claims, or public distribution.
