# Spring Boot Production Scenarios

Senior Spring Boot interviews often move from API definitions to production incidents. Use these scenarios to practice diagnosis, trade-offs and failure handling.

## 1. API latency suddenly increases

### Strong answer

Start with evidence: request latency percentiles, error rate, throughput, database timings and dependency timings. Check whether the regression is isolated to an endpoint, release or dependency. Inspect thread pools, connection pools, GC and database query performance before changing code.

### Trap

Do not immediately increase server resources. First identify the bottleneck.

## 2. Database connection pool is exhausted

### Investigate

- Long-running queries.
- Transactions that remain open too long.
- Connection leaks.
- Pool sizing versus application concurrency.
- Slow downstream calls performed inside transactions.
- Database capacity and connection limits.

### Key point

Increasing the pool can make an overloaded database worse. Pool sizing is a system-level trade-off.

## 3. A request succeeds but the client receives a 500

Trace the request across controller, service, repository and exception handling. Check whether an exception is being transformed incorrectly, whether a response is committed before failure, and whether logs contain a correlation/request identifier.

A production-ready API should expose a safe error response without leaking internal exception details.

## 4. Two users update the same record

Ask whether last-write-wins is acceptable. If not, consider optimistic locking with a version field, explicit conflict handling, or a domain-specific concurrency strategy.

### Follow-up

Why not put a synchronized block around the service method?

Because synchronization only coordinates threads within one JVM. It does not solve concurrency across multiple application instances.

## 5. An event is published but the database transaction rolls back

Do not assume a database transaction and message broker transaction are automatically atomic together. Consider the transactional outbox pattern when reliable publication is required: persist the domain change and an outgoing event in the same database transaction, then publish the event asynchronously and track delivery state.

## 6. A Spring Boot service becomes slow under load

Investigate CPU, memory, GC, thread pools, database pools, downstream calls, lock contention, request payloads and query plans. Use profiling and metrics to establish the dominant bottleneck before optimizing.

### Senior-level point

Throughput, latency, availability and resource usage must be considered together. Optimizing one can worsen another.

## 7. An endpoint needs authentication but some requests bypass it

Verify the security filter chain, request matcher ordering, authorization rules and method-level security. Test both positive and negative cases. Do not rely on controller-level checks alone when the application has centralized security requirements.

## 8. A downstream service is intermittently failing

A resilient design should define timeout, retry, backoff, circuit-breaking and fallback behavior appropriate to the dependency. Retries must be bounded and should normally use jitter; blindly retrying non-idempotent operations can duplicate side effects.

## 9. A deployment works locally but fails in production

Compare configuration, environment variables, profiles, dependency versions, database schema, network access, secrets, JVM/runtime settings and external service availability. Reproduce with production-like configuration rather than assuming the application code is the only difference.

## 10. “How would you improve this Spring Boot application?”

Start with the problem and evidence. Establish the business impact, measure the system, identify the dominant constraint, make the smallest safe change, add observability around the failure mode, and verify the result under realistic load.

A strong Spring Boot answer connects framework features to distributed-system behavior rather than simply naming annotations.
