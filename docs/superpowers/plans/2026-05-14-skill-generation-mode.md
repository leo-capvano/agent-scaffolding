# Skill Generation Mode — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a global toggle to agent-scaffolding so it can generate discoverable, platform-specific skills (with agent guidance) instead of plain .md context files.

**Architecture:** Dual-mode approach — each existing `-md-init` generator gets a sibling `-skill-init` generator. The orchestrator asks the user which mode and platform, then dispatches to the correct set. AGENTS.md always stays as plain markdown with a mode-appropriate template.

**Tech Stack:** Pure markdown (SKILL.md files with YAML frontmatter). No code, no build system.

---

## File Structure

| Action | Path | Responsibility |
|--------|------|---------------|
| Create | `skills/testing-skill-init/SKILL.md` | Generates a testing skill in the target project |
| Create | `skills/deploy-skill-init/SKILL.md` | Generates a deploy skill in the target project |
| Create | `skills/gotchas-skill-init/SKILL.md` | Generates a gotchas skill in the target project |
| Create | `skills/architecture-skill-init/SKILL.md` | Generates an architecture skill in the target project |
| Create | `skills/agents-md-init/template-skills.md` | Minimal AGENTS.md template for skill mode (no context file references) |
| Modify | `skills/agents-md-init/SKILL.md` | Add mode awareness — pick the correct template based on orchestrator input |
| Modify | `skills/agent-scaffolding-init/SKILL.md` | Add mode toggle + platform question, dispatch to correct generator set |
| Modify | `README.md` | Document the new skill mode option |

---

### Task 1: Create `template-skills.md` for AGENTS.md skill-mode variant

**Files:**
- Create: `skills/agents-md-init/template-skills.md`

This is the simplest piece — a stripped-down template with only the general rules section (no context file references).

- [ ] **Step 1: Create the file**

```markdown
# General rules
Before doing any action, think deeply and plan your work. Always try to leverage subagents by splitting your task in little subtasks that can be executed by a subagent. If some of those tasks are independent call subagents in parallel.

After any task, consider to upgrade the tests and the documentation files.
```

This is identical to the first section of `template.md` but without the "Context files" section. The rationale: in skill mode, context is delivered via platform skill discovery, not by reading sibling .md files.

- [ ] **Step 2: Commit**

```bash
git add skills/agents-md-init/template-skills.md
git commit -m "add template-skills.md for skill-mode AGENTS.md generation"
```

---

### Task 2: Update `agents-md-init` to support both modes

**Files:**
- Modify: `skills/agents-md-init/SKILL.md`

The skill needs to accept a "mode" context from the orchestrator and pick the right template.

- [ ] **Step 1: Rewrite the SKILL.md**

Replace the full content of `skills/agents-md-init/SKILL.md` with:

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git add skills/agents-md-init/SKILL.md
git commit -m "update agents-md-init to support plain and skills mode templates"
```

---

### Task 3: Create `testing-skill-init`

**Files:**
- Create: `skills/testing-skill-init/SKILL.md`

This skill mirrors the exploration logic of `testing-md-init` but outputs a platform-specific skill file instead of a plain TESTING.md.

- [ ] **Step 1: Create the file**

```markdown
---
name: testing-skill-init
description: >
  Generates a discoverable testing skill for the target project.
  The skill provides testing strategy, conventions, commands, and agent guidance
  in a platform-specific skill format.
---

# generate-testing-skill

## What this skill does

Produces a testing skill file for the target project. Unlike the plain TESTING.md, this skill is discoverable by the agent platform and includes guidance on when to use it, how to apply it, and how to verify compliance.

## Inputs

This skill receives a **platform** from the orchestrator:
- `opencode` → output to `.opencode/skills/<project>-testing/SKILL.md`
- `claude` → output to `.claude/skills/<project>-testing/SKILL.md`
- `copilot` → output to `.github/skills/<project>-testing/SKILL.md`

If multiple platforms are specified, produce the skill in each location.

`<project>` is the project name derived from the repository root directory name or package.json name field.

## Steps

1. **Read the repository** before writing anything:
   - package.json, pyproject.toml, Makefile, pom.xml, Cargo.toml, or equivalent
   - Existing test directories: tests/, __tests__/, spec/, test/
   - CI configs: .github/workflows/, .gitlab-ci.yml, Jenkinsfile
   - Any existing TESTING.md, README test sections, or test scripts

