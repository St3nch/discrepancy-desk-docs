# Claude MCP Build Prompt

Use after research is complete and accepted.

```markdown
Build or repair my local ChatGPT MCP server in:

```text
C:\dev\gpt-mcp
```

Use:

- FastMCP v3+
- Python
- HTTP / Streamable HTTP transport
- local endpoint `http://127.0.0.1:8787/mcp`
- public ngrok endpoint `https://chatgpt-mcp.ngrok.dev/mcp`

Do not build a Claude Desktop stdio-only MCP server.

Initial roots:

```text
x_root = C:\dev\x
discrepancy_desk_docs = C:\dev\x\discrepancy-desk-docs
discrepancy_desk = C:\dev\x\discrepancy-desk
observatory = C:\dev\observatory
kaizen_docs = C:\dev\kaizen-docs
gpt_mcp = C:\dev\gpt-mcp
```

Required tools:

- server_status
- list_roots
- list_dir
- tree
- stat_path
- read_text
- grep_text
- search_files
- mkdir
- write_text
- append_text
- replace_text
- apply_unified_patch
- git_status
- git_diff
- git_log
- git_branch
- git_init
- git_add
- git_commit
- git_remote_list
- git_remote_add
- git_push
- run_command with allowlist only
- hash_file
- hash_text

Safety:

- strict root confinement
- reject `..` traversal
- reject symlink escapes
- reject absolute paths outside roots
- no unrestricted shell
- no env dumps
- no destructive recursive delete tool
- no hidden writes

Deliver:

- files changed
- start command
- ngrok command
- connector endpoint
- validation results
- known limitations
```
