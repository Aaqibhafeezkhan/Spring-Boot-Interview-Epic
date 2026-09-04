# Advanced Spring Boot Interview Drills

This companion set pushes beyond annotation recall into senior-level reasoning. For each prompt, explain the diagnosis, the safest design choice, and the trade-off.

## 1. Idempotent command endpoint under retries

A client retries `POST /payments` after a timeout, but the first request may already have succeeded.

### Strong answer

Use an idempotency key that is persisted with the operation result or durable command state. Treat the key as scoped to the business operation, define how long it is retained, and return the original outcome for a duplicate request.

### Follow-up

Why is a request ID alone insufficient?

Because tracing an attempt is different from identifying that multiple attempts represent the same business operation.

## 2. Cache and database disagree

A product is updated successfully, but reads still return the old value from Redis.

### Strong answer

Define the consistency model first. For cache-aside, update the database as the source of truth and invalidate or update the cache using a deliberate ordering. Consider races between concurrent readers and writers, failure during invalidation, and whether stale reads are acceptable.

### Senior-level point

There is no universally correct cache invalidation sequence; correctness depends on the data and consistency requirement.

## 3. Message consumer processes the same event twice

A broker delivers the same event more than once.

### Strong answer

Assume at-least-once delivery unless the system explicitly guarantees otherwise. Make the consumer idempotent using a durable event/operation identifier, unique constraints, or a state transition that safely rejects duplicates.

### Follow-up

Why not keep processed IDs only in application memory?

An in-memory set disappears on restart and is inconsistent across multiple instances.

## 4. `@Transactional` method calls a slow HTTP dependency

An order service updates the database and then calls another service before returning.

### Strong answer

Question whether the remote call belongs inside the database transaction. Holding a connection while waiting on a network dependency can reduce pool availability and increase tail latency. Narrow the transaction boundary where the business invariant permits it, or use an asynchronous/outbox workflow when eventual consistency is acceptable.

## 5. One tenant is exhausting shared resources

A multi-tenant API becomes unstable whenever one customer sends a traffic spike.

### Strong answer

Introduce tenant-aware rate limits, concurrency limits or quotas, and isolate expensive work where necessary. Observe per-tenant traffic and resource consumption so the noisy-neighbor problem is measurable.

### Trade-off

Strong isolation improves fairness but increases operational and architectural complexity.

## 6. Memory usage grows slowly over hours

Heap usage rises after every traffic cycle and GC does not return the process to its normal baseline.

### Strong answer

Determine whether this is actual retention, cache growth, queued work, large object churn or expected heap behavior. Compare heap dumps over time, inspect dominators and retention paths, and correlate with application metrics before changing JVM settings.

### Trap

Do not assume that increasing heap size fixes a leak; it can only delay failure.

## 7. Readiness is healthy but users still see failures

Kubernetes reports the pod as ready, yet requests fail because a critical dependency is unavailable.

### Strong answer

Separate process health from ability to serve the workload. Define readiness around the dependencies and startup state that genuinely determine whether traffic should be accepted, while keeping liveness focused on whether the process is fundamentally alive.

### Follow-up

What is the danger of putting every dependency into liveness?

A temporary dependency outage can cause unnecessary restarts and create a cascading failure.

## 8. A new index fixes one query but hurts writes

A database query becomes much faster after adding an index, but write throughput drops.

### Strong answer

Measure both read benefit and write cost. Evaluate query frequency, selectivity, index size, maintenance overhead and whether the index supports the actual query shape. Indexes are workload trade-offs, not free optimizations.

## 9. A feature flag changes behavior in only some instances

Two application instances appear to use different configuration for the same flag.

### Strong answer

Establish the configuration source, refresh model, caching behavior and rollout semantics. Treat feature flags as distributed configuration and ensure observability shows the effective value per instance or request when diagnosing inconsistent behavior.

## 10. Production incident with no obvious single bottleneck

Latency, error rate and queue depth all rise together after a deployment.

### Strong answer

Build a timeline around the deployment boundary, then correlate request latency with saturation signals: CPU, GC, thread pools, connection pools, downstream latency and queue depth. Check whether one early degradation caused secondary saturation elsewhere.

### Senior-level point

Distributed incidents are often chains of failure rather than one isolated defect. The interview answer should describe how evidence distinguishes primary from secondary symptoms.
