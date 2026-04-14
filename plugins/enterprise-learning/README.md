# enterprise-learning

Learn enterprise Java by re-implementing real-world projects from any language into enterprise-grade Java. Every technology choice, architecture decision, and design trade-off is documented with rich Mermaid visualizations and ★ Insight blocks explaining the **why** behind each choice.

## How It Works

Point the plugin at any source project (Go, Python, Rust, Node.js, C#, Ruby, Java/Quarkus/Micronaut, etc.) and it:

1. **Classifies** the project as Application, Library, or Framework
2. **Maps** every source technology to its Java equivalent
3. **Produces** an enterprise Java re-implementation with rich tutorials

| Source Type | Java Stack | Example |
| --- | --- | --- |
| **Application** | Spring Boot 4 + PostgreSQL/Redis/Kafka | Go microservice → Spring Boot 4 app |
| **Library** | Plain Java 21+ (no Spring) | Python SDK → Java HTTP client library |
| **Framework** | Plain Java 21+ (no Spring) | Express.js → Java web framework |

## Skills

| Skill | Trigger | What It Does |
| --- | --- | --- |
| `/enterprise-blueprint` | "analyze this project for enterprise Java", "enterprise blueprint" | Analyzes source project, classifies type, produces outline + technology mapping + deviation report + project skeleton |
| `/enterprise-builder` | "implement Feature Name", "next feature" | Implements one feature with working code + 10-section tutorial chapter with Mermaid diagrams and ★ Insights |

## Agents

| Agent | Role | Color |
| --- | --- | --- |
| `enterprise-code-explorer` | Multi-language source analysis, project type classification, technology identification | Yellow |
| `enterprise-code-architect` | Enterprise Java architecture design, technology mapping, module structure, build sequence | Green |
| `enterprise-code-reviewer` | Quality validation — correct stack, tutorial completeness, visualization quality, insight depth | Red |

## Usage

```bash
# Step 1: Analyze a source project
/enterprise-blueprint /path/to/source-project

# Step 2: Implement features one at a time
/enterprise-builder "User Authentication"
/enterprise-builder "next feature"
```

## Output Structure

### Applications
```
enterprise-<project-name>/
├── enterprise-outline.md          (type, Quick Start, Learning Path)
├── technology-mapping.md          (source tech → Java, with ★ Insights)
├── deviation-report.md            (gaps, alternatives, reasoning)
├── docker-compose.yml             (PostgreSQL, Redis, Kafka, Neo4j)
├── pom.xml                        (Spring Boot 4 parent POM)
├── src/main/java/...
├── src/test/java/...
└── enterprise-docs/
    ├── ch01_<feature>.md
    └── ...
```

### Libraries & Frameworks
```
enterprise-<project-name>/
├── enterprise-outline.md          (type, Quick Start, Learning Path)
├── technology-mapping.md          (source tech → Java, with ★ Insights)
├── deviation-report.md            (gaps, alternatives, reasoning)
├── pom.xml                        (standard Maven POM, no Spring)
├── src/main/java/...
├── src/test/java/...
└── enterprise-docs/
    ├── ch01_<feature>.md
    └── ...
```

## Key Design Principles

- **The code is the curriculum** — every re-implementation decision is a teaching moment
- **Right tools for the job** — Spring Boot 4 for applications, plain Java 21+ for libraries and frameworks
- **Virtual threads always** — no WebFlux, no Reactive Streams, no Mutiny
- **Real infrastructure** — PostgreSQL, Redis, Kafka (no H2, no in-memory substitutes) for applications
- **Jakarta EE** — `jakarta.*` namespace throughout, never `javax.*`
- **Modern Java** — records, sealed classes, pattern matching, virtual threads (Java 21+)
- **Structural fidelity** — faithfully reproduce source architecture, don't impose patterns

## Shared Standards

| File | Purpose |
| --- | --- |
| `shared/diagram-standards.md` | Mermaid color palette, diagram rules, emoji markers |
| `shared/insight-format.md` | ★ Insight block template, minimum fields, ordering rules |
| `shared/quality-checklist.md` | Reviewer rubric, review loop protocol, stop criteria |
| `shared/technology-defaults.md` | Default Java stack per project type, substitution rules |

## License

Apache-2.0
