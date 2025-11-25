# DDD Tactical Patterns

Domain-Driven Design (DDD) provides tactical building blocks for modeling complex business domains:

## Aggregates
- Cluster of domain objects treated as a single unit
- Enforces invariants and transactional consistency
- Example: `Order` aggregate with `OrderItem` entities

## Entities
- Objects with a distinct identity (ID)
- Mutable, tracked over time
- Example: `User`, `OrderItem`

## Value Objects
- Objects defined only by their attributes, no identity
- Immutable, replaceable
- Example: `Money`, `Address`

## Domain Services
- Stateless operations that don't fit naturally in an entity or value object
- Example: `PaymentService`, `PricingService`

## Repository Interfaces
- Abstractions for data access, defined in domain layer
- Implemented in infrastructure layer

## Domain Events
- Represent something that happened in the domain
- Used for side effects, integrations, notifications

---

**Best Practices:**
- Keep aggregates small and focused
- Use value objects for complex attributes
- Emit domain events for important state changes
- Define repository interfaces in domain, implement in infrastructure
