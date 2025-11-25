# Clean Architecture: 4 Layers Explained

Clean Architecture organizes code into concentric layers where dependencies flow inward. The inner layers contain business logic and are independent of external frameworks, databases, and UI.

## 🎯 The Four Layers

```
┌─────────────────────────────────────────────────────┐
│              Interface Layer (Adapters)             │
│  HTTP Handlers, WebSocket, gRPC, GraphQL, CLI       │
└────────────────────┬────────────────────────────────┘
                     │ Dependencies flow inward →
┌────────────────────▼────────────────────────────────┐
│           Application Layer (Use Cases)             │
│  Orchestrators, Services, Event Dispatcher          │
└────────────────────┬────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────┐
│           Domain Layer (Business Logic)             │
│  Aggregates, Entities, Value Objects, Interfaces    │
└─────────────────────────────────────────────────────┘
                     ▲
┌────────────────────┴────────────────────────────────┐
│        Infrastructure Layer (Implementation)        │
│  Database, Cache, Message Broker, External APIs     │
└─────────────────────────────────────────────────────┘
```

---

## 1️⃣ Domain Layer (Core Business Logic)

### Purpose
Contains **pure business logic** with zero external dependencies.

### Responsibilities
- Define business entities and aggregates
- Enforce business rules and invariants
- Define repository interfaces (dependency inversion)
- Emit domain events for state changes
- Contain domain services (single-aggregate logic)

### Structure
```
internal/domain/
├── order/
│   ├── order.go              # Aggregate root
│   ├── order_item.go         # Entity
│   ├── money.go              # Value object
│   ├── repository.go         # Interface
│   ├── service.go            # Domain service
│   ├── events.go             # Domain events
│   └── errors.go             # Domain errors
├── payment/
│   ├── payment.go
│   ├── repository.go
│   └── ...
└── shared/
    ├── entity.go             # Base entity
    └── value_objects.go      # Shared VOs
```

### Key Principles
✅ **No external dependencies** (only standard library + UUID)  
✅ **Framework-agnostic** (no HTTP, DB, or messaging libs)  
✅ **Testable in isolation** (pure functions)  
✅ **Rich domain model** (behavior > data)  

### Example
```go
// internal/domain/order/order.go
package order

import (
    "errors"
    "time"
    "github.com/google/uuid"
)

// Aggregate Root
type Order struct {
    ID          uuid.UUID
    UserID      uuid.UUID
    Items       []OrderItem
    TotalAmount Money
    Status      OrderStatus
    CreatedAt   time.Time
    UpdatedAt   time.Time
}

// Business rule: Order must have at least one item
func NewOrder(userID uuid.UUID, items []OrderItem) (*Order, error) {
    if len(items) == 0 {
        return nil, errors.New("order must have at least one item")
    }
    
    order := &Order{
        ID:        uuid.New(),
        UserID:    userID,
        Items:     items,
        Status:    OrderStatusPending,
        CreatedAt: time.Now(),
        UpdatedAt: time.Now(),
    }
    
    order.calculateTotal()
    return order, nil
}

// Business logic
func (o *Order) Confirm() error {
    if o.Status != OrderStatusPending {
        return errors.New("only pending orders can be confirmed")
    }
    o.Status = OrderStatusConfirmed
    o.UpdatedAt = time.Now()
    return nil
}

// Repository interface (defined in domain, implemented in infrastructure)
type Repository interface {
    Create(ctx context.Context, order *Order) error
    FindByID(ctx context.Context, id uuid.UUID) (*Order, error)
    Update(ctx context.Context, order *Order) error
}
```

---

## 2️⃣ Application Layer (Orchestration)

### Purpose
Coordinates **use cases** by orchestrating domain objects and infrastructure services.

### Responsibilities
- Implement use cases (business workflows)
- Coordinate multiple domain aggregates
- Manage transactions
- Publish integration events via Outbox Pattern
- Enforce application-level policies

### Structure
```
internal/application/
├── services/
│   ├── order_orchestrator.go      # Order use cases
│   ├── payment_orchestrator.go    # Payment use cases
│   └── user_orchestrator.go       # User use cases
├── events/
│   ├── event_dispatcher.go        # Event routing
│   ├── domain_event.go            # Domain event wrapper
│   └── integration_event.go       # Integration event wrapper
├── integration/
│   └── outbox/
│       ├── outbox_publisher.go    # Outbox Pattern
│       └── outbox_event.go        # Outbox entity
└── interfaces/
    ├── event_bus.go               # EventBus interface
    └── outbox_publisher.go        # OutboxPublisher interface
```

