# Phase 1 — Java and JVM Foundations

This phase is focused on the Java depth expected from a senior Spring Boot engineer. The goal is not memorizing APIs; it is being able to explain runtime behavior, choose the right abstraction, diagnose failures, and discuss trade-offs.

## 1. Modern Java fundamentals

### Q1. What is the difference between JDK, JRE and JVM?
The JVM executes bytecode. The JRE conceptually combines the JVM with the libraries/runtime needed to run Java applications. The JDK adds the compiler, debugger and development tooling. In modern Java distributions, the separate JRE distribution is no longer the normal packaging model, but the conceptual distinction is still useful in interviews.

### Q2. Why are String objects immutable?
Immutability gives String stable identity and hash codes, makes strings safe to share, enables string-pool reuse, and simplifies concurrent use. It also prevents code holding a String reference from changing the underlying value.

### Q3. ArrayList vs LinkedList?
ArrayList is normally the default List because contiguous storage gives efficient indexed access and good cache locality. LinkedList has cheap insertion/removal only when the node is already located, while finding that location is linear. In real applications, ArrayList is usually preferable unless profiling and access patterns justify another structure.

### Q4. HashMap vs ConcurrentHashMap?
HashMap is not safe for concurrent mutation. ConcurrentHashMap provides thread-safe concurrent access with substantially better concurrency characteristics than synchronizing an entire map. It is appropriate when multiple threads genuinely share mutable map state; it does not make compound application-level operations automatically atomic.

### Q5. What makes an object immutable?
Its state cannot change after construction. Typical techniques are final fields, no mutating methods, defensive copies for mutable inputs/outputs, and safe publication. Records provide concise value-oriented data carriers but do not magically make referenced mutable objects immutable.

### Q6. What are records?
Records are concise classes for transparent data carriers. They automatically provide components, accessors, equals, hashCode and toString. They work well for API DTOs and immutable-style values when their semantics fit. A record is shallowly immutable, not recursively immutable.

### Q7. What are sealed classes?
Sealed classes/interfaces restrict which types may extend or implement them. They are useful when a domain has a deliberately closed set of variants and can pair well with pattern matching and exhaustive reasoning.

### Q8. What is type erasure?
Java generics are primarily a compile-time type-safety feature. Generic type information is generally erased from ordinary runtime representation, which explains restrictions such as not being able to directly instantiate a generic type parameter and some limitations around generic arrays.

## 2. Streams and functional programming

### Q9. Stream vs collection?
A collection represents stored data. A Stream represents a computation pipeline over data. Streams are lazy until a terminal operation and should be used when they make transformation/filtering logic clearer, not simply because they are newer syntax.

### Q10. map() vs flatMap()?
map transforms one element into one result. flatMap transforms each element into a stream and flattens the resulting streams. It is useful for one-to-many transformations.

### Q11. When should you avoid streams?
Avoid them when a conventional loop is materially clearer, when debugging becomes unnecessarily difficult, or when side effects make the pipeline misleading. Streams are not automatically faster; parallel streams especially require evidence and suitable workloads.

### Q12. What is Optional for?
Optional communicates that a value may be absent, especially as a return type. It should not normally be used as a field, method parameter, or serialization model merely to eliminate every null. It is a tool for explicit absence, not a universal replacement for null.

## 3. Exceptions and resource management

### Q13. Checked vs unchecked exceptions?
Checked exceptions participate in compile-time handling requirements. Unchecked exceptions derive from RuntimeException and commonly represent programming errors or failures callers are not expected to recover from at every layer. The important senior-level question is API design: an exception should communicate an actionable failure boundary rather than force boilerplate.

### Q14. What is try-with-resources?
It automatically closes AutoCloseable resources, even when the body throws. It is the preferred pattern for files, streams, database resources and similar lifecycle-sensitive objects.

## 4. Concurrency

### Q15. Concurrency vs parallelism?
Concurrency is structuring multiple tasks that can make progress independently. Parallelism is actually executing work simultaneously, typically on multiple cores. A concurrent program does not necessarily run in parallel.

### Q16. What is a race condition?
A race occurs when correctness depends on timing/interleaving between concurrent operations. The fix is not automatically synchronized; first identify the shared state and establish the required invariant, then choose synchronization, confinement, immutability, atomic operations, locks, queues or another design.

### Q17. synchronized vs volatile?
volatile provides visibility and ordering guarantees for reads/writes of a variable but does not make compound operations such as increment atomic. synchronized provides mutual exclusion plus visibility guarantees around the critical section.

### Q18. What is the Java Memory Model?
The JMM defines visibility, ordering and happens-before relationships between threads. Senior answers should focus on why one thread is allowed to observe stale state without synchronization and which synchronization mechanisms establish the required happens-before relationship.

