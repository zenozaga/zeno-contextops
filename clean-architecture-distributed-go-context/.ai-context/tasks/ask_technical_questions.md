# Technical Questions for Clean Architecture Go Project

**Description:**  
This task collects 6 critical technical decisions that will shape the entire project structure. These answers determine which templates to apply, what dependencies to install, and how to configure the infrastructure.

## Step 0: Template Application Confirmation
**BEFORE asking questions, you MUST:**
1. **Ask user confirmation**: "Do you want me to proceed with applying the clean-architecture-distributed-go-context template?"
2. **Wait for explicit approval** before copying any files
3. **Copy the entire .ai-context folder** to the target project
4. **Only then proceed** with the questions below

---

## 🎯 Objectives
- Gather technical configuration for the project
- Validate compatibility between choices
- Determine which conditional templates to apply
- Configure infrastructure clients and dependencies

---

## ❓ The 6 Technical Questions

Ask the user these questions **one by one** and record their answers:

### 1️⃣ Database Selection

```
Question: What database will you use?

Options:
a) PostgreSQL 16+ (recommended for production)
b) MySQL 8.0+
c) CockroachDB (PostgreSQL-compatible, distributed)
d) SQLite (development/testing only)

Recommendation: PostgreSQL offers the best feature set including LISTEN/NOTIFY 
for ultra-low latency event notifications (<10ms).
```

**Record answer as:** `DATABASE_TYPE`  
**Valid values:** `postgres` | `mysql` | `cockroachdb` | `sqlite`

---

### 2️⃣ Cache Strategy

```
Question: What caching solution do you need?

Options:
a) DragonflyDB (25x faster than Redis, recommended)
b) Redis 7+
c) Memcached
d) None (no caching layer)

Recommendation: DragonflyDB provides Redis compatibility with significantly 
better performance (1M+ ops/s) and lower memory usage.
```

**Record answer as:** `CACHE_TYPE`  
**Valid values:** `dragonfly` | `redis` | `memcached` | `none`

---

### 3️⃣ Event Broker

```
Question: What message broker will you use for events?

Options:
a) NATS JetStream (10M+ msg/s, recommended for high throughput)
b) Apache Kafka (high durability, complex setup)
c) RabbitMQ (easy setup, moderate performance)
d) AWS SQS/SNS (managed service, cloud-native)
e) None (in-memory events only, no persistence)

Recommendation: NATS JetStream offers the best balance of performance, 
simplicity, and reliability for distributed systems.
```

**Record answer as:** `EVENT_BROKER`  
**Valid values:** `nats` | `kafka` | `rabbitmq` | `aws` | `none`

---

### 4️⃣ Outbox Notifier Strategy

```
Question: What notification strategy for Outbox Pattern?

Options:
a) PostgreSQL LISTEN/NOTIFY (<10ms latency, requires PostgreSQL)
b) Redis Pub/Sub (~20ms latency, any database, recommended)
c) Polling (1-2s latency, any database, fallback)

Recommendation: 
- If using PostgreSQL → Option (a) for ultra-low latency
- If using other DB or already have Redis → Option (b) 
- For development/testing → Option (c)
```

**Record answer as:** `OUTBOX_NOTIFIER`  
**Valid values:** `postgres` | `redis` | `polling`

**⚠️ VALIDATION REQUIRED:**
- If `postgres` selected → `DATABASE_TYPE` must be `postgres` or `cockroachdb`
- If `redis` selected → `CACHE_TYPE` must be `redis` or `dragonfly`

---

### 5️⃣ API Protocol

```
Question: What API protocol(s) do you need?

Options:
a) REST + WebSocket (full-duplex communication, recommended for real-time)
b) REST only (HTTP/JSON, simplest)
c) gRPC (high-performance RPC, requires protobuf)
d) GraphQL (flexible queries, complex setup)
e) REST + gRPC (hybrid, best of both)

Recommendation: REST + WebSocket covers most use cases and provides 
real-time capabilities without the complexity of gRPC or GraphQL.
```

**Record answer as:** `API_PROTOCOL`  
**Valid values:** `rest-websocket` | `rest` | `grpc` | `graphql` | `rest-grpc`

---

### 6️⃣ Observability Stack

