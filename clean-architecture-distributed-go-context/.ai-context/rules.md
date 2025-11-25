# Clean Architecture Rules for Distributed Go Systems

This document defines **mandatory architectural rules** for projects generated with this template.

## 🏛️ Layer Dependency Rules (CRITICAL)

### Rule 1: Dependency Direction
**Dependencies MUST flow inward only:**
```
Interface Layer → Application Layer → Domain Layer
Infrastructure Layer → Application Layer → Domain Layer
```

**❌ VIOLATIONS:**
- Domain importing from Application/Infrastructure/Interface
- Application importing from Infrastructure/Interface
- Infrastructure importing from Interface

**✅ CORRECT:**
- All layers can import from Domain
- Application can import from Domain only
- Infrastructure implements interfaces defined in Domain/Application
- Interface layer orchestrates through Application layer

### Rule 2: Domain Layer Purity
**Domain layer MUST be free of external dependencies:**
- ✅ Standard library only (except UUID generation)
- ✅ Domain logic, aggregates, entities, value objects
- ✅ Repository interfaces (no implementations)
- ✅ Domain events (no event publishers)
- ❌ NO database drivers (GORM, pgx, etc.)
- ❌ NO HTTP frameworks (Fiber, Echo, etc.)
- ❌ NO messaging clients (NATS, Kafka, etc.)
- ❌ NO cache clients (Redis, Memcached, etc.)

### Rule 3: Repository Pattern
**Repositories MUST follow interface-implementation separation:**
- ✅ Repository **interface** defined in Domain layer
- ✅ Repository **implementation** in Infrastructure layer
- ✅ Use Dependency Injection (constructor injection)
- ❌ NO direct instantiation of repositories in domain

## 🎯 Event-Driven Architecture Rules

### Rule 4: Event Classification
**Distinguish between Domain Events and Integration Events:**

**Domain Events (Best-Effort):**
- Published directly via EventBus
- Use for: WebSocket updates, real-time analytics, internal notifications
- Delivery: At-most-once (fire-and-forget)
- Latency: <10ms

**Integration Events (Guaranteed Delivery):**
- Published via Outbox Pattern (transactional)
- Use for: Automations, webhooks, billing, critical business processes
- Delivery: At-least-once with retries
- Latency: ~50-100ms

### Rule 5: Outbox Pattern Mandatory
**ALL Integration Events MUST use Outbox Pattern:**
```go
// ❌ WRONG: Direct publish (can lose events)
eventPublisher.Publish(event)

// ✅ CORRECT: Outbox Pattern (guaranteed delivery)
outboxPublisher.Publish(ctx, tx, event) // Transactional
tx.Commit() // Event saved in same transaction as domain data
```

### Rule 6: Event Naming Convention
**Events MUST follow this format:**
- Pattern: `{aggregate}.{action}` (lowercase, dot-separated)
- Examples: `order.created`, `payment.processed`, `user.registered`
- ❌ Avoid: `OrderCreated`, `order_created`, `create-order`

## 🔄 CQRS Rules

### Rule 7: Command/Query Separation
**Write and read operations MUST be separated:**

**Commands (Write):**
- Go through Orchestrators/Application Services
- Modify state and publish events
- Use GORM for transactional writes
- Return minimal data (ID, timestamp)

**Queries (Read):**
- Go directly to Repositories
- Read-only operations
- Use pgxpool for high-performance reads
- Return rich data structures

### Rule 8: No Business Logic in Queries
**Query operations MUST NOT contain business logic:**
- ✅ Filtering, sorting, pagination
- ✅ Data transformation (mapping to DTOs)
- ❌ State changes
- ❌ Event publishing
- ❌ Domain validation

## 🗄️ Repository Pattern Rules

### Rule 9: Hybrid Repository Pattern
**Use GORM for writes, pgxpool for performance-critical reads:**

**GORM (Writes):**
```go
func (r *OrderRepository) Create(ctx context.Context, order *domain.Order) error {
    return r.db.WithContext(ctx).Create(order).Error
}
```

**pgxpool (Reads):**
```go
func (r *OrderRepository) FindByStatus(ctx context.Context, status string) ([]*domain.Order, error) {
    rows, err := r.pool.Query(ctx, "SELECT * FROM orders WHERE status = $1", status)
    // Manual scanning for performance
}
```

### Rule 10: Transaction Management
**Transactions MUST be managed in Application layer:**
```go
// Application/Orchestrator
func (o *OrderOrchestrator) CreateOrder(ctx context.Context, input CreateOrderInput) error {
    tx := o.db.Begin() // Start transaction
    
    // 1. Create domain entity
    order := domain.NewOrder(input)
    if err := o.repo.Create(ctx, tx, order); err != nil {
        tx.Rollback()
        return err
    }
    
    // 2. Publish event via Outbox (same transaction)
    event := order.Events()[0]
    if err := o.outbox.Publish(ctx, tx, event); err != nil {
        tx.Rollback()
        return err
    }
    
    // 3. Commit atomically
    return tx.Commit()
}
```