### Key Principles
✅ **Orchestrate, don't implement** business logic  
✅ **Manage transactions** across aggregates  
✅ **Publish events** after state changes  
✅ **Depend on interfaces** from domain layer  

### Example
```go
// internal/application/services/order_orchestrator.go
package services

type OrderOrchestrator struct {
    orderRepo       order.Repository
    paymentRepo     payment.Repository
    db              *gorm.DB
    outboxPublisher outbox.Publisher
    eventBus        events.EventBus
}

func (o *OrderOrchestrator) CreateOrder(ctx context.Context, input CreateOrderInput) (*order.Order, error) {
    // Start transaction
    tx := o.db.Begin()
    
    // 1. Create domain entity (business logic in domain layer)
    newOrder, err := order.NewOrder(input.UserID, input.Items)
    if err != nil {
        return nil, err
    }
    
    // 2. Persist to database
    if err := o.orderRepo.Create(ctx, tx, newOrder); err != nil {
        tx.Rollback()
        return nil, err
    }
    
    // 3. Publish integration event via Outbox (guaranteed delivery)
    event := &events.IntegrationEvent{
        Type:          "order.created",
        AggregateID:   newOrder.ID.String(),
        AggregateType: "order",
        Payload:       newOrder,
    }
    if err := o.outboxPublisher.Publish(ctx, tx, event); err != nil {
        tx.Rollback()
        return nil, err
    }
    
    // 4. Commit transaction (data + event atomically)
    if err := tx.Commit().Error; err != nil {
        return nil, err
    }
    
    // 5. Publish domain event (best-effort, real-time)
    o.eventBus.Publish(ctx, &events.DomainEvent{
        Type:    "order.created",
        Payload: newOrder,
    })
    
    return newOrder, nil
}
```

---

## 3️⃣ Infrastructure Layer (External Concerns)

### Purpose
Implements **technical details** like database access, caching, messaging, and external APIs.

### Responsibilities
- Implement repository interfaces from domain
- Manage database connections and queries
- Implement cache clients
- Implement message broker publishers/consumers
- Manage external API integrations

### Structure
```
internal/infrastructure/
├── persistence/
│   └── postgres/
│       ├── order_repository.go    # Repository implementation
│       └── payment_repository.go
├── database/
│   ├── connection.go              # DB connection pool
│   └── migrations/
├── cache/
│   ├── redis_client.go
│   └── dragonfly_client.go
├── messaging/
│   ├── nats_client.go
│   ├── kafka_client.go
│   └── event_publisher.go
├── outbox/
│   ├── outbox_worker.go           # Background processor
│   ├── postgres_notifier.go       # LISTEN/NOTIFY
│   ├── redis_notifier.go          # Redis Pub/Sub
│   └── polling_notifier.go        # Polling fallback
└── observability/
    ├── prometheus_metrics.go
    └── logger.go
```

### Key Principles
✅ **Implement interfaces** from domain/application  
✅ **Hide implementation details** from upper layers  
✅ **Use dependency injection** for testability  
✅ **Optimize for performance** (connection pooling, batching)  

### Example
```go
// internal/infrastructure/persistence/postgres/order_repository.go
package postgres

import (
    "context"
    "github.com/jackc/pgx/v5/pgxpool"
    "gorm.io/gorm"
    "your-project/internal/domain/order"
)

type OrderRepository struct {
    db   *gorm.DB       // For writes (transactional)
    pool *pgxpool.Pool  // For reads (high-performance)
}

// Implement domain interface
func (r *OrderRepository) Create(ctx context.Context, tx *gorm.DB, o *order.Order) error {
    return tx.WithContext(ctx).Create(o).Error
}

func (r *OrderRepository) FindByID(ctx context.Context, id uuid.UUID) (*order.Order, error) {
    var o order.Order
    err := r.db.WithContext(ctx).
        Preload("Items").
        First(&o, "id = ?", id).Error
    return &o, err
}

// High-performance read with pgxpool
func (r *OrderRepository) FindByStatus(ctx context.Context, status string) ([]*order.Order, error) {
    rows, err := r.pool.Query(ctx, `
        SELECT id, user_id, status, total_amount, created_at
        FROM orders
        WHERE status = $1
        ORDER BY created_at DESC
    `, status)
    if err != nil {
        return nil, err
    }
    defer rows.Close()
    
    var orders []*order.Order
    for rows.Next() {
        var o order.Order
        err := rows.Scan(&o.ID, &o.UserID, &o.Status, &o.TotalAmount, &o.CreatedAt)
        if err != nil {
            return nil, err
        }
        orders = append(orders, &o)
    }
    return orders, nil
}
```

