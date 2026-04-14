# Technology Defaults by Project Type

## Project Type Classification

Every source project MUST be classified into one of three types. When a project spans multiple types (e.g., a framework with a CLI tool), classify each module independently.

| Type | Definition | Examples | Java Stack |
| --- | --- | --- | --- |
| **Application** | Deploys and runs as a service/process | Web servers, microservices, CLI tools, batch jobs, desktop apps | Spring Boot 4 + Spring ecosystem + real infrastructure |
| **Library** | Imported as a dependency by other projects | Utilities, SDKs, data structures, algorithms, API clients | Plain Java 21+ (no Spring). Published as Maven artifact. |
| **Framework** | Provides a programming model for others to build on | Web frameworks, DI containers, ORMs, test frameworks | Plain Java 21+ (no Spring). Published as Maven artifact. |

## Application Stack

| Category | Technology | Version | Why This Choice |
| --- | --- | --- | --- |
| Language | Java | 21+ | Records, sealed classes, pattern matching, virtual threads |
| Framework | Spring Boot | 4.x (Spring 7) | Most adopted enterprise Java framework |
| Security | Spring Security | 7.x | Integrated with Spring Boot 4 |
| AI/ML | Spring AI | Latest GA | Unified AI abstraction for Java |
| Batch | Spring Batch | 6.x | De facto standard for batch processing in Java |
| Concurrency | Virtual Threads | Java 21+ | Lightweight threads — simpler code, same throughput |
| RDBMS | PostgreSQL | 16+ | Most capable open-source RDBMS |
| ORM | Spring Data JPA / Hibernate | Latest | Standard Java persistence |
| Cache | Redis (Spring Data Redis) | Latest | Most adopted distributed cache |
| Messaging | Apache Kafka (Spring Kafka) | Latest | Industry standard event streaming |
| Graph DB | Neo4j (Spring Data Neo4j) | Latest | Most adopted graph database |
| Build | Maven (preferred) | Latest | Better multi-module support |
| Testing | JUnit 5 + AssertJ + Testcontainers | Latest | Modern Java testing stack |
| Namespace | Jakarta EE (`jakarta.*`) | 10+ | Required by Spring Boot 3+ |

## Library & Framework Stack

| Category | Technology | Version | Why This Choice |
| --- | --- | --- | --- |
| Language | Java | 21+ | Records, sealed classes, pattern matching, virtual threads |
| Framework | None (plain Java) | — | Libraries/frameworks must be framework-agnostic |
| HTTP | java.net.http.HttpClient | Java 11+ | Standard library, no external dependency |
| Concurrency | java.util.concurrent + Virtual Threads | Java 21+ | Standard library concurrency primitives |
| JSON | Jackson or Gson | Latest | Most adopted JSON libraries in Java |
| Logging | SLF4J | Latest | Facade pattern — consumer chooses implementation |
| Build | Maven (preferred) | Latest | Standard artifact publishing |
| Testing | JUnit 5 + AssertJ (+ Testcontainers for infra tests) | Latest | Modern Java testing stack |
| Namespace | Jakarta EE (`jakarta.*`) where applicable | 10+ | Standard APIs |

## Technology Substitution Rules

When mapping source technologies to Java equivalents:

1. **Direct equivalent exists**: Use it. Document with ★ Insight explaining the mapping.
2. **Close equivalent exists**: Use it. Document capabilities lost with ★ Insight in the deviation report.
3. **No equivalent exists**: Choose the closest alternative. Create a detailed ★ Insight block in the deviation report with:
   - Source technology and what it does
   - Chosen Java alternative
   - Strong reasoning for the choice
   - Capabilities lost or different
   - Alternatives considered and why rejected

## Concurrency Rules

| Source Pattern | Java Equivalent | Never Use |
| --- | --- | --- |
| Goroutines (Go) | Virtual Threads | Spring WebFlux, RxJava |
| async/await (JS/C#/Python) | Virtual Threads | CompletableFuture chains, Reactive Streams |
| Tokio tasks (Rust) | Virtual Threads | Spring WebFlux, Mutiny |
| Event loop (Node.js) | Virtual Threads | Netty event loop directly |
| Actors (Erlang/Akka) | Virtual Threads + ConcurrentHashMap | Akka for Java |

🚫 **MUST NOT use**: Spring WebFlux, Reactive Streams, Mutiny, RxJava — virtual threads replace all reactive/async patterns.

## Infrastructure Rules (Applications Only)

| Need | Use | Never Use |
| --- | --- | --- |
| RDBMS | PostgreSQL | H2, SQLite, embedded databases |
| Cache | Redis | In-memory maps, Caffeine as primary |
| Messaging | Apache Kafka | In-memory queues, embedded brokers |
| Graph DB | Neo4j | In-memory graph structures |
| Search | Elasticsearch / OpenSearch | In-memory search |

🚫 **No in-memory substitutes for applications.** Libraries and frameworks have no mandatory infrastructure.

## Java-to-Java Re-implementation Rules

When source is a Java application using a non-Spring framework:

| Source Framework | Focus Area |
| --- | --- |
| Quarkus | CDI → Spring DI, SmallRye → Spring Kafka, Extensions → Starters |
| Micronaut | Compile-time DI → Runtime DI, Micronaut Data → Spring Data |
| Vert.x | Event bus → Spring Events + Virtual Threads, Verticles → @Service |
| Jakarta EE | CDI → Spring DI, JAX-RS → Spring MVC, JPA stays JPA |
| Spring Boot 2.x/3.x | Upgrade patterns, deprecated API replacements, Security migration |

Technology mapping for Java sources becomes a **framework concept mapping** document with ★ Insight blocks contrasting design philosophies.

## Deviation Report Format

Each entry in `deviation-report.md` follows this structure:

```markdown
### 🔀 [Source Technology Name]

> ★ **Insight** -------------------------------------------
> - **Why [Java alternative] replaces [source tech]?** [Strong reasoning with context]
> - **Trade-off:** [Capabilities lost, behavioral differences, performance implications]
> - **Recommend:** [When to use the Java alternative vs. seeking other options]
> - **Where:** [→ src/path/File.java — where the replacement is implemented]
> - **When:** [At what point in the application lifecycle this matters]
> - **How to verify:** [Test or check that confirms the replacement works correctly]
> -----------------------------------------------------------

**Alternatives considered:**
| Alternative | Why Rejected |
| --- | --- |
| [alt 1] | [reason] |
| [alt 2] | [reason] |
```
