# Phase 3 — APIs and Application Design

This phase covers the HTTP and application-layer decisions that turn Spring services into maintainable APIs. The focus is on contracts, validation, failures, evolution, and clear application boundaries.

## 1. REST and HTTP semantics

### Q1. What makes an API RESTful?
A REST-style API models resources and uses standard HTTP semantics for interaction. The important interview point is not whether every endpoint follows a textbook REST constraint, but whether the API has predictable resources, methods, representations, status codes, and contracts.

### Q2. GET vs POST vs PUT vs PATCH?
GET reads a resource representation. POST commonly creates a subordinate resource or triggers a non-idempotent operation. PUT replaces a representation at a known resource URI and is defined as idempotent. PATCH applies a partial modification and its idempotency depends on the operation.

### Q3. What does idempotent mean?
An operation is idempotent when repeating the same request has the same intended effect as applying it once. Idempotency matters for retries because clients and gateways may repeat requests after timeouts.

### Q4. 200 vs 201 vs 202?
200 indicates a successful request with a normal response. 201 indicates a resource was created and normally identifies the created resource. 202 means the request was accepted for processing but the work is not yet complete.

### Q5. When should an API return 204?
Use 204 when the operation succeeds and there is intentionally no response body. It is common for successful deletes or updates where the client does not need a representation.

## 2. Resource design

### Q6. How would you design an order API?
Start from domain resources rather than database tables. For example, `/orders/{id}` can represent an order, with nested resources or actions only when they express a clear domain operation.

### Q7. Should every database entity become a REST resource?
No. Database structure and API structure serve different consumers. An API resource should match the client-facing contract and domain semantics rather than exposing persistence internals.

### Q8. Why avoid deeply nested URLs?
Deep nesting can couple clients to relationship structure and make authorization/query behavior harder to reason about. Keep URLs readable and use identifiers/reference links where deeper nesting does not add real meaning.

## 3. DTO boundaries

### Q9. Why use DTOs instead of returning JPA entities?
DTOs separate the external contract from the persistence model. They reduce accidental field exposure, prevent serialization surprises, and allow the database model to evolve without automatically changing the public API.

### Q10. What is a common serialization mistake?
Returning a persistence graph directly can trigger lazy-loading problems, circular references, excessive payloads, or hidden database access during serialization.

### Q11. Should request and response DTOs be the same class?
Not necessarily. Requests and responses often have different mutability, validation, security, and lifecycle concerns. Separate models can make the contract clearer when those concerns diverge.

## 4. Validation

### Q12. Where should request validation happen?
Boundary validation belongs at the API boundary, typically using Bean Validation annotations with `@Valid` or `@Validated` as appropriate. Business rules that require domain knowledge still belong in the service/domain layer.

### Q13. Syntax validation vs business validation?
Syntax validation checks shape and basic constraints, such as a required field or valid email format. Business validation checks rules such as whether an order may be cancelled in its current state.

### Q14. Why should validation errors be consistent?
Clients need a predictable error contract. A stable structure makes it easier to display field errors, log failures, retry safely, and evolve clients independently.

### Q15. Example request DTO
```java
public record CreateOrderRequest(
    @NotBlank String customerId,
    @NotEmpty List<@Valid OrderLineRequest> lines
) {}
```

The controller should reject malformed requests before invoking deeper application logic.

## 5. Exception handling

### Q16. Why use @RestControllerAdvice?
It centralizes translation from application exceptions to HTTP responses. Controllers can focus on the success path while a shared handler establishes a consistent error contract.

### Q17. Should every exception become 500?
No. Expected client errors should be mapped deliberately to 4xx responses. Unexpected failures should generally remain server errors while exposing only safe information to clients.

### Q18. What should an API error contain?
A useful contract normally includes a stable machine-readable error code, human-readable message where appropriate, request/correlation information when useful, and field-level details for validation failures. Do not expose stack traces or sensitive internal details.

### Q19. Should you catch RuntimeException everywhere?
No. Catch exceptions at meaningful boundaries. Catching broad exceptions inside every service method often destroys useful stack information and makes behavior inconsistent.

### Q20. What is Problem Details?
Problem Details is a standardized HTTP error representation that provides fields such as type, title, status, detail, and instance. It can be used as a common contract instead of inventing unrelated error formats.

## 6. Serialization and API contracts

### Q21. Why can changing a JSON field be a breaking change?
Clients may depend on field names, types, nullability, or structure. A rename or incompatible type change can break deserialization even if the server still compiles.

### Q22. How do you evolve a response safely?
Prefer additive changes when possible. Introduce a new field before making a required assumption, maintain old fields during a deprecation window, and communicate contract changes to consumers.

### Q23. How do you handle enum evolution?
Clients that treat enums as exhaustive can break when a new value appears. Define how unknown values should be handled and avoid assuming clients will always understand every future enum member.

## 7. API versioning

### Q24. Do you always need /v1 and /v2?
No. Versioning is a compatibility strategy, not a requirement for every service. If additive evolution preserves the existing contract, a new URI version may create unnecessary operational and client complexity.

