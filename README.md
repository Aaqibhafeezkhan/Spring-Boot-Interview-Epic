# Spring Boot Interview Epic

A practical, exhaustive Spring Boot interview question bank — written from a developer's point of view, not as a collection of random trivia.

If I am preparing for a Spring Boot interview, these are the areas I want to be ready for: Java fundamentals, Spring itself, Spring Boot, web APIs, data access, security, testing, production concerns, architecture, and the "what actually happens under the hood?" questions that usually separate a basic answer from a strong one.

> **Important:** Spring changes. So do the answers. This repo is intentionally version-aware. Version-sensitive answers should be checked against the current official Spring documentation before being treated as authoritative.

## What this repo covers

- Java fundamentals that matter in Spring applications
- Spring Framework and dependency injection
- Spring Boot fundamentals and auto-configuration
- Configuration, profiles and property binding
- Beans, scopes and the application context
- Spring MVC and REST APIs
- Validation and exception handling
- Spring Data JPA and Hibernate
- Transactions and transaction propagation
- Database performance and N+1 problems
- Spring Security and OAuth2/JWT concepts
- Spring Boot Actuator and observability
- Caching and scheduling
- Async processing
- WebFlux and reactive programming
- Messaging and event-driven applications
- Testing with JUnit, Mockito and Spring test support
- Test slices and integration testing
- Docker, deployment and production configuration
- AOT, native images and modern Spring Boot
- Microservices and distributed-system questions
- System design
- Debugging and production troubleshooting
- Coding / machine-round exercises
- Senior and staff-level questions
- Scenario-based questions
- Rapid-fire revision questions

---

# 1. Java fundamentals for Spring developers

1. What is the difference between JDK, JRE and JVM?
2. How does the JVM execute Java bytecode?
3. What is the difference between a class and an object?
4. Explain encapsulation, inheritance, abstraction and polymorphism.
5. Interface vs abstract class — when would you use each?
6. What is method overloading vs method overriding?
7. What is the difference between `==` and `equals()`?
8. Why must `hashCode()` be consistent with `equals()`?
9. What happens if you override `equals()` but not `hashCode()`?
10. Why are `String` objects immutable?
11. String vs StringBuilder vs StringBuffer.
12. How does the Java String pool work?
13. What are checked and unchecked exceptions?
14. When should you create a custom exception?
15. `final`, `finally` and `finalize` — what is the modern answer?
16. What is try-with-resources?
17. What is the difference between `List`, `Set` and `Map`?
18. ArrayList vs LinkedList.
19. HashMap vs ConcurrentHashMap.
20. How does HashMap work internally?
21. What happens when two keys have the same hash?
22. What makes an object immutable?
23. What are Java records and where are they useful in APIs?
24. What are sealed classes?
25. What are generics and why do they matter in Spring code?
26. What is type erasure?
27. Explain Java streams.
28. Stream vs collection.
29. `map()` vs `flatMap()`.
30. What are intermediate and terminal stream operations?
31. What is lazy evaluation in streams?
32. When should you avoid streams?
33. What is `Optional` and when should you not use it?
34. What is a functional interface?
35. What are lambdas?
36. Explain `CompletableFuture`.
37. What is the difference between concurrency and parallelism?
38. What is a race condition?
39. What is a deadlock?
40. What does `synchronized` actually do?
41. `volatile` vs `synchronized`.
42. What is the Java Memory Model?
43. What are virtual threads and where can they help a Spring Boot application?
44. Platform threads vs virtual threads.
45. When can virtual threads make things worse rather than better?

# 2. Spring Framework fundamentals

