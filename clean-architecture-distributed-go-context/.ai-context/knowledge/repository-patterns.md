# Repository Patterns

Repositories abstract data access and persistence:

## Interface-Implementation Separation
- Define repository interfaces in domain layer
- Implement repositories in infrastructure layer

## Hybrid Approach
- Use GORM for writes (transactions, relationships)
- Use pgxpool for reads (performance, custom queries)

## Dependency Injection
- Pass repository implementations via constructors
- Enables testing and flexibility

---

**Best Practices:**
- Avoid direct instantiation in domain layer
- Test repositories with real database (no mocks)
- Pin dependency versions in go.mod
