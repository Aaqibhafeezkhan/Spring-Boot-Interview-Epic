# Spring Boot + Web + Data — Questions & Answers

## Spring Boot

### 1. What is Spring Boot?
**Answer:** Spring Boot is an opinionated layer on top of Spring that makes it much faster to build and run Spring applications. It gives you auto-configuration, starters, embedded servers, externalized configuration, production features and sensible defaults.

### 2. Spring Framework vs Spring Boot?
**Answer:** Spring Framework provides the core programming model and infrastructure. Spring Boot makes using that ecosystem easier by reducing configuration and providing an opinionated application setup. Boot does not replace Spring Framework.

### 3. What does `@SpringBootApplication` do?
**Answer:** It is a convenience annotation combining the main configuration role, component scanning and Spring Boot auto-configuration. It is normally placed on the main application class.

### 4. What is auto-configuration?
**Answer:** Boot examines the classpath, existing beans and configuration and conditionally configures infrastructure for you. For example, adding the appropriate database dependencies can cause Boot to configure a `DataSource` when you have not supplied one yourself.

### 5. How does auto-configuration back off?
**Answer:** Boot's conditions commonly check whether classes, properties or beans exist. A condition such as `@ConditionalOnMissingBean` allows Boot to provide a default only when you have not supplied your own bean. This is why adding your own infrastructure often replaces the default.

### 6. How do you debug auto-configuration?
**Answer:** Start by looking at the condition evaluation report, enable the appropriate debug logging, and inspect the actual dependency graph and configuration. Don't guess which auto-configuration is responsible; find the condition that matched or failed.

### 7. What are starters?
**Answer:** Starters are dependency descriptors that give you a convenient set of dependencies for a common capability. They reduce the amount of dependency-selection boilerplate and let Boot's dependency management keep compatible versions together.

### 8. What happens when `SpringApplication.run()` executes?
**Answer:** It bootstraps the application, prepares the environment, creates the application context, loads bean definitions and configuration, refreshes the context, starts the embedded web server for web applications, and runs startup callbacks. The exact lifecycle has many extension points, so this is a good area for follow-up questions.

### 9. `CommandLineRunner` vs `ApplicationRunner`?
**Answer:** Both run after the application context is created. `CommandLineRunner` receives raw string arguments; `ApplicationRunner` receives parsed `ApplicationArguments`.

### 10. Why can startup fail even though the code compiles?
**Answer:** Compilation only proves the source can be compiled. Startup can fail because of missing beans, invalid configuration, incompatible dependencies, database connectivity, port conflicts, classpath problems or failed initialization logic.

## Configuration

### 11. `@Value` vs `@ConfigurationProperties`?
**Answer:** `@Value` is convenient for a small number of values. `@ConfigurationProperties` is better when a feature has a group of related settings because it gives you a typed configuration object, cleaner validation and better structure.

### 12. What is externalized configuration?
**Answer:** Application configuration is kept outside the compiled code so the same artifact can run in different environments. Spring Boot supports multiple property sources, with precedence rules determining which value wins.

### 13. How should secrets be handled?
**Answer:** Don't commit credentials into source control or bake them into images. Inject them through a proper secret-management mechanism or environment-specific secret store, restrict access, rotate them, and avoid logging them.

### 14. What are profiles?
**Answer:** Profiles let you activate environment-specific beans and configuration. They can be useful for genuinely different environment behavior, but they shouldn't become an excuse for maintaining a maze of mutually inconsistent application configurations.

### 15. How do you troubleshoot a property that is being ignored?
**Answer:** Check the exact property name, active profiles, property-source precedence, environment variables, configuration binding, indentation if YAML is involved, and whether the expected bean is actually being created. Actuator/environment diagnostics can help in controlled environments, but don't expose sensitive configuration publicly.

## Spring MVC / REST

### 16. What is `DispatcherServlet`?
**Answer:** It is the central front controller in Spring MVC. It receives HTTP requests and coordinates mapping, argument resolution, controller invocation, exception handling and response processing.