46. What problem does Spring solve?
47. What is Inversion of Control?
48. What is Dependency Injection?
49. Constructor injection vs field injection.
50. Why is constructor injection generally preferred?
51. What is the Spring IoC container?
52. `BeanFactory` vs `ApplicationContext`.
53. What is a Spring bean?
54. How does Spring create and manage a bean?
55. What is component scanning?
56. What does `@Component` do?
57. `@Component` vs `@Service` vs `@Repository`.
58. What does `@Controller` do?
59. `@RestController` vs `@Controller`.
60. What is `@Configuration`?
61. What does `@Bean` do?
62. `@Component` vs `@Bean`.
63. What happens when multiple beans have the same type?
64. `@Primary` vs `@Qualifier`.
65. What is `@Fallback` and when is it useful?
66. What are bean scopes?
67. Singleton vs prototype scope.
68. What happens with a prototype bean injected into a singleton?
69. What are request and session scoped beans?
70. What is the Spring bean lifecycle?
71. What are `@PostConstruct` and `@PreDestroy` used for?
72. What is `BeanPostProcessor`?
73. What is `FactoryBean`?
74. What is circular dependency?
75. Why are constructor-based circular dependencies problematic?
76. How would you redesign a circular dependency?
77. What is lazy initialization?
78. What is `@Lazy`?
79. What is an application context hierarchy?
80. What are Spring events?
81. How does `@EventListener` work?
82. What is SpEL?
83. What is Spring AOP?
84. What is a proxy in Spring?
85. JDK dynamic proxy vs CGLIB-style subclass proxy.
86. What are the limitations of proxy-based AOP?
87. What is self-invocation and why can it break advice such as transactions?

# 3. Spring Boot fundamentals

88. What is Spring Boot?
89. Spring Framework vs Spring Boot.
90. What does Spring Boot add on top of Spring?
91. What is convention over configuration?
92. What is `@SpringBootApplication`?
93. What annotations are effectively combined by `@SpringBootApplication`?
94. What is auto-configuration?
95. How does Spring Boot decide which auto-configurations to apply?
96. What is conditional configuration?
97. Explain `@ConditionalOnClass`.
98. Explain `@ConditionalOnMissingBean`.
99. How can you exclude an auto-configuration?
100. How do you debug why an auto-configuration was or was not applied?
101. What is Spring Boot's condition evaluation report?
102. What are Spring Boot starters?
103. Why are starters useful?
104. What is dependency management in Spring Boot?
105. What is the Spring Boot parent POM?
106. Can you use Spring Boot without the parent POM?
107. Maven vs Gradle for a Spring Boot project.
108. What does the Spring Boot Maven/Gradle plugin do?
109. What is an executable jar?
110. How does embedded Tomcat/Jetty work?
111. What is `SpringApplication`?
112. What happens when `SpringApplication.run()` is called?
113. What is `CommandLineRunner`?
114. `CommandLineRunner` vs `ApplicationRunner`.
115. What are startup failures and how do you diagnose them?

# 4. Configuration and profiles

116. `application.properties` vs `application.yaml`.
117. How does Spring Boot resolve configuration properties?
118. What are configuration property sources?
119. What are environment variables used for?
120. What is externalized configuration?
121. What is `@Value`?
122. Why might `@ConfigurationProperties` be preferable to lots of `@Value` fields?
123. How does `@ConfigurationProperties` work?
124. How do you validate configuration properties?
125. What are profiles?
126. How do you activate a profile?
127. `@Profile` vs profile-specific configuration files.
128. How would you structure dev, test and production configuration?
129. How should secrets be handled?
130. What should never be committed to Git?
131. How would you troubleshoot a property that appears to be ignored?
132. How do environment variables override configuration?
133. What is configuration metadata?
134. How can custom configuration properties get IDE metadata?

# 5. Spring MVC and REST APIs

