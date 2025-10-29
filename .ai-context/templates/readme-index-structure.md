# Root README.md Index Template

Use this template structure when updating the root README.md index with new templates.

## Template Entry Format

For each new template, add an entry following this format:

```markdown
### 🔄 {{TEMPLATE_NAME}}
**Purpose:** {{ONE_LINE_PURPOSE}}  
**Use Cases:** {{USE_CASE_1}}, {{USE_CASE_2}}, {{USE_CASE_3}}  
**Quick Start:** Copy `{{FOLDER_NAME}}/` to your project and run `run task:1`  

{{DETAILED_DESCRIPTION_PARAGRAPH}}
```

## Categories Structure

Organize templates in the index using these categories:

```markdown
## 📋 Available Templates

### 🚀 Project Lifecycle
Templates for different stages of project development.

### 🛠️ Development Domains  
Templates focused on specific technical domains.

### ⚙️ Operations & Infrastructure
Templates for deployment, monitoring, and maintenance.

### 🎯 Technology-Specific
Templates tailored for specific frameworks or languages.
```

## Complete Index Section Template

```markdown
## 📋 Available Templates

### 🚀 Project Lifecycle

### 🔄 Migrate Project Context
**Purpose:** Systematically migrate existing projects to new technologies or architectures  
**Use Cases:** Legacy system modernization, framework upgrades, cloud migration  
**Quick Start:** Copy `migrate-project-context/` to your project and run `run task:1`  

This template provides a structured approach to project migration with analysis tools, migration planning, validation frameworks, and rollback strategies. Perfect for moving from legacy systems to modern architectures while minimizing risk and downtime.

### 🆕 New Project Context  
**Purpose:** Bootstrap new projects with best practices and modern tooling  
**Use Cases:** Greenfield development, POC creation, starter templates  
**Quick Start:** Copy `new-project-context/` to your project and run `run task:1`  

Accelerate new project setup with pre-configured development environments, testing frameworks, CI/CD pipelines, and documentation templates. Includes decision trees for architecture choices and technology selection guidance.

### 🛠️ Development Domains

### 🌐 API Development Context
**Purpose:** Build robust, scalable APIs with comprehensive testing and documentation  
**Use Cases:** REST APIs, GraphQL services, microservice development  
**Quick Start:** Copy `api-development-context/` to your project and run `run task:1`  

Complete API development workflow including OpenAPI specification, automated testing, security implementation, performance optimization, and deployment strategies. Supports both REST and GraphQL paradigms.

### ⚙️ Operations & Infrastructure  

### 🚀 CI/CD Setup Context
**Purpose:** Implement automated deployment pipelines and quality gates  
**Use Cases:** DevOps automation, deployment standardization, quality assurance  
**Quick Start:** Copy `ci-cd-setup-context/` to your project and run `run task:1`  

End-to-end CI/CD implementation with multi-environment support, automated testing, security scanning, and deployment strategies. Includes templates for GitHub Actions, GitLab CI, and Jenkins.
```

## Index Maintenance Rules

1. **Alphabetical Order**: Within each category, maintain alphabetical order
2. **Consistent Format**: Use exact same format for all entries  
3. **Clear Descriptions**: One-line purpose + detailed paragraph
4. **Action-Oriented**: Use active verbs in descriptions
5. **Benefit-Focused**: Emphasize what users gain from the template