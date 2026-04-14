# Outline Template (Enterprise Java)

Use this template for the `enterprise-outline.md` file. Replace all `{{placeholders}}`.

---

```markdown
# Enterprise {{Project Name}} — Learn Enterprise Java by Re-implementation

> Learn enterprise Java by re-implementing **{{source project name}}** ({{source language}})
> into **enterprise-grade Java** — every technology choice and design decision is a teaching moment
> with ★ Insight blocks and Mermaid visualizations.

## Project Type Classification

{{Classification: Application / Library / Framework}}

> ★ **Insight** -------------------------------------------
> - **Why classified as {{type}}?** {{Evidence: entry points, deployment config, published artifact, extension model — and what this means for the Java stack}}
> - **Trade-off:** {{What this classification means — Spring Boot 4 + real infra vs. plain Java 21+}}
> - **Recommend:** {{When this classification might be different for a similar project}}
> -----------------------------------------------------------

**Java Stack**: {{Spring Boot 4 + Spring ecosystem + PostgreSQL/Redis/Kafka | Plain Java 21+ (no Spring)}}

## Quick Start

{{FOR APPLICATIONS:}}
```bash
# Prerequisites: Java 21+, Maven, Docker

# 1. Start infrastructure
docker-compose up -d

# 2. Build and test
./mvnw verify

# 3. Run the application
./mvnw spring-boot:run

# 4. Smoke test
curl http://localhost:8080/{{endpoint}}
# Expected: {{expected response}}
```

{{FOR LIBRARIES/FRAMEWORKS:}}
```bash
# Prerequisites: Java 21+, Maven (no Docker needed)

# 1. Build and test
./mvnw verify

# 2. Example usage
{{3-5 lines showing how to use the library/framework}}
```

## Architecture Overview

{{One paragraph: what does the source project do? Not what it IS internally, but what value it provides to its users. Keep this language-agnostic.}}

<!-- diagram: system_context -->
```mermaid
flowchart TD
    subgraph "Enterprise {{Project Name}}"
        {{components}}
    end
    {{external systems}}
    style {{nodes}} fill:#45b7d1,color:#fff
```

## Technology Stack

<!-- diagram: technology_comparison -->
```mermaid
flowchart LR
    subgraph "Source ({{Language}})"
        {{source technologies}}
    end
    subgraph "Enterprise Java"
        {{java technologies}}
    end
    {{source tech}} -->|"replaces"| {{java tech}}
```

| Source Technology | Role | Java Equivalent | ★ Why |
| --- | --- | --- | --- |
| {{source tech}} | {{role}} | {{java equivalent}} | {{brief rationale}} |

See [technology-mapping.md](./technology-mapping.md) for detailed ★ Insights per mapping.
See [deviation-report.md](./deviation-report.md) for technologies with no direct equivalent.

## Module Structure

{{FOR MULTI-MODULE:}}
<!-- diagram: module_dependencies -->
```mermaid
flowchart TD
    {{module dependency graph with labeled arrows}}
```

| Module | Source Module | Purpose |
| --- | --- | --- |
| {{java module}} | {{source module}} | {{what it does}} |

{{FOR SINGLE MODULE: describe the package structure}}

## Learning Path

<!-- diagram: learning_roadmap -->
```mermaid
flowchart TD
    F1[Feature 1: {{name}}]
    F2[Feature 2: {{name}}]
    F3[Feature 3: {{name}}]
    F1 -->|"enables"| F2
    F1 -->|"enables"| F3
    style F1 fill:#27ae60,color:#fff
    style F2 fill:#45b7d1,color:#fff
    style F3 fill:#45b7d1,color:#fff
```

**Recommended reading order:**
1. [technology-mapping.md](./technology-mapping.md) — understand the technology substitutions
2. [deviation-report.md](./deviation-report.md) — understand where Java differs
3. Tutorial chapters (in order below)

| Chapter | Feature | Prerequisites | Status |
| --- | --- | --- | --- |
| Ch 01 | {{Feature 1 name}} | None | ⬜ |
| Ch 02 | {{Feature 2 name}} | Ch 01 | ⬜ |
| {{...}} | {{...}} | {{...}} | ⬜ |

## Features

### Feature 1: {{Feature Name}}
⬜ Not started

**What it does**: {{One sentence describing the capability from the user's perspective}}

**Source technologies**: {{technologies used in the source for this feature}}
**Java technologies**: {{Java equivalents chosen for this feature}}

**Source files**: {{key source files involved}}

**Depends on**: None (first feature)
**Complexity**: {{Low / Medium / High}} · {{estimated new classes}} new classes

**Concrete deliverables**:
- [ ] {{Java source files}}
- [ ] {{Test files}}
- [ ] `enterprise-docs/ch01_{{feature_name}}.md` — tutorial chapter

---

### Feature 2: {{Feature Name}}
⬜ Not started

**What it does**: {{one sentence, user perspective}}

**Source technologies**: {{technologies}}
**Java technologies**: {{Java equivalents}}

**Source files**: {{...}}

**Depends on**: Feature 1 (reuses {{specific component}})
**Complexity**: {{...}} · {{N}} new classes, {{M}} modified

**Concrete deliverables**:
- [ ] ...

---

{{Continue for all features...}}

## Implementation Notes

### Infrastructure Requirements

{{FOR APPLICATIONS:}}
| Service | Docker Image | Port | Purpose |
| --- | --- | --- | --- |
| PostgreSQL | postgres:16 | 5432 | {{purpose}} |
| Redis | redis:7 | 6379 | {{purpose}} |
| Kafka | confluentinc/cp-kafka:7.6.0 | 9092 | {{purpose}} |
| Neo4j | neo4j:5 | 7474/7687 | {{purpose}} |

{{FOR LIBRARIES/FRAMEWORKS:}}
No infrastructure required. All tests run with plain JUnit 5.
{{If Testcontainers needed for integration tests, note it here.}}

### Simplification Strategy

| Source Concept | Enterprise Java Version | Why |
| --- | --- | --- |
| {{complex thing}} | {{enterprise approach}} | {{reason — what learning goal it serves}} |

### In Scope
- {{Features being implemented}}

### Out of Scope
- {{Features or optimizations deferred}}

### Java Version
- Java 21+ (records, sealed classes, pattern matching, virtual threads)

## Status Markers
- ⬜ = not started
- ✅ = implemented, tests passing, tutorial written
```