135. How does a request travel through Spring MVC?
136. What is `DispatcherServlet`?
137. What is a controller?
138. `@RequestMapping` vs `@GetMapping`, `@PostMapping`, etc.
139. `@PathVariable` vs `@RequestParam`.
140. `@RequestBody` vs `@ModelAttribute`.
141. How does JSON request deserialization work?
142. How does response serialization work?
143. What is `HttpMessageConverter`?
144. How do you return different HTTP status codes?
145. `ResponseEntity` vs returning a DTO directly.
146. Why should APIs usually return DTOs rather than JPA entities?
147. How should REST endpoints be designed?
148. PUT vs PATCH.
149. POST vs PUT.
150. What does idempotency mean?
151. Which HTTP methods are idempotent?
152. What is content negotiation?
153. What is CORS?
154. CORS vs CSRF.
155. How do you configure CORS in Spring?
156. What is global exception handling?
157. How does `@ControllerAdvice` work?
158. `@ExceptionHandler` vs `@RestControllerAdvice`.
159. How would you design a consistent API error response?
160. What is RFC 7807 / Problem Details?
161. How should validation errors be returned?
162. What is `@Valid`?
163. `@Valid` vs `@Validated`.
164. How do validation groups work?
165. How do you create a custom Bean Validation constraint?
166. How would you version an API?
167. URI versioning vs header/media-type versioning.
168. How would you implement pagination?
169. Offset pagination vs cursor/keyset pagination.
170. How would you implement sorting and filtering safely?
171. How do you prevent mass assignment / over-posting?
172. How do you handle file uploads?
173. How do you stream a large response without loading everything into memory?

# 6. Spring Data JPA and Hibernate

174. What is Spring Data JPA?
175. JPA vs Hibernate vs Spring Data JPA.
176. What is an entity?
177. What does `@Entity` do?
178. What is an entity identifier?
179. `@Id` and generated identifiers.
180. Entity lifecycle states in JPA.
181. What is a persistence context?
182. What is the first-level cache?
183. What is dirty checking?
184. What is flush?
185. `save()` vs `saveAndFlush()`.
186. What is the second-level cache?
187. Lazy vs eager loading.
188. Why is eager loading not automatically the solution to N+1?
189. What is the N+1 query problem?
190. How do you detect N+1 queries?
191. How do you fix N+1 queries?
192. `JOIN FETCH` vs `@EntityGraph`.
193. What is an entity graph?
194. What are derived query methods?
195. JPQL vs native SQL.
196. When would you use a native query?
197. What is a projection?
198. Interface projection vs DTO projection.
199. What are JPA relationships?
200. `@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`.
201. Owning side vs inverse side of a relationship.
202. What does `mappedBy` mean?
203. Cascade types.
204. What does `orphanRemoval` mean?
205. Why can `CascadeType.ALL` be dangerous?
206. Why is `@ManyToMany` often a design smell in complex domains?
207. How would you model a many-to-many relationship with extra attributes?
208. What is optimistic locking?
209. What is `@Version`?
210. Optimistic vs pessimistic locking.
211. How do database indexes affect JPA queries?
212. How do you diagnose a slow JPA query?
213. What is the Open Session in View pattern?
214. Why can Open Session in View hide bad query design?

# 7. Transactions

215. What is a database transaction?
216. What does `@Transactional` do?
217. How does Spring implement declarative transactions?
218. What is transaction propagation?
219. Explain REQUIRED.
220. Explain REQUIRES_NEW.
221. Explain SUPPORTS, NOT_SUPPORTED and NEVER.
222. What is transaction isolation?
223. READ_COMMITTED vs REPEATABLE_READ vs SERIALIZABLE.
224. What are dirty reads, non-repeatable reads and phantom reads?
225. What is a transaction rollback?
226. Which exceptions cause Spring transactions to roll back by default?
227. How do you configure rollback rules?
228. Why can `@Transactional` appear not to work?
229. What is self-invocation in a transactional service?
230. Should `@Transactional` be placed on a controller, service or repository?
231. Can you use `@Transactional` on private methods?
232. What happens when a transactional method calls another transactional method?
233. What is read-only transaction semantics?
234. Can a read-only transaction guarantee that no writes happen?
235. How would you handle a transaction that spans multiple services?
236. Why are distributed transactions difficult?
237. What is the outbox pattern?
238. What is eventual consistency?

# 8. Spring Security

