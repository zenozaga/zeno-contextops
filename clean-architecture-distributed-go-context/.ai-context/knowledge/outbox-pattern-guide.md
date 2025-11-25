# Outbox Pattern: Guaranteed Event Delivery

The Outbox Pattern ensures **transactional consistency** between database changes and event publishing, guaranteeing that events are never lost even if the message broker is temporarily unavailable.

## 🎯 The Problem

Direct event publishing after database commit can fail, leading to inconsistent state:

```go
// ❌ PROBLEM: Events can be lost
func (o *OrderOrchestrator) CreateOrder(ctx context.Context, input CreateOrderInput) error {
    // 1. Save to database
    tx := o.db.Begin()
    order := domain.NewOrder(input)
    tx.Create(order)
    tx.Commit()  // ✅ Data saved
    
    // 2. Publish event
    event := NewOrderCreatedEvent(order)
    o.eventPublisher.Publish(event)  // ❌ Can fail (network issue, NATS down)
    
    // Result: Database updated but event NOT published = Inconsistent state!
}
```

**Consequences:**
- Automation workflows don't trigger
- Webhooks not sent
- Analytics miss events
- Billing systems out of sync

---

## ✅ The Solution: Outbox Pattern

Save events in the database **within the same transaction** as domain changes, then process them asynchronously with retries.

### Architecture

```
┌─────────────────────────────────────────────────────┐
│         Application Transaction                     │
│  ┌────────────────────────────────────────────┐    │
│  │ 1. Update Domain Data (orders table)      │    │
│  │ 2. Insert Event (outbox_events table)     │    │ ← Same Transaction
│  │ 3. Commit                                  │    │
│  └────────────────────────────────────────────┘    │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│         Outbox Worker (Background Process)          │
│  ┌────────────────────────────────────────────┐    │
│  │ 1. Notifier signals new event (<50ms)     │    │
│  │ 2. Read pending events from outbox        │    │
│  │ 3. Publish to NATS/Kafka                  │    │
│  │ 4. Mark as processed                      │    │
│  │ 5. Retry on failure (exponential backoff) │    │
│  └────────────────────────────────────────────┘    │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
               NATS/Kafka/RabbitMQ
```

---

## 📊 Outbox Event Table

```sql
CREATE TABLE outbox_events (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    event_type      VARCHAR(255) NOT NULL,
    aggregate_id    VARCHAR(255) NOT NULL,
    aggregate_type  VARCHAR(100) NOT NULL,
    payload         JSONB NOT NULL,
    metadata        JSONB,
    
    -- Processing fields
    status          VARCHAR(50) NOT NULL DEFAULT 'pending',  -- pending, processing, processed, failed
    priority        INT NOT NULL DEFAULT 3,                  -- 1=critical, 2=high, 3=medium, 4=low
    retry_count     INT NOT NULL DEFAULT 0,
    max_retries     INT NOT NULL DEFAULT 5,
    next_retry_at   TIMESTAMP,
    error_message   TEXT,
    
    -- Timestamps
    created_at      TIMESTAMP NOT NULL DEFAULT NOW(),
    processed_at    TIMESTAMP,
    
    -- Indexes
    INDEX idx_outbox_pending (status, priority DESC, next_retry_at) WHERE status = 'pending',
    INDEX idx_outbox_created (created_at DESC)
);
```

---

## 🔔 Notifier Strategies

The Outbox Worker needs to know when new events are inserted. We provide **3 strategies**:

### 1️⃣ PostgreSQL LISTEN/NOTIFY (Recommended for PostgreSQL)

**Latency:** <10ms  
**Database:** PostgreSQL only  
**Setup:** Requires database trigger  

#### Trigger Setup
```sql
-- Notify on new outbox events
CREATE OR REPLACE FUNCTION notify_outbox_event()
RETURNS TRIGGER AS $$
BEGIN
    PERFORM pg_notify('outbox_events', NEW.id::text);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER outbox_events_notify
AFTER INSERT ON outbox_events
FOR EACH ROW
WHEN (NEW.status = 'pending')
EXECUTE FUNCTION notify_outbox_event();
```

#### Implementation
```go
// internal/infrastructure/outbox/postgres_notifier.go
type PostgresNotifier struct {
    db *sql.DB
}

func (n *PostgresNotifier) Listen(ctx context.Context) (<-chan string, error) {
    events := make(chan string, 100)
    
    conn, err := n.db.Conn(ctx)
    if err != nil {
        return nil, err
    }
    
    _, err = conn.ExecContext(ctx, "LISTEN outbox_events")
    if err != nil {
        return nil, err
    }
    
    go func() {
        for {
            notification, err := conn.(*pq.Conn).WaitForNotification(ctx)
            if err != nil {
                log.Error("notification error", err)
                continue
            }
            events <- notification.Extra // event ID
        }
    }()
    
    return events, nil
}
```

