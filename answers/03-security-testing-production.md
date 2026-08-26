# Security + Testing + Production — Questions & Answers

## Spring Security

### 1. Authentication vs authorization?
**Answer:** Authentication answers **who are you?** Authorization answers **what are you allowed to do?** A user can be authenticated successfully and still receive 403 because they are not authorized for the resource.

### 2. What is the Spring Security filter chain?
**Answer:** HTTP requests pass through a chain of security filters before reaching application code. Those filters can handle authentication, authorization, CSRF, security headers, exception handling and other security concerns.

### 3. What is `SecurityContext`?
**Answer:** It holds the security information for the current execution, including the authenticated principal and authorities. How it is associated with the current request/thread depends on the application model.

### 4. What is `UserDetailsService`?
**Answer:** It is a strategy for loading user information by username. It is not itself an authentication mechanism; an authentication provider can use it as part of verifying credentials.

### 5. Why use a `PasswordEncoder`?
**Answer:** Passwords should be stored as slow, salted password hashes rather than plaintext or reversible encryption. Spring Security's password encoder abstraction lets the application use an appropriate password-hashing algorithm without hard-coding the hashing implementation everywhere.

### 6. What is CSRF?
**Answer:** Cross-Site Request Forgery tricks a user's browser into making an authenticated request that the user did not intend. It is particularly relevant when authentication credentials are automatically attached by the browser, such as session cookies.

### 7. Is CSRF irrelevant for JWT APIs?
**Answer:** Not automatically. If a bearer token is manually supplied in an authorization header and is not automatically attached by the browser, the classic cookie-based CSRF scenario is different. If the token is stored in a cookie and automatically sent, CSRF remains relevant.

### 8. Session authentication vs JWT?
**Answer:** Session authentication normally keeps server-side session state and sends a session identifier to the client. JWT authentication puts signed claims in a token that the resource server can validate. JWTs can reduce server-side session state but introduce token lifetime, revocation and key-management considerations.

### 9. What is a JWT?
**Answer:** A JWT is a signed token containing claims. A signature protects integrity; it does not automatically encrypt the contents. Never put secrets in ordinary JWT claims just because the token is signed.

### 10. What should go in a JWT?
**Answer:** Only claims the resource server genuinely needs, such as subject, issuer, audience, expiry and carefully chosen authorization information. Keep it small and avoid sensitive data.

### 11. Access token vs refresh token?
**Answer:** An access token is used to access protected resources and should generally have a relatively short lifetime. A refresh token is used to obtain new access tokens and needs stronger protection and lifecycle controls.

### 12. Can you revoke a JWT?
**Answer:** A self-contained JWT normally remains valid until expiry unless the server adds state or changes the signing/key strategy. Practical revocation strategies include short-lived access tokens, refresh-token rotation/revocation, token introspection, or maintaining a deny-list where the operational cost is justified.

### 13. OAuth 2.0 vs OpenID Connect?
**Answer:** OAuth 2.0 is an authorization framework. OpenID Connect builds an identity layer on OAuth 2.0 and defines how clients can authenticate users and receive identity information.

### 14. What is a resource server?
**Answer:** It is the application/API that receives protected requests and validates access tokens before allowing access to resources.

### 15. 401 vs 403?
**Answer:** 401 means the request lacks valid authentication credentials or authentication failed. 403 means the request is understood but the authenticated principal is not allowed to perform the operation.

### 16. What is method-level security?
**Answer:** It protects individual service/controller methods based on the current authentication and authorities. Annotations such as `@PreAuthorize` can express authorization rules close to the operation they protect.

### 17. How would you secure a REST API?
**Answer:** Start with HTTPS, strong authentication, explicit authorization, safe password/token handling, input validation, least privilege, secure headers, appropriate CORS/CSRF decisions, rate limiting where needed, safe error handling, dependency patching and monitoring. Security is a system property, not one annotation.

## Testing

### 18. Unit vs integration vs end-to-end tests?
**Answer:** A unit test isolates a small piece of code. An integration test verifies multiple components working together, often including infrastructure such as a database. An end-to-end test validates a larger user/business flow through the deployed system. Good test suites use all three at appropriate boundaries.

