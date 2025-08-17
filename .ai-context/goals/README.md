# Goals

Goals are **step-by-step executable tasks** for the AI agent.

- Format: `.goal.yaml` (YAML, 2 spaces indentation, no tabs).
- Each goal must define: `name`, optional `description`, `inputs`, `steps`, and optional `constraints`.
- Tools referenced in steps must exist (CLI, MCP tool, or project script).

**Example:** `create-adapter.goal.yaml` (see file in this folder).
