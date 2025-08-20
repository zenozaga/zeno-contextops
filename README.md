# zeno/contextops

**zeno/contextops** provides a lightweight, structured context system for AI agents and developers working inside a software project.

---

## What is zeno/contextops?

This architecture introduces a dedicated `.ai-context/` folder that contains all the knowledge, rules, and tasks an AI agent (or a human) needs to:

- Understand project conventions and best practices
- Access technical documentation and APIs
- Use code and test templates
- Follow step-by-step goals
- Execute or list project-specific tasks

By providing this context, agents can:
- Avoid improvisation
- Adhere to project standards
- Save developer time

---

## How does it work?


The `.ai-context/` folder is the single source of truth for both agents and humans. It is organized as follows (but is highly customizable):


```text
.ai-context/
	tasks/                # Agent-executable tasks (README.md is for instructions only)
		project_review.md   # Example: reviews and describes the project
		...                 # Add more tasks as needed
		README.md           # Instructions for the agent (not a task)
	rules.md              # Coding conventions and error handling
	knowledge/            # Technical notes and ADRs
	apis/                 # OpenAPI/Swagger contracts
	templates/            # Code and test scaffolding templates
	goals/                # Step-by-step executable goals
	.aicontext.yaml       # Main context configuration
```

> **Note:** This structure is highly customizable. You can add more folders (e.g., `analysis/`, `scripts/`, `datasets/`, etc.) to fit your project's needs. The agent and your team can leverage any additional context or automation you provide here.

---

## How to use tasks

- All files in `.ai-context/tasks` (except `README.md`) are considered executable tasks for the agent.
- To list or run a task, use the format:

	```
	run task:<number>
	```

	For example, to run the first task:

	```
	run task:1
	```

- The `README.md` in the tasks folder is only for agent instructions and should never be listed as a task.

---

## For agents and humans

- **Agents**: Use `.ai-context/` to understand the project, follow rules, and execute tasks as described above.
- **Humans**: Use this structure to document, automate, and standardize your project for both AI and team members.