239. What problem does Spring Security solve?
240. Authentication vs authorization.
241. What is the Spring Security filter chain?
242. How does a request get authenticated?
243. What is `SecurityContext`?
244. What is `SecurityContextHolder`?
245. What is `UserDetailsService`?
246. What is a `PasswordEncoder`?
247. Why should passwords never be stored directly?
248. What is CSRF?
249. When does CSRF matter for a browser application?
250. Why is CSRF handling different for cookie-based sessions and bearer-token APIs?
251. Session-based authentication vs stateless JWT authentication.
252. What is a JWT?
253. What should and should not be stored in a JWT?
254. How do you validate a JWT?
255. Access token vs refresh token.
256. How do you revoke a JWT?
257. OAuth 2.0 vs OpenID Connect.
258. Resource server vs authorization server.
259. What is a bearer token?
260. What is method-level security?
261. `@PreAuthorize` vs `@Secured`.
262. How would you secure a REST API?
263. How do you handle CORS with Spring Security?
264. How would you prevent privilege escalation?
265. What is security filter ordering and why does it matter?
266. How do you diagnose a 401 vs 403?
267. How would you secure actuator endpoints?

# 9. Actuator and production observability

268. What is Spring Boot Actuator?
269. What are actuator endpoints?
270. What is `/health` used for?
271. What is readiness vs liveness?
272. How would you expose actuator endpoints safely?
273. What information should never be publicly exposed through actuator?
274. What is Micrometer?
275. What is an application metric?
276. Counter vs gauge vs timer.
277. What is distributed tracing?
278. What is a trace ID?
279. How would you correlate logs across microservices?
280. What is structured logging?
281. How would you investigate a production latency spike?
282. How would you investigate high CPU?
283. How would you investigate high memory usage?
284. How would you investigate database connection pool exhaustion?
285. How would you investigate thread pool exhaustion?
286. What is a health indicator?
287. How would you create a custom actuator endpoint or health indicator?

# 10. Caching

288. What is caching?
289. What does Spring's cache abstraction provide?
290. What does `@Cacheable` do?
291. `@CachePut` vs `@CacheEvict`.
292. What makes a good cache key?
293. What is cache stampede?
294. What is cache penetration?
295. What is cache avalanche?
296. Local cache vs distributed cache.
297. When would you use Redis?
298. How do you invalidate cached data correctly?
299. What consistency problems can caching introduce?

# 11. Async and scheduling

300. What does `@Async` do?
301. What executor does async work use?
302. Why should you configure executors explicitly in production?
303. How do you return results from asynchronous work?
304. How do exceptions propagate from `@Async` methods?
305. Why can self-invocation break `@Async`?
306. What does `@Scheduled` do?
307. Fixed rate vs fixed delay vs cron.
308. How do you prevent duplicate scheduled jobs across multiple application instances?
309. When would you use a distributed scheduler or job system instead?

# 12. WebFlux and reactive Spring

310. What is Spring WebFlux?
311. Spring MVC vs WebFlux.
312. What problem does reactive programming solve?
313. What are `Mono` and `Flux`?
314. What is backpressure?
315. What is non-blocking I/O?
316. Why is putting blocking JPA code into WebFlux a problem?
317. When should you choose MVC instead of WebFlux?
318. What is `WebClient`?
319. `RestClient` vs `WebClient`.
320. How do you handle timeouts in reactive applications?
321. What is reactive context?
322. How do you debug reactive pipelines?

# 13. Messaging and event-driven applications

323. Why use asynchronous messaging?
324. Queue vs topic.
325. Kafka vs traditional message queues.
326. What is at-least-once delivery?
327. What is at-most-once delivery?
328. What does exactly-once mean in practice?
329. What is idempotent message processing?
330. How do you make a consumer idempotent?
331. What happens when message processing fails?
332. What is a dead-letter queue/topic?
333. What is consumer lag?
334. What is the outbox pattern?
335. What is the inbox/idempotency pattern?
336. Event-driven architecture vs request-response architecture.
337. How would you handle schema evolution for events?
338. How would you debug a message that appears to have disappeared?

# 14. Testing

339. Unit test vs integration test.
340. What is Spring's TestContext Framework?
341. What does `@SpringBootTest` do?
342. When should you avoid `@SpringBootTest`?
343. What are test slices?
344. What does `@WebMvcTest` test?
345. What does `@DataJpaTest` test?
346. What is `MockMvc`?
347. When would you use `WebTestClient`?
348. Mockito mock vs spy.
349. What makes a unit test valuable?
350. What makes a test brittle?
351. How do you test exception handling?
352. How do you test validation?
353. How do you test database interactions?
354. How do Testcontainers help?
355. Why can H2-based tests give false confidence?
356. How would you test a transactional service?
357. How do you test security rules?
358. How do you test an external API integration?
359. Contract testing vs integration testing.
360. How do you keep a large Spring test suite fast?

