---
name: testing-md-init
description: >
  Instructs the agent to write a short TESTING.md for the current project.
  Covers testing strategy, conventions, and the exact commands to run tests.
  The agent uses this file as a checklist every time it needs to verify work.
---

# write-testing-md

## What this skill does

Produces a concise TESTING.md (30–50 lines max) that serves two purposes:
1. **For humans** — documents the test strategy and conventions for this project.
2. **For the agent** — acts as a verification checklist to run after every implementation.

## Steps

1. **Read the repository** before writing anything:
   - package.json, pyproject.toml, Makefile, pom.xml, Cargo.toml, or equivalent
   - Existing test directories: tests/, __tests__/, spec/, test/
   - CI configs: .github/workflows/, .gitlab-ci.yml, Jenkinsfile
   - Any existing TESTING.md, README test sections, or test scripts

2. **Identify** the test layers present: unit, integration, e2e, contract, etc.

3. **Write TESTING.md** using the structure below.

## Output structure

```markdown
# TESTING.md

## Strategy
One or two sentences on what is tested and at which layer.

## Conventions
- Short bullet list of project-specific rules (naming, file location, fixtures, etc.)

## Commands

# Run all tests
<command>

# Unit tests only
<command>

# Integration tests only
<command>

# E2E tests only (if present)
<command>

## Definition of done
Short bullet list: what must pass before a task is considered complete.
```

## Rules

- Use **real commands** found in the codebase. Use `${PLACEHOLDER}` only for genuinely unknown values.
- Keep each section minimal — bullets over prose, commands over explanations.
- If a layer does not exist in the project, omit its command entirely.
- **No** tutorials, no tool explanations, no setup guides.
- The agent must be able to read this file and immediately know what to run.
- Hard limit: 50 lines.
