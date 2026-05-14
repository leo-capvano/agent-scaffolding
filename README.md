# Agent Scaffolding

> Reusable, lightweight scaffolding to help coding agents bootstrap high-quality project documentation and workflows.

This repository provides a set of focused **skills** that guide an agent through creating essential project artifacts such as architecture, testing, and deployment documentation.

## Why this repository?

When an agent starts in a new codebase, context quality determines output quality.  
These skills are designed to make that context explicit, consistent, and production-ready.

## Included skills

| Skill | Purpose |
|---|---|
| `agent-scaffolding-init` | Orchestrates initialization — asks for output mode and invokes the appropriate skills |
| **Plain .md generators** | |
| `architecture-md-init` | Creates an `ARCHITECTURE.md` based on the project structure and architecture.md conventions |
| `testing-md-init` | Produces a concise `TESTING.md` with strategy, conventions, and real test commands |
| `deploy-md-init` | Produces a concise `DEPLOY.md` with exact deploy and rollback commands |
| `gotchas-md-init` | Creates a `GOTCHAS.md` — chronological log of pitfalls for cross-session agent learning |
| **Discoverable skill generators** | |
| `architecture-skill-init` | Generates a discoverable architecture skill for the target platform |
| `testing-skill-init` | Generates a discoverable testing skill for the target platform |
| `deploy-skill-init` | Generates a discoverable deploy skill for the target platform |
| `gotchas-skill-init` | Generates a discoverable gotchas skill for the target platform |
| **Always generated** | |
| `agents-md-init` | Creates an `AGENTS.md` from a bundled template — shared instructions for all coding agents |

## Repository structure

```text
.
├── skills/
│   ├── agent-scaffolding-init/
│   │   └── SKILL.md
│   ├── architecture-md-init/
│   │   └── SKILL.md
│   ├── architecture-skill-init/
│   │   └── SKILL.md
│   ├── testing-md-init/
│   │   └── SKILL.md
│   ├── testing-skill-init/
│   │   └── SKILL.md
│   ├── deploy-md-init/
│   │   └── SKILL.md
│   ├── deploy-skill-init/
│   │   └── SKILL.md
│   ├── gotchas-md-init/
│   │   └── SKILL.md
│   ├── gotchas-skill-init/
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
- **Composable**: independent skills that can be orchestrated together.
- **Agent-friendly**: deterministic structure that improves repeatability.

## Contributing

Contributions are welcome. Improvements to skill clarity, output quality, and robustness are encouraged.

## License

This project is licensed under the terms of the [LICENSE](./LICENSE) file.