# 15. Docker and deployment

361. How do you containerize a Spring Boot application?
362. JAR vs WAR deployment.
363. What should a production Dockerfile for Spring Boot consider?
364. What is a multi-stage Docker build?
365. How should configuration be passed into a container?
366. How should secrets be passed into production?
367. How do you expose a Spring Boot application from Docker?
368. What is graceful shutdown?
369. How do you configure JVM memory inside a container?
370. What is a readiness probe?
371. What is a liveness probe?
372. How would you deploy a Spring Boot application with zero/minimal downtime?
373. What happens during a rolling deployment?
374. How do you handle database migrations during deployments?
375. Flyway vs Liquibase.

# 16. AOT, native images and modern Spring Boot

376. What is AOT processing?
377. Why does AOT matter for Spring applications?
378. What is GraalVM Native Image?
379. JVM deployment vs native executable.
380. What trade-offs come with native images?
381. What is runtime reflection and why can it matter for native images?
382. What is Spring's AOT engine doing?
383. What is checkpoint/restore and where does it fit?
384. When would you choose a native image?
385. When would you stick with a normal JVM deployment?

# 17. Spring Boot 4.x / current-version questions

386. What changed when moving from Spring Boot 3.x to 4.x?
387. What Spring Framework version does Spring Boot 4.x use?
388. What Java baseline does current Spring Boot require?
389. What changed around Jakarta APIs?
390. What changed around Jackson in Spring Boot 4?
391. What happened to Undertow support in Spring Boot 4?
392. What changes are relevant when upgrading a Boot 3.5 application to Boot 4?
393. What dependency or package changes can break an existing application?
394. How would you approach a major Spring Boot upgrade safely?
395. How would you verify that an upgrade did not silently change runtime behavior?

> Version-sensitive answers in this section should always be checked against the current Spring Boot release documentation. Do not memorize an old version's answer and assume it still applies.

# 18. Microservices

396. Monolith vs modular monolith vs microservices.
397. When should you NOT use microservices?
398. How do you identify service boundaries?
399. What is bounded context?
400. Synchronous vs asynchronous service communication.
401. REST vs messaging between services.
402. What is service discovery?
403. What is an API gateway?
404. What is a circuit breaker?
405. What is retry with backoff?
406. Why can retries make an outage worse?
407. What is a bulkhead pattern?
408. What is a timeout budget?
409. What is rate limiting?
410. How do you handle distributed tracing?
411. How do you handle distributed transactions?
412. Saga vs two-phase commit.
413. What is eventual consistency?
414. How do you make a distributed operation idempotent?
415. How do you version microservice APIs?
416. How do you evolve event schemas safely?
417. How do you handle partial failure?
418. How would you debug a request crossing five services?

# 19. Performance

419. How would you profile a slow Spring Boot application?
420. What causes high application latency?
421. How do connection pools affect performance?
422. What is HikariCP?
423. How do you choose database pool sizes?
424. Why can increasing a thread pool make performance worse?
425. How do you detect thread starvation?
426. How do you detect memory leaks?
427. Heap vs stack memory.
428. What causes excessive garbage collection?
429. How do you investigate GC pressure?
430. How would you optimize a slow API endpoint?
431. How do you distinguish application latency from database latency?
432. How can serialization become a bottleneck?
433. How can logging hurt application performance?
434. How do you benchmark a Spring Boot endpoint correctly?

# 20. Security and production hardening

435. How do you secure secrets in production?
436. How do you prevent SQL injection with Spring Data/JPA?
437. What is input validation?
438. Validation vs sanitization.
439. How do you prevent insecure direct object references?
440. How do you prevent excessive data exposure from APIs?
441. How do you protect actuator endpoints?
442. How do you configure security headers?
443. How do you prevent brute-force attacks?
444. How should passwords be hashed?
445. Why is encryption not the same as password hashing?
446. How do you rotate signing keys?
447. How do you handle expired access tokens?
448. How do you secure service-to-service communication?
449. What is mTLS?
450. What security checks belong in CI/CD?

