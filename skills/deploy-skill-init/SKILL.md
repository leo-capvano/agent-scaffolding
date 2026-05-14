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
