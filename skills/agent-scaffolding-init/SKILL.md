---
name: "agent-scaffolding-init"
description: "Initialize agent scaffolding for a project — generates context files or discoverable skills with a single invocation."
---

# Agent Scaffolding Initialization

Generates context artifacts for coding agents in the target project. Supports two output modes and handles all artifacts through a single skill with bundled reference files.

## Workflow

### 1. Ask output mode

Ask the user:
- "Do you want to generate **plain .md context files** or **discoverable skills**?"

### 2. Ask platform (skills mode only)

If the user chose skills:
- "Which agent platform(s) are you targeting?"
  - OpenCode → `.opencode/skills/<project>-<artifact>/SKILL.md`
  - Claude Code → `.claude/skills/<project>-<artifact>/SKILL.md`
  - Copilot CLI → `.github/skills/<project>-<artifact>/SKILL.md`
  - Multiple → ask which combination, generate to each selected platform's path

### 3. Dispatch artifact generators in parallel

Spawn 4 subagents in parallel. Each subagent receives:
- The content of its reference file (read from `references/<artifact>.md`)
- The chosen mode (`plain` or `skills`)
- The platform (if skills mode)

Each subagent independently:
1. Explores the project to gather context relevant to its artifact
2. Generates the artifact following the instructions in its reference file
3. If the artifact already exists: validates, updates, and lints it (does NOT overwrite)

Artifacts and their reference files:
| Artifact | Reference File |
|----------|---------------|
| Architecture | `references/architecture.md` |
| Testing | `references/testing.md` |
| Deploy | `references/deploy.md` |
| Gotchas | `references/gotchas.md` |

### 4. Generate AGENTS.md (always last)

After all 4 artifact subagents complete, generate AGENTS.md:

1. Read the appropriate template:
   - Plain mode: `references/agents-template.md`
   - Skills mode: `references/agents-template-skills.md`

2. Adapt the template:
   - **Plain mode**: Under "Context files", include only the files that actually exist in the project. If additional relevant docs exist (e.g., API.md), add a short entry following the same pattern.
   - **Skills mode**: Write the template as-is (no adaptation needed — context is delivered via platform skill discovery).

3. Write `AGENTS.md` to the project root.
   - If it already exists, update structure while preserving user customizations.

## Single Artifact Mode

If the user requests only one specific artifact (e.g., "just generate testing"):
- Still ask mode and platform questions
- Dispatch only that artifact's subagent
- Skip AGENTS.md generation unless explicitly requested

## Notes

- Reference files are the source of truth for each artifact's generation instructions.
- Always read reference files from disk — do NOT hardcode their content.
- Create platform directory structure if it does not exist (skills mode).
- The project name for skill paths comes from: package.json name → Cargo.toml [package] name → root directory name (in that priority order).