# 21. Debugging scenarios

451. The application starts locally but fails in production. What do you check first?
452. A bean suddenly cannot be autowired. How do you debug it?
453. Two beans now match the same interface. What happened?
454. `@Transactional` is not rolling back. Why?
455. A query works in development but times out in production. How do you investigate?
456. CPU jumps to 100%. Walk through your investigation.
457. Memory usage continuously increases. What do you inspect?
458. API latency doubles after a release. How do you narrow it down?
459. Database connections are exhausted. What could cause it?
460. Users receive intermittent 401 responses. What do you investigate?
461. A scheduled job executes twice. Why?
462. Kafka consumers are falling behind. What do you check?
463. Requests are timing out only under load. What are the likely bottlenecks?
464. A service is healthy but traffic still fails. What does that tell you?
465. A deployment passes CI but fails at runtime. How do you approach it?

# 22. System design questions

466. Design a URL shortener using Spring Boot.
467. Design a notification service.
468. Design an order management service.
469. Design a payment service.
470. Design an authentication service.
471. Design a rate limiter.
472. Design a file upload service.
473. Design a distributed job scheduler.
474. Design a ticketing system.
475. Design a news aggregation backend.
476. Design a social media backend.
477. Design a real-time chat backend.
478. Design an audit logging platform.
479. Design an API gateway.
480. Design a feature flag service.
481. Design a metrics ingestion service.
482. Design a scalable search API.
483. Design an event-driven order workflow.
484. Design a multi-tenant SaaS backend.
485. Design a high-volume reporting API.

For each design question, be prepared to discuss:

- Requirements
- APIs
- Data model
- Service boundaries
- Storage choice
- Caching
- Concurrency
- Transactions
- Failure modes
- Idempotency
- Security
- Observability
- Scaling strategy
- Deployment strategy
- Trade-offs

# 23. Coding / machine-round exercises

486. Build a CRUD REST API.
487. Add request validation.
488. Add global exception handling.
489. Add pagination and sorting.
490. Implement JWT authentication.
491. Implement role-based authorization.
492. Implement optimistic locking.
493. Fix an N+1 query problem.
494. Add a cache to an expensive endpoint.
495. Implement retry with backoff.
496. Add an idempotency mechanism.
497. Implement an asynchronous processing flow.
498. Build a scheduled cleanup job.
499. Add integration tests with Testcontainers.
500. Add API documentation.
501. Add health and readiness endpoints.
502. Add metrics and tracing.
503. Containerize the application.
504. Add database migrations.
505. Diagnose a deliberately broken Spring application.

# 24. Senior-level questions

506. Why did you choose Spring Boot for your last project?
507. What part of the Spring stack have you actually debugged under the hood?
508. What is the most difficult production issue you have solved in Spring Boot?
509. Tell me about a performance problem you fixed.
510. Tell me about a database problem you fixed.
511. Tell me about a security issue you prevented or fixed.
512. Tell me about a bad architectural decision you would make differently now.
513. How do you decide whether logic belongs in a controller, service or repository?
514. How do you prevent a service layer from becoming a giant god class?
515. How do you structure a large Spring Boot codebase?
516. How do you keep business logic independent from framework code?
517. How do you review Spring Boot code written by another developer?
518. What Spring conventions do you deliberately avoid?
519. Which Spring features are commonly overused?
520. When would you choose a plain Java solution instead of a Spring abstraction?

# 25. Staff / architecture-level questions

521. How would you define engineering standards for Spring Boot services across an organization?
522. How would you create a common Spring Boot platform without creating a dependency nightmare?
523. How would you standardize observability across hundreds of services?
524. How would you manage framework upgrades across a large organization?
525. How would you migrate a large monolith toward modular architecture?
526. How would you decide whether a service should be extracted?
527. How would you design platform-level authentication and authorization?
528. How would you design a resilient service-to-service communication standard?
529. How would you define SLOs for Spring Boot services?
530. How would you build a production readiness checklist?
531. How would you balance developer productivity against framework standardization?
532. How would you reduce operational complexity in a large Spring ecosystem?

