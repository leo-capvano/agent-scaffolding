---
name: "agent-scaffolding-init"
description: "Initialize a new agent scaffolding project with the necessary files and structure."
---
# Agent Scaffolding Initialization
This skill initializes a new agent scaffolding project by creating the necessary files and directory structure. It sets up a basic framework for building and developing agents, allowing developers to quickly get started with their projects.

# Workflow
1. **Explore the project structure**: use subagent to explore the project structure and understand the necessary files and directories.
2. **Use architecture-md-init skill**: invoke the `architecture-md-init` skill
3. **Use testing-md-init skill**: invoke the `testing-md-init` skill
4. **Use deploy-md-init skill**: invoke the `deploy-md-init` skill
5. **Use gotchas-md-init skill**: invoke the `gotchas-md-init` skill
6. **Use agents-md-init skill**: invoke the `agents-md-init` skill

For steps 1-5, spawn a subagent and give it the exploration results as initial context. The subagent will then execute the respective skill to create the necessary files and structure for the agent scaffolding project.
At the end spawn a final subagent to run the `agents-md-init` skill, which will create the AGENTS.md file based on the template and the actual context files present in the project.

Some of the scaffolding files may already exist; in this case the subagent will verify if their content is correct and update them if necessary.