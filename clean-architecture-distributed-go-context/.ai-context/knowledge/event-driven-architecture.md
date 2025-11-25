# Event-Driven Architecture

Event-driven systems react to changes by publishing and consuming events:

## Domain Events
- Internal, best-effort delivery
- Used for real-time updates, analytics, notifications
- Example: `order.created`, `user.registered`

## Integration Events
- External, guaranteed delivery (Outbox Pattern)
- Used for automations, webhooks, billing, cross-service communication
- Example: `payment.processed`, `shipment.sent`

## Event Streams
- NATS JetStream, Kafka, RabbitMQ, AWS SQS/SNS
- Partitioned by aggregate or event type

## Event Consumers
- Services or workers that react to events
- Can trigger workflows, update read models, send notifications

---

**Best Practices:**
- Use Outbox Pattern for all integration events
- Name events as `{aggregate}.{action}`
- Monitor event delivery latency and retries
