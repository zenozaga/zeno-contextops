# Create Template Through Questions

**Description:**
This task will gather all necessary information through 10 strategic questions to create a complete .ai-context template. The agent will ask questions one by one, wait for responses, and use the information to build a comprehensive template following all established standards.

## How It Works

The agent will ask 10 specific questions, **one at a time**, waiting for your complete response before proceeding to the next question. After all questions are answered, the agent will create the entire template structure automatically.

## The 10 Questions Process

### Question 1: Template Identity
**"What is the name and primary purpose of this template?"**
- Expected: Template name (kebab-case) and one-sentence purpose
- Example: "migrate-legacy-java-context - Systematically migrate legacy Java applications to modern Spring Boot architecture"

### Question 2: Problem Definition  
**"What specific problem does this template solve and why is it important?"**
- Expected: Detailed problem description, pain points, current challenges
- This will populate the "What This Template Solves" section

### Question 3: Target Use Cases
**"What are the 3-5 main use cases where someone would need this template?"**  
- Expected: Specific scenarios and situations
- These become the use cases section and help define scope

### Question 4: Prerequisites & Requirements
**"What should users have in place before using this template?"**
- Expected: Technical requirements, knowledge prerequisites, tools needed
- This defines the prerequisites section and setup validation

### Question 5: Core Rules & Standards
**"What are the essential rules, standards, or conventions this template should enforce?"**
- Expected: Coding standards, process rules, quality gates, mandatory practices
- This populates the rules.md file content

### Question 6: Domain Knowledge & Context
**"What key knowledge, concepts, or background information do users need to understand?"**
- Expected: Domain concepts, best practices, common patterns, important references
- This fills the knowledge/ folder content

### Question 7: Essential Tasks & Workflow
**"What are the main tasks users need to accomplish, and in what order?"**
- Expected: Step-by-step workflow, key activities, validation points
- This creates the tasks/ folder structure and content

### Question 8: Templates & Scaffolding  
**"What code templates, configuration files, or document templates should be included?"**
- Expected: File templates, code snippets, configuration examples
- This populates the templates/ folder

### Question 9: Goals & Success Criteria
**"What are the main goals and how do you measure successful completion?"**
- Expected: Objectives, success metrics, validation criteria
- This creates the goals/ folder and completion validation

### Question 10: APIs & Specifications
**"Are there any APIs, specifications, or external contracts that are important for this template?"**
- Expected: API specs, external dependencies, integration requirements  
- This populates the apis/ folder (can be empty if not applicable)

## Expected Output

After all 10 questions are answered, the agent will automatically create:

1. **Complete Template Folder Structure**
   - `{template-name}-context/` folder
   - Full `.ai-context/` hierarchy inside
   - All required files and documentation

2. **Populated Content**
   - `.aicontext.yaml` with complete configuration
   - `rules.md` with specific rules from your answers
   - `knowledge/` folder with domain information
   - `tasks/` folder with workflow tasks
   - `templates/` folder with code/document templates
   - `goals/` folder with objectives and criteria
   - `apis/` folder with specifications (if applicable)
   - Root `README.md` with comprehensive documentation

3. **Updated Repository Index**
   - Root `README.md` updated with new template entry
   - Proper categorization and description
   - Alphabetical ordering maintained

4. **Quality Validation**
   - All content follows established standards
   - Templates tested for completeness
   - Documentation comprehensive and clear

## Instructions for Agent

1. **Ask questions sequentially** - Wait for complete answer before next question
2. **Clarify when needed** - Ask follow-up questions if answers are unclear
3. **Take detailed notes** - Capture all information for template creation
4. **Validate understanding** - Summarize key points before proceeding to creation
5. **Create systematically** - Follow template creation rules and patterns
6. **Test completeness** - Ensure all generated content is functional and complete