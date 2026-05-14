# Agent Scaffolding

> Reusable, lightweight scaffolding to help coding agents bootstrap high-quality project documentation and workflows.

This repository provides a set of focused **skills** that guide an agent through creating essential project artifacts such as architecture, testing, and deployment documentation.

## Why this repository?

When an agent starts in a new codebase, context quality determines output quality.  
These skills are designed to make that context explicit, consistent, and production-ready.

## Included skills

| Skill | Purpose |
|---|---|
| `agent-scaffolding-init` | Orchestrates initialization by invoking the other scaffolding skills |
| `architecture-md-init` | Creates an `ARCHITECTURE.md` based on the project structure and architecture.md conventions |
| `testing-md-init` | Produces a concise `TESTING.md` with strategy, conventions, and real test commands |
| `deploy-md-init` | Produces a concise `DEPLOY.md` with exact deploy and rollback commands |
| `gotchas-md-init` | Creates a `GOTCHAS.md` — chronological log of pitfalls for cross-session agent learning |
| `agents-md-init` | Creates an `AGENTS.md` from a bundled template — shared instructions for all coding agents |

## Repository structure

```text
.
├── skills/
│   ├── agent-scaffolding-init/
│   │   └── SKILL.md
│   ├── architecture-md-init/
│   │   └── SKILL.md
│   ├── testing-md-init/
│   │   └── SKILL.md
│   ├── deploy-md-init/
│   │   └── SKILL.md
│   ├── gotchas-md-init/
│   │   └── SKILL.md
│   └── agents-md-init/
│       ├── SKILL.md
│       └── template.md
├── LICENSE
└── README.md
```

## How to use

1. Start from `skills/agent-scaffolding-init/SKILL.md`.
2. Let your agent execute the workflow and invoke downstream skills.
3. Review generated docs (`ARCHITECTURE.md`, `TESTING.md`, `DEPLOY.md`, `GOTCHAS.md`, `AGENTS.md`) and refine for project-specific details.

## Design principles

- **Concise over verbose**: short, actionable outputs.
- **Reality-based**: commands and guidance should match the actual repository.
- **Composable**: independent skills that can be orchestrated together.
- **Agent-friendly**: deterministic structure that improves repeatability.

## Contributing

Contributions are welcome. Improvements to skill clarity, output quality, and robustness are encouraged.

## License

This project is licensed under the terms of the [LICENSE](./LICENSE) file.
