# Claude MCP Research Prompt

```markdown
Research the current best way to build a ChatGPT-compatible MCP server for local Windows project work.

Context:
- Local repo path: `C:\dev\gpt-mcp`
- Target client: ChatGPT Developer Mode / custom MCP connector
- Framework: FastMCP v3+
- Transport: HTTP / Streamable HTTP, not Claude stdio-only
- Local endpoint target: `http://127.0.0.1:8787/mcp`
- Public tunnel: `https://chatgpt-mcp.ngrok.dev/mcp`
- Tunnel command target: `ngrok http --domain=chatgpt-mcp.ngrok.dev 8787`
- Needed roots:
  - `C:\dev\x`
  - `C:\dev\x\discrepancy-desk-docs`
  - `C:\dev\x\discrepancy-desk`
  - `C:\dev\observatory`
  - `C:\dev\kaizen-docs`
  - `C:\dev\gpt-mcp`

Research official/current docs for:
1. OpenAI / ChatGPT custom MCP connector requirements.
2. FastMCP v3+ HTTP / Streamable HTTP server setup.
3. Required endpoint shape, especially `/mcp`.
4. Ngrok tunnel requirements for ChatGPT MCP.
5. `MCP_ALLOWED_HOSTS` / `MCP_ALLOWED_ORIGINS` or equivalent host/origin protections.
6. Known ChatGPT MCP limitations, tool schema refresh issues, auth requirements, and troubleshooting.
7. Safe filesystem MCP design patterns for local repo work.

Do not implement yet.

Deliver:
- verified links/sources
- recommended architecture
- exact run shape
- risks/gotchas
- open questions before implementation
```
