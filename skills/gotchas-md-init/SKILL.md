---
name: gotchas-md-init
description: >
  Instructs the agent to create or update a GOTCHAS.md file for the current project.
  This file is a chronological log of pitfalls encountered by coding agents,
  enabling future agents to avoid repeating the same mistakes.
---

# write-gotchas-md

## What this skill does

Creates (or verifies) a GOTCHAS.md file — a chronological, append-only log of non-obvious pitfalls discovered during development. Each entry is short and information-dense: just enough context for the next agent to avoid the same trap.

## Steps

1. **Check if GOTCHAS.md already exists** at the project root.
   - If it exists and has correct structure, leave it as-is.
   - If it exists but is malformed, fix the structure while preserving existing entries.
   - If it does not exist, create it with the template below.

2. **Scan for common gotcha sources** (optional, if the project has history):
   - Git log for reverts or fix commits that hint at past pitfalls
   - CI configs for non-obvious environment requirements
   - README or docs mentioning known issues
   - If you find clear gotchas, seed 1-3 entries. Otherwise leave the log empty.

## Output structure

```markdown
# GOTCHAS.md

Chronological log of non-obvious pitfalls. Read when stuck. Append before ending your session.

## Format

Each entry: `- YYYY-MM-DD: <one-line gotcha>. Fix: <one-line fix or workaround>`

## Log

- (entries here)
```

## Rules

- Entries must be **one line each**: what went wrong + how to fix it.
- No categories, no headers per entry, no multi-line explanations.
- Newest entries go at the bottom (append-only).
- Maximum entry length: 150 characters. If you can't say it in 150 chars, you're saying too much.
- Do NOT remove or edit existing entries unless they are factually wrong.
- The file must be scannable in under 10 seconds.
- Hard limit: no max lines (this file grows over time), but each entry stays minimal.
