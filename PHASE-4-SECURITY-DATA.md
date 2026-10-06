# Phase 4 — Security and Data

Security and persistence concerns overlap in real Spring applications, but the interview goal is to keep the boundaries clear: protect the API, model trust correctly, and understand how JPA transactions and persistence behavior affect correctness and performance.

## 1. Spring Security mental model

### Q1. What problem does Spring Security solve?
Spring Security provides a framework for authentication, authorization, request protection, and security-related integration. The important interview point is that the framework enforces decisions at defined security boundaries rather than making authentication a controller-specific concern.

### Q2. Authentication vs authorization?
Authentication establishes who or what the caller is. Authorization decides what that authenticated principal is allowed to do.

### Q3. What is SecurityFilterChain?
It defines how incoming HTTP requests pass through Spring Security's web security filters and which security rules apply. The configuration determines authentication mechanisms, authorization rules, CSRF handling, headers, and related concerns.

### Q4. Where should authorization decisions live?
Request-level rules can be enforced at the HTTP boundary, while method-level authorization can protect application operations. The chosen boundary should match the security requirement and avoid relying only on hidden UI restrictions.

## 2. Authorities and roles

### Q5. Role vs authority?
An authority is a granted permission. A role is commonly represented as a named authority with framework conventions around the role prefix. In an interview, focus on the authorization model rather than relying on string-prefix folklore.

### Q6. How do you avoid authorization scattered through controllers?
Centralize coarse request rules and use method/domain boundaries for sensitive operations. Keep authorization intent close to the operation it protects without duplicating the same rule in many layers.

### Q7. Authentication vs identity data?
The authenticated principal provides identity/claims needed for authorization, but business data such as account ownership should still be checked against server-side state. Never trust an identifier simply because the client supplied it.

## 3. Sessions vs tokens

### Q8. Session authentication vs JWT?
Session authentication keeps server-managed session state and usually sends a session identifier to the client. A JWT carries signed claims that the service can validate without looking up the entire session, but token revocation, rotation, audience, expiry, and storage still need careful design.

### Q9. Is JWT automatically more secure?
No. Security depends on key management, expiry, storage, audience validation, algorithm handling, and authorization design. JWT mainly changes where some state is represented.

### Q10. What belongs in a token?
Only claims required by the trust model and consumers. Avoid putting sensitive or highly volatile business data into access tokens just because it is convenient.

## 4. Passwords

### Q11. How should passwords be stored?
Store password verifiers produced by a slow password-hashing function designed for password storage, such as a modern adaptive hash. Never store plaintext passwords or reversible encryption of passwords.

### Q12. Why is hashing different from encryption?
Hashing is designed as a one-way verification mechanism; encryption is reversible with a key. Password storage needs a password hashing scheme because the application should not need to recover the original password.

## 5. CSRF, CORS and browser security

### Q13. What is CSRF?
Cross-site request forgery abuses ambient browser credentials to make a victim's browser perform an unwanted action. It matters especially for cookie-based authentication.

### Q14. What is CORS?
CORS is a browser mechanism for controlling which origins may make certain cross-origin requests. It is not an authentication or authorization system.

### Q15. Should you disable CSRF for every REST API?
Not automatically. The decision depends on how authentication credentials are presented and how the clients are used. Stateless APIs using bearer credentials may have a different CSRF exposure than browser sessions using cookies.

### Q16. What are security headers?
Response headers can control browser behavior such as framing, content interpretation, transport security, and related protections. They should be enabled and tailored through the security configuration rather than copied blindly.

## 6. Method security and API boundaries

### Q17. Why use method-level security?
It can protect application operations independently of a specific HTTP endpoint, which helps when the same operation has multiple entry points.

### Q18. What is insecure direct object reference risk?
A caller changes an object identifier and gains access to another user's resource because the service checks authentication but not ownership/authorization for that specific object. Authorization must include the resource relationship when required.

### Q19. Is hiding an endpoint enough?
No. UI visibility and endpoint discoverability are not security controls. The server must enforce authorization for the operation.

## 7. JPA and the persistence context

### Q20. What is the persistence context?
It is the set of managed entity instances associated with an EntityManager. Within a persistence context, entity identity and change tracking let JPA detect modifications and coordinate persistence.

### Q21. What does managed vs detached mean?
A managed entity is associated with the active persistence context and is tracked for changes. A detached entity is no longer managed by that context, so changing it does not automatically trigger persistence.

### Q22. Why is the first-level cache important?
A persistence context maintains identity for managed entities and can avoid repeated database retrieval of the same entity within that context. It should not be confused with a distributed application cache.

## 8. Lazy vs eager loading

### Q23. Lazy vs eager loading?
Lazy loading delays related data until it is accessed. Eager loading loads it earlier. Neither is universally better; the right choice depends on the use case, query shape, payload, and transaction boundary.

### Q24. Why can lazy loading fail in web responses?
If the persistence context is no longer available when serialization accesses an unloaded relation, the application can fail with a lazy-loading exception. The better solution is to define the query and DTO boundary explicitly rather than depending on accidental session behavior.

## 9. N+1 queries

### Q25. What is an N+1 query?
The application executes one query to load a collection and then another query for each item to load related data. It may work in small development data and become a serious production latency problem as the collection grows.

### Q26. How do you fix N+1?
First reproduce and measure the SQL. Then choose an intentional fetch strategy such as a targeted join, entity graph, batch strategy, or projection based on the endpoint's required data.