2. **Identify** the test layers present: unit, integration, e2e, contract, etc.

3. **Determine the project name** from package.json `name` field, Cargo.toml `[package] name`, or the root directory name as fallback.

4. **Write the skill file** at the platform-specific path using the output structure below.

## Output structure

```markdown
---
name: "<project>-testing"
description: "Use when writing, running, or modifying tests in this project"
---

# Testing — <Project Name>

## When to use this skill
Load this skill when:
- Writing new tests or modifying existing ones
- Running the test suite to verify changes
- Setting up test infrastructure or fixtures
- Reviewing code that affects test coverage

## Content

### Strategy
<One or two sentences on what is tested and at which layer.>

### Conventions
<Short bullet list of project-specific rules: naming, file location, fixtures, etc.>

### Commands
<Real commands found in the codebase:>
- All tests: `<command>`
- Unit tests: `<command>`
- Integration tests: `<command>`
- E2E tests: `<command>` (only if present)

## How to apply
- Run the full suite before committing any changes
- New features require unit tests at minimum
- API changes require integration tests
- Use the project's naming conventions for test files and test cases
- <any project-specific guidance derived from the codebase>

## Verification
- All tests pass: `<exact command>`
- Coverage meets threshold: `<exact command>` (only if coverage tooling exists)
- No skipped tests introduced without justification
```

## Rules

- Use **real commands** found in the codebase. Use `${PLACEHOLDER}` only for genuinely unknown values.
- If a test layer does not exist, omit it entirely.
- The "How to apply" section must contain actionable instructions, not generic advice.
- The "Verification" section must contain copy-pasteable commands.
- Create the platform directory structure if it does not exist.
- If the skill file already exists at the target path, update it while preserving any user customizations.
```

- [ ] **Step 2: Commit**

```bash
git add skills/testing-skill-init/SKILL.md
git commit -m "add testing-skill-init generator"
```

---

### Task 4: Create `deploy-skill-init`

**Files:**
- Create: `skills/deploy-skill-init/SKILL.md`

- [ ] **Step 1: Create the file**

```markdown
---
name: deploy-skill-init
description: >
  Generates a discoverable deploy skill for the target project.
  The skill provides deployment steps, rollback commands, and agent guidance
  in a platform-specific skill format.
---

# generate-deploy-skill

## What this skill does

Produces a deploy skill file for the target project. Unlike the plain DEPLOY.md, this skill is discoverable by the agent platform and includes guidance on when to use it, how to apply it, and how to verify a successful deployment.

## Inputs

This skill receives a **platform** from the orchestrator:
- `opencode` → output to `.opencode/skills/<project>-deploy/SKILL.md`
- `claude` → output to `.claude/skills/<project>-deploy/SKILL.md`
- `copilot` → output to `.github/skills/<project>-deploy/SKILL.md`

If multiple platforms are specified, produce the skill in each location.

`<project>` is the project name derived from the repository root directory name or package.json name field.

## Steps

1. **Read the repository** before writing anything:
   - Dockerfile, docker-compose*.yml, Makefile
   - CI/CD configs: .github/workflows/, .gitlab-ci.yml, Jenkinsfile
   - Deployment scripts: scripts/, infra/, k8s/, deploy/, helm/
   - ARCHITECTURE.md if present

2. **Identify the real deploy method** used by this project (Docker, Kubernetes, Helm, docker-compose, a plain Makefile target, a cloud CLI, etc.).

3. **Determine the project name** from package.json `name` field, Cargo.toml `[package] name`, or the root directory name as fallback.

4. **Write the skill file** at the platform-specific path using the output structure below.

## Output structure

```markdown
---
name: "<project>-deploy"
description: "Use when deploying this project or troubleshooting deployment issues"
---

# Deploy — <Project Name>

## When to use this skill
Load this skill when:
- Deploying the application to any environment
- Troubleshooting deployment failures
- Modifying CI/CD pipelines or deployment configuration
- Rolling back a bad release

## Content

### Deploy sequence
<Numbered, copy-pasteable commands:>
1. `<command>` — <short label>
2. `<command>` — <short label>
...

### Rollback
`<single rollback command>`

## How to apply
- Follow the deploy sequence in exact order
- If a step is automated by CI, it will be noted — do not run it manually
- Verify each step succeeds before proceeding to the next
- <any project-specific deployment guidance>

## Verification
- Deployment succeeded: `<health check or status command>`
- Rollback succeeded: `<verification command>`
```