**Pros:**
- ✅ Ultra-low latency (<10ms)
- ✅ No polling overhead
- ✅ Built-in PostgreSQL feature

**Cons:**
- ❌ PostgreSQL only (not for MySQL, SQLite, etc.)
- ❌ Requires database trigger

---

### 2️⃣ Redis Pub/Sub (Recommended for Multi-Database)

**Latency:** ~20ms  
**Database:** Any (PostgreSQL, MySQL, CockroachDB, SQLite)  
**Setup:** Requires Redis/DragonflyDB  

#### Implementation
```go
// internal/infrastructure/outbox/redis_notifier.go
type RedisNotifier struct {
    client *redis.Client
}

func (n *RedisNotifier) Notify(ctx context.Context, eventID string) error {
    return n.client.Publish(ctx, "outbox:events", eventID).Err()
}

func (n *RedisNotifier) Listen(ctx context.Context) (<-chan string, error) {
    events := make(chan string, 100)
    
    pubsub := n.client.Subscribe(ctx, "outbox:events")
    
    go func() {
        for msg := range pubsub.Channel() {
            events <- msg.Payload // event ID
        }
    }()
    
    return events, nil
}
```

#### Application Layer
```go
// Publish event via Outbox
event := &IntegrationEvent{...}
if err := outboxPublisher.Publish(ctx, tx, event); err != nil {
    return err
}

// Commit transaction
if err := tx.Commit().Error; err != nil {
    return err
}

// Notify worker immediately (non-blocking)
go notifier.Notify(context.Background(), event.ID)
```

**Pros:**
- ✅ Database-agnostic (works with any DB)
- ✅ Low latency (~20ms)
- ✅ Horizontal scaling (multiple workers)
- ✅ No database triggers needed

**Cons:**
- ❌ Requires Redis/DragonflyDB
- ❌ Slightly higher latency than Postgres LISTEN/NOTIFY

---

### 3️⃣ Polling (Fallback for Development)

**Latency:** 1-2s  
**Database:** Any  
**Setup:** None required  

#### Implementation
```go
// internal/infrastructure/outbox/polling_notifier.go
type PollingNotifier struct {
    db       *gorm.DB
    interval time.Duration
}

func (n *PollingNotifier) Start(ctx context.Context) error {
    ticker := time.NewTicker(n.interval)
    defer ticker.Stop()
    
    for {
        select {
        case <-ctx.Done():
            return nil
        case <-ticker.C:
            n.processPendingEvents(ctx)
        }
    }
}

func (n *PollingNotifier) processPendingEvents(ctx context.Context) {
    var events []*OutboxEvent
    err := n.db.Where("status = ? AND (next_retry_at IS NULL OR next_retry_at <= ?)", 
        "pending", time.Now()).
        Order("priority ASC, created_at ASC").
        Limit(100).
        Find(&events).Error
    
    if err != nil {
        log.Error("failed to fetch outbox events", err)
        return
    }
    
    for _, event := range events {
        n.publishEvent(ctx, event)
    }
}
```

**Pros:**
- ✅ Database-agnostic
- ✅ No external dependencies
- ✅ Simple to implement

**Cons:**
- ❌ Higher latency (1-2s)
- ❌ Constant database queries (polling overhead)
- ❌ Not suitable for real-time systems

---

## 🔄 Outbox Worker Implementation