## 🧪 Testing Rules

### Rule 11: Test Coverage Requirements
**Minimum test coverage by layer:**
- Domain Layer: **90%+** (pure business logic, easy to test)
- Application Layer: **80%+** (orchestration logic)
- Infrastructure Layer: **70%+** (integration tests)
- Interface Layer: **60%+** (handler tests)

### Rule 12: No Mocks for Repositories
**Use real repository implementations in tests:**
- ✅ Use test database (Docker PostgreSQL)
- ✅ Test actual SQL queries and transactions
- ✅ Verify database state after operations
- ❌ Avoid mocking repositories (brittle, unrealistic)

### Rule 13: Benchmark Critical Operations
**Performance-critical operations MUST have benchmarks:**
```go
func BenchmarkCreateOrder(b *testing.B) {
    // Setup
    for i := 0; i < b.N; i++ {
        orchestrator.CreateOrder(ctx, input)
    }
}
```

## 📊 Observability Rules

### Rule 14: Prometheus Metrics Required
**ALL projects MUST expose Prometheus metrics:**
- Business metrics: `{project}_orders_created_total`, `{project}_payments_processed_total`
- Technical metrics: `{project}_http_requests_total`, `{project}_db_query_duration_seconds`
- Outbox metrics: `{project}_outbox_pending_events`, `{project}_outbox_processing_duration`

### Rule 15: Structured Logging
**Use structured logging with context:**
```go
logger.Info("Order created",
    "order_id", order.ID,
    "user_id", order.UserID,
    "amount", order.Amount,
)
```

### Rule 16: Health Checks
**Expose health check endpoint:**
```go
GET /health
{
  "status": "ok",
  "database": "connected",
  "event_broker": "connected",
  "cache": "connected"
}
```

## 🚀 Performance Rules

### Rule 17: Connection Pooling
**Use connection pooling for all external services:**
- PostgreSQL: Max 25 connections (pgxpool)
- Cache: Max 10 connections
- NATS: Single connection per worker

### Rule 18: Context Propagation
**ALWAYS propagate context for timeouts and cancellation:**
```go
func (s *OrderService) CreateOrder(ctx context.Context, input CreateOrderInput) error {
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()
    
    return s.repo.Create(ctx, order)
}
```

### Rule 19: Avoid N+1 Queries
**Use joins or batch loading:**
```go
// ❌ BAD: N+1 queries
for _, orderID := range orderIDs {
    order := repo.FindByID(ctx, orderID) // N queries
}

// ✅ GOOD: Single query with IN clause
orders := repo.FindByIDs(ctx, orderIDs) // 1 query
```

## 🔐 Security Rules

### Rule 20: Input Validation
**Validate ALL inputs at Interface layer:**
```go
type CreateOrderRequest struct {
    UserID   string  `json:"user_id" validate:"required,uuid"`
    Amount   float64 `json:"amount" validate:"required,gt=0"`
    Currency string  `json:"currency" validate:"required,len=3"`
}
```

### Rule 21: SQL Injection Prevention
**Use parameterized queries ALWAYS:**
```go
// ✅ CORRECT: Parameterized
db.Query("SELECT * FROM orders WHERE user_id = $1", userID)

// ❌ WRONG: String interpolation
db.Query(fmt.Sprintf("SELECT * FROM orders WHERE user_id = '%s'", userID))
```

## 📦 Dependency Rules

### Rule 22: Go Module Management
**Use Go modules for dependency management:**
```bash
go mod tidy        # Clean up dependencies
go mod vendor      # Vendor dependencies for reproducibility
```

### Rule 23: Dependency Versions
**Pin critical dependencies:**
```go
require (
    github.com/gofiber/fiber/v2 v2.52.0
    gorm.io/gorm v1.25.5
    github.com/nats-io/nats.go v1.31.0
)
```

## 🎯 Template Compliance

When generating projects with this template, agents MUST:
1. ✅ Validate layer dependencies (use `go mod graph`)
2. ✅ Ensure Domain layer has no external dependencies
3. ✅ Verify Outbox Pattern for all integration events
4. ✅ Generate tests for all layers
5. ✅ Include Prometheus metrics
6. ✅ Create Docker Compose for local development
7. ✅ Generate comprehensive README with architecture diagram

**Non-compliance is NOT acceptable** - these rules ensure the architecture remains clean, testable, and maintainable at scale.