## Rules

- Use **real values** found in the codebase. Use `${PLACEHOLDER}` only for genuinely unknown values.
- If a step is automated by CI, write: `# Handled by CI — trigger via <method>` and skip the manual command.
- Every command must be copy-pasteable with no hidden assumptions.
- The "How to apply" section must contain actionable instructions specific to this project.
- The "Verification" section must contain real commands to confirm success.
- Create the platform directory structure if it does not exist.
- If the skill file already exists at the target path, update it while preserving any user customizations.
```

- [ ] **Step 2: Commit**

```bash
git add skills/deploy-skill-init/SKILL.md
git commit -m "add deploy-skill-init generator"
```

---

### Task 5: Create `gotchas-skill-init`

**Files:**
- Create: `skills/gotchas-skill-init/SKILL.md`

- [ ] **Step 1: Create the file**

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git add skills/gotchas-skill-init/SKILL.md
git commit -m "add gotchas-skill-init generator"
```

---

### Task 6: Create `architecture-skill-init`

**Files:**
- Create: `skills/architecture-skill-init/SKILL.md`

- [ ] **Step 1: Create the file**

```markdown
---
name: architecture-skill-init
description: >
  Generates a discoverable architecture skill for the target project.
  The skill provides software architecture context and agent guidance
  in a platform-specific skill format.
---

# generate-architecture-skill

## What this skill does

Produces an architecture skill file for the target project. Unlike the plain ARCHITECTURE.md, this skill is discoverable by the agent platform and includes guidance on when to load it and how to apply architectural knowledge during development.

## Inputs

This skill receives a **platform** from the orchestrator:
- `opencode` → output to `.opencode/skills/<project>-architecture/SKILL.md`
- `claude` → output to `.claude/skills/<project>-architecture/SKILL.md`
- `copilot` → output to `.github/skills/<project>-architecture/SKILL.md`

If multiple platforms are specified, produce the skill in each location.

`<project>` is the project name derived from the repository root directory name or package.json name field.

## Steps

1. **Fetch the ARCHITECTURE.md example** from https://architecture.md/ for reference on what good architecture documentation looks like.
   If you don't have a native webfetch tool, use curl.

2. **Analyze the project** to collect architecture information.
   Use subagents (parallel when possible) to explore:
   - Project structure and module boundaries
   - Entry points and data flow
   - Key abstractions and their relationships
   - External dependencies and integrations
   - Design patterns in use

3. **Determine the project name** from package.json `name` field, Cargo.toml `[package] name`, or the root directory name as fallback.

4. **Write the skill file** at the platform-specific path using the output structure below.

## Output structure

```markdown
---
name: "<project>-architecture"
description: "Use when making structural decisions, adding modules, or understanding how components connect"
---

# Architecture — <Project Name>

## When to use this skill
Load this skill when:
- Making structural decisions (where to put new code)
- Adding new modules or components
- Trying to understand how existing components connect
- Evaluating the impact of a change across the system

## Content

### Overview
<2-3 sentences: what this system does and its primary architectural style.>

### Structure
<Module/directory layout with one-line descriptions of each top-level unit.>

### Data flow
<How data moves through the system — entry points, processing, storage, output.>

### Key abstractions
<The core types/interfaces/classes that everything else builds on.>

### External dependencies
<Services, databases, APIs this system depends on and why.>

## How to apply
- New code goes in the module that matches its responsibility (see Structure)
- Follow existing patterns for module boundaries — don't introduce new architectural styles without justification
- Changes that cross module boundaries should be flagged for review
- <any project-specific architectural guidance>

## Verification
- New files are placed in the correct module per the Structure section
- No circular dependencies introduced between modules
- External integrations follow the existing patterns documented above
```

## Rules

- Be concise — write only meaningful data, not boilerplate.
- Take the fetched architecture.md example as structural inspiration, not a rigid template.
- If you are unsure about a section, analyze the codebase using subagents before writing.
- The "How to apply" section must contain actionable rules specific to this project's architecture.
- Create the platform directory structure if it does not exist.
- If the skill file already exists at the target path, update it while preserving any user-added sections.
```