### Q27. Why not make every relationship eager?
That can replace an N+1 problem with oversized joins, unnecessary data loading, and more complex queries. Fetch strategy should follow use cases rather than a blanket setting.

## 10. Transactions

### Q28. What does a transaction give you?
A transaction groups database operations under a defined atomicity and consistency boundary. Isolation and durability depend on the database and configuration.

### Q29. Where should @Transactional usually live?
Put transaction boundaries around application operations that need an atomic unit of work, commonly at the service/application layer. The exact placement should reflect the business operation, not simply the repository method.

### Q30. Why doesn't self-invocation always trigger @Transactional?
In proxy-based transaction management, an internal method call on the same object bypasses the proxy. The transactional interceptor therefore does not necessarily run for that call.

### Q31. What causes a transaction not to roll back?
Common causes include calling a transactional method through self-invocation, using an exception/rollback rule that does not trigger rollback as expected, placing the boundary at the wrong abstraction, or having multiple transaction managers/configurations. Debug the actual proxy and transaction boundary rather than guessing from the annotation alone.

## 11. Isolation

### Q32. What does transaction isolation control?
Isolation controls how concurrent transactions can observe each other's changes. Different isolation levels trade consistency guarantees against concurrency and database work.

### Q33. Do all databases implement isolation identically?
No. The SQL standard defines concepts, while database engines implement them with their own mechanisms and details. Interview answers should distinguish logical guarantees from vendor-specific behavior.

## 12. Locking and concurrency

### Q34. Optimistic vs pessimistic locking?
Optimistic locking detects conflicting updates, typically using a version value, and fails or retries when the version has changed. Pessimistic locking acquires a database lock to prevent conflicting work during the protected operation.

### Q35. When is optimistic locking attractive?
It is useful when conflicts are relatively rare and you want high concurrency without holding database locks for long periods.

### Q36. When is pessimistic locking useful?
It can be appropriate when conflicting updates are common or when the business rule requires serialized access to a row. The cost is reduced concurrency and potential lock contention/deadlocks.

### Q37. What is a lost update?
Two transactions read the same state and both write changes, causing one update to overwrite the other unintentionally. Version checks or appropriate locking can detect or prevent this.

## 13. Data access boundaries

### Q38. Why prefer DTO/projection queries for some endpoints?
When an endpoint needs a specific read model, fetching only the required columns can reduce memory, serialization, and database work. It also prevents accidental traversal of an entire entity graph.

### Q39. Should repositories contain business rules?
No. Repositories should express data access and persistence queries. Business rules belong in application/domain logic where they can be coordinated with other dependencies and policies.

### Q40. How do you avoid exposing tenant data?
Every tenant-scoped query and write must apply the tenant boundary from a trusted server-side context. Never rely only on a client-provided tenant identifier.

## 14. Debugging scenarios

### Scenario: user A can retrieve user B's object by changing an ID
Trace the authorization path. Check whether the endpoint validates both permission and resource ownership rather than only checking that the caller is authenticated.

### Scenario: one endpoint suddenly issues hundreds of SQL queries
Inspect SQL logs and entity access patterns. Look for lazy relation traversal during mapping or serialization and replace accidental graph loading with an intentional fetch/projection.

### Scenario: @Transactional appears ignored
Check the actual call path, bean proxying, method visibility, transaction manager, exception behavior, and transaction logs. Self-invocation is a classic cause, but it is not the only one.

### Scenario: production deadlocks appear after a new feature
Identify the transaction statements and lock order for competing operations. Standardize lock acquisition order where possible, shorten transactions, and investigate indexes/isolation with database-specific evidence.

### Scenario: a list endpoint becomes slow after data growth
Measure SQL count, row counts, execution plans, pagination, fetch strategy, and serialization. Do not assume the controller is the bottleneck simply because the latency is observed at the API.

## 15. Senior trade-offs

### Sessions vs JWT
Sessions simplify revocation and server-side lifecycle management but require shared session state or an equivalent distributed session strategy. JWTs can reduce server-side session lookups but increase token lifecycle and revocation complexity.

### Method authorization vs URL authorization
URL rules are easy to see at the HTTP boundary. Method authorization can protect operations across multiple entry points. Sensitive systems often benefit from defense in depth without duplicating the same policy everywhere.

### Lazy loading vs explicit fetch plans
Lazy loading is useful as a default object-navigation strategy, but endpoint code should still define the data it needs explicitly. Fetch plans make query costs visible and reduce accidental database access.

### Optimistic vs pessimistic locking
Optimistic locking maximizes concurrency when conflicts are uncommon. Pessimistic locking can provide stronger serialization at the cost of contention and deadlock risk.

### Entity graph vs projection
Entity graphs keep domain entities but control fetches. Projections can be more efficient for read-specific views. Choose based on the read model and lifecycle rather than forcing every endpoint through entities.

## Senior follow-ups

- How would you secure a multi-tenant Spring API beyond checking a tenant ID in the URL?
- When would you choose session authentication over JWT for an internal platform?
- How would you diagnose an authorization bug that only affects one endpoint?
- How would you fix N+1 without making every relationship eager?
- What evidence would you collect before changing transaction isolation?
- How would you design an order update so concurrent requests cannot silently overwrite each other?
- What happens when a long transaction holds locks while calling another service?
- Where would you place the transaction boundary in a multi-step business workflow?

## Definition of Done

A candidate should be able to explain Spring Security boundaries, authentication and authorization choices, JPA persistence behavior, N+1 diagnosis, transaction and isolation semantics, and locking trade-offs in terms of real production correctness and risk.
