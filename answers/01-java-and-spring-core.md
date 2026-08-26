# Java + Spring Core — Questions & Answers

This is the part I would actually revise before a Spring interview. The answers are deliberately short first, then explain the bit that usually gets a follow-up question.

> **Version note:** Java and Spring APIs move. For version-sensitive questions, check the version note in the repo and the linked official docs before using an answer in an interview.

## Java fundamentals

### 1. JDK vs JRE vs JVM?
**Answer:** The JVM runs Java bytecode. The JRE is the JVM plus the runtime libraries needed to run Java applications. The JDK is the development kit: it includes the runtime plus tools such as `javac`, `javadoc` and other developer tooling.

**Interview follow-up:** Modern Java distributions are generally installed as a JDK; the old idea that you need to separately install a JRE is mostly historical.

### 2. `==` vs `equals()`?
**Answer:** For primitives, `==` compares values. For references, `==` compares whether two references point to the same object. `equals()` is intended to compare logical equality, provided the class implements it correctly.

### 3. Why must `hashCode()` agree with `equals()`?
**Answer:** Hash-based collections use `hashCode()` to find a bucket and `equals()` to confirm equality. If two objects are equal, they must have the same hash code. The reverse is not required: two unequal objects can have the same hash code.

### 4. Why is `String` immutable?
**Answer:** Immutability makes strings safe to share, works well with the string pool, makes cached hash codes possible, and avoids surprising changes when strings are used as map keys or across threads.

### 5. `StringBuilder` vs `StringBuffer`?
**Answer:** Both are mutable string builders. `StringBuffer` has synchronized methods and is therefore generally slower; `StringBuilder` is the normal choice when the instance is not shared between threads.

### 6. Checked vs unchecked exceptions?
**Answer:** Checked exceptions must be declared or handled. Unchecked exceptions extend `RuntimeException` and do not have that compiler requirement. In Spring applications, business/domain errors are often represented with unchecked exceptions so service APIs don't become dominated by checked-exception plumbing.

### 7. What is try-with-resources?
**Answer:** It automatically closes objects implementing `AutoCloseable`, even when an exception occurs. It is the preferred way to manage files, streams, JDBC resources and similar resources.

### 8. `List` vs `Set` vs `Map`?
**Answer:** `List` is ordered and allows duplicates. `Set` models unique elements. `Map` stores key/value associations. The right collection follows the domain requirement rather than performance folklore.

### 9. How does `HashMap` work?
**Answer:** It hashes a key, uses that hash to locate a bucket, and then compares keys using equality when necessary. Modern Java implementations can treeify heavily-colliding buckets, but the important interview point is that correct `equals()`/`hashCode()` implementations are essential.

### 10. `HashMap` vs `ConcurrentHashMap`?
**Answer:** `HashMap` is not thread-safe. `ConcurrentHashMap` is designed for concurrent access and provides atomic compound operations such as `computeIfAbsent`. Making a `HashMap` field `volatile` does not make its operations thread-safe.

### 11. What is type erasure?
**Answer:** Java generics are mostly a compile-time type-safety feature. Generic type information is erased in the normal runtime representation, which is why you cannot generally do `new T()` or `instanceof List<String>`.

### 12. `map()` vs `flatMap()` in streams?
**Answer:** `map()` transforms each element into one result. `flatMap()` transforms each element into a stream and flattens those streams into one stream. A common example is turning `List<List<String>>` into `List<String>`.

### 13. Why is `Optional` useful?
**Answer:** `Optional<T>` explicitly represents a value that may be absent and is useful as a return type. It is usually not a good choice for every field, parameter or serialization model. Don't use it just to make null disappear syntactically.

### 14. What is `CompletableFuture`?
**Answer:** It represents an asynchronous computation and lets you compose dependent operations without manually managing callback plumbing. It can also combine multiple asynchronous operations and handle failures.

### 15. Concurrency vs parallelism?
**Answer:** Concurrency is about dealing with multiple tasks during overlapping periods. Parallelism means tasks are actually executing simultaneously, usually on different cores. A concurrent program does not necessarily run in parallel.

### 16. What is a race condition?
**Answer:** It occurs when the result depends on the timing/interleaving of concurrent operations accessing shared state. The fix is not automatically `synchronized`; better designs often reduce shared mutable state or use appropriate concurrency primitives.

### 17. `volatile` vs `synchronized`?
**Answer:** `volatile` gives visibility and ordering guarantees for a variable, but it does not make compound operations such as `count++` atomic. `synchronized` provides mutual exclusion plus memory-visibility guarantees.

### 18. What are virtual threads?
**Answer:** Virtual threads are lightweight Java threads managed by the JVM rather than one-to-one with OS threads. They are especially useful for applications with large numbers of blocking operations because they make thread-per-request designs much cheaper. They do not make CPU-bound work magically faster.

### 19. When can virtual threads hurt?
**Answer:** They can still be limited by external resources such as database connection pools. Pinning, excessive synchronization, unbounded concurrency and blocking operations that don't cooperate well can also remove much of the benefit. You still need backpressure and resource limits.

## Spring Core

### 20. What problem does Spring solve?
**Answer:** Spring gives you infrastructure for creating objects, wiring dependencies, managing lifecycle, applying cross-cutting behavior, handling web requests, transactions and more. The big idea is that application code doesn't have to manually construct and coordinate every dependency.

