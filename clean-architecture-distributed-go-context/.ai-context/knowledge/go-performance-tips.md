# Go Performance Tips

Optimize your Go project for high throughput and low latency:

## Concurrency
- Use goroutines for parallelism
- Use channels for communication
- Avoid blocking operations

## Connection Pooling
- Use pgxpool for PostgreSQL (max 25 connections)
- Pool cache and event broker connections

## Benchmarking
- Use Go's `testing` and `bench` for performance tests
- Benchmark critical operations (writes, queries, event publishing)

## Profiling
- Use pprof for CPU and memory profiling
- Identify bottlenecks and optimize

---

**Best Practices:**
- Avoid N+1 queries (batch loading)
- Use context for timeouts and cancellations
- Monitor Prometheus metrics for latency and throughput
