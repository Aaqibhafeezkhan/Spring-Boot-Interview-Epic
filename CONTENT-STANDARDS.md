# Spring Boot Interview Content Standard

## Purpose

This repository is a senior-level interview reference. New material should help a candidate explain not only what something is, but why it exists, how it behaves, when it fails, and what trade-offs an experienced engineer considers.

## Canonical answer shape

Use this structure for substantive answers when the topic benefits from depth:

1. **Interview answer** — 2–5 sentences that can be spoken clearly in an interview.
2. **How it works** — the mechanism or lifecycle behind the answer.
3. **Practical example** — a small Spring/Java example when code makes the idea concrete.
4. **Trade-offs and caveats** — alternatives, failure modes, version-sensitive details, or operational implications.
5. **Senior follow-up** — one or two questions an interviewer could naturally ask next.

Not every rapid-fire question needs every section. Avoid padding short factual answers just to satisfy a template.

## Content levels

- **Foundation:** definitions, core mechanisms, basic examples.
- **Practical:** production usage, configuration, common mistakes, debugging.
- **Senior:** trade-offs, failure modes, concurrency, data consistency, security, operations, architecture.
- **Staff/system design:** boundaries, scalability, reliability, organizational constraints, migration strategy, cost and observability.

Answers should make the expected depth obvious rather than mixing beginner and staff-level discussion into one undifferentiated paragraph.

## Topic ownership

Keep related questions together under the existing README topic taxonomy:

- Java/JVM
- Spring Core
- Spring Boot
- Configuration
- MVC/REST APIs
- JPA/Hibernate
- Transactions
- Security
- Observability/Actuator
- Caching
- Async/Scheduling
- Reactive/WebFlux
- Messaging
- Testing
- Deployment/production
- Microservices/distributed systems
- Performance
- System design
- Debugging/scenarios
- Senior/staff topics

If a question genuinely spans topics, place it under the primary concept and cross-reference the related concept instead of duplicating the full answer.

## Duplication policy

Before adding a question:

1. Search the repository for the concept and likely wording.
2. Prefer improving an existing canonical question over adding a near-duplicate.
3. If two questions test different levels of the same concept, keep both but distinguish their intent.
4. Reuse shared explanations by linking to the canonical answer rather than copying large blocks.

Known overlap risks in the current baseline include transaction proxy/self-invocation concepts, REST error/validation handling, security authentication/authorization terminology, and production troubleshooting topics appearing across the main bank and scenario/drill documents. These are candidates for consolidation as later phases deepen the answers.

## Version-awareness

Spring and Java evolve. Version-sensitive claims must state the relevant version or generation when it materially changes behavior. Prefer official documentation for current framework behavior and mark older behavior explicitly when it is useful for interviews.

Do not present an implementation detail as a permanent contract when the framework documentation treats it as internal behavior.

## Code examples

Code should be:

- minimal and directly related to the question
- syntactically plausible and consistent with modern Spring Boot/Java
- free of unnecessary boilerplate
- explicit about imports/configuration when omission would make the example misleading
- followed by the behavior or trade-off the interviewer should notice

Avoid examples that teach insecure defaults, hidden global state, or production-hostile configuration without calling out the limitation.

## Baseline inventory

The current README is a broad question bank spanning Java fundamentals through Spring internals, Boot, configuration, MVC/REST, JPA/Hibernate, transactions, security, observability, caching, async/scheduling, reactive programming, messaging, testing, deployment, microservices, performance, system design, debugging, coding exercises, and senior/staff scenarios.

The repository also contains dedicated answered-guide, production-scenario, and advanced-drill material. Phase 0 intentionally does not rewrite that content. It establishes the rules future phases should use while expanding and consolidating it.

## Review checklist for future content

- Does the answer explain the concept accurately?
- Is the spoken interview answer concise enough to use live?
- Is deeper mechanism explained only where useful?
- Is the example minimal and realistic?
- Are trade-offs and failure modes covered for senior topics?
- Are version-sensitive claims identified?
- Is this question genuinely new or a better treatment of an existing question?
- Does the navigation remain predictable?
