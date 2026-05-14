---
name: gotchas-skill-init
description: >
  Generates a discoverable gotchas skill for the target project.
  The skill provides a chronological log of pitfalls with agent guidance
  on when to read and when to append entries.
---

# generate-gotchas-skill

## What this skill does

Produces a gotchas skill file for the target project. Unlike the plain GOTCHAS.md, this skill is discoverable by the agent platform and includes explicit guidance on when to read it, how to append entries, and verification of correct usage.

## Inputs

This skill receives a **platform** from the orchestrator:
- `opencode` → output to `.opencode/skills/<project>-gotchas/SKILL.md`
- `claude` → output to `.claude/skills/<project>-gotchas/SKILL.md`
- `copilot` → output to `.github/skills/<project>-gotchas/SKILL.md`

If multiple platforms are specified, produce the skill in each location.

`<project>` is the project name derived from the repository root directory name or package.json name field.

## Steps

1. **Determine the project name** from package.json `name` field, Cargo.toml `[package] name`, or the root directory name as fallback.

2. **Scan for common gotcha sources** (optional, if the project has history):
   - Git log for reverts or fix commits that hint at past pitfalls
   - CI configs for non-obvious environment requirements
   - README or docs mentioning known issues
   - If you find clear gotchas, seed 1-3 entries. Otherwise leave the log empty.

3. **Write the skill file** at the platform-specific path using the output structure below.

## Output structure

```markdown
---
name: "<project>-gotchas"
description: "Use when stuck on an issue, encountering unexpected behavior, or ending a work session"
---

# Gotchas — <Project Name>

## When to use this skill
Load this skill when:
- You are stuck on a problem or encountering unexpected behavior
- You are about to end a work session (to append new discoveries)
- You are starting work in an unfamiliar area of the codebase
- A test fails for a non-obvious reason

## Content

Chronological log of non-obvious pitfalls. Newest at bottom.

### Format
Each entry: `- YYYY-MM-DD: <one-line gotcha>. Fix: <one-line fix or workaround>`

### Log

<seeded entries if found, otherwise empty>

## How to apply
- **Read** the log when you are stuck — your problem may already be documented
- **Append** a new entry before ending your session if you discovered a non-obvious pitfall
- Keep entries to one line, max 150 characters
- Do NOT edit or remove existing entries unless they are factually wrong

## Verification
- If you encountered a pitfall during this session, confirm you appended an entry
- New entries follow the format: `- YYYY-MM-DD: <gotcha>. Fix: <fix>`
- No existing entries were removed or modified
```

## Rules

- Entries must be **one line each**: what went wrong + how to fix it.
- Newest entries at the bottom (append-only).
- Maximum entry length: 150 characters.
- Do NOT remove or edit existing entries unless they are factually wrong.
- Create the platform directory structure if it does not exist.
- If the skill file already exists at the target path, preserve all existing log entries and update only the structural sections if needed.
