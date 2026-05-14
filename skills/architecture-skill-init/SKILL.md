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

````markdown
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
````

## Rules

- Be concise — write only meaningful data, not boilerplate.
- Take the fetched architecture.md example as structural inspiration, not a rigid template.
- If you are unsure about a section, analyze the codebase using subagents before writing.
- The "How to apply" section must contain actionable rules specific to this project's architecture.
- Create the platform directory structure if it does not exist.
- If the skill file already exists at the target path, update it while preserving any user-added sections.
