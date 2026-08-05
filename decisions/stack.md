# Stack

D16. Decided after the first specification was published, because ticket 01 cannot
be built without it. Not fog that was deliberately parked — it simply had not come
up.

The binding form of the transport decision is `docs/adr/0010-two-transports-one-service-layer.md`
in the code repository. The language and database choices are recorded here rather
than as ADRs: both follow closely enough from the constraints already fixed that
neither is surprising, which is the bar an ADR has to clear.

---

## D16 — Python, SQLite, FastAPI, TypeScript client

### Database: SQLite

Close to forced by the local-first, single-operator, no-server-administration shape
of the product. The previous build used it with STRICT tables and Alembic
migrations, and nothing has changed that would make Postgres worth its operational
cost here.

### Backend language: Python

**The parsing pipeline is the hard part of this system and it is Python's home
ground.** Capture-then-cite means every HTML page, PDF, and eventually every
transcript must become a stable element structure with addressable locators. That
is `trafilatura`, `selectolax`, `pypdfium2`, `pdfplumber`, later `faster-whisper`.
Node equivalents exist but are thinner, and PDF text extraction with position data
in particular is materially worse. R-M06-04 (document normalization and parser
provenance, 1,297 lines) is written against this ecosystem.

**Working Python MCP code already exists** — the workbench server built for this
project uses the same shape `propose_claim` needs: `MCPServer`, Pydantic input
models, fail-closed refusals returning structured JSON.

**Pydantic fits the evidence model.** Six dimensions with enum constraints,
validated at the boundary, refusing invalid combinations, in one place rather than
scattered through handlers.

**The case for TypeScript, stated fairly:** Pocock's skills lean TypeScript and
their examples would read more naturally; one language across backend and client;
the MCP TypeScript SDK is the reference implementation and tends to get features
first. None of these outweighs the parsing argument, and the client does not need
to share a language with the backend.

**Migrations:** Alembic, which handles SQLite and which the previous build used.

### Client: TypeScript in the browser

Thin by constraint — no privileged logic, calls governed operations only, renders
what comes back.

### Reversibility, stated explicitly

- **The client is the cheapest layer to replace.** What would be rewritten is
  rendering and form handling against an HTTP API that does not change. Nothing in
  the Vault, the Record, the verification chain, or the tool surface knows what is
  drawing the screens. Cost is bounded by whatever UI exists at the time.
- **SQLite to Postgres would be a migration**, not a rewrite — doable via Alembic,
  but a project, and every raw-SQL assumption would need revisiting.
- **Python is effectively permanent**, because the parsing pipeline is the deep
  investment and it is the thing that would have to be rebuilt.

Uncertainty is therefore correctly parked on the client and should not be resolved
by picking a UI framework before the interaction model is known. The web UX shape
remains fog (see `architecture-decisions.md`).