### 19. What is Mockito good for?
**Answer:** Mockito is useful for isolating a unit from collaborators and controlling their behavior. It should not become a way to mock every class in the application; over-mocking can make tests verify implementation details instead of behavior.

### 20. What is a Spring test slice?
**Answer:** A test slice loads only part of the Spring application context for a focused test, such as MVC or JPA behavior. This is faster and makes the test's boundary clearer than loading the entire application for every test.

### 21. When would you use `@SpringBootTest`?
**Answer:** When you actually need broad application-context integration testing. It is powerful but relatively expensive, so using a narrower test slice or plain unit test is often better for focused tests.

### 22. Why use Testcontainers?
**Answer:** Testcontainers lets tests run against real infrastructure in disposable containers. It is especially valuable for databases, brokers and other systems where an in-memory fake behaves differently from production.

### 23. What makes a good integration test?
**Answer:** It verifies a meaningful boundary, controls its data, is deterministic, cleans up after itself, and tests behavior rather than internal implementation. If a test is slow and flaky, the answer is not always to delete it; first identify the environment or synchronization problem.

## Production

### 24. What is Actuator?
**Answer:** Spring Boot Actuator exposes production-oriented endpoints and instrumentation for things such as health, metrics and application information. The important production question is not only how to enable it, but what you expose and who can access it.

### 25. Liveness vs readiness?
**Answer:** Liveness answers whether the application process should be restarted. Readiness answers whether the application is currently able to receive traffic. A dependency outage should not automatically be treated as a reason to restart a healthy process.

### 26. What is Micrometer?
**Answer:** Micrometer provides a vendor-neutral instrumentation API for metrics. Spring applications can record counters, gauges and timers and export them to monitoring systems through supported registries.

### 27. Counter vs gauge vs timer?
**Answer:** A counter measures a value that increases, such as requests processed. A gauge represents a current value, such as queue depth. A timer measures durations and can also expose counts and distributions.

### 28. How would you investigate high latency?
**Answer:** Start with the request path and a time window. Check application latency metrics, traces, logs, thread pools, connection pools, downstream calls and database timings. Break the total latency into components instead of guessing that the controller is slow.

### 29. How would you investigate high CPU?
**Answer:** Determine which process/container and which threads are consuming CPU. Capture thread dumps/profiles, look for hot methods, excessive serialization, tight loops, expensive queries or runaway retries, then correlate the behavior with a deployment or traffic change.

### 30. How would you investigate high memory?
**Answer:** Check heap usage and GC behavior, container limits, allocation rate and retained objects. A heap dump/profile can identify objects keeping memory alive. Also check off-heap/native memory when heap metrics don't explain the container's usage.

### 31. What is connection pool exhaustion?
**Answer:** The application has more concurrent database work than the connection pool can serve within the configured timeout. Causes include slow queries, leaked/unclosed work, long transactions, too-small pools or traffic exceeding the database's capacity. Increasing the pool blindly can make the database failure worse.

### 32. What is thread-pool exhaustion?
**Answer:** All worker threads are busy or queued work is growing faster than it can be processed. Look for blocked I/O, slow downstream services, insufficient pool sizing, deadlocks and unbounded queues. Thread pools are resource controls, not infinite capacity.

### 33. How do you handle graceful shutdown?
**Answer:** Stop accepting new work, allow in-flight work to complete within a deadline, and close resources cleanly. In a containerized deployment, this must be coordinated with readiness and the orchestrator's termination grace period.

### 34. What should never be logged?
**Answer:** Passwords, access/refresh tokens, private keys, session secrets and unnecessary personal or financial data. Even useful identifiers should be handled carefully because logs often have broad access and long retention.

### 35. How do you debug a production 500?
**Answer:** Correlate the request using a trace/request ID, find the server-side exception and stack trace, identify the failing dependency, check whether the failure is isolated or systemic, and compare with recent deployments/configuration changes. Don't ask the client to send stack traces back and forth if your observability should already have them.

## Sources

- [Spring Security reference](https://docs.spring.io/spring-security/reference/)
- [Spring Security features](https://docs.spring.io/spring-security/reference/7.0/features/index.html)
- [Spring Security migration](https://docs.spring.io/spring-security/reference/7.0/migration/index.html)
- [Spring Boot documentation](https://docs.spring.io/spring-boot/reference/)