- [ ] **Step 2: Commit**

```bash
git add skills/architecture-skill-init/SKILL.md
git commit -m "add architecture-skill-init generator"
```

---

### Task 7: Update the orchestrator

**Files:**
- Modify: `skills/agent-scaffolding-init/SKILL.md`

This is the key change — adding the mode toggle and platform question.

- [ ] **Step 1: Rewrite the orchestrator SKILL.md**

Replace the full content of `skills/agent-scaffolding-init/SKILL.md` with:

```markdown
---
name: "agent-scaffolding-init"
description: "Initialize a new agent scaffolding project with the necessary files and structure."
---

# Agent Scaffolding Initialization

This skill initializes a new agent scaffolding project by creating the necessary context files (or discoverable skills) and directory structure. It sets up a framework for coding agents to work effectively in the project.

# Workflow

1. **Explore the project structure**: use a subagent to explore the project structure and understand the codebase.

2. **Ask the user for output mode**:
   - "Do you want to generate **plain .md context files** or **discoverable skills**?"
   - If the user chooses **skills**, ask: "Which agent platform(s) are you targeting?"
     - OpenCode
     - Claude Code
     - Copilot CLI
     - Multiple (ask which combination)

3. **Dispatch to generators** based on the chosen mode:

   **Plain mode** — invoke these skills (via subagents, with exploration results as context):
   - `architecture-md-init`
   - `testing-md-init`
   - `deploy-md-init`
   - `gotchas-md-init`

   **Skills mode** — invoke these skills (via subagents, with exploration results AND platform choice as context):
   - `architecture-skill-init`
   - `testing-skill-init`
   - `deploy-skill-init`
   - `gotchas-skill-init`

4. **Generate AGENTS.md** (always last, in both modes):
   - Invoke `agents-md-init` via a subagent, passing the chosen mode (`plain` or `skills`) so it picks the correct template.

## Notes

- For steps 3-4, spawn a subagent for each skill and give it the exploration results as initial context.
- In skills mode, each subagent also receives the platform choice (e.g., `opencode`, `claude`, `copilot`, or a list for multiple).
- Some files/skills may already exist; subagents will verify content is correct and update if necessary.
- The AGENTS.md step runs last because it depends on knowing which context files or skills were generated.
```

- [ ] **Step 2: Commit**

```bash
git add skills/agent-scaffolding-init/SKILL.md
git commit -m "update orchestrator with mode toggle and platform selection"
```

---

### Task 8: Update README.md

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Read the current README**

Read `README.md` to understand its current structure before modifying.

- [ ] **Step 2: Add skill mode documentation**

Add a section after the existing "Included skills" or "How to use" section that documents the new mode:

```markdown
## Output modes

When you run `agent-scaffolding-init`, it asks which output mode you prefer:

### Plain .md (default)
Generates documentation files at the project root:
- `ARCHITECTURE.md`, `TESTING.md`, `DEPLOY.md`, `GOTCHAS.md`, `AGENTS.md`

These are passive context files that agents read when instructed.

### Discoverable skills
Generates platform-specific skill files that agents discover and load automatically:
- `<project>-architecture`, `<project>-testing`, `<project>-deploy`, `<project>-gotchas`

Each skill includes trigger descriptions, application guidance, and verification steps — not just reference content. You choose the target platform (OpenCode, Claude Code, or Copilot CLI) and skills are placed in the correct directory.

`AGENTS.md` is always generated as a plain file regardless of mode.
```

Also update the "Included skills" list to mention the new `-skill-init` variants.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "document skill generation mode in README"
```

---

## Verification

After all tasks are complete, verify the final state:

- [ ] All 4 new skill files exist:
  - `skills/architecture-skill-init/SKILL.md`
  - `skills/testing-skill-init/SKILL.md`
  - `skills/deploy-skill-init/SKILL.md`
  - `skills/gotchas-skill-init/SKILL.md`
- [ ] `skills/agents-md-init/template-skills.md` exists
- [ ] `skills/agents-md-init/SKILL.md` handles both modes
- [ ] `skills/agent-scaffolding-init/SKILL.md` has mode toggle and platform question
- [ ] `README.md` documents the new option
- [ ] All files have valid YAML frontmatter (name + description)
- [ ] Each `-skill-init` file references all 3 platform output paths
- [ ] Git log shows clean, atomic commits for each task