---

## 4️⃣ Interface Layer (Presentation)

### Purpose
Handles **external communication** through various protocols (HTTP, WebSocket, gRPC, CLI).

### Responsibilities
- Receive and validate user input
- Route requests to application layer
- Transform responses to appropriate format
- Handle protocol-specific concerns (HTTP status codes, etc.)
- Apply middleware (auth, CORS, rate limiting)

### Structure
```
internal/interfaces/
├── http/
│   ├── handlers/
│   │   ├── order_handler.go
│   │   ├── payment_handler.go
│   │   └── user_handler.go
│   ├── middleware/
│   │   ├── auth.go
│   │   ├── cors.go
│   │   └── rate_limit.go
│   └── rest/
│       ├── router.go
│       └── response.go
├── websocket/
│   ├── hub.go
│   └── client.go
├── grpc/
│   ├── order_service.go
│   └── proto/
└── graphql/
    ├── schema.graphqls
    └── resolver.go
```

### Key Principles
✅ **Thin layer** (delegate to application layer)  
✅ **Validate inputs** before passing to application  
✅ **Transform outputs** to appropriate format  
✅ **Handle errors** gracefully  

### Example
```go
// internal/interfaces/http/handlers/order_handler.go
package handlers

type OrderHandler struct {
    orchestrator *services.OrderOrchestrator
}

func (h *OrderHandler) CreateOrder(c *fiber.Ctx) error {
    // 1. Parse and validate input
    var req CreateOrderRequest
    if err := c.BodyParser(&req); err != nil {
        return c.Status(400).JSON(fiber.Map{"error": "invalid request"})
    }
    
    if err := validator.Validate(req); err != nil {
        return c.Status(422).JSON(fiber.Map{"error": err.Error()})
    }
    
    // 2. Delegate to application layer
    order, err := h.orchestrator.CreateOrder(c.Context(), services.CreateOrderInput{
        UserID: req.UserID,
        Items:  req.Items,
    })
    if err != nil {
        return c.Status(500).JSON(fiber.Map{"error": err.Error()})
    }
    
    // 3. Transform to response format
    return c.Status(201).JSON(fiber.Map{
        "data": OrderResponse{
            ID:     order.ID,
            Status: string(order.Status),
            Total:  order.TotalAmount.Amount,
        },
    })
}
```

---

## 🔄 Layer Communication Flow

### Example: Create Order Flow

```
1. HTTP Request → Interface Layer (OrderHandler)
   ↓
2. Validate input → Application Layer (OrderOrchestrator)
   ↓
3. Create entity → Domain Layer (order.NewOrder)
   ↓
4. Persist data → Infrastructure Layer (OrderRepository)
   ↓
5. Publish event → Application Layer (OutboxPublisher)
   ↓
6. Return response → Interface Layer (OrderHandler)
```

### Dependency Direction
```
Interface Layer ──→ Application Layer ──→ Domain Layer
                                             ↑
Infrastructure Layer ─────────────────────────┘
```

---

## ✅ Benefits of Clean Architecture

1. **Testability**: Pure business logic is easy to test
2. **Flexibility**: Swap infrastructure without changing business logic
3. **Maintainability**: Clear separation of concerns
4. **Scalability**: Independent deployment of layers
5. **Framework Independence**: Not tied to specific frameworks

---

## ⚠️ Common Mistakes

❌ **Domain depending on Infrastructure**
```go
// WRONG: Domain importing GORM
import "gorm.io/gorm"

type Order struct {
    gorm.Model  // ❌ Couples domain to GORM
}
```

✅ **Correct: Domain stays pure**
```go
type Order struct {
    ID        uuid.UUID
    CreatedAt time.Time  // ✅ Standard library only
}
```

---

❌ **Application Layer with business logic**
```go
// WRONG: Business logic in orchestrator
func (o *OrderOrchestrator) CreateOrder(...) {
    if len(items) == 0 {  // ❌ Business rule in application layer
        return errors.New("order must have items")
    }
}
```

✅ **Correct: Business logic in domain**
```go
// Domain layer
func NewOrder(items []OrderItem) (*Order, error) {
    if len(items) == 0 {  // ✅ Business rule in domain
        return nil, errors.New("order must have items")
    }
}

// Application layer
func (o *OrderOrchestrator) CreateOrder(...) {
    order, err := order.NewOrder(items)  // ✅ Delegate to domain
}
```

---

This 4-layer architecture ensures your Go project remains clean, testable, and maintainable as it scales.
