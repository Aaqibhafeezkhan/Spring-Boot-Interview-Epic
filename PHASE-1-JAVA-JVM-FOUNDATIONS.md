# Phase 1 — Java and JVM Foundations

This phase strengthens the Java/JVM knowledge expected in senior Spring Boot interviews, with emphasis on mechanisms, production consequences, debugging, and trade-offs.

## Modern Java fundamentals

### Records

Records are concise data-carrier classes with generated accessors, `equals`, `hashCode`, and `toString`. They are useful for immutable request/response models and value-like data, but they do not make referenced objects deeply immutable.

### Sealed classes

Sealed classes restrict which types may extend or implement a type. They are useful when a domain has a deliberately closed set of variants and can make exhaustive handling clearer.

### `Optional`

`Optional` communicates that a value may be absent, especially as a return type. It should not be used mechanically for every field, parameter, or internal variable; clear APIs matter more than wrapping everything.

## Collections and identity

### ArrayList vs LinkedList

`ArrayList` provides efficient indexed access and excellent locality for typical workloads. `LinkedList` has constant-time insertion/removal when the node is already known, but traversal and allocation overhead often make it a poor default. Choose based on the access pattern rather than the textbook complexity table alone.

### HashMap

A `HashMap` uses hashes to locate buckets and compares keys for equality when collisions occur. Correct `equals`/`hashCode` behavior is therefore essential. Mutable keys are dangerous because changing fields used by hashing after insertion can make an entry difficult to retrieve.

### ConcurrentHashMap

`ConcurrentHashMap` supports concurrent access without synchronizing the entire map for ordinary operations. It is appropriate when shared mutable map state is genuinely required, but it does not make compound application-level operations automatically atomic.

## Streams and functional programming

### What is stream laziness?

Intermediate stream operations such as `map` and `filter` describe a pipeline. Work generally occurs when a terminal operation consumes the stream. Laziness can avoid unnecessary intermediate work, but streams are not automatically faster than straightforward loops.

### When should you avoid streams?

Avoid streams when a loop is substantially clearer, when complex control flow is required, or when debugging the pipeline would be harder than the equivalent imperative code. Senior-level code favors clarity over using streams for their own sake.

## Concurrency

### Concurrency vs parallelism

Concurrency is about structuring multiple tasks that can make progress independently. Parallelism is executing work simultaneously, typically on multiple cores. A concurrent program does not necessarily run in parallel.

### Race condition

A race condition occurs when correctness depends on the timing of concurrent operations. Shared mutable state is a common source. The fix is not always `synchronized`; safer designs often reduce shared state or make ownership explicit.

### `synchronized`

`synchronized` provides mutual exclusion and establishes memory-visibility guarantees around the monitor. It protects critical sections but can reduce throughput or create contention when the critical section is too large.

### `volatile` vs `synchronized`

`volatile` provides visibility and ordering guarantees for a variable but does not make compound operations such as incrementing an integer atomic. `synchronized` provides mutual exclusion as well as visibility guarantees. Use the weakest mechanism that correctly preserves the invariant.

### CompletableFuture

`CompletableFuture` represents asynchronous computation and supports composition of dependent or independent stages. Avoid accidentally running blocking work on an executor intended for lightweight asynchronous tasks, and make failure handling explicit.

```java
CompletableFuture<User> user = CompletableFuture.supplyAsync(() -> userService.load(id), executor);
CompletableFuture<Account> account = user.thenCompose(u -> accountService.load(u.id()));
```

The interviewer should notice that executor choice and downstream failure behavior are part of the design, not incidental details.

## Virtual threads

Virtual threads make blocking-style concurrency much cheaper in terms of thread resources for suitable workloads. They are particularly useful for applications that spend substantial time waiting on I/O, but they do not make CPU-bound work faster and do not remove database, connection-pool, or downstream-service limits.

### When can virtual threads make things worse?

They can expose bottlenecks that were previously hidden by a small concurrency level. A service can create many concurrent database or remote calls while the downstream resource remains bounded. Pinning, blocking native operations, excessive synchronization, or unbounded concurrency can also undermine the benefits.

## Java Memory Model

### What does the Java Memory Model explain?

The Java Memory Model defines how threads interact through memory, including visibility, ordering, and the happens-before relationship. Correct concurrency requires more than understanding CPU instructions; the program needs synchronization or another mechanism that establishes the required ordering and visibility.

### Why can a race appear to work in testing?

Timing-dependent bugs may disappear under a debugger or low load. Hardware, compiler, JIT, and scheduling behavior can change the observed result. Passing tests without a defined happens-before relationship is not evidence of correctness.

## JVM execution and memory

### What happens to Java bytecode?

Java source is compiled to bytecode. The JVM loads classes, verifies and links them, interprets or JIT-compiles hot code, and manages runtime memory. The exact implementation is JVM-specific, so avoid treating one HotSpot detail as a Java language guarantee.

### Heap vs stack

Objects generally live in heap-managed memory while each thread has its own stack containing frames for active method calls and local execution state. The exact optimization performed by the JVM can differ, so interview answers should distinguish the conceptual model from implementation optimizations such as escape analysis.

### Garbage collection

Garbage collection reclaims objects that are no longer reachable. Different collectors make different latency and throughput trade-offs. A memory problem is not automatically a GC problem; first determine allocation rate, retained objects, heap pressure, and the actual symptom.

## Production debugging

### CPU suddenly reaches 100%

Start by determining which process and threads consume CPU. Correlate the spike with deployment and traffic changes, inspect thread dumps and profiles, and identify whether the work is application CPU, GC, serialization, contention, or another source. Avoid changing JVM flags before identifying the workload.

### Memory usage keeps growing

Distinguish high allocation from retention. Check heap usage after GC, allocation rates, object histograms, caches, collections, listeners, ThreadLocals, and request-scoped data. A growing heap after repeated full collections is more suggestive of retention than ordinary allocation churn.

### Deadlock suspected

Capture thread dumps and look for threads waiting on locks held by each other. Then map the lock ownership back to application code and establish a consistent lock-ordering strategy or remove the shared-lock design where possible.

## Senior trade-offs

- Prefer immutable values and clear ownership before adding synchronization.
- Choose concurrency primitives based on the invariant being protected.
- Treat executor sizing, connection pools, and downstream limits as one system.
- Distinguish Java language guarantees from JVM implementation details.
- Diagnose production symptoms from evidence before tuning JVM flags.
- Optimize for clarity first; micro-optimizations should follow measurement.

## Phase 1 checklist

- Modern Java language features
- Collections and equality/hash contracts
- Streams and functional programming
- Concurrency and synchronization
- CompletableFuture
- Virtual threads
- Java Memory Model
- JVM execution and memory
- Garbage collection fundamentals
- Production CPU/memory/deadlock debugging
- Senior trade-offs and failure modes
