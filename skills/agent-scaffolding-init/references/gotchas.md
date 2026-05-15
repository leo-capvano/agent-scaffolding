# Gotchas Artifact

## Exploration

Scan for common gotcha sources (if the project has history):
- Git log for reverts or fix commits that hint at past pitfalls
- CI configs for non-obvious environment requirements
- README or docs mentioning known issues
- Existing GOTCHAS.md

If you find clear gotchas, seed 1-3 entries. Otherwise leave the log empty.

## Plain Mode Output

Write `GOTCHAS.md` at the project root.

Structure:

# GOTCHAS.md

Chronological log of non-obvious pitfalls. Read when stuck. Append before ending your session.

## Format

Each entry: `- YYYY-MM-DD: <one-line gotcha>. Fix: <one-line fix or workaround>`

## Log

- (seeded entries if found, otherwise empty)

Rules:
- Entries must be one line each: what went wrong + how to fix it.
- No categories, no headers per entry, no multi-line explanations.
- Newest entries go at the bottom (append-only).
- Maximum entry length: 150 characters.
- Do NOT remove or edit existing entries unless they are factually wrong.
- The file must be scannable in under 10 seconds.

## Skills Mode Output

Write to platform-specific path:
- OpenCode: `.opencode/skills/<project>-gotchas/SKILL.md`
- Claude Code: `.claude/skills/<project>-gotchas/SKILL.md`
- Copilot CLI: `.github/skills/<project>-gotchas/SKILL.md`

`<project>` = package.json name, Cargo.toml [package] name, or root directory name.

Use this structure:

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
- Read the log when you are stuck — your problem may already be documented
- Append a new entry before ending your session if you discovered a non-obvious pitfall
- Keep entries to one line, max 150 characters
- Do NOT edit or remove existing entries unless they are factually wrong

## Verification
- If you encountered a pitfall during this session, confirm you appended an entry
- New entries follow the format: `- YYYY-MM-DD: <gotcha>. Fix: <fix>`
- No existing entries were removed or modified

Rules:
- Entries must be one line each: what went wrong + how to fix it.
- Newest entries at the bottom (append-only).
- Maximum entry length: 150 characters.
- Do NOT remove or edit existing entries unless they are factually wrong.
- Preserve all existing log entries when updating.

## Existing File Handling

- If the artifact already exists, read it first
- If structure is correct, leave it as-is (this file grows over time)
- If structure is malformed, fix it while preserving all existing entries
- Never remove or reorder existing log entries