```
Question: What observability tools will you use?

Options:
a) Prometheus + Grafana (open-source, self-hosted, recommended)
b) Datadog (enterprise SaaS, comprehensive)
c) New Relic (enterprise SaaS, easy setup)
d) OpenTelemetry (vendor-agnostic, flexible)
e) Basic (health checks + structured logging only)

Recommendation: Prometheus + Grafana is production-proven, free, and 
provides excellent visibility into system performance.
```

**Record answer as:** `OBSERVABILITY_STACK`  
**Valid values:** `prometheus` | `datadog` | `newrelic` | `opentelemetry` | `basic`

---

## ✅ Validation Rules

After collecting all answers, **validate compatibility**:

### Validation 1: Outbox Notifier vs Database
```
IF OUTBOX_NOTIFIER == "postgres" THEN
  DATABASE_TYPE MUST BE IN ["postgres", "cockroachdb"]
  ELSE ERROR: "PostgreSQL LISTEN/NOTIFY requires PostgreSQL or CockroachDB"
```

### Validation 2: Outbox Notifier vs Cache
```
IF OUTBOX_NOTIFIER == "redis" THEN
  CACHE_TYPE MUST BE IN ["redis", "dragonfly"]
  ELSE ERROR: "Redis Pub/Sub notifier requires Redis or DragonflyDB cache"
```

### Validation 3: gRPC Requirements
```
IF API_PROTOCOL IN ["grpc", "rest-grpc"] THEN
  CHECK: protoc compiler installed
  IF NOT FOUND THEN WARN: "Install protobuf compiler: https://grpc.io/docs/protoc-installation/"
```

### Validation 4: Performance Warning
```
IF EVENT_BROKER == "kafka" AND OUTBOX_NOTIFIER == "polling" THEN
  WARN: "Polling notifier adds 1-2s latency. Consider Redis notifier for <50ms event delivery."
```

---

## 📝 Record Answers

Create a configuration summary and store it:

```yaml
# .ai-context-config.yaml (generated after questions)
project_config:
  database_type: postgres
  cache_type: dragonfly
  event_broker: nats
  outbox_notifier: redis
  api_protocol: rest-websocket
  observability_stack: prometheus
  
  # Derived configuration
  use_gorm: true
  use_pgxpool: true
  use_websocket: true
  use_grpc: false
  use_graphql: false
  
  # Dependencies to install
  dependencies:
    - "gorm.io/gorm"
    - "gorm.io/driver/postgres"
    - "github.com/jackc/pgx/v5/pgxpool"
    - "github.com/gofiber/fiber/v2"
    - "github.com/gofiber/contrib/websocket"
    - "github.com/nats-io/nats.go"
    - "github.com/redis/go-redis/v9"
    - "github.com/prometheus/client_golang"
```

---

## 🎯 Next Steps

After validating answers:

1. **Show configuration summary** to user for confirmation
2. **Proceed to next task:** `validate_compatibility.md`
3. **Generate project structure** based on configuration
4. **Apply conditional templates** (e.g., skip gRPC if not selected)

---

## 💡 Example Interaction

```
Agent: Do you want me to proceed with applying the clean-architecture-distributed-go-context template?
User: Yes

Agent: Great! I'll ask you 6 technical questions to configure your project.

Agent: 1️⃣ What database will you use?
a) PostgreSQL 16+ (recommended)
b) MySQL 8.0+
c) CockroachDB
d) SQLite

User: a

Agent: 2️⃣ What caching solution do you need?
a) DragonflyDB (25x faster than Redis, recommended)
b) Redis 7+
c) Memcached
d) None

User: a

... (continues with remaining questions)

Agent: ✅ Configuration Summary:
- Database: PostgreSQL 16+
- Cache: DragonflyDB
- Event Broker: NATS JetStream
- Outbox Notifier: Redis Pub/Sub
- API Protocol: REST + WebSocket
- Observability: Prometheus + Grafana

All compatibility checks passed! Proceeding to generate project structure...
```

---

## ⚠️ Important Notes

- **Never assume answers** - always ask the user
- **Validate immediately** after collecting all answers
- **Explain incompatibilities** clearly if validation fails
- **Offer alternatives** when validation errors occur
- **Store configuration** for use in subsequent tasks

This configuration drives the entire project generation process. Accuracy is critical!
