# Buzz as the Development Environment

Working note, 2026-08-03. Buzz is where coding agents build The Discrepancy Desk.
It is not part of the product and nothing in the product depends on it.

Findings here come from Buzz's shipped documentation and maintainer issues as of
this date. Buzz moves fast — two of the three open questions from the earlier
research pass had already changed by the time this note was written. Re-check
before relying on any of it.

---

## Two things called "an MCP server for Discrepancy Desk"

Keep these separate. They are unrelated.

**The product's tool surface** — `claim_next_run`, `capture_url`, `propose_claim`,
and the rest. Consumed by a research executor at runtime. This is what the agents
are going to **build**, starting at ticket 01.

**`buzz-dev-mcp`** — shell, `str_replace`, `todo`, with `rg` and `tree` on PATH.
This is the agents' working environment **while** they build. It ships with Buzz.
Nothing to write.

A third thing exists and is also unrelated: the `workbench` MCP server at
`~/projects/mcp/claude-linux-bs-mcp/`, which gives a chat-interface Claude scoped
filesystem access. Chat sessions have no shell; Buzz agents do. The workbench has
no role in Buzz.

---

## Runtime status

Buzz Desktop now runs a two-tier runtime catalog.

**Tier-1** — compiled in, with auto-installers, auth probes, and first-class
onboarding: Goose, Claude Code, Codex, Buzz Agent. Their IDs are reserved.

**Tier-2** — preset catalog, always present, PATH-probed for availability, not
editable or deletable: Cursor, Oh My Pi, **Grok Build**, OpenCode, Kimi Code, Amp,
Hermes Agent, OpenClaw. Shown with a logo; if not installed, a docs link appears
instead.

**Grok's status has changed.** The earlier research pass found no first-class path
and concluded Grok had to be hand-configured as a custom command. It is now Tier-2.
Grok Build speaks ACP natively over stdio (`grok agent stdio`) with no separate
adapter; install is `curl -fsSL https://x.ai/cli/install.sh | bash`, binary under
`~/.grok/bin`.

Buzz Desktop also supports registering any ACP-speaking tool as a selectable
runtime without a PR, so nothing here is a hard limit.

---

## One MCP server per agent

**Constraint:** the runtime uses a single `mcp_command` per agent record, so an
agent gets `buzz-dev-mcp` **or** a domain MCP server, never both. An operator who
attached only a domain server reported their agent being offered 61 domain tools
and zero shell or buzz tools — while the heartbeat prompt was instructing it to run
`buzz messages send --reply-to`, which it had no tool to do.

**Why this does not block us:** coding agents want the shell. With a shell they can
do everything a domain server would give them. Set `BUZZ_ACP_MCP_COMMAND=buzz-dev-mcp`
and take nothing else.

A fix making `--mcp-command` repeatable is in flight upstream (issue #2899). The
persona layer already models multiple servers; it is simply not plumbed to the
runtime.

---

## Separate Unix user per agent

Buzz supplies no host isolation. A seated agent is a Unix process holding a Nostr
key; its relay powers are gated by channel membership, its **host** powers are
exactly those of the OS user that launched it. There is no per-agent user setting,
no filesystem jail, and no config flag — and desktop-managed agents auto-approve
tool permissions, so `approvals.mode: manual` on the guest side does not help.

**But it works, because `buzz-acp` is a standalone binary configured entirely
through environment variables** (every one has a matching CLI flag), and it spawns
the agent as a subprocess. Whatever UID launches the harness is inherited by
everything downstream. Run one harness per agent, each as its own Unix user, each
with its own `BUZZ_PRIVATE_KEY`. They connect to the same relay and appear as
separate identities in the channel — which is wanted anyway.

This is Pattern A + Pattern E from the earlier isolation research, and an
independent integration writeup confirms the systemd path in practice, with two
gotchas worth knowing in advance: **systemd strips PATH** (add it to `[Service]`),
and **`EnvironmentFile=` is required** rather than a plain `.env`, which does not
load reliably.

### Minimum setup

```bash
sudo sed -i 's/^DIR_MODE=.*/DIR_MODE=0750/' /etc/adduser.conf
sudo adduser --disabled-password --gecos "" agent-codex
sudo adduser --disabled-password --gecos "" agent-claude
sudo chmod 700 /home/agent-codex /home/agent-claude
sudo chmod 700 /home/chaz
```

The last line is the one most setups skip. Stock Pop!_OS creates home at 0755,
exposing `~/.aws`, `~/.config`, browser profiles, and `~/.gitconfig` to any other
user on the box.

Prefer `LoadCredential=` over environment variables for the agent keys — systemd
reads the file as root and projects it to `$CREDENTIALS_DIRECTORY` at mode 0400,
backed by unswappable tmpfs, so the key never enters the environment and never
appears in `/proc/<pid>/environ`. Upstream issue #2883 tracks the same concern from
Buzz's side and is open.

### Repository access

Each agent needs its own **full clone** inside its own home, coordinating through
GitHub. Not a shared worktree: all worktrees share one `.git`, so a hook installed
in one agent's worktree runs on the operator's next commit in the main repo. A
worktree is not an isolation boundary.

Handing files back to the operator needs a shared group or ACLs, since agent homes
are 0700.

---

## Open

- **Whether the desktop-managed agent flow can launch a harness under a different
  Unix user**, or whether separate users requires running `buzz-acp` standalone as
  a service. Desktop stores keys in the OS keyring per desktop-user and injects
  them at spawn, which may fight this. The standalone systemd path is documented
  and known to work; the managed path is unverified.
- **Machine capacity** for running multiple harnesses plus their agent
  subprocesses. Not assessed.
- **Whether isolation is being done at all.** If every agent runs as the operator,
  "the agent focuses on the project repos" is a scoping intent, not an enforcement
  — one `cd` reaches anything the operator can reach, and not from malice: an agent
  debugging an import error greps around, one resolving a merge conflict reads
  config.

---

## Review independence

The prior project's audit value came from the implementing agent and the reviewing
agent not coordinating on what would be checked. A shared Buzz channel removes that
by default — review criteria stated in-channel are read and built toward.

**The six standing checks may be public.** Vocabulary reconciliation, fail-open
inventory, destructive-write inventory, dead-capability inventory, write-once
inventory, projection completeness. An implementing agent building against those is
a feature, not a leak.

**Diff-specific review criteria should not be written down anywhere.** Not in the
repo, and not in a file on the machine — a seated agent running as the operator can
read the operator's home. If isolation is not in place, the only reliable boundary
is that the file does not exist.
