# Audit Artifact Boundary

## Status

Owner-directed governance rule recorded on 2026-07-20.

## Rule

Live audit prompts must not be committed to the project repositories.

The repositories may retain:

- completed audit reports;
- accepted finding dispositions;
- correction plans;
- correction closure records;
- evidence bindings;
- owner decisions derived from an audit.

The repositories must not retain:

- prompts intended to instruct Claude, ChatGPT, or another reviewer during an active assignment;
- conversational sync prompts;
- temporary reviewer instructions;
- prompts that may be mistaken for project doctrine or audited source material.

## Reason

A reviewer inspecting the repository may treat a committed prompt as part of the project under review, quote it back, audit its wording, or follow it as repository authority. That contaminates the independence and clarity of the review.

## Required Workflow

```text
prepare audit prompt outside the repositories
→ provide it directly to the reviewer
→ receive the completed report
→ store the final report in the audit directory
→ record owner dispositions and corrections
```

If a prompt must be preserved temporarily, it belongs outside both governed repositories and must not be cited as project authority.
