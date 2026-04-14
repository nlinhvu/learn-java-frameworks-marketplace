# Source Project Analysis Checklist (Multi-Language)

Use this checklist to systematically analyze a source project from any language for enterprise Java re-implementation. The order matters — start with classification, then move to technologies and business logic.

## Phase 1: Project Type Classification

### 1.1 Entry Points
- [ ] Does the project have a `main()` function, entry script, or server bootstrap?
- [ ] Is there a Dockerfile, Procfile, or deployment configuration?
- [ ] Does the README say "install and run" (application) or "import and use" (library)?
- [ ] Are there CLI arguments, server ports, or scheduled job configurations?

### 1.2 Published Artifacts
- [ ] Is the project published to a package registry (npm, PyPI, crates.io, Maven Central)?
- [ ] Does the package metadata define it as a library/module for others to import?
- [ ] Is there a separate `-api` or `-core` artifact?

### 1.3 Extension Model
- [ ] Does the project provide middleware, plugin, or extension interfaces?
- [ ] Do users implement interfaces or extend base classes from this project?
- [ ] Is there a lifecycle, hook, or event system for extension?

### 1.4 Classification Decision
- [ ] **Application**: Has entry points + deployment config → Spring Boot 4 + real infra
- [ ] **Library**: Published artifact + public API + no entry point → Plain Java 21+
- [ ] **Framework**: Extension model + programming model for others → Plain Java 21+
- [ ] **Mixed**: Multiple types across modules → classify each module independently
- [ ] Classification documented with rationale for ★ Insight block

## Phase 2: Project Structure

### 2.1 Module Layout
- [ ] Single module or multi-module project?
- [ ] Module boundaries identified and documented?
- [ ] Inter-module dependencies mapped?
- [ ] Build system identified (Make, npm, Cargo, Go modules, Maven, Gradle)?

### 2.2 Directory Structure
- [ ] Source code directories and their purposes?
- [ ] Test directories and test infrastructure?
- [ ] Configuration file locations?
- [ ] Static assets, templates, or resource files?
- [ ] Documentation and examples?

### 2.3 Build & CI
- [ ] Build scripts and their commands?
- [ ] CI/CD configuration (GitHub Actions, GitLab CI, etc.)?
- [ ] Linting, formatting, static analysis tools?
- [ ] Docker build configuration?

## Phase 3: Technology Identification

### 3.1 Core Technologies
- [ ] Programming language and version
- [ ] Web framework / HTTP library
- [ ] Database(s) and ORM/query builder
- [ ] Cache system
- [ ] Message queue / event bus
- [ ] Authentication / authorization library
- [ ] Serialization format (JSON, protobuf, msgpack)

### 3.2 Infrastructure
- [ ] Databases: type (SQL/NoSQL/graph), engine, connection method
- [ ] Caching: Redis, Memcached, in-memory
- [ ] Messaging: Kafka, RabbitMQ, NATS, SQS, in-memory
- [ ] Search: Elasticsearch, Solr, Algolia
- [ ] Storage: S3, local filesystem, CDN

### 3.3 Java Mapping
For each technology:
- [ ] Direct Java equivalent exists? → Document mapping
- [ ] Close equivalent exists? → Document mapping + capabilities lost
- [ ] No equivalent exists? → Flag for deviation report with alternatives

### 3.4 Concurrency Model
- [ ] What concurrency primitives does the source use?
- [ ] How does the source handle parallel work? (goroutines, async/await, event loop, actors)
- [ ] What synchronization mechanisms? (mutexes, channels, locks)
- [ ] All map to Java virtual threads — document the mapping

## Phase 4: Business Logic

### 4.1 Domain Model
- [ ] What entities/models exist?
- [ ] How do entities relate to each other? (for ER diagram)
- [ ] What are the entity lifecycle states? (for state diagrams)
- [ ] What business rules constrain operations?

### 4.2 Operations
- [ ] CRUD operations on which entities?
- [ ] Workflow/process operations?
- [ ] Batch/scheduled operations?
- [ ] External system integrations?
- [ ] Event-driven operations?

### 4.3 API Surface
For **Applications**:
- [ ] HTTP endpoints (method, path, request/response types)
- [ ] GraphQL queries/mutations
- [ ] gRPC services
- [ ] WebSocket handlers
- [ ] CLI commands
- [ ] Scheduled jobs / cron tasks

For **Libraries**:
- [ ] Public classes/functions/types
- [ ] Configuration options
- [ ] Extension interfaces
- [ ] Usage examples

For **Frameworks**:
- [ ] Programming model (annotations, middleware, hooks)
- [ ] Lifecycle management
- [ ] Extension interfaces
- [ ] Convention-over-configuration rules

## Phase 5: Feature Planning

### 5.1 Feature Boundaries
For each candidate feature:
- [ ] What source functionality does it cover?
- [ ] What technologies does it use?
- [ ] Can it produce a visible result after implementation?
- [ ] What other features does it depend on?

### 5.2 Complexity Assessment
- [ ] How many source files are involved?
- [ ] How many new Java classes will be needed?
- [ ] Are there complex technology substitutions?
- [ ] Are there deviations that need careful explanation?

### 5.3 Learning Value
- [ ] Which features teach the most important enterprise Java concepts?
- [ ] Which features have the most interesting technology substitutions?
- [ ] Which features have deviations that are valuable learning moments?
- [ ] Order by: core concepts first, then progressively complex features

## Phase 6: Java-to-Java Specifics (When Source is Java)

### 6.1 Framework Identification
- [ ] Source framework: Quarkus, Micronaut, Vert.x, Jakarta EE, Spring Boot 2.x/3.x?
- [ ] Framework version and key features used?
- [ ] Framework-specific annotations, configurations, and patterns?

### 6.2 Concept Mapping
- [ ] DI container: CDI (@ApplicationScoped, @Inject) → Spring DI (@Component, @Autowired)?
- [ ] Web layer: JAX-RS → Spring MVC, Vert.x Web → Spring MVC?
- [ ] Data access: Micronaut Data → Spring Data, Panache → Spring Data JPA?
- [ ] Messaging: SmallRye Reactive → Spring Kafka, Vert.x Event Bus → Spring Events?
- [ ] Security: Quarkus Security → Spring Security?
- [ ] Configuration: MicroProfile Config → Spring Configuration Properties?

### 6.3 Philosophy Contrasts
- [ ] Compile-time DI vs. runtime DI?
- [ ] Reactive vs. imperative (virtual threads)?
- [ ] Extension model differences?
- [ ] Build-time optimization vs. runtime flexibility?
- [ ] Each contrast documented as ★ Insight block
