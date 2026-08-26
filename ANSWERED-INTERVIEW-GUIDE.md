# Spring Boot Interview Epic — Answered Guide

The original question list is only the index. **This is the part you actually study.**

A question without an answer is just a checklist. The goal of this repository is to build a practical interview reference where every important question eventually has:

- a direct answer you can say in an interview
- the reasoning behind it
- common follow-up questions
- production caveats where they matter
- version notes when Spring/Java behavior changes
- official sources for claims that are easy to get stale

## Answered sections

| Area | Status |
|---|---|
| Java + Spring Core | ✅ Answered |
| Spring Boot + Web + Data | ✅ Answered |
| Spring Security + Testing + Production | ✅ Answered |
| Caching + Async + Scheduling | 🚧 Expand next |
| WebFlux / Reactive | 🚧 Expand next |
| Messaging / Kafka / Events | 🚧 Expand next |
| Docker / Deployment / AOT / Native | 🚧 Expand next |
| Microservices / Distributed Systems | 🚧 Expand next |
| System Design | 🚧 Expand next |
| Coding / Machine Rounds | 🚧 Expand next |
| Senior / Staff / Scenario Questions | 🚧 Expand next |

## Answer files

- [Java + Spring Core](answers/01-java-and-spring-core.md)
- [Spring Boot + Web + Data](answers/02-boot-web-data.md)
- [Security + Testing + Production](answers/03-security-testing-production.md)

## How the answers are written

I'm not trying to write textbook definitions here. A good interview answer should usually start with the **one or two sentences I'd actually say out loud**, then go deeper if the interviewer asks a follow-up.

For example, for `@Transactional`, saying *"it manages transactions"* is not enough. You should be ready for:

1. How does it work?
2. Is it proxy-based?
3. What happens on self-invocation?
4. What is the default rollback behavior?
5. What are propagation levels?
6. What is isolation?
7. Where should the boundary live?
8. What happens when another service is called inside the transaction?
9. What happens when the database commits but the Kafka publish fails?
10. How would you debug a transaction that appears not to work?

That is the standard I want for the whole repo.

## The actual target

This should eventually cover **everything that can reasonably show up in a Spring Boot interview**, not just annotation trivia:

### Java

- OOP
- collections
- generics
- streams
- functional programming
- exceptions
- JVM basics
- memory model
- garbage collection
- concurrency
- executors
- `CompletableFuture`
- virtual threads
- records and sealed types
- modern Java language features

### Spring

- IoC / DI
- bean creation
- bean lifecycle
- scopes
- configuration
- component scanning
- conditional beans
- profiles
- events
- AOP
- proxies
- transactions
- caching
- scheduling
- async execution

### Spring Boot

- auto-configuration
- starters
- dependency management
- application startup
- configuration binding
- external configuration
- embedded servers
- Actuator
- observability
- AOT
- native images
- version migrations

### Web

- HTTP
- REST
- MVC request lifecycle
- filters
- interceptors
- validation
- exception handling
- Problem Details
- pagination
- idempotency
- CORS
- CSRF
- content negotiation
- file upload/download
- streaming
- API versioning
- rate limiting

### Data

- SQL fundamentals
- JPA
- Hibernate
- persistence context
- dirty checking
- fetch strategies
- N+1
- entity graphs
- projections
- locking
- indexes
- transactions
- connection pools
- migrations
- database performance

### Security

- authentication
- authorization
- filter chain
- sessions
- JWT
- OAuth2
- OIDC
- resource servers
- authorization servers
- password hashing
- CSRF
- CORS
- security headers
- method security
- least privilege
- common API vulnerabilities

### Production

- logging
- metrics
- tracing
- health checks
- liveness/readiness
- graceful shutdown
- thread pools
- connection pools
- memory
- CPU
- GC
- latency
- retries
- timeouts
- circuit breakers
- backpressure
- incident debugging

### Distributed systems

- microservices
- service discovery
- API gateways
- synchronous vs asynchronous communication
- Kafka
- messaging semantics
- retries
- dead-letter queues
- idempotent consumers
- ordering
- eventual consistency
- outbox pattern
- sagas
- distributed locks
- distributed transactions
- caching

### Interview reality

Also include the questions interviewers ask after seeing your project/resume:

- Why did you choose Spring Boot?
- Why this architecture?
- Why JPA instead of JDBC?
- Why did you choose this database?
- What was the hardest production bug you fixed?
- What happened when traffic increased?
- How did you improve an endpoint from 2 seconds to 200 ms?
- How did you handle duplicate requests?
- How did you handle concurrent updates?
- How did you secure the API?
- How did you monitor it?
- What would you redesign today?
- What trade-off did you knowingly make?
- What would break first at 10x traffic?

These are often more valuable than another 50 annotation questions.

## Verification standard

Spring Boot and the surrounding Spring ecosystem are versioned products. This repository should not quietly repeat old interview answers as if they were current facts.

For version-sensitive material, prefer official documentation and explicitly identify the relevant major/minor line. At the time this guide was updated, the official Spring Boot documentation lists 4.1.1, 4.0.8 and 3.5.16 as stable lines, while Spring Security lists 7.1.1 as its latest stable release. Always re-check the official release documentation before an interview where version details matter.

### Official references

- https://docs.spring.io/spring-boot/reference/
- https://docs.spring.io/spring-boot/system-requirements.html
- https://docs.spring.io/spring-framework/reference/
- https://docs.spring.io/spring-security/reference/
- https://docs.spring.io/spring-data/jpa/reference/

## Rule for contributors

**Do not add a question just to make the number bigger.**

If an answer is uncertain, version-dependent or based on folklore, investigate it before committing it. If there are two valid approaches, explain the trade-off instead of pretending one is universally correct.

The goal is not *"the biggest Spring Boot question list on GitHub."*

The goal is:

> **If I have a Spring Boot interview tomorrow, I should be able to use this repo to prepare for almost anything the interviewer can reasonably throw at me — and trust the answers.**
