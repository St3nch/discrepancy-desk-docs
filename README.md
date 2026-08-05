# The Discrepancy Desk — Documentation

Governing documents for The Discrepancy Desk. Implementation lives in a separate
repository; this one holds what is true about the project regardless of how it is
built.

## Start here

**`VISION.md`** — what the Desk is, why it exists, and how it works. Self-contained.
Paste it whole into a fresh model to bring it up to speed. Everything else in this
repository elaborates on it or records how it came to say what it says.

## What lives where

| Directory | Holds |
|---|---|
| `decisions/` | Locked architecture decisions, each with the alternative that was rejected and why |
| `doctrine/` | Rules the product must obey — editorial standard, evidence model, publication boundaries |
| `brand/` | Quinton Clearance: persona, voice, style, visual direction |
| `operations/` | What actually happened when the system ran. Case notes, failures, working material |
| `reference/` | Pointers outward — the v1 repository inventory, external research |

## What does not live here

**Glossary and ADRs live in the code repository.** `CONTEXT.md` and `docs/adr/` are
read by the agent working the code; separating them guarantees they go stale. The
`decisions/` directory here holds the reasoning; the ADRs there hold the binding
form.

**No planning directory.** Planning is tickets in the tracker. The previous project
put 131 files in `05-implementation-planning/` and shipped nothing.

**No audits directory.** Reviews belong in the tracker or the conversation that
produced them. When a review produces a durable rule, the rule goes in `doctrine/`
and the review is disposable. The previous project accumulated 99 audit files.

Both exclusions share one failure mode: a file per event, forever, with deletion
feeling like lost history.

## The rule this repository is built against

The previous documentation repository reached 309 files. 230 were milestone and
audit machinery with no lasting value, and the project never published a post from
the system it was documenting.

> Governance must not outrun execution.

A document earns its place here by being read. If a directory is filling up with
files nobody opens, that is the failure recurring, and the correct response is to
delete rather than to reconcile.
