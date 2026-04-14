# Technology Mapping Template

Use this template for the `technology-mapping.md` file. Replace all `{{placeholders}}`.

---

```markdown
# Technology Mapping: {{Source Project}} ({{Source Language}}) → Enterprise Java

> Every technology substitution is a learning moment. This document maps every source technology
> to its Java equivalent with ★ Insight blocks explaining **why** each choice was made.

## Overview

<!-- diagram: technology_stack_comparison -->
```mermaid
flowchart LR
    subgraph source["Source Stack ({{Language}})"]
        S1[{{source tech 1}}]
        S2[{{source tech 2}}]
        S3[{{source tech 3}}]
    end
    subgraph java["Enterprise Java Stack"]
        J1[{{java tech 1}}]
        J2[{{java tech 2}}]
        J3[{{java tech 3}}]
    end
    S1 -->|"replaces"| J1
    S2 -->|"replaces"| J2
    S3 -->|"replaces"| J3
    style S1 fill:#f7dc6f,color:#333
    style S2 fill:#f7dc6f,color:#333
    style S3 fill:#f7dc6f,color:#333
    style J1 fill:#45b7d1,color:#fff
    style J2 fill:#45b7d1,color:#fff
    style J3 fill:#45b7d1,color:#fff
```

## Summary Table

| Category | Source Technology | Java Equivalent | Deviation? |
| --- | --- | --- | --- |
| Language | {{source language}} | Java 21+ | No |
| Web Framework | {{framework}} | {{Spring Boot 4 / java.net.http}} | {{Yes/No}} |
| Database | {{database}} | {{PostgreSQL + Spring Data JPA / JDBC}} | {{Yes/No}} |
| Cache | {{cache}} | {{Redis / N/A}} | {{Yes/No}} |
| Messaging | {{messaging}} | {{Kafka / N/A}} | {{Yes/No}} |
| Concurrency | {{concurrency model}} | Virtual Threads | No |
| Testing | {{test framework}} | JUnit 5 + AssertJ + Testcontainers | No |
| Build | {{build tool}} | Maven | No |

Entries marked "Deviation? Yes" are documented in detail in [deviation-report.md](./deviation-report.md).

## Detailed Mappings

### 🔀 {{Category}}: {{Source Technology}} → {{Java Equivalent}}

> ★ **Insight** -------------------------------------------
> - **Why {{Java equivalent}} replaces {{source tech}}?** {{Rationale: what problem it solves, why it's the right choice for enterprise Java, what the industry standard is}}
> - **Trade-off:** {{What's different: performance characteristics, API style, ecosystem maturity, learning curve}}
> - **Recommend:** {{When to use this choice vs. alternatives in real projects}}
> - **Where:** [→ src/path/File.java — where this technology appears in the re-implementation]
> - **When:** {{At what point in the application lifecycle this matters}}
> - **How to verify:** {{Test or check that confirms the mapping works correctly}}
> -----------------------------------------------------------

{{Repeat for each technology mapping...}}

### 🔀 Concurrency: {{Source Concurrency Model}} → Java Virtual Threads

> ★ **Insight** -------------------------------------------
> - **Why virtual threads replace {{source concurrency}}?** Virtual threads provide the same lightweight threading model as {{source concurrency}} but with simpler imperative code. No callback chains, no reactive operators, no colored functions. Each virtual thread is cheap to create (~1KB stack) and the JVM schedules them onto carrier threads automatically.
> - **Trade-off:** Virtual threads are not a 1:1 replacement — {{source concurrency}} may have features like {{channels/select/actors}} that virtual threads don't directly replicate. Java uses `java.util.concurrent` primitives (locks, queues, semaphores) for coordination instead.
> - **Recommend:** Use virtual threads for all I/O-bound concurrency. For CPU-bound parallelism, use `Executors.newFixedThreadPool()` with platform threads. Never mix virtual threads with `synchronized` blocks that protect long-running I/O — use `ReentrantLock` instead (pinning issue).
> -----------------------------------------------------------

{{FOR JAVA-TO-JAVA RE-IMPLEMENTATIONS, replace the above with Framework Concept Mapping:}}

## Framework Concept Mapping

{{This section replaces the technology mapping above when the source is a Java project
using a non-Spring framework.}}

### 🔑 {{Source Framework Concept}} → {{Spring Equivalent}}

> ★ **Insight** -------------------------------------------
> - **Why {{Spring concept}} replaces {{source concept}}?** {{Design philosophy contrast: compile-time vs. runtime, reactive vs. imperative, etc.}}
> - **Trade-off:** {{What each approach optimizes for — startup time, flexibility, testability, etc.}}
> - **Recommend:** {{When each framework's approach is better suited}}
> -----------------------------------------------------------

| {{Source Framework}} | Spring Boot 4 | Key Difference |
| --- | --- | --- |
| {{@ApplicationScoped}} | {{@Component}} | {{CDI uses proxies; Spring uses CGLIB/JDK proxies}} |
| {{@Inject}} | {{@Autowired / constructor injection}} | {{CDI is spec-based; Spring extends it}} |
| {{SmallRye Reactive Messaging}} | {{Spring Kafka + Virtual Threads}} | {{Reactive → imperative with same throughput}} |
```