### 17. `@RequestParam` vs `@PathVariable`?
**Answer:** A path variable identifies a resource as part of the URL path, such as `/users/42`. A request parameter is normally used for optional/filtering/query input, such as `/users?active=true`.

### 18. What does `@RequestBody` do?
**Answer:** It tells Spring to deserialize the HTTP request body using an appropriate message converter into the target Java type. With JSON, that usually involves Jackson in a conventional MVC setup.

### 19. Why use DTOs instead of returning entities directly?
**Answer:** DTOs separate the API contract from the persistence model. They prevent accidental exposure of fields, reduce coupling, make validation and versioning clearer, and avoid problems caused by serializing lazy JPA relationships.

### 20. `ResponseEntity` vs returning an object?
**Answer:** Returning an object is clean when the status and headers are straightforward. `ResponseEntity` is useful when the endpoint needs explicit control over status codes, headers or an optional response body.

### 21. PUT vs PATCH?
**Answer:** PUT conventionally represents replacement of a resource representation and is idempotent when designed correctly. PATCH represents a partial modification. The exact semantics depend on the API contract, so don't claim that every PATCH implementation is automatically idempotent.

### 22. What is idempotency?
**Answer:** An operation is idempotent when repeating the same request has the same intended effect as performing it once. HTTP defines idempotency for methods such as GET, PUT and DELETE, but application-level behavior still matters.

### 23. How do you implement global exception handling?
**Answer:** Use `@ControllerAdvice`/`@RestControllerAdvice` with `@ExceptionHandler` methods. Map domain/application exceptions to deliberate HTTP responses instead of leaking stack traces or database exceptions to clients.

### 24. What is Problem Details?
**Answer:** Problem Details is a standardized HTTP error representation. Spring's web stack supports producing structured problem responses so clients don't have to reverse-engineer arbitrary error JSON shapes.

### 25. `@Valid` vs `@Validated`?
**Answer:** Both participate in Bean Validation, but `@Validated` also supports Spring's validation groups and is commonly used at the type level for method validation. Use the mechanism that matches where and how validation is being applied.

### 26. How do you prevent mass assignment?
**Answer:** Don't bind arbitrary request JSON directly into privileged domain/entity objects. Define request DTOs containing only fields the client is allowed to supply and map them deliberately.

### 27. Offset vs cursor pagination?
**Answer:** Offset pagination is simple and works well for many administrative screens, but deep offsets can become expensive and results can shift while data changes. Cursor/keyset pagination is usually better for large, frequently changing datasets when you can define a stable ordering key.

### 28. How do you stream a large response?
**Answer:** Stream data rather than materializing the complete result in memory. Choose the appropriate Spring HTTP streaming mechanism and make sure the database access layer also avoids loading the entire dataset at once.

## JPA / Hibernate

### 29. JPA vs Hibernate vs Spring Data JPA?
**Answer:** JPA/Jakarta Persistence is the persistence specification. Hibernate is a popular implementation of that specification. Spring Data JPA is a Spring abstraction that reduces repository boilerplate and builds on JPA.

### 30. What is the persistence context?
**Answer:** It is the set of managed entity instances associated with an EntityManager. It provides identity guarantees within the context and enables features such as dirty checking and automatic synchronization with the database.

### 31. What is dirty checking?
**Answer:** Hibernate tracks managed entities. During flush it can detect changes and generate the necessary SQL without requiring an explicit update call for every field change.

### 32. What is flush?
**Answer:** Flush synchronizes pending persistence-context changes with the database. It is not the same thing as committing the transaction. A flush can happen before transaction commit.

### 33. What is lazy loading?
**Answer:** Lazy loading defers loading an association or other data until it is actually accessed. It can reduce unnecessary database work, but accessing lazy data after the persistence context is unavailable can cause failures.

### 34. What is N+1?
**Answer:** You load one set of records with one query and then accidentally trigger another query for each record when accessing an association. The result is 1 + N queries. It is a query-shape problem, not something you should fix by blindly making every relationship eager.