### 21. IoC vs DI?
**Answer:** Inversion of Control is the broader principle: control over object creation/wiring is moved away from the application objects. Dependency Injection is one way of implementing IoC: dependencies are supplied to an object instead of the object looking them up itself.

### 22. Why prefer constructor injection?
**Answer:** Dependencies are explicit, required dependencies can be final, the object cannot normally be constructed in an invalid state, and unit tests can instantiate it without a Spring container. It also makes circular dependencies visible early.

### 23. What is the IoC container?
**Answer:** It is the Spring infrastructure responsible for creating, configuring and managing beans. `ApplicationContext` is the commonly used higher-level container and adds features such as events, resource loading and message resolution on top of the basic bean factory capabilities.

### 24. `@Component` vs `@Bean`?
**Answer:** `@Component` marks a class for component scanning. `@Bean` marks a method whose returned object should be registered as a bean. Use `@Bean` when you need explicit construction/configuration, especially for third-party classes you cannot annotate.

### 25. `@Component`, `@Service`, `@Repository`?
**Answer:** All are component stereotypes. `@Service` communicates service-layer intent; `@Repository` communicates persistence-layer intent and participates in Spring's persistence exception translation; `@Component` is the generic stereotype. The important point is that the specialized annotations communicate architecture, not that they create three different DI systems.

### 26. `@Controller` vs `@RestController`?
**Answer:** `@RestController` is effectively `@Controller` plus response-body semantics. A normal `@Controller` commonly returns a view name, while a REST controller normally serializes return values into HTTP responses.

### 27. `@Primary` vs `@Qualifier`?
**Answer:** `@Primary` gives one candidate preference when multiple beans match. `@Qualifier` selects a particular candidate explicitly. If the dependency has different implementations with meaningful roles, a qualifier is often clearer than relying on a global default.

### 28. What is a Spring bean?
**Answer:** A bean is an object managed by the Spring container. Spring owns its creation/configuration and, depending on its scope and lifecycle, its destruction as well.

### 29. What is component scanning?
**Answer:** Spring scans configured packages for component stereotypes and registers matching classes as bean definitions. Package placement therefore matters; putting the main application class too far from your components can result in beans not being discovered.

### 30. What is the bean lifecycle?
**Answer:** At a high level: Spring creates the bean, injects dependencies, runs initialization callbacks and post-processors, and later invokes destruction callbacks for applicable scopes. Exact details matter when discussing `BeanPostProcessor`, proxies and initialization ordering.

### 31. What does `@PostConstruct` do?
**Answer:** It marks an initialization callback that runs after dependency injection and before the bean is ready for normal use. It should not be used as a substitute for proper constructor validation or for long-running startup work.

### 32. What is a `BeanPostProcessor`?
**Answer:** It can inspect or modify bean instances before and/or after initialization. Spring uses this extension point heavily for framework features, including creating or wrapping beans with proxies.

### 33. What is a Spring proxy?
**Answer:** A proxy is an object that stands in front of the target object and can intercept method calls. Spring uses proxies for things such as transactions, security, caching and AOP advice.

### 34. JDK proxy vs class-based proxy?
**Answer:** A JDK dynamic proxy works through interfaces. A class-based proxy subclasses the target class. The practical interview point is that proxying has limitations: final classes/methods and calls that bypass the proxy can affect interception.

### 35. What is self-invocation?
**Answer:** If method A on a bean directly calls method B on `this`, the call does not pass through the Spring proxy. Proxy-based features such as `@Transactional`, `@Cacheable` and `@Async` can therefore appear not to work on that internal call. The usual fix is to redesign the boundary rather than forcing proxy access through awkward workarounds.

### 36. What is a circular dependency?
**Answer:** Bean A needs B while B needs A. Constructor injection makes this problem obvious because neither object can be constructed first. The better fix is usually to break the dependency cycle by changing responsibilities, not to hide it with lazy injection.

### 37. Singleton vs prototype scope?
**Answer:** Singleton means one bean instance per Spring container. Prototype means Spring creates a new instance when asked for one. A prototype injected into a singleton does not magically become a new instance for every method call; the injection normally happens once unless you use a scoped lookup/provider mechanism.

### 38. What is `@Lazy`?
**Answer:** It delays bean initialization until the bean is first needed rather than eagerly creating it during context startup. It can reduce startup cost, but it can also move failures from startup time to first use.

### 39. What is Spring AOP?
**Answer:** AOP separates cross-cutting behavior such as logging, transactions and security from business code. In typical Spring applications it is proxy-based, which is why proxy boundaries and self-invocation matter.

### 40. What is SpEL?
**Answer:** Spring Expression Language lets Spring configuration and annotations evaluate expressions against objects, properties and context information. It is powerful, but business logic hidden inside expressions can become difficult to understand and test.

## Sources

- [Spring Framework IoC and DI](https://docs.spring.io/spring-framework/reference/core/beans/dependencies.html)
- [Spring Framework `@Bean`](https://docs.spring.io/spring-framework/reference/core/beans/java/bean-annotation.html)
- [Spring Boot auto-configuration](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)
- [Spring Boot system requirements](https://docs.spring.io/spring-boot/system-requirements.html)
