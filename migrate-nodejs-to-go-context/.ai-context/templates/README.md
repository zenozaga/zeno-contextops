# Code and Infrastructure Templates

This folder contains templates for Go project structure, deployment configurations, and architecture patterns.

## 🏗️ Available Templates

### Go Project Structure
- `go-project-structure.md` - Standard Go project layout and organization
- `go-mod-template.txt` - Basic go.mod file template
- `main-go-template.go` - Application entry point template

### Deployment Infrastructure  
- `dockerfile-template` - Multi-stage Docker build for Go applications
- `docker-compose-dev.yml` - Development environment with dependencies
- `kubernetes-manifests/` - K8s deployment, service, and ingress templates

### Architecture Abstractions
- `logging-interface.go` - Structured logging interface template
- `config-management.go` - Configuration handling patterns
- `error-handling.go` - Go-idiomatic error handling patterns
- `testing-template.go` - Test structure and patterns

### CI/CD Templates
- `github-actions.yml` - Go build, test, and deploy pipeline
- `makefile-template` - Common Go project tasks

## 📋 Usage Instructions

1. **Copy relevant templates** to your migrated Go project
2. **Customize placeholders** with your project-specific values
3. **Adapt patterns** to match your architecture decisions
4. **Test templates** work with your specific requirements

## 🎯 Template Categories

- **Structure**: Project organization and module layout
- **Infrastructure**: Deployment and containerization  
- **Patterns**: Code organization and best practices
- **Testing**: Test structure and validation approaches