# 26. Rapid-fire revision

533. Spring vs Spring Boot?
534. IoC vs DI?
535. Bean vs component?
536. `@Component` vs `@Bean`?
537. `@Service` vs `@Repository`?
538. `@Controller` vs `@RestController`?
539. `@Primary` vs `@Qualifier`?
540. Singleton vs prototype?
541. Lazy vs eager?
542. `@Value` vs `@ConfigurationProperties`?
543. `@Valid` vs `@Validated`?
544. PUT vs PATCH?
545. Authentication vs authorization?
546. 401 vs 403?
547. Session vs JWT?
548. CSRF vs CORS?
549. JPA vs Hibernate?
550. Lazy vs eager loading?
551. `save()` vs `saveAndFlush()`?
552. Optimistic vs pessimistic locking?
553. `@Transactional` propagation?
554. MVC vs WebFlux?
555. Mono vs Flux?
556. Unit vs integration test?
557. `@SpringBootTest` vs test slices?
558. Liveness vs readiness?
559. Cache vs database?
560. Retry vs circuit breaker?
561. Synchronous vs asynchronous messaging?
562. At-least-once vs exactly-once?
563. Monolith vs microservices?
564. JVM vs native image?

# 27. How I would actually prepare

Don't try to memorize all 560+ questions.

A better approach is to work through the repo in layers:

### Round 1 — Fundamentals

Java → Spring Core → Spring Boot → REST → JPA → Transactions.

### Round 2 — Real application development

Security → validation → testing → configuration → caching → messaging.

### Round 3 — Production

Actuator → observability → performance → Docker → deployment → troubleshooting.

### Round 4 — Senior interviews

Architecture → microservices → distributed systems → system design → trade-offs.

### Round 5 — Your own projects

For every project on your resume, be able to answer:

- Why did you choose this architecture?
- Why Spring Boot?
- Why this database?
- What were the bottlenecks?
- What failed in production?
- What would you change today?
- How did you test it?
- How did you secure it?
- How did you deploy it?
- How would you scale it 10x?

Those questions are often more important than another 50 annotation trivia questions.

---

# Version and verification policy

This repository is intended to stay useful as Spring evolves.

For version-sensitive questions, verify against the official documentation before updating an answer. In particular, check:

- Spring Boot reference documentation
- Spring Framework reference documentation
- Spring Boot release notes
- Spring Security reference documentation
- Spring Data documentation
- Java documentation for JVM/language features

The current Spring Boot documentation lists **4.1.1** as the latest stable release, with **4.0.8** and **3.5.16** also maintained. Current Spring Framework documentation lists **7.0.9** and **6.2.19** as stable lines. Spring Boot 4.1.1 requires at least Java 17 and supports Java versions through 26. These values are intentionally kept explicit here because they will change. 

Spring Boot 4 also contains meaningful upgrade differences from the 3.x line. For example, Boot 4 uses Jackson 3 as its preferred JSON library, and Undertow support was removed because Boot 4 moved to a Servlet 6.1 baseline. Treat upgrade questions as version-specific rather than assuming a single timeless answer.

---

## Official references

- [Spring Boot Reference Documentation](https://docs.spring.io/spring-boot/reference/)
- [Spring Framework Reference Documentation](https://docs.spring.io/spring-framework/reference/)
- [Spring Boot System Requirements](https://docs.spring.io/spring-boot/system-requirements.html)
- [Spring Boot Release Notes](https://github.com/spring-projects/spring-boot/wiki)

## A note on answers

The next step for this repo is to turn the question bank into **verified answers**, not to blindly generate an answer under every question.

For each answer, the standard should be:

1. Explain it in plain developer language.
2. Give a small practical example where useful.
3. Mention the common misconception.
4. Call out version-specific behavior when it matters.
5. Prefer official Spring/Java documentation as the source of truth.
6. Avoid outdated interview folklore.

That is the difference between a list of interview questions and an interview resource I would actually trust.