### 35. How do you fix N+1?
**Answer:** First prove where it happens with SQL/query metrics. Then choose the right solution: fetch joins, entity graphs, projections, batch fetching, or a deliberately different query. The correct fix depends on what data the use case actually needs.

### 36. What is `@EntityGraph`?
**Answer:** It lets you specify which associations should be fetched for a particular repository operation without changing the default fetch strategy of the entity mapping.

### 37. What is optimistic locking?
**Answer:** It detects conflicting updates rather than locking the row for the entire transaction. A version field such as `@Version` is checked when updating, and a conflict causes the update to fail so the application can handle it.

### 38. Optimistic vs pessimistic locking?
**Answer:** Optimistic locking assumes conflicts are relatively uncommon and detects them at update time. Pessimistic locking asks the database to hold locks while the transaction operates. Optimistic locking usually scales better for ordinary web workloads, but the domain and contention pattern decide.

### 39. Why can `CascadeType.ALL` be dangerous?
**Answer:** Cascades propagate persistence operations across relationships. `ALL` includes remove, so deleting one aggregate/entity can unexpectedly delete associated records. Cascades should follow actual aggregate ownership rather than being added because they make code convenient.

### 40. Why can `@ManyToMany` be problematic?
**Answer:** A direct many-to-many mapping can become awkward once the relationship itself has attributes, lifecycle rules or large-scale query requirements. An explicit join entity is often clearer and more controllable.

### 41. What is Open Session in View?
**Answer:** It keeps the persistence context available into the web request so lazy associations can still be loaded during response rendering. It can be convenient, but it can also hide inefficient query behavior and cause database access from serialization/view code.

## Transactions

### 42. What does `@Transactional` do?
**Answer:** It tells Spring to execute a method within transaction semantics. In the common proxy-based model, the proxy starts/joins a transaction before invoking the target and commits or rolls it back according to the configured rules.

### 43. What is transaction propagation?
**Answer:** It defines how a method behaves when it is invoked while a transaction already exists. `REQUIRED` joins an existing transaction or starts one; `REQUIRES_NEW` suspends the current transaction and starts a separate one.

### 44. What is transaction isolation?
**Answer:** Isolation controls what concurrent transactions can observe. Common levels trade consistency guarantees against concurrency and database overhead. The exact behavior is database-specific, so don't treat the names as identical implementation guarantees across every database.

### 45. What causes rollback by default?
**Answer:** Spring's default transaction interceptor rolls back for unchecked exceptions and `Error`, not for every checked exception. You can explicitly configure rollback rules when the domain requires different behavior.

### 46. Why does `@Transactional` sometimes not work?
**Answer:** Common causes include self-invocation, putting the annotation on a method that cannot be intercepted as expected, calling the object without going through the Spring proxy, or transaction configuration not being active. Always check the actual proxy boundary before blaming the annotation.

### 47. Can a private method be transactional?
**Answer:** In proxy-based Spring transaction management, putting `@Transactional` on a private method does not give you a separately intercepted transaction boundary because external proxy interception cannot call a private method that way.

### 48. Should transactions live in controllers?
**Answer:** Usually no. Transaction boundaries generally belong around application/service operations because the service layer knows the unit of work. Controllers should translate HTTP concerns into application calls.

### 49. What is the outbox pattern?
**Answer:** When a database change and an event publication must be coordinated, write the business change and an outbox event in the same database transaction. A separate publisher then delivers the outbox event. This avoids the classic failure where the DB commits but the message publish fails.

### 50. What is eventual consistency?
**Answer:** Different parts of the system may temporarily disagree but converge after asynchronous processing completes. It is a deliberate distributed-systems trade-off, not simply a synonym for an unreliable system.

## Sources

- [Spring Boot auto-configuration](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)
- [Spring Boot system requirements](https://docs.spring.io/spring-boot/system-requirements.html)
- [Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/)
- [Spring Data JPA locking](https://docs.spring.io/spring-data/jpa/reference/jpa/locking.html)
