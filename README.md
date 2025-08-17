# zeno/contextops

**zeno/contextops** is a lightweight architecture for giving AI agents **structured context** inside a software project.  
It introduces a dedicated `.ai-context/` folder that contains all the knowledge, rules, and tasks an AI needs to work effectively — without improvisation.  

---

## ✨ Why zeno/contextops?

AI can generate code, but without context it often:  
- ignores project conventions,  
- re-implements patterns differently,  
- or produces incomplete scaffolding.  

With **zeno/contextops**, developers can save hours of work by:  
- Defining **rules** for style and error handling.  
- Providing **APIs** (OpenAPI/Swagger) as the single source of truth.  
- Using **templates** for scaffolding code and tests.  
- Writing **goals** as step-by-step tasks for the AI to execute.  
- Storing **knowledge** (technical notes, ADRs) for long-term reference.  

---

## 📂 Folder Structure

A typical `.ai-context/` folder looks like this:

