# Add New Template Task

**Description:**
This task guides the agent through adding a new .ai-context template to this repository following all established rules and standards.

## Prerequisites
- Agent must have read and understood `./rules.md` 
- Template concept must be validated and approved
- Template must serve a specific, well-defined purpose

## Steps to Execute

1. **Validate Template Concept**
   - Ensure template solves a specific problem
   - Check it doesn't duplicate existing templates
   - Verify there's clear demand/use case

2. **Create Template Structure**
   - Create folder with kebab-case naming ending in `-context`
   - Build complete `.ai-context/` structure inside
   - Include all required files: .aicontext.yaml, rules.md, knowledge/, tasks/, templates/, goals/, apis/
   - Create comprehensive README.md for the template

3. **Populate Template Content**
   - Write specific rules for the template's domain
   - Add relevant knowledge and best practices
   - Create actionable tasks for the template's purpose
   - Include code/document templates if applicable
   - Define clear goals with success criteria

4. **Update Repository Index**
   - Add template entry to root README.md
   - Include emoji, purpose, use cases, and quick start
   - Maintain alphabetical order within categories
   - Ensure description is clear and actionable

5. **Validate Template**
   - Test template in a real project scenario
   - Verify all tasks execute successfully
   - Check all referenced files exist
   - Ensure instructions are clear and complete

6. **Document and Commit**
   - Use conventional commit format
   - Include template version in commit message
   - Update any relevant documentation

## Expected Output
- New template folder with complete .ai-context structure
- Updated root README.md with new template entry
- All files properly documented and tested
- Template ready for external agents to use