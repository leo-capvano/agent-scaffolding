---
name: "agent-scaffolding-init"
description: "Initialize a new agent scaffolding project with the necessary files and structure."
---

# Agent Scaffolding Initialization

This skill initializes a new agent scaffolding project by creating the necessary context files (or discoverable skills) and directory structure. It sets up a framework for coding agents to work effectively in the project.

# Workflow

1. **Explore the project structure**: use a subagent to explore the project structure and understand the codebase.

2. **Ask the user for output mode**:
   - "Do you want to generate **plain .md context files** or **discoverable skills**?"
   - If the user chooses **skills**, ask: "Which agent platform(s) are you targeting?"
     - OpenCode
     - Claude Code
     - Copilot CLI
     - Multiple (ask which combination)

3. **Dispatch to generators** based on the chosen mode:

   **Plain mode** — invoke these skills (via subagents, with exploration results as context):
   - `architecture-md-init`
   - `testing-md-init`
   - `deploy-md-init`
   - `gotchas-md-init`

   **Skills mode** — invoke these skills (via subagents, with exploration results AND platform choice as context):
   - `architecture-skill-init`
   - `testing-skill-init`
   - `deploy-skill-init`
   - `gotchas-skill-init`

4. **Generate AGENTS.md** (always last, in both modes):
   - Invoke `agents-md-init` via a subagent, passing the chosen mode (`plain` or `skills`) so it picks the correct template.

## Notes

- For steps 3-4, spawn a subagent for each skill and give it the exploration results as initial context.
- In skills mode, each subagent also receives the platform choice (e.g., `opencode`, `claude`, `copilot`, or a list for multiple).
- Some files/skills may already exist; subagents will verify content is correct and update if necessary.
- The AGENTS.md step runs last because it depends on knowing which context files or skills were generated.
