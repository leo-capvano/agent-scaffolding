# Skill Generation Mode — Design Spec

## Overview

Add a "skill mode" to agent-scaffolding. When enabled, instead of generating plain `.md` context files, the scaffolding generates platform-specific discoverable skills with enhanced agent guidance. The user chooses the output mode (plain md or skills) via a global toggle at scaffolding time.

## Approach

**Dual-mode generator skills (Approach A)**: Each existing `-md-init` generator gets a sibling `-skill-init` generator. The orchestrator asks the user which mode they want and dispatches to the appropriate set. No conditionals inside individual skills — each is focused and self-contained.

## Project Structure

```
skills/
  agent-scaffolding-init/SKILL.md           # orchestrator (updated)

  # Plain .md generators (existing, unchanged)
  architecture-md-init/SKILL.md
  testing-md-init/SKILL.md
  deploy-md-init/SKILL.md
  gotchas-md-init/SKILL.md
  agents-md-init/SKILL.md + template.md + template-skills.md

  # Skill generators (new)
  architecture-skill-init/SKILL.md
  testing-skill-init/SKILL.md
  deploy-skill-init/SKILL.md
  gotchas-skill-init/SKILL.md
```

No `agents-skill-init` — AGENTS.md is always generated as a plain markdown file regardless of mode.

## Orchestrator Flow

1. Explore the project structure (unchanged)
2. Ask the user: "Do you want to generate plain .md context files or discoverable skills?"
   - If **skills**: ask which platform(s) — OpenCode / Claude Code / Copilot CLI / multiple
3. Dispatch to the appropriate generators:
   - **Plain mode**: `architecture-md-init`, `testing-md-init`, `deploy-md-init`, `gotchas-md-init`
   - **Skill mode**: `architecture-skill-init`, `testing-skill-init`, `deploy-skill-init`, `gotchas-skill-init` (with platform choice passed as context)
4. **Always last, in both modes**: `agents-md-init` (picks the correct template based on mode)

## Platform Selection

The scaffolding **always asks the user** which platform they're targeting. No auto-detection.

Prompt: "Which agent platform(s) are you targeting?"
- OpenCode
- Claude Code
- Copilot CLI
- Multiple (then asks which combination)

The question is asked once by the orchestrator and the answer is passed to all downstream skill generators.

## Output Paths

| Platform   | Output path                          |
|------------|--------------------------------------|
| OpenCode   | `.opencode/skills/<name>/SKILL.md`   |
| Claude Code| `.claude/skills/<name>/SKILL.md`     |
| Copilot CLI| `.github/skills/<name>/SKILL.md`     |

If the user selects multiple platforms, skills are generated in each location.

## Generated Skill Format

Each `-skill-init` generator produces a SKILL.md with this structure:

```markdown
---
name: "<project>-<domain>"
description: "<one-line trigger description>"
---

# <Domain> — <Project Name>

## When to use this skill
<Agent-facing guidance on when to load this skill. Describes the triggers/situations.>

## Content
<The actual domain knowledge — same depth as the plain .md equivalent,
but written as instructions/context for the agent rather than passive documentation.>

## How to apply
<Concrete guidance on what to do with this information during a task.>

## Verification
<How the agent can verify it followed this skill correctly.>
```

### Example — generated `myapp-testing` skill

```markdown
---
name: "myapp-testing"
description: "Use when writing, running, or modifying tests in this project"
---

# Testing — MyApp

## When to use this skill
Load this skill when:
- Writing new tests or modifying existing ones
- Running the test suite to verify changes
- Setting up test infrastructure or fixtures

## Content
- Framework: Vitest + React Testing Library
- Test location: colocated `*.test.ts` files next to source
- Commands: `npm test` (all), `npm test -- --run` (CI mode)
- Coverage threshold: 80% lines

## How to apply
- Run the full suite before committing
- New features require unit tests; API changes require integration tests
- Use `describe/it` naming: "describe <unit> > it <behavior>"

## Verification
- All tests pass: `npm test -- --run`
- Coverage meets threshold: `npm test -- --coverage`
```

## AGENTS.md Templates

Two templates in `skills/agents-md-init/`:

### `template.md` (existing, unchanged — for plain mode)

Contains general rules + context file references (ARCHITECTURE.md, TESTING.md, DEPLOY.md, GOTCHAS.md).

### `template-skills.md` (new — for skill mode)

Contains general rules only. No context file/skill references — the platform's skill infrastructure already makes skills discoverable to the agent.

```markdown
# General rules
Before doing any action, think deeply and plan your work. Always try to leverage subagents...
After any task, consider to upgrade the tests and the documentation files.
```

The `agents-md-init` skill selects the template based on which mode the orchestrator passed.

## Key Differences: Plain .md vs Generated Skill

| Aspect | Plain .md | Generated skill |
|--------|-----------|-----------------|
| Discovery | Agent must be told to read it | Platform surfaces it automatically |
| Trigger | None — passive document | Description field triggers loading |
| Content | Facts and reference | Facts + when to use + how to apply + verification |
| Location | Project root | Platform-specific skill directory |
| AGENTS.md | References the files | No references needed (skills self-describe) |

## Design Principles

- **No auto-detection**: Always ask the user for platform choice
- **Clean separation**: Each mode has its own skills, no branching logic
- **AGENTS.md is always plain markdown**: It's project memory, not a skill
- **Skills are enhanced, not just wrapped**: They add agent guidance (when/how/verify) beyond raw content
- **Platform question asked once**: Orchestrator asks, all generators receive the answer