### Q19. What is deadlock?
A deadlock occurs when threads wait indefinitely for resources held by one another. Common prevention strategies include consistent lock ordering, minimizing lock scope, avoiding unnecessary nested locks, and using timed lock acquisition where appropriate.

### Q20. What is CompletableFuture?
CompletableFuture represents an asynchronous computation and supports composition through operations such as thenApply, thenCompose, exceptionally and handle. The important production concerns are executor choice, exception propagation, cancellation, timeouts and avoiding accidental blocking.

### Q21. When should you use virtual threads?
Virtual threads are lightweight threads designed for high-concurrency workloads that spend significant time waiting on blocking I/O. They can simplify request-per-task programming. They do not make CPU-bound work faster, and synchronization, native calls, external limits and downstream capacity still matter.

### Q22. What is a thread pool exhaustion incident?
Typical symptoms include growing latency, queued work, timeouts and eventually rejected tasks. Diagnose executor configuration, queue depth, task duration, blocking calls, downstream latency and whether callers are creating more work than the system can sustainably process.

## 5. JVM fundamentals

### Q23. What memory areas matter when diagnosing a Java process?
Think about heap objects, thread stacks, metaspace/class metadata, native memory, direct buffers and runtime structures. Heap-only reasoning can miss native-memory or thread-related failures.

### Q24. What does garbage collection do?
GC identifies objects that are no longer reachable and reclaims their memory. Different collectors make different throughput, latency and footprint trade-offs. The correct choice depends on workload and measurements rather than a universal best collector.

### Q25. What is a memory leak in Java?
It is usually accidental retention: objects remain reachable even though the application no longer needs them. Common causes include unbounded caches, static collections, listeners, ThreadLocal misuse and retained request data. GC cannot collect reachable objects.

### Q26. What is JIT compilation?
The JVM can compile frequently executed bytecode into optimized machine code at runtime. It uses runtime profiling information and optimizations such as inlining. Warm-up behavior means a benchmark should not assume the first few executions represent steady-state performance.

### Q27. What is class loading?
Classes are loaded, linked and initialized according to JVM rules. Class-loader boundaries matter for application servers, plugins and dependency isolation. Class-loading failures should be distinguished from initialization failures and linkage errors.

## 6. Senior debugging scenarios

### Scenario: CPU suddenly reaches 100%
1. Confirm whether CPU is application or system level.
2. Capture thread dumps and identify hot/runnable threads.
3. Correlate with deployment and traffic changes.
4. Inspect profiling data if needed.
5. Check infinite loops, excessive serialization, regex, GC pressure and unexpected concurrency.
6. Mitigate safely, then fix the underlying cause.

### Scenario: Heap usage grows continuously
Inspect heap trends and GC behavior first. If objects survive collections, take heap dumps and identify dominant retained objects and GC roots. Check caches, collections, listeners, ThreadLocals and request/session retention.

### Scenario: Requests become slow under load
Do not immediately increase thread count. Check downstream latency, connection pools, executor queues, lock contention, GC pauses, CPU saturation and database behavior. Increasing concurrency can amplify an already saturated dependency.

### Scenario: Two threads update the same business value
Define the business invariant first. Then decide whether atomic primitives, a lock, database optimistic locking, a transaction, serialization through a queue, or another approach is appropriate. The right answer depends on where the source of truth lives.

## 7. Trade-off questions

### When is synchronization the wrong answer?
When shared mutable state can be removed through immutability, ownership, message passing or better architecture. Synchronization protects state; it does not repair a poor ownership model.

### When is a concurrent collection not enough?
When multiple operations must succeed or fail as one business-level operation. Thread-safe individual methods do not automatically make a sequence atomic.

### When should you use a virtual thread instead of reactive programming?
Prefer the model that gives the team the simplest correct architecture for the workload. Virtual threads can make high-concurrency blocking I/O straightforward; reactive programming remains useful for end-to-end non-blocking pipelines and ecosystems built around reactive composition. Measure before deciding.

## 8. Interview follow-ups

- Why can volatile not replace synchronized for i++?
- What establishes a happens-before relationship?
- Why can a ConcurrentHashMap still produce a business-level race?
- Why can increasing a thread pool reduce throughput?
- How would you prove a memory leak rather than simply observe high heap usage?
- How would you distinguish a GC problem from a CPU problem?
- What happens if a CompletableFuture chain blocks its executor?
- Why can a virtual-thread application still exhaust a database connection pool?
- What changes when Java code runs inside a container?
- Which JVM metrics would you monitor in production?

## Definition of Done

A candidate should be able to explain the above concepts in their own words, reason from symptoms to likely causes, write small examples, and discuss trade-offs without relying on memorized folklore.
