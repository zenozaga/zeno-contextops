# Node.js to Go Migration Rules

These rules guide the systematic migration process from Node.js to Go applications.

## 🎯 Architecture Selection Rules

### User Consultation Required
- **ALWAYS ask user for target Go architecture** before starting migration
- **Gather additional requirements** and customization preferences upfront
- **Document architecture choice** and reasoning in progress tracking
- **Validate architecture alignment** with user expectations before proceeding

### Architecture-Driven Migration Order
- **Analyze coupling dependencies** before determining migration sequence
- **Start with chosen architecture** as the foundation structure
- **Detect migration priorities** based on architectural patterns:
  - **Microservices**: Migrate services by dependency order
  - **Monolith**: Migrate by layers (data → business → presentation)
  - **Serverless**: Migrate by function isolation and event dependencies
  - **API-focused**: Migrate by endpoint complexity and interdependence

## 🔄 Systematic Migration Approach

### Minimal Coupling First Principle
- **Identify isolated components**: Types, utilities, configuration, constants
- **Map dependency relationships**: Create coupling matrix for migration order
- **Start with zero-dependency modules**: Pure functions, data structures, helpers
- **Progress to low-coupling components**: Single-dependency modules, adapters
- **Handle complex coupling last**: Core business logic, orchestration, main application flow

### Progressive Complexity Rule
- **Phase 1**: Standalone utilities and types (no external dependencies)
- **Phase 2**: Data models and simple business objects (minimal coupling)
- **Phase 3**: Service layers and middleware (moderate coupling)  
- **Phase 4**: API handlers and controllers (higher coupling)
- **Phase 5**: Application orchestration and main entry points (highest coupling)

## 👥 User Collaboration Standards

### Information Gathering Rules
- **Complete information requirement**: Users must provide ALL relevant details upfront
- **No assumption making**: Agent cannot guess missing information - must ask
- **Critical details checklist**: Architecture preferences, performance requirements, existing constraints, deployment targets
- **Active user participation**: Users responsible for providing context agent cannot infer

### User Education Requirements  
- **Process explanation**: Clearly explain each phase before execution
- **Decision rationale**: Explain why certain components migrate in specific order
- **Risk communication**: Inform users of potential issues and mitigation strategies
- **Progress transparency**: Regular updates on migration status and next steps

## 🧪 Quality Assurance Rules

### Test-Driven Migration
- **Create tests AFTER each step**: Every migrated component must have corresponding tests
- **Preserve functionality**: Migrated code must maintain identical behavior to Node.js version
- **Performance validation**: Measure and compare performance improvements at each phase
- **Regression prevention**: Automated testing to catch breaking changes during migration

### Progress Documentation
- **Document everything**: Every decision, change, and outcome in `.ai-context/progress/`
- **Track component status**: Clear status for each migrated component (planned/in-progress/completed/validated)
- **Record issues and solutions**: Document problems encountered and how they were resolved
- **Maintain rollback information**: Keep track of changes for potential rollback scenarios

## ⚡ Performance Optimization Rules

### Go Best Practices Enforcement
- **Idiomatic Go code**: Follow Go conventions, not direct Node.js translations
- **Leverage Go strengths**: Use goroutines, channels, and Go-specific patterns appropriately
- **Memory efficiency**: Optimize for Go's garbage collector and memory model
- **Concurrency patterns**: Replace Node.js async patterns with Go concurrency where beneficial

### Validation Requirements
- **Benchmark comparisons**: Before/after performance measurements required
- **Functionality parity**: All features must work identically to original Node.js version
- **Error handling**: Implement Go-idiomatic error handling, replacing Node.js error patterns
- **Resource optimization**: Ensure migrated code uses Go's efficiency advantages

## 🚫 Anti-Patterns to Avoid

### Migration Don'ts
- **No direct translation**: Don't convert Node.js code line-by-line to Go
- **No skipping analysis**: Never start migration without thorough codebase understanding
- **No assumption-based decisions**: Always confirm architecture and requirements with user
- **No untested migration**: Never proceed to next component without validating current one
- **No documentation shortcuts**: Always maintain complete progress records