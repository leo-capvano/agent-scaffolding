---
name: deploy-md-init
description: >
  Instructs the agent to write a short, command-only DEPLOY.md file for the current project.
  Covers the exact deploy sequence and a rollback command. No tutorials, no padding.
---

# write-deploy-md

## What this skill does

Produces a concise DEPLOY.md file (20–40 lines max) containing only:
1. The exact ordered commands to deploy this project from build to live.
2. A rollback command to undo a bad deploy.

Nothing else. No prerequisites tables, no environment variable sections, no theory.

## Steps

1. **Read the repository** before writing anything:
   - Dockerfile, docker-compose*.yml, Makefile
   - CI/CD configs: .github/workflows/, .gitlab-ci.yml, Jenkinsfile
   - Deployment scripts: scripts/, infra/, k8s/, deploy/, helm/
   - ARCHITECTURE.md if present

2. **Identify the real deploy method** used by this project (Docker, Kubernetes, Helm,
   docker-compose, a plain Makefile target, a cloud CLI, etc.).

3. **Write DEPLOY.md** using the structure below.

## Output structure

```markdown
# DEPLOY.md

## Deploy

# 1. <short label>
<command>

# 2. <short label>
<command>

...

## Rollback

<command>
```

## Rules

- Use **real values** found in the codebase. Use `${PLACEHOLDER}` only for genuinely unknown values.
- If a step is automated by CI, write: `# Handled by CI — trigger via <method>.` and skip the manual command.
- Every command must be copy-pasteable with no hidden assumptions.
- **No** section other than `## Deploy` and `## Rollback` is allowed.
- **No** explanations beyond the one-line `# label` above each command.
- The file must be readable in under 30 seconds.
- Hard limit: 40 lines.
