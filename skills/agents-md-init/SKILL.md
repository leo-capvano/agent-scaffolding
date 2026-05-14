---
name: agents-md-init
description: >
  Instructs the agent to create an AGENTS.md file at the project root.
  Uses a bundled template and adapts it to the target project's context files.
---

# write-agents-md

## What this skill does

Creates an `AGENTS.md` file at the project root — the shared instruction set that every coding agent reads before acting. It is derived from the template bundled with this skill (`template.md`) and adapted to match the project's actual context files.

## Steps

1. **Read the template** at `skills/agents-md-init/template.md` (relative to this repo).

2. **Scan the project root** for existing context files:
   - ARCHITECTURE.md
   - TESTING.md
   - DEPLOY.md
   - GOTCHAS.md
   - Any other documentation files that agents should know about

3. **Adapt the template**:
   - Keep the "General rules" section as-is from the template.
   - Under "Context files", include only the files that actually exist (or will be created by the scaffolding run).
   - If the project has additional relevant docs (e.g., API.md, CONTRIBUTING.md), add a short entry for them following the same pattern.

4. **Write AGENTS.md** to the project root.
   - If it already exists, compare with the template and update structure while preserving any project-specific customizations the user added.

## Rules

- The template (`template.md`) is the single source of truth for the base content. When the template evolves, re-running this skill picks up the changes.
- Do NOT hardcode the template content in this skill file — always read it from `template.md`.
- Keep the output concise: the file should be scannable in under 15 seconds.
- Do NOT add sections for context files that do not exist in the project.
- Preserve any custom sections the user may have added manually (append-safe).
