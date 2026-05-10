---
name: architecture-md-skill
description: Use when is asked to create the ARCHITECTURE.md file
---
# Skill ARCHITECTURE.md skill
This skill allows to create tha ARCHITECTURE.md file

## 1. Fetch the ARCHITECTURE.md example
Fetch from web the official example of the ARCHITECTURE.md file at https://architecture.md/.
If you don't have a native webfetch tool use curl.

## 2. Analyze the project
Analyze the current project to collect useful information that can be written into ARCHITECTURE.md file. 
Use subagents (parallel when possible) to explore the codebase. 

# 3. Create ARCHITECTURE.md file
Create the file by taking the one you fetched as an example, be concise. Take the fetched file as an example, write only meaningful data.
If you are not sure about a chapter or an information, analyze the codebase using subagents.
