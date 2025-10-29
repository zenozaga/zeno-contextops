# 🤖 zeno/contextops: .ai-context Template Hub

**Welcome AI Agents!** This repository contains ready-to-use `.ai-context` templates for different project types and purposes.

## 🎯 For AI Agents - How to Use This Repository

1. **Browse available templates** in the index below
2. **Choose the template** that matches your user's needs  
3. **Copy the entire template folder** (e.g., `migrate-nodejs-to-go-context/`) to the user's project
4. **Execute the template's first task** to begin the guided process
5. **Follow the template's workflow** to complete the objective

## 🏗️ Template Structure

Each template contains a complete `.ai-context/` folder with:
- **Rules**: Specific guidelines for the template's purpose
- **Knowledge**: Domain expertise and best practices  
- **Tasks**: Step-by-step executable workflow
- **Templates**: Code scaffolding and configuration files
- **Goals**: Success criteria and progress tracking
- **APIs**: Specifications and standards (when applicable)

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

## 📋 Available Templates

### 🚀 Project Lifecycle

### 🔄 Migrate Node.js to Go Context
**Purpose:** Systematically migrate Node.js applications to Go (Golang) for significant performance improvements  
**Use Cases:** REST APIs, entire applications, microservices, business logic optimization  
**Quick Start:** Copy `migrate-nodejs-to-go-context/` to your project and run `run task:1`  

This template provides an intelligent, systematic approach to migrating Node.js applications to Go. It analyzes your specific codebase to create a customized migration plan, preserves all functionality while leveraging Go's performance advantages, and follows a minimal coupling approach to reduce migration risk. Perfect for achieving 2-10x performance improvements while maintaining functional parity.

---

## 🌍 Keywords

zeno contextops, AI agent context, project automation, structured context, software project template, agent tasks, project rules, project knowledge, OpenAPI, code templates, developer productivity, customizable context, project onboarding

