# Template Creation Rules

This file defines **rules for creating and maintaining .ai-context templates** in this repository.

## Template Structure Rules

1. **Folder Naming**: Use kebab-case with descriptive names ending in `-context`
   - ✅ `migrate-project-context/`
   - ✅ `new-api-development-context/`
   - ❌ `migration/` or `MigrateProject/`

2. **Required Structure**: Each template MUST contain:
   ```
   template-name-context/
   ├── .ai-context/
   │   ├── .aicontext.yaml      # Template configuration
   │   ├── rules.md             # Project-specific rules
   │   ├── knowledge/           # Domain knowledge
   │   ├── tasks/              # Executable tasks
   │   ├── templates/          # Code scaffolding
   │   ├── goals/              # Step-by-step objectives
   │   └── apis/               # API specifications
   └── README.md               # Template description & usage
   ```

3. **Template Documentation**: Each template's README.md MUST include:
   - Clear purpose and use case
   - Prerequisites and requirements
   - Step-by-step usage instructions
   - Expected outcomes
   - Examples of when to use this template

## Index Management Rules

4. **README.md Updates**: When adding a new template, ALWAYS update the root README.md:
   - Add template to the index with emoji and description
   - Include usage instructions
   - Maintain alphabetical order within categories

5. **Template Description**: Each template entry in index must have:
   - 🎯 **Purpose**: What problem it solves
   - 📋 **Use Cases**: When to use it
   - 🚀 **Quick Start**: How to apply it

## Quality Standards

6. **Content Requirements**:
   - All content MUST be in English
   - Use clear, actionable language
   - Provide specific, concrete examples
   - Include validation steps/criteria

7. **Testing**: Before adding a template:
   - Test the template in a real project scenario
   - Verify all tasks are executable
   - Ensure all referenced files exist

## Template Application Rules

8. **User Confirmation Mandatory**: Before applying ANY template to a user's project:
   - MUST ask user which template they want to apply
   - MUST ask explicit confirmation: "Do you want me to proceed with applying the [TEMPLATE_NAME] template?"
   - MUST wait for user approval before copying files
   - NEVER proceed without explicit user consent
   - This rule applies to ALL agents using this template hub

## Maintenance Rules

9. **Version Control**: 
   - Each template should have version info in its .aicontext.yaml
   - Document breaking changes in template README.md
   - Keep templates updated with current best practices