### Q25. When is explicit versioning useful?
It helps when a change cannot be made backward-compatible and different consumer populations need distinct contracts. Versioning should have an ownership and retirement plan rather than creating permanent parallel APIs.

### Q26. What is a backward-compatible change?
Examples include adding an optional response field, accepting a broader input set when safe, or adding a new endpoint. Removing/renaming fields or changing semantics is typically more dangerous.

## 8. Pagination, filtering and sorting

### Q27. Why paginate collection endpoints?
Unbounded collections create latency, memory, serialization, and bandwidth problems. Pagination gives the service a predictable work limit and gives clients manageable result sets.

### Q28. Offset vs cursor pagination?
Offset pagination is simple and useful for many administrative screens, but large or frequently changing datasets can suffer from skipped/duplicated records and increasingly expensive offsets. Cursor pagination uses a stable position and is often better for large ordered datasets or feed-like workloads.

### Q29. Why whitelist sort fields?
Allowing arbitrary sort expressions can expose implementation details or create unsafe queries. Define a supported set of sortable fields and map public names to known query attributes.

### Q30. What makes filtering an API design problem?
Filter semantics affect indexes, query cost, caching, backward compatibility, and client expectations. Keep the query contract explicit rather than exposing a generic database query language by accident.

## 9. Idempotency for writes

### Q31. How would you make POST payment creation safe to retry?
Use an idempotency key supplied by the client for the logical operation. Persist the key with the resulting operation state/result and return the existing result for a duplicate key rather than creating a second payment.

### Q32. What makes an idempotency implementation incomplete?
Checking a key only after starting the side effect creates a race. The key and operation state must be coordinated atomically enough that concurrent duplicates cannot both perform the business operation.

### Q33. How long should idempotency keys live?
The retention period should match the client's realistic retry window and the business risk of duplicate operations. There is no universal duration; it is a contract decision.

## 10. Application layering

### Q34. What belongs in a controller?
Protocol concerns: request parsing, boundary validation, authentication/authorization integration, status-code mapping, and calling the application service. Controllers should not become the place where business workflows are implemented.

### Q35. What belongs in a service?
Application/domain orchestration, business rules, transaction boundaries where appropriate, and coordination between collaborators.

### Q36. What belongs in a repository?
Persistence access and query concerns. A repository should not quietly become a second service layer with unrelated business rules.

### Q37. Should service methods return entities?
Not as a rule. Application services should return the model appropriate for their caller. Mapping to API DTOs at the boundary can prevent persistence concerns leaking outward.

## 11. Debugging scenarios

### Scenario: validation errors return different JSON from different controllers
Centralize the HTTP error contract and exception mapping. Search for controller-local exception handling and duplicated response models.

### Scenario: a client breaks after a harmless-looking response refactor
Compare the actual serialized contract, not only Java types. Field names, nullability, enum values, nesting, and date formats are all part of the API.

### Scenario: POST retries create duplicate orders
Check whether the client can retry safely, whether the service has an idempotency mechanism, and whether concurrent duplicate requests are serialized correctly.

### Scenario: an endpoint becomes slow when the dataset grows
Inspect pagination, filtering, sort fields, generated SQL, indexes, response size, and whether the endpoint loads a full collection before slicing it.

### Scenario: a controller contains hundreds of lines
Move business decisions and orchestration into services/domain components. Keep the controller as an HTTP adapter rather than the application's central workflow engine.

## 12. Senior trade-offs

### Entities vs DTOs
Entities are convenient inside persistence, but DTOs provide a stable boundary. The mapping cost is usually justified when an API is public, long-lived, or consumed by multiple teams.

### Versioned endpoints vs compatible evolution
Explicit versions can isolate incompatible contracts but increase maintenance and deployment surface. Prefer compatibility where practical, and version only when the contract genuinely needs to diverge.

### Offset vs cursor pagination
Offset is simpler and often adequate for small or stable datasets. Cursor pagination provides more stable traversal for changing large datasets but requires a carefully designed ordering/key.

### Centralized error handling vs local handling
Centralized handling produces consistency. Local handling is appropriate when a boundary intentionally needs different semantics, but the exception contract should still remain predictable.

## Senior follow-ups

- How would you make an order-creation endpoint safe under network retries?
- Which API changes would you classify as breaking even though Java compilation still succeeds?
- How would you design an error contract used by web, mobile, and partner consumers?
- When would you choose cursor pagination over offset pagination?
- How would you migrate an API from entity responses to DTOs without breaking consumers?
- What belongs in a controller versus an application service?
- How would you detect an accidental N+1 introduced by serialization?
- When is API versioning worth the long-term maintenance cost?

## Definition of Done

A candidate should be able to design a stable Spring API, validate requests, produce consistent errors, separate HTTP contracts from persistence models, handle retries safely, evolve APIs without unnecessary breakage, and explain controller/service/repository boundaries in terms of responsibility rather than convention.
