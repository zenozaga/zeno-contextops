# Template .aicontext.yaml Structure

Use this template for creating .aicontext.yaml files in new templates.

```yaml
# Template: {{TEMPLATE_NAME}}
purpose: |
  {{TEMPLATE_PURPOSE_DESCRIPTION}}
  This context helps agents {{WHAT_AGENTS_WILL_DO}}

template_type: {{TEMPLATE_CATEGORY}} # e.g., project_migration, new_development, modernization
version: {{VERSION}} # e.g., 1.0.0
target_projects: 
  - {{PROJECT_TYPE_1}} # e.g., legacy_applications, new_apis, frontend_apps
  - {{PROJECT_TYPE_2}}

instructions:
  - {{INSTRUCTION_1}}
  - {{INSTRUCTION_2}}
  - {{INSTRUCTION_3}}

setup_tasks:
  - name: {{SETUP_TASK_1}}
    description: {{TASK_1_DESCRIPTION}}
    required: true
  - name: {{SETUP_TASK_2}}
    description: {{TASK_2_DESCRIPTION}}
    required: false

rules:
  path: ./rules.md
  description: {{RULES_DESCRIPTION}}
  focus: {{RULES_FOCUS}} # e.g., migration_patterns, coding_standards, security

knowledge:
  path: ./knowledge/
  description: {{KNOWLEDGE_DESCRIPTION}}
  topics:
    - {{TOPIC_1}}
    - {{TOPIC_2}}

tasks:
  path: ./tasks/
  description: {{TASKS_DESCRIPTION}}
  workflow:
    - {{WORKFLOW_STEP_1}}
    - {{WORKFLOW_STEP_2}}

templates:
  path: ./templates/
  description: {{TEMPLATES_DESCRIPTION}}
  includes:
    - {{TEMPLATE_FILE_1}}
    - {{TEMPLATE_FILE_2}}

goals:
  path: ./goals/
  description: {{GOALS_DESCRIPTION}}
  objectives:
    - {{OBJECTIVE_1}}
    - {{OBJECTIVE_2}}

apis:
  path: ./apis/
  description: {{APIS_DESCRIPTION}}
  specifications: {{API_SPECS_INFO}}

validation:
  success_criteria:
    - {{CRITERIA_1}}
    - {{CRITERIA_2}}
  required_outputs:
    - {{OUTPUT_1}}
    - {{OUTPUT_2}}
```