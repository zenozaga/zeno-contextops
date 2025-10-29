# Template Categories and Patterns

This document defines common template categories and their typical patterns.

## Template Categories

### 1. Project Lifecycle Templates
- **new-project-context**: Starting fresh projects from scratch
- **migrate-project-context**: Moving existing projects to new technologies  
- **modernize-legacy-context**: Updating old codebases with modern practices
- **project-audit-context**: Analyzing and documenting existing projects

### 2. Development Domain Templates
- **api-development-context**: Building REST/GraphQL APIs
- **frontend-development-context**: Web/mobile UI development
- **data-pipeline-context**: ETL and data processing workflows
- **microservices-context**: Distributed system architecture

### 3. Operational Templates  
- **ci-cd-setup-context**: Deployment pipeline configuration
- **monitoring-setup-context**: Observability and alerting
- **security-audit-context**: Security analysis and hardening
- **performance-optimization-context**: Speed and efficiency improvements

### 4. Technology-Specific Templates
- **react-project-context**: React-specific development patterns
- **node-backend-context**: Node.js server development
- **python-ml-context**: Machine learning project structure
- **docker-containerization-context**: Container deployment setup

## Common Template Patterns

### Setup Task Pattern
All templates should include:
1. **Environment Analysis**: Understand current state
2. **Requirements Gathering**: What needs to be achieved  
3. **Planning**: Create step-by-step plan
4. **Execution**: Implement changes
5. **Validation**: Verify success

### Knowledge Structure Pattern
- **Overview**: High-level concepts and principles
- **Best Practices**: Proven approaches and patterns
- **Common Pitfalls**: What to avoid and why  
- **Resources**: Links to documentation and tools
- **Examples**: Real-world implementation samples

### Rules Structure Pattern
- **Mandatory Rules**: Non-negotiable requirements
- **Recommended Practices**: Strongly suggested approaches
- **Style Guidelines**: Code formatting and naming
- **Quality Standards**: Testing and documentation requirements
- **Process Rules**: Workflow and collaboration guidelines

### Task Workflow Pattern
1. **Initialization**: Setup and preparation tasks
2. **Analysis**: Understanding and assessment tasks  
3. **Planning**: Strategy and design tasks
4. **Implementation**: Execution and building tasks
5. **Validation**: Testing and verification tasks
6. **Documentation**: Recording and sharing tasks

## Template Naming Conventions

### Folder Names
- Use kebab-case: `migrate-project-context`
- End with `-context`: `api-development-context`
- Be descriptive: `legacy-modernization-context`
- Avoid abbreviations: `continuous-integration-context` not `ci-context`

### File Names
- Use lowercase with hyphens: `setup-project.md`
- Be action-oriented: `analyze-codebase.md` 
- Include purpose: `validate-migration.md`
- Group by type: `template-dockerfile.hbs`

### Task Names
- Start with verb: `analyze`, `create`, `validate`
- Be specific: `analyze-dependencies` not `analyze`
- Use present tense: `setup-environment.md`
- Indicate sequence: `01-initialize.md`, `02-analyze.md`