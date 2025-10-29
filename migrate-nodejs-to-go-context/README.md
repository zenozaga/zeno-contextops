# Migrate Node.js to Go Context Template

**Purpose:** Systematically migrate Node.js applications to Go (Golang) to achieve significant performance improvements, higher speed, and optimized service execution through intelligent analysis-driven approach.

## 🎯 What This Template Solves

Migrating from Node.js to Go manually often results in suboptimal architectures, missed performance opportunities, and broken functionality. This template provides an intelligent, systematic approach that:

- **Analyzes your specific codebase** to create a customized migration plan
- **Preserves all functionality** while leveraging Go's performance advantages
- **Follows minimal coupling approach** to reduce migration risk
- **Provides step-by-step guidance** with validation at each phase
- **Creates comprehensive documentation** for the entire migration process

## 📋 Use Cases

- **REST API Migration**: Move Express.js, Fastify, or NestJS APIs to high-performance Go services
- **Entire Application Migration**: Migrate complete Node.js applications with all components  
- **Microservices Optimization**: Convert Node.js microservices to Go for better resource efficiency
- **Business Logic Enhancement**: Migrate core business logic to leverage Go's concurrency and performance
- **Performance-Critical Systems**: Move bottleneck services from Node.js to Go for speed improvements

## 📚 Prerequisites

- **Node.js source code** available for analysis (complete codebase access required)
- **Running Node.js application** to understand current functionality and performance
- **User availability** for architecture decisions and requirement gathering
- **Basic Go knowledge** helpful but not required (template provides guidance)
- **Testing capability** to validate migrated components

## 🚀 How to Use This Template

### For AI Agents:
1. **Ask confirmation** before applying template
2. **Copy .ai-context folder** to target project
3. **Run tasks sequentially** starting with `analyze_codebase.md`
4. **Follow user collaboration guidelines** in knowledge folder

### For Users:
1. **Confirm template application** when agent asks
2. **Provide complete information** about your Node.js project
3. **Review each migration step** before proceeding
4. **Test components** as they are migrated

## 📁 What's Included

### Rules (`rules.md`)
Comprehensive migration rules including architecture selection guidelines, minimal coupling approach, user collaboration standards, and quality assurance requirements. Ensures systematic migration without assumptions.

### Knowledge (`knowledge/`)
- **Node.js vs Go differences** and translation patterns
- **User collaboration guide** for effective agent-human partnership
- **Migration patterns** and best practices
- **Performance optimization** strategies specific to Go

### Tasks (`tasks/`)
Sequential workflow from analysis to deployment:
1. **Codebase Analysis** - Deep analysis of Node.js structure and dependencies  
2. **Requirements Gathering** - Architecture preferences and constraints collection
3. **Migration Planning** - Customized step-by-step migration strategy
4. **Component Migration** - Systematic migration following minimal coupling approach
5. **Validation & Testing** - Comprehensive testing and performance validation

### Templates (`templates/`)
- **Go project structure** templates and best practices
- **Docker and Kubernetes** deployment configurations
- **Docker Compose** for development testing with dependencies
- **Architecture abstractions** for logging, security, and testing patterns

### Goals (`goals/`)
Migration objectives with progress tracking system, success criteria definition, and comprehensive validation checkpoints to ensure successful completion.

### APIs (`apis/`)
Flexible framework for preserving API contracts while implementing Go-specific improvements and optimizations.

## ✅ Expected Outcomes

After applying this template, you should have:
- **Complete Go application** with identical functionality to Node.js version
- **Significant performance improvements** (typically 2-10x faster response times, 30-70% less memory)
- **Systematic documentation** of entire migration process in `.ai-context/progress/`
- **Comprehensive test suite** validating all migrated functionality
- **Production-ready deployment** configuration and infrastructure setup
- **Clear migration knowledge** for future reference and team onboarding

## 🔄 Next Steps

1. **Apply Template**: Copy `.ai-context/` folder to your Node.js project
2. **Start Analysis**: Execute `run task:1` to begin codebase analysis
3. **Engage Actively**: Participate in architecture and requirement discussions
4. **Follow Sequence**: Complete each task fully before proceeding to next
5. **Validate Continuously**: Test and approve each migration phase
6. **Monitor Progress**: Review `.ai-context/progress/` files regularly

## 📝 Example Projects

This template works well for projects like:
- **Express.js REST APIs** with database integration and authentication
- **NestJS enterprise applications** with complex business logic and microservice architecture  
- **Node.js backends** serving mobile apps or SPAs with performance requirements
- **Real-time services** using WebSockets or event-driven architectures
- **Data processing applications** that would benefit from Go's concurrency model

---

**Version:** 1.0.0  
**Last Updated:** October 28, 2025  
**Maintainer:** zeno-contextops template system