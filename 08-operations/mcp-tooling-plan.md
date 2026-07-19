# MCP Tooling Plan

## Purpose

Create a safe local MCP server so ChatGPT can work directly with local project repos.

## Canonical MCP Repo Path

```text
C:\dev\gpt-mcp
```

## Target Client

ChatGPT Developer Mode / custom MCP connector.

## Transport

FastMCP v3+ HTTP / Streamable HTTP.

Local endpoint:

```text
http://127.0.0.1:8787/mcp
```

Ngrok public endpoint:

```text
https://chatgpt-mcp.ngrok.dev/mcp
```

Tunnel command:

```powershell
ngrok http --domain=chatgpt-mcp.ngrok.dev 8787
```

## Required Roots

```text
x_root = C:\dev\x
discrepancy_desk_docs = C:\dev\x\discrepancy-desk-docs
discrepancy_desk = C:\dev\x\discrepancy-desk
observatory = C:\dev\observatory
kaizen_docs = C:\dev\kaizen-docs
gpt_mcp = C:\dev\gpt-mcp
```

## Must-Have Tools

- server_status
- list_roots
- list_dir
- tree
- read_text
- grep_text
- write_text
- replace_text
- mkdir
- git_status
- git_diff
- git_init
- git_add
- git_commit
- run_command
- hash_file

## Safety Rules

- no unrestricted filesystem
- no unrestricted shell
- every path resolved under a configured root
- no symlink escapes
- no env dumps
- no secrets exposure
- no hidden writes

## Design Preference

Boring, safe, stable.

This MCP is the trusted hands, not the weird machine.
