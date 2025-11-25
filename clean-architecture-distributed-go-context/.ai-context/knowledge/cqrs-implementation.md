# CQRS Implementation

Command Query Responsibility Segregation (CQRS) separates write and read operations:

## Commands (Write)
- Go through orchestrators/application services
- Modify state, publish events
- Use GORM for transactional writes

## Queries (Read)
- Go directly to repositories
- Read-only, high-performance
- Use pgxpool for optimized queries

## Benefits
- Scalability: Separate scaling for reads/writes
- Performance: Optimized queries, batch loading
- Maintainability: Clear separation of concerns

---

**Best Practices:**
- No business logic in queries
- Use DTOs for query results
- Benchmark critical queries
