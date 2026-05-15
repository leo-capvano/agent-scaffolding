# Testing Artifact

## Exploration

Read these files in the target project:
- package.json, pyproject.toml, Makefile, pom.xml, Cargo.toml, or equivalent
- Existing test directories: tests/, __tests__/, spec/, test/
- CI configs: .github/workflows/, .gitlab-ci.yml, Jenkinsfile
- Any existing TESTING.md, README test sections, or test scripts
- Test configuration: jest.config.*, vitest.config.*, pytest.ini, .mocharc.*

Identify the test layers present: unit, integration, e2e, contract, etc.

## Plain Mode Output

Write `TESTING.md` at the project root (30-50 lines max).

Structure:

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

Rules:
- Use real commands found in the codebase. Use ${PLACEHOLDER} only for genuinely unknown values.
- Keep each section minimal — bullets over prose, commands over explanations.
- If a layer does not exist, omit its command entirely.
- No tutorials, no tool explanations, no setup guides.
- Hard limit: 50 lines.

## Skills Mode Output

Write to platform-specific path:
- OpenCode: `.opencode/skills/<project>-testing/SKILL.md`
- Claude Code: `.claude/skills/<project>-testing/SKILL.md`
- Copilot CLI: `.github/skills/<project>-testing/SKILL.md`

`<project>` = package.json name, Cargo.toml [package] name, or root directory name.

Use this structure:

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
- All tests: `<command>`
- Unit tests: `<command>`
- Integration tests: `<command>`
- E2E tests: `<command>` (only if present)

## How to apply
- Run the full suite before committing any changes
- New features require unit tests at minimum
- API changes require integration tests
- Use the project's naming conventions for test files and test cases

## Verification
- All tests pass: `<exact command>`
- Coverage meets threshold: `<exact command>` (only if coverage tooling exists)
- No skipped tests introduced without justification

Rules:
- Use real commands found in the codebase. Use ${PLACEHOLDER} only for genuinely unknown values.
- If a test layer does not exist, omit it entirely.
- The "How to apply" section must contain actionable instructions, not generic advice.
- The "Verification" section must contain copy-pasteable commands.

## Existing File Handling

- If the artifact already exists, read it first
- Validate it follows the expected structure (correct sections, real commands)
- Update outdated commands (e.g., test runner changed, new test layers added)
- Fix formatting issues
- Preserve any user-added custom sections or conventions
- Do NOT overwrite — merge changes in
