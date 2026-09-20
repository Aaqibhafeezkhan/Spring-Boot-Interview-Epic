# Phase 2 — Spring Core and Boot

## IoC and dependency injection
Inversion of Control means the container manages object construction and dependency wiring. Constructor injection makes required dependencies explicit and keeps classes easier to test.

## Bean lifecycle and scopes
A typical bean moves through instantiation, dependency injection, initialization callbacks, use, and destruction when the context shuts down. Singleton is the default scope. Prototype creates a new instance when requested, but injecting it directly into a singleton does not create one per method call.

## Typed configuration
ConfigurationProperties gives configuration a typed boundary and supports validation. It is easier to organize than scattering string-based configuration lookups across services.

## Profiles and environment
Profiles select environment-specific configuration and beans. Keep profile combinations small enough to test. Deployment-specific values should come from external configuration, and secrets should use appropriate secret management.

## Auto-configuration
Spring Boot auto-configuration supplies conditional defaults based on the classpath and application configuration. Conditions determine whether a configuration applies. When a bean appears unexpectedly, inspect the condition evaluation before overriding it.

## Startup and debugging
SpringApplication bootstraps the environment and application context, creates beans, and starts the application. For startup failures, identify the first meaningful cause rather than focusing only on the final wrapper exception.

## Production scenarios
### Dependency cycle
Map the cycle and redesign ownership before reaching for lazy initialization.

### Multiple implementations
Use a qualifier or primary bean when multiple implementations are intentional. Otherwise fix accidental duplicate registration.

### Binding failure
Check property names, active profiles, environment overrides, target types, and validation messages.

### Slow startup
Measure expensive initialization, scanning, bean creation, and external calls before disabling broad auto-configuration.

## Senior trade-offs
- Constructor injection improves explicitness and testability.
- Typed configuration improves validation and structure.
- Auto-configuration reduces boilerplate but adds conditional behavior to understand.
- Profiles help isolate environments but can become difficult to reason about.
- Startup validation catches invalid configuration early but can also block startup when dependencies are unavailable.

## Phase 2 checklist
- IoC and dependency injection
- Bean lifecycle and scopes
- Typed configuration
- Profiles and environment
- Auto-configuration
- Application startup
- Production debugging
- Senior trade-offs