```go
// internal/infrastructure/outbox/outbox_worker.go
type OutboxWorker struct {
    db              *gorm.DB
    eventPublisher  messaging.EventPublisher
    notifier        Notifier
    batchSize       int
    maxRetries      int
    retryBackoff    time.Duration
}

func (w *OutboxWorker) Start(ctx context.Context) error {
    events, err := w.notifier.Listen(ctx)
    if err != nil {
        return err
    }
    
    for {
        select {
        case <-ctx.Done():
            return nil
        case eventID := <-events:
            w.processEvent(ctx, eventID)
        }
    }
}

func (w *OutboxWorker) processEvent(ctx context.Context, eventID string) {
    // 1. Fetch event with row-level lock
    var event OutboxEvent
    err := w.db.Clauses(clause.Locking{Strength: "UPDATE", Options: "SKIP LOCKED"}).
        First(&event, "id = ? AND status = ?", eventID, "pending").Error
    
    if err != nil {
        return // Event already processed or locked by another worker
    }
    
    // 2. Mark as processing
    event.Status = "processing"
    w.db.Save(&event)
    
    // 3. Publish to message broker
    err = w.eventPublisher.Publish(ctx, messaging.Event{
        Type:    event.EventType,
        Payload: event.Payload,
    })
    
    if err != nil {
        // Retry logic
        if event.RetryCount < w.maxRetries {
            event.Status = "pending"
            event.RetryCount++
            event.NextRetryAt = time.Now().Add(w.calculateBackoff(event.RetryCount))
            event.ErrorMessage = err.Error()
        } else {
            // Max retries exceeded, move to dead letter queue
            event.Status = "failed"
            event.ErrorMessage = err.Error()
        }
    } else {
        // Success
        event.Status = "processed"
        event.ProcessedAt = time.Now()
    }
    
    w.db.Save(&event)
}

func (w *OutboxWorker) calculateBackoff(retryCount int) time.Duration {
    // Exponential backoff: 1s, 2s, 4s, 8s, 16s
    return time.Duration(math.Pow(2, float64(retryCount))) * time.Second
}
```

---

## 📊 Performance Comparison

| Notifier | Latency P50 | Latency P99 | DB Queries/s | Production Ready |
|----------|-------------|-------------|--------------|------------------|
| **Postgres LISTEN/NOTIFY** | <10ms | <50ms | 0 (reactive) | ✅ Yes (PostgreSQL only) |
| **Redis Pub/Sub** | ~20ms | ~100ms | 0 (reactive) | ✅ **Recommended** (any DB) |
| **Polling (2s)** | ~1s | ~2.5s | 0.5 per worker | ⚠️ Development only |

---

## ✅ Benefits

1. **Guaranteed Delivery**: Events never lost, even if broker is down
2. **Transactional Consistency**: Events and data updated atomically
3. **Automatic Retries**: Failed events retried with exponential backoff
4. **Horizontal Scaling**: Multiple workers can process events
5. **Audit Trail**: Complete event history in database
6. **Database Portability**: Redis notifier works with any database

---

## 🚀 Usage Example

```go
func (o *OrderOrchestrator) CreateOrder(ctx context.Context, input CreateOrderInput) error {
    tx := o.db.Begin()
    
    // 1. Create domain entity
    order, err := domain.NewOrder(input)
    if err != nil {
        return err
    }
    
    // 2. Save to database
    if err := o.orderRepo.Create(ctx, tx, order); err != nil {
        tx.Rollback()
        return err
    }
    
    // 3. Publish event via Outbox (transactional guarantee)
    event := &events.IntegrationEvent{
        Type:          "order.created",
        AggregateID:   order.ID.String(),
        AggregateType: "order",
        Payload: map[string]interface{}{
            "order_id": order.ID,
            "user_id":  order.UserID,
            "total":    order.Total,
        },
    }
    
    if err := o.outboxPublisher.Publish(ctx, tx, event); err != nil {
        tx.Rollback()
        return err
    }
    
    // 4. Commit (data + event atomically)
    if err := tx.Commit().Error; err != nil {
        return err
    }
    
    // 5. Optional: Immediate notification for low latency
    go o.notifier.Notify(context.Background(), event.ID)
    
    return nil
}
```

---

## 🎯 Configuration

```bash
# Choose notifier strategy
OUTBOX_NOTIFIER_TYPE=redis  # postgres | redis | polling

# Redis Notifier (if OUTBOX_NOTIFIER_TYPE=redis)
REDIS_HOST=localhost
REDIS_PORT=6379

# PostgreSQL Notifier (if OUTBOX_NOTIFIER_TYPE=postgres)
DATABASE_URL=postgresql://user:pass@localhost:5432/db

# Polling Notifier (if OUTBOX_NOTIFIER_TYPE=polling)
OUTBOX_POLL_INTERVAL=2s

# Worker configuration
OUTBOX_BATCH_SIZE=100
OUTBOX_MAX_RETRIES=5
OUTBOX_RETRY_BACKOFF=1s
```

---

The Outbox Pattern is **non-negotiable** for production systems requiring reliable event delivery. Choose the notifier strategy based on your infrastructure:

- **PostgreSQL only?** → Use Postgres LISTEN/NOTIFY
- **Multi-database or already using Redis?** → Use Redis Pub/Sub (recommended)
- **Development/testing?** → Use Polling

All strategies guarantee eventual consistency and zero event loss.
