# Analyze Node.js Codebase

**Description:**
Perform comprehensive analysis of the existing Node.js project to understand structure, dependencies, and coupling relationships. This analysis forms the foundation for creating a customized migration plan.

## Step 0: Template Application Confirmation
**BEFORE starting analysis, you MUST:**
1. **Ask user confirmation**: "Do you want me to proceed with applying the migrate-nodejs-to-go-context template?"
2. **Wait for explicit approval** before copying any files
3. **Copy the entire .ai-context folder** to the target project
4. **Only then proceed** with the analysis below

## Objectives
- Map project structure and identify all components
- Analyze dependency relationships and coupling strength
- Identify migration complexity for each component
- Document current architecture patterns and data flows

## Prerequisites
- Access to complete Node.js source code
- Project runs successfully in current environment
- Basic documentation or understanding of project functionality

## Analysis Steps

### 1. Project Structure Mapping
- **Identify entry points**: Main application files, server startup scripts
- **Map directory structure**: Organize components by type (routes, models, services, etc.)
- **Document file relationships**: Which files import/require which others
- **Identify configuration files**: Environment configs, package.json, Docker files

### 2. Dependency Analysis
- **External dependencies**: All npm packages and their purposes
- **Internal dependencies**: How project modules depend on each other
- **Database dependencies**: Schema, ORM usage, query patterns
- **External service dependencies**: APIs, message queues, file systems

### 3. Coupling Assessment
Create coupling matrix to determine migration order:
- **Zero coupling**: Standalone utilities, constants, pure functions
- **Low coupling**: Simple models, basic services with single dependencies
- **Medium coupling**: Complex services with multiple dependencies
- **High coupling**: Core orchestration, main application flow

### 4. Architecture Pattern Identification
- **Current patterns**: MVC, microservices, serverless, etc.
- **Framework usage**: Express.js, Fastify, NestJS patterns
- **Data flow patterns**: Request/response cycles, event handling
- **Authentication/authorization flows**: How security is implemented

## Analysis Deliverables

### 1. Component Inventory
```markdown
# Component Analysis Results

## Zero Coupling Components (Migrate First)
- utils/helpers.js - Pure utility functions
- constants/index.js - Application constants
- types/definitions.js - Type definitions

## Low Coupling Components  
- models/User.js - Basic data model
- services/validation.js - Input validation service

## Medium Coupling Components
- services/auth.js - Authentication service
- middleware/security.js - Security middleware

## High Coupling Components (Migrate Last)
- routes/index.js - Main route orchestration
- app.js - Application entry point
```

### 2. Dependency Map
```markdown
# Dependency Relationships

## External Dependencies
- express: Web framework
- mongoose: MongoDB ORM
- jsonwebtoken: JWT handling
- bcrypt: Password hashing

## Internal Dependencies
app.js → routes/index.js → controllers/* → services/* → models/*
```

### 3. Migration Complexity Assessment
```markdown
# Migration Complexity

## Low Complexity (Direct Translation)
- Pure functions and utilities
- Simple data structures
- Basic CRUD operations

## Medium Complexity (Pattern Adaptation)
- Middleware chains
- Authentication flows
- Database integration

## High Complexity (Architectural Changes)
- Async/await patterns to Go concurrency
- Event-driven architecture
- Complex business orchestration
```

## Expected Outcomes

1. **Clear migration sequence** based on coupling analysis
2. **Identified challenges** and potential blockers
3. **Component complexity ratings** for effort estimation  
4. **Architecture understanding** for Go target design
5. **Foundation for migration planning** in next task

## Progress Tracking

Create `/.ai-context/progress/01-codebase-analysis.md` with:
- Complete component inventory
- Dependency relationships
- Coupling matrix
- Complexity assessment
- Recommended migration sequence

## Success Criteria

- [ ] All project components identified and categorized
- [ ] Dependency relationships mapped completely
- [ ] Coupling analysis completed with migration order
- [ ] Architecture patterns documented
- [ ] Complexity assessment completed
- [ ] Progress documented in tracking file
- [ ] User review and confirmation of analysis accuracy