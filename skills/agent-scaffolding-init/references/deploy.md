# Deploy Artifact

## Exploration

Read these files in the target project:
- Dockerfile, docker-compose*.yml, Makefile
- CI/CD configs: .github/workflows/, .gitlab-ci.yml, Jenkinsfile
- Deployment scripts: scripts/, infra/, k8s/, deploy/, helm/
- ARCHITECTURE.md if present
- Cloud configs: serverless.yml, app.yaml, fly.toml, railway.json, vercel.json

Identify the real deploy method used (Docker, Kubernetes, Helm, docker-compose, Makefile target, cloud CLI, etc.).

## Plain Mode Output

Write `DEPLOY.md` at the project root (20-40 lines max).

Structure:

# DEPLOY.md

## Deploy

# 1. <short label>
<command>

# 2. <short label>
<command>

...

## Rollback

<command>

Rules:
- Use real values found in the codebase. Use ${PLACEHOLDER} only for genuinely unknown values.
- If a step is automated by CI, write: `# Handled by CI — trigger via <method>.` and skip the manual command.
- Every command must be copy-pasteable with no hidden assumptions.
- No section other than ## Deploy and ## Rollback is allowed.
- No explanations beyond the one-line # label above each command.
- Hard limit: 40 lines.

## Skills Mode Output

Write to platform-specific path:
- OpenCode: `.opencode/skills/<project>-deploy/SKILL.md`
- Claude Code: `.claude/skills/<project>-deploy/SKILL.md`
- Copilot CLI: `.github/skills/<project>-deploy/SKILL.md`

`<project>` = package.json name, Cargo.toml [package] name, or root directory name.

Use this structure:

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
1. `<command>` — <short label>
2. `<command>` — <short label>
...

### Rollback
`<single rollback command>`

## How to apply
- Follow the deploy sequence in exact order
- If a step is automated by CI, it will be noted — do not run it manually
- Verify each step succeeds before proceeding to the next

## Verification
- Deployment succeeded: `<health check or status command>`
- Rollback succeeded: `<verification command>`

Rules:
- Use real values found in the codebase. Use ${PLACEHOLDER} only for genuinely unknown values.
- If a step is automated by CI, write: `# Handled by CI — trigger via <method>` and skip the manual command.
- Every command must be copy-pasteable with no hidden assumptions.
- The "Verification" section must contain real commands to confirm success.

## Existing File Handling

- If the artifact already exists, read it first
- Validate it follows the expected structure (Deploy + Rollback sections only)
- Update outdated commands (e.g., deploy target changed, new steps added)
- Fix formatting issues
- Preserve user customizations
- Do NOT overwrite — merge changes in
