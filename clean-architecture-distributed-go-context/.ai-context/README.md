# Clean Architecture Distributed Go Context

## Purpose
Generate production-ready Go projects with Clean Architecture, Domain-Driven Design (DDD), Event-Driven patterns, and Outbox Pattern. Optimized for distributed systems requiring high throughput, guaranteed event delivery, and 4-20x performance improvements.

## How This Template Works

- **Architecture Patterns (Non-Negotiable):**
  - Clean Architecture (4 layers: Domain, Application, Infrastructure, Interface)
  - Domain-Driven Design (Aggregates, Entities, Value Objects, Domain Services)
  - Event-Driven Architecture (Domain Events + Integration Events)
  - Outbox Pattern (Guaranteed event delivery with transactional consistency)
  - CQRS Lite (Command/Query separation)
  - Hybrid Repository Pattern (GORM for writes, pgxpool for reads)

- **Workflow:**
  1. Ask user 6 technical questions (database, cache, event broker, outbox notifier, API protocol, observability)
  2. Validate compatibility between answers (e.g., Postgres LISTEN/NOTIFY requires PostgreSQL)
  3. Generate project structure based on configuration
  4. Apply conditional templates based on user choices
  5. Create aggregates specific to user's domain
  6. Setup infrastructure clients (DB, cache, event broker)
  7. Implement Outbox Pattern with chosen notifier strategy
  8. Generate observability setup (metrics, health checks, logging)
  9. Create Docker Compose for local development
  10. Generate comprehensive documentation

- **Key Guidelines:**
  - ALWAYS start with task: `ask_technical_questions.md`
  - VALIDATE cross-dependencies before generating code
  - NEVER violate Clean Architecture layer dependencies
  - ENSURE all integration events use Outbox Pattern
  - PROVIDE performance benchmarks for critical operations
  - INCLUDE comprehensive testing structure (unit + integration)

## Mandatory Architectural Rules (Summary)

- Dependencies MUST flow inward only (Interface → Application → Domain)
- Domain layer MUST be free of external dependencies (no DB, HTTP, messaging, cache clients)
- Repository interfaces in Domain, implementations in Infrastructure
- Use Dependency Injection for all implementations
- Outbox Pattern is required for all integration events
- Event naming: `{aggregate}.{action}` (e.g., `order.created`)
- CQRS: Separate command (write) and query (read) operations
- Prometheus metrics required for business and technical KPIs
- Minimum test coverage by layer: Domain 90%, Application 80%, Infrastructure 70%, Interface 60%
- Structured logging and health checks are mandatory

## Usage for Agents

1. Ask user for confirmation before applying the template
2. Copy the entire `.ai-context` folder to the target project
3. Start with `ask_technical_questions.md` and follow the workflow
4. Validate all answers and dependencies before generating code
5. Apply conditional templates and generate documentation

## Recommended Next Steps
- Review the rules in `rules.md` for compliance
- Use the knowledge base for architecture, DDD, event-driven, and outbox best practices
- Follow the tasks sequentially for a complete, production-ready setup
