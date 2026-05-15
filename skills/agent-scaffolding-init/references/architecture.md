# Architecture Artifact

## Exploration

Read these files/directories in the target project:
- Project root: package.json, Cargo.toml, pyproject.toml, go.mod, pom.xml
- Directory structure: top-level folders, src/, lib/, app/
- Existing ARCHITECTURE.md or architecture docs
- Entry points: main files, index files, server files
- CI configs for build/deploy hints about architecture

Use subagents (parallel when possible) to explore:
- Project structure and module boundaries
- Entry points and data flow
- Key abstractions and their relationships
- External dependencies and integrations
- Design patterns in use

Also fetch https://architecture.md/ for reference on good architecture documentation.

## Plain Mode Output

Write `ARCHITECTURE.md` at the project root.

Take the fetched architecture.md example as structural inspiration. Be concise — write only meaningful data. If unsure about a section, analyze the codebase before writing.

No line limit, but keep each section minimal: bullets over prose, structure over explanation.

## Skills Mode Output

Write to platform-specific path:
- OpenCode: `.opencode/skills/<project>-architecture/SKILL.md`
- Claude Code: `.claude/skills/<project>-architecture/SKILL.md`
- Copilot CLI: `.github/skills/<project>-architecture/SKILL.md`

`<project>` = package.json name, Cargo.toml [package] name, or root directory name.

Use this structure:

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
- Follow existing patterns for module boundaries
- Changes that cross module boundaries should be flagged for review

## Verification
- New files are placed in the correct module per the Structure section
- No circular dependencies introduced between modules
- External integrations follow existing patterns

## Existing File Handling

- If the artifact already exists, read it first
- Validate it follows the expected structure
- Update outdated information (e.g., modules that no longer exist, new dependencies)
- Fix formatting issues
- Preserve any user-added custom sections
- Do NOT overwrite — merge changes in
