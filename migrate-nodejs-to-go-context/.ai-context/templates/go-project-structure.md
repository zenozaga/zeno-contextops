# Standard Go Project Structure

This template provides the recommended project layout for migrated Node.js applications.

## 📁 Recommended Directory Structure

```
your-go-app/
├── cmd/                    # Main applications
│   └── server/
│       └── main.go        # Application entry point
├── internal/              # Private application code
│   ├── config/           # Configuration handling
│   ├── handlers/         # HTTP handlers (from Node.js routes)
│   ├── middleware/       # HTTP middleware
│   ├── models/          # Data models (from Node.js models)
│   ├── services/        # Business logic (from Node.js services)
│   └── repository/      # Data access layer
├── pkg/                  # Public library code
│   ├── logger/          # Logging utilities
│   ├── validator/       # Input validation
│   └── utils/           # Common utilities
├── deployments/         # Deployment configurations
│   ├── docker/         # Docker files
│   └── k8s/            # Kubernetes manifests
├── scripts/             # Build and deployment scripts
├── tests/              # Test files and test data
├── docs/               # Documentation
├── go.mod              # Go module definition
├── go.sum              # Go module checksums
├── Makefile           # Build automation
├── docker-compose.yml # Development environment
└── README.md          # Project documentation
```

## 📦 Package Organization Principles

### Internal Packages (`internal/`)
- **Purpose**: Private to your application, cannot be imported by other projects
- **Use for**: Core business logic, handlers, models specific to your app

### Public Packages (`pkg/`)
- **Purpose**: Can be imported by other projects if needed
- **Use for**: Reusable utilities, common interfaces, shared components

### Command Packages (`cmd/`)
- **Purpose**: Application entry points and main functions
- **Use for**: Different executables (server, CLI tools, workers)

## 🔄 Migration Mapping

### From Node.js Structure to Go

```
Node.js Structure          →    Go Structure
─────────────────────────       ──────────────────
routes/                   →    internal/handlers/
controllers/              →    internal/handlers/
services/                 →    internal/services/
models/                   →    internal/models/
middleware/               →    internal/middleware/
utils/                    →    pkg/utils/
config/                   →    internal/config/
tests/                    →    tests/
package.json              →    go.mod
app.js                    →    cmd/server/main.go
```

## 🎯 Package Naming Guidelines

### Follow Go Conventions
- **Lowercase, single words**: `user`, `auth`, `config`
- **No underscores or hyphens**: Use `userservice` not `user_service`
- **Descriptive names**: `repository` not `repo`, `handlers` not `hdlrs`

### Package Purpose Clarity
```go
// Good: Clear purpose
internal/handlers/     // HTTP request handlers
internal/services/     // Business logic services
internal/repository/   // Data access layer

// Avoid: Vague names
internal/common/       // Too generic
internal/stuff/        // Not descriptive
```

## 🚀 Best Practices

### Module Organization
- **One concept per package**: Don't mix unrelated functionality
- **Dependency direction**: Higher-level packages import lower-level ones
- **Circular dependencies**: Structure to avoid circular imports

### File Naming
```
handlers/
├── user.go           # User-related handlers
├── auth.go           # Authentication handlers
└── health.go         # Health check handlers

models/
├── user.go           # User model and methods
├── product.go        # Product model and methods
└── common.go         # Shared model utilities
```