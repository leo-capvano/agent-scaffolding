# Agent Scaffolding

> Reusable, lightweight scaffolding to help coding agents bootstrap high-quality project documentation and workflows.

This repository provides a single **skill** that guides an agent through creating essential project artifacts such as architecture, testing, and deployment documentation.

## Why this repository?

When an agent starts in a new codebase, context quality determines output quality.  
These skills are designed to make that context explicit, consistent, and production-ready.

## Included skill

The repository contains a single unified skill — `agent-scaffolding-init` — that orchestrates the entire initialization workflow. It asks for the output mode, then dispatches subagents with instructions drawn from bundled reference files:

| Reference file | Purpose |
|---|---|
| `architecture.md` | Instructions for creating an `ARCHITECTURE.md` based on the project structure |
| `testing.md` | Instructions for producing a concise `TESTING.md` with strategy, conventions, and real test commands |
| `deploy.md` | Instructions for producing a concise `DEPLOY.md` with exact deploy and rollback commands |
| `gotchas.md` | Instructions for creating a `GOTCHAS.md` — chronological log of pitfalls for cross-session agent learning |
| `agents-template.md` | Template for `AGENTS.md` — shared instructions for all coding agents |
| `agents-template-skills.md` | Template variant for `AGENTS.md` when using discoverable skills mode |

## Repository structure

```text
.
├── skills/
│   └── agent-scaffolding-init/
│       ├── SKILL.md
│       └── references/
│           ├── architecture.md
│           ├── testing.md
│           ├── deploy.md
│           ├── gotchas.md
│           ├── agents-template.md
│           └── agents-template-skills.md
├── LICENSE
└── README.md
```

## How to use

1. Start from `skills/agent-scaffolding-init/SKILL.md`.
2. Let your agent execute the workflow — it dispatches subagents with reference file instructions to generate each artifact.
3. Review generated docs (`ARCHITECTURE.md`, `TESTING.md`, `DEPLOY.md`, `GOTCHAS.md`, `AGENTS.md`) and refine for project-specific details.

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

## Design principles

- **Concise over verbose**: short, actionable outputs.
- **Reality-based**: commands and guidance should match the actual repository.
- **Composable**: independent reference files orchestrated by a single skill.
- **Agent-friendly**: deterministic structure that improves repeatability.

## Contributing

Contributions are welcome. Improvements to skill clarity, output quality, and robustness are encouraged.

## License

This project is licensed under the terms of the [LICENSE](./LICENSE) file.
