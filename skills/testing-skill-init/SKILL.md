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
