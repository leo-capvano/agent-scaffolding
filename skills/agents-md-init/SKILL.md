---
name: agents-md-init
description: >
  Instructs the agent to create an AGENTS.md file at the project root.
  Uses a bundled template and adapts it to the target project's context files.
---

# write-agents-md

## What this skill does

Creates an `AGENTS.md` file at the project root — the shared instruction set that every coding agent reads before acting. It is derived from the template bundled with this skill and adapted to match the project's actual context.

## Inputs

This skill receives a **mode** from the orchestrator:
- **plain** (default): Uses `template.md` — includes context file references.
- **skills**: Uses `template-skills.md` — general rules only (context is delivered via platform skill discovery).

If no mode is specified, assume **plain**.

## Steps

1. **Determine the template** based on mode:
   - If mode is `plain`: read `skills/agents-md-init/template.md`
   - If mode is `skills`: read `skills/agents-md-init/template-skills.md`

2. **Scan the project root** for existing context files (only relevant in plain mode):
   - ARCHITECTURE.md
   - TESTING.md
   - DEPLOY.md
   - GOTCHAS.md
   - Any other documentation files that agents should know about

3. **Adapt the template**:
   - Keep the "General rules" section as-is from the template.
   - **Plain mode only**: Under "Context files", include only the files that actually exist (or will be created by the scaffolding run). If the project has additional relevant docs (e.g., API.md, CONTRIBUTING.md), add a short entry following the same pattern.
   - **Skills mode**: No adaptation needed beyond the general rules — write the template as-is.

4. **Write AGENTS.md** to the project root.
   - If it already exists, compare with the template and update structure while preserving any project-specific customizations the user added.

## Rules

- The template files are the single source of truth. Always read from file — do NOT hardcode content.
- Keep the output concise: scannable in under 15 seconds.
- In plain mode, do NOT add sections for context files that do not exist.
- Preserve any custom sections the user may have added manually (append-safe).
