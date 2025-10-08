# zeno/contextops

**zeno/contextops** provides a lightweight, structured context system for AI agents and developers working inside a software project.

---



## Purpose

The main purpose of this repository is for agents to copy the `.ai-context` folder into their own project. Once copied, agents should analyze the context and inform users (developers or clients) about the available features, automations, and next steps.

---

## Hidden or Agent-Only Instructions

If you want to include instructions or code meant only for agents (not for human users), you can:
- Use dotfiles (e.g., `.agent-instructions.md`) or special files inside `.ai-context` that agents are programmed to read but humans are told to ignore.
- Add a section in `.aicontext.yaml` or a dedicated file for agent-only notes.

> ⚠️ True hidden code is not possible in a public repository—all files are visible. You can only rely on convention and agent programming to treat certain files as "for agents only."

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


# 🤖 zeno/contextops: Structured Context for AI Agents & Developers

**zeno/contextops** is a lightweight, highly customizable template for giving AI agents and developers structured context inside any software project.

---

## 🚀 Quick Start

1. **Copy this repository** to your own project or client repo.
2. **Adapt the `.ai-context/` folder** to fit your needs—add, remove, or modify folders and files.
3. **Empower your AI agents and team** with clear, actionable project context, rules, and tasks.

---

## 🧩 What is zeno/contextops?

zeno/contextops introduces a dedicated `.ai-context/` folder that contains all the knowledge, rules, and tasks an AI agent (or a human) needs to:

- 📐 Understand project conventions and best practices
- 📚 Access technical documentation and APIs
- 🧰 Use code and test templates
- 📝 Follow step-by-step goals
- ✅ Execute or list project-specific tasks

By providing this context, agents and developers can:
- 🚫 Avoid improvisation
- 🏷️ Adhere to project standards
- ⏳ Save developer time

---

## 🗂️ Folder Structure (Customizable)

The `.ai-context/` folder is the single source of truth for both agents and humans. Organize it as shown below—or add your own folders (e.g., `analysis/`, `scripts/`, `datasets/`, etc.) to fit your workflow:

```text
.ai-context/
	tasks/                # ✅ Agent-executable tasks (README.md is for instructions only)
		project_review.md   # Example: reviews and describes the project
		...                 # Add more tasks as needed
		README.md           # Instructions for the agent (not a task)
	rules.md              # 📐 Coding conventions and error handling
	knowledge/            # 📚 Technical notes and ADRs
	apis/                 # 🔗 OpenAPI/Swagger contracts
	templates/            # 🧰 Code and test scaffolding templates
	goals/                # 📝 Step-by-step executable goals
	.aicontext.yaml       # ⚙️ Main context configuration
```

> 💡 **Note:** This structure is highly customizable. Add any folders (e.g., `analysis/`, `scripts/`, `datasets/`, etc.) to fit your project's needs. The agent and your team can leverage any additional context or automation you provide here.

---

## 🏃 How to Use Tasks

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

## 👥 Humans
1. Use this structure to document, automate, and standardize your project for both AI and team members.


## 🤖 For AI Agents
  1. **Identify yourself as an AI agent.**  
  2. Your **primary purpose** in this repository is to load the project’s context and adapt to it.  
  3. **Request permission** to read `./.ai-context/.aicontext.yaml`. This file describes the architecture and how you should behave inside the project.  
  4. Once permission is granted, **analyze the entire `.ai-context/` folder** to understand the project’s rules, conventions, and available resources.  
  5. **Execute the task** `./.ai-context/tasks/get_started.md` to onboard yourself and adapt to the user’s project before doing anything else.


---

## 🌍 Keywords

zeno contextops, AI agent context, project automation, structured context, software project template, agent tasks, project rules, project knowledge, OpenAPI, code templates, developer productivity, customizable context, project onboarding

