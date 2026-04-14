---
name: enterprise-blueprint
version: 1.0.0
description: >
  "analyze this project for enterprise Java", "enterprise blueprint for X", "enterprise-blueprint",
  "re-implement this in enterprise Java", "create an enterprise learning outline",
  "map this project to enterprise Java", "classify this project", "Spring Boot version of X",
  "production-grade Java reimplementation", "enterprise-grade Java" — this skill analyzes a
  source project (any language, including Java) for enterprise Java re-implementation. Classifies
  as Application/Library/Framework, produces enterprise-outline.md, technology-mapping.md,
  deviation-report.md, and a buildable Maven project skeleton with Mermaid visualizations and
  Insight blocks.
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
  - Agent
file_references:
  - shared/diagram-standards.md
  - shared/insight-format.md
  - shared/quality-checklist.md
  - shared/technology-defaults.md
---

# Enterprise Blueprint — Enterprise Java Learning Outline

## Philosophy: Learn Enterprise Java by Re-implementing Real Projects

The fastest way to learn enterprise Java is to re-implement a project you already understand (or want to understand) in enterprise-grade Java. Every technology choice, architecture decision, and design trade-off becomes a teaching moment with ★ Insight blocks and Mermaid visualizations.

This skill:

1. **Classifies the source project** — Application, Library, or Framework — and selects the appropriate Java stack
2. **Maps every technology** — source tech → Java equivalent, with deviations documented when no equivalent exists
3. **Designs the enterprise architecture** — module structure, build configuration, infrastructure (applications), and feature sequence
4. **Produces a rich learning outline** — with Quick Start, Learning Path, Mermaid diagrams, and ★ Insight blocks

The right tools for the job: Spring Boot 4 for applications, plain Java 21+ for libraries and frameworks.

## MANDATORY: Use Extended Thinking

**Use extended thinking for the analysis phase.** The quality of the outline depends on deep upfront reasoning about the source project's architecture, technologies, and the Java re-implementation strategy. Before writing any output:

- Classify the project type and justify the classification
- Map every source technology to a Java equivalent (or flag as deviation)
- Design the module structure to faithfully reproduce source architecture
- Sequence features by dependency order and learning value
- Identify concurrency patterns and plan virtual thread migration
- For Java sources: identify framework concept mappings

Skipping extended thinking leads to misclassified projects, incomplete technology mappings, or feature sequences that don't respect dependencies.

## Input

The user provides the source project path as an argument. If omitted, use the current working directory. Example:
```
/enterprise-blueprint /path/to/source-project
```

## Workflow

### Step 1: Explore the Source Project (Agent-Powered)

**Dispatch 2-3 `enterprise-learning:enterprise-code-explorer` agents in parallel** to deeply analyze the source project. Each agent focuses on a different aspect:

- **Agent 1 — Project Type & Structure**: Project type classification (Application/Library/Framework), module boundaries, directory layout, package managers, build configuration, entry points, deployment artifacts
- **Agent 2 — Technology & Dependency Mapping**: Every dependency and technology used, role each plays, Java equivalents, deviation candidates, concurrency model analysis
- **Agent 3 — Business Logic & API Surface** (for medium/large projects): Business domain entities, operations, rules, API endpoints (applications), public API (libraries), programming model (frameworks), external integrations

Give each agent the source project path and a specific focus area. Use the Agent tool with `subagent_type="enterprise-learning:enterprise-code-explorer"` for each dispatch. Example prompt for Agent 1:
```
Analyze the project at <path> to classify it as Application, Library, or Framework.
Focus on:
- Entry points (main(), Dockerfile, server config, CLI args)
- Package metadata (go.mod/package.json/Cargo.toml/pom.xml/etc.)
- Published artifacts and deployment configuration
- Module boundaries and multi-module layout
- README: does it say "install and run" or "import and use"?
Provide a definitive classification with rationale for a ★ Insight block.
```

After agents return, **read the files they identify as essential** before proceeding. Synthesize their findings into a unified understanding of the source project.

Read [references/analysis-checklist.md](references/analysis-checklist.md) and verify coverage against its 6-phase checklist.

### Step 2: Classify Project Type

Based on explorer findings, definitively classify the source project:

| Type | Definition | Java Stack |
| --- | --- | --- |
| **Application** | Deploys and runs as a service/process | Spring Boot 4 + Spring ecosystem + real infrastructure |
| **Library** | Imported as a dependency | Plain Java 21+ (no Spring). Maven artifact. |
| **Framework** | Programming model for others | Plain Java 21+ (no Spring). Maven artifact. |

For mixed projects (e.g., a framework with a CLI tool), classify each module independently.

Refer to [shared/technology-defaults.md](shared/technology-defaults.md) for the complete stack definitions per project type. Refer to [shared/insight-format.md](shared/insight-format.md) for the ★ Insight block format.

Write a ★ Insight block explaining the classification rationale:
```markdown
> ★ **Insight** -------------------------------------------
> - **Why classified as [Application/Library/Framework]?** [Evidence and reasoning]
> - **Trade-off:** [What this classification means for the Java stack]
> - **Recommend:** [How this affects what the learner will build]
> -----------------------------------------------------------
```

### Step 3: Design Enterprise Architecture (Agent-Powered)

**Dispatch 1-2 `enterprise-learning:enterprise-code-architect` agents** (use the Agent tool with `subagent_type="enterprise-learning:enterprise-code-architect"`) to design the enterprise Java architecture:

- **Agent A — Module Structure & Technology Mapping**: Maven/Gradle module design, technology mapping table, deviation list, build configuration, Docker Compose design (applications)
- **Agent B — Feature Sequencing & Build Order** (for medium/large projects): Feature definition, dependency ordering, complexity estimation, learning path design

Include the Mermaid color palette from [shared/diagram-standards.md](shared/diagram-standards.md) in each architect agent's prompt so diagrams follow the standardized palette.

Example prompt:
```
Design the enterprise Java architecture for re-implementing <project> at <path>.
Classification: [Application/Library/Framework]
Explorer findings: <paste key findings>
Design: module structure, technology mapping, feature sequence, build configuration.
Stack: [Spring Boot 4 with real infra / Plain Java 21+]
Every design decision needs a ★ Insight block explaining WHY.
Mermaid color palette: Teal #4ecdc4 (input), Blue #45b7d1 (logic), Purple #bb8fce (data),
Orange #f39c12 (infra), Red #ff6b6b (error), Green #27ae60 (success), Yellow #f7dc6f (external).
```

### Step 4: Produce Technology Mapping

Create the output directory `enterprise-<project-name>/` in the current working directory. If the directory already exists, ask the user to confirm overwrite before proceeding. All output files (outline, technology mapping, deviation report, skeleton) go under this directory.

Read [references/technology-mapping-template.md](references/technology-mapping-template.md) for the exact template.

Create `technology-mapping.md` with:
- A mapping table for every source technology → Java equivalent
- ★ Insight block for each mapping explaining the rationale
- Mermaid diagram showing the technology stack comparison (source vs. Java)
- For Java-to-Java: framework concept mapping with philosophy contrasts

### Step 5: Produce Deviation Report

Refer to the "Deviation Report Format" section in [shared/technology-defaults.md](shared/technology-defaults.md) for the exact entry structure.

Create `deviation-report.md` for every technology where no direct Java equivalent exists. Each entry must include:
- Source technology name and what it does
- Chosen Java alternative with **strong reasoning**
- Capabilities lost or different
- Alternatives considered and why rejected (table format)
- Mermaid diagram comparing source approach vs. Java approach
- Each entry as a ★ Insight block (per format in [shared/insight-format.md](shared/insight-format.md))

If all source technologies have direct Java equivalents, create the file with a note: "All source technologies have direct Java equivalents. See technology-mapping.md for details."

### Step 6: Generate the Enterprise Outline

Read [references/outline-template.md](references/outline-template.md) for the exact template. Refer to [shared/diagram-standards.md](shared/diagram-standards.md) for the Mermaid color palette and diagram rules.

The outline (`enterprise-outline.md`) includes:
- **Project Type Classification** with ★ Insight block
- **Quick Start** section:
  - Applications: `docker-compose up -d`, `./mvnw verify`, expected results, smoke-test URL
  - Libraries/Frameworks: `./mvnw verify` (no Docker needed), expected test results, example usage
  - Prerequisites (Java 21+, Maven, Docker for applications)
- **Architecture Overview** with Mermaid C4 diagrams
- **Technology Stack** summary with Mermaid comparison diagram
- **Module Structure** with dependency graph
- **Learning Path** with:
  - Mermaid roadmap of the learning journey (feature dependency graph annotated with learning objectives)
  - Links to all tutorial chapters in recommended reading order
  - Links to technology-mapping.md and deviation-report.md
  - Prerequisite knowledge for each chapter
  - Progress tracking with ⬜/✅ markers
- **Feature List** ordered by dependency and learning value, each with:
  - Status: ⬜ / ✅
  - What it does (from the user's perspective)
  - Source technologies involved
  - Java technologies used
  - Dependencies on other features
  - Complexity estimate
  - Concrete deliverables

### Step 7: Generate Project Skeleton

Create a buildable Maven or Gradle project based on the project type:

#### Applications
```
enterprise-<project-name>/
├── enterprise-outline.md
├── technology-mapping.md
├── deviation-report.md
├── docker-compose.yml
├── .gitignore
├── mvnw / mvnw.cmd
├── .mvn/
├── pom.xml                        (Spring Boot 4 parent POM)
├── <module-1>/
│   ├── pom.xml
│   ├── src/main/java/...
│   ├── src/main/resources/
│   │   └── application.yml        (with dev/test/prod profiles)
│   └── src/test/java/...
└── enterprise-docs/
    └── (populated by /enterprise-builder)
```

#### Libraries & Frameworks
```
enterprise-<project-name>/
├── enterprise-outline.md
├── technology-mapping.md
├── deviation-report.md
├── .gitignore
├── mvnw / mvnw.cmd
├── .mvn/
├── pom.xml                        (standard Maven POM, NO Spring Boot parent)
├── <module-1>/
│   ├── pom.xml
│   ├── src/main/java/...
│   └── src/test/java/...
└── enterprise-docs/
    └── (populated by /enterprise-builder)
```

#### Build Configuration

**Applications (pom.xml)**:
- Spring Boot 4 parent POM
- Java 21+
- spring-boot-starter-web, spring-boot-starter-data-jpa, spring-boot-starter-test
- Additional starters based on source technologies (security, kafka, redis, etc.)
- JUnit 5, AssertJ, Testcontainers
- Jakarta EE namespace

**Libraries/Frameworks (pom.xml)**:
- Standard Maven POM (no Spring Boot parent)
- Java 21+
- SLF4J for logging
- Jackson or Gson for JSON (if needed)
- JUnit 5, AssertJ
- Testcontainers (test scope only, if infrastructure tests needed)
- Proper groupId, artifactId, version for Maven Central publishing

#### Generate Maven Wrapper

Run `mvn wrapper:wrapper` if Maven is available, or create the wrapper files directly:
```bash
cd enterprise-<project-name> && mvn wrapper:wrapper
```
If Maven is not available, create a minimal `mvnw` script and `.mvn/wrapper/maven-wrapper.properties` pointing to a stable Maven version (3.9.9).

### Step 8: Quality Review (Agent-Powered)

**Dispatch an `enterprise-learning:enterprise-code-reviewer` agent** (use the Agent tool with `subagent_type="enterprise-learning:enterprise-code-reviewer"`) to validate the blueprint:

```
Review the enterprise blueprint for <project-name> at <path>.
Project type: [Application/Library/Framework]
Check: project type classification correctness, technology mapping completeness,
deviation report quality, Mermaid diagram validity, ★ Insight depth, outline completeness,
Quick Start accuracy, Learning Path coherence, skeleton compilation.
```

**Review loop orchestration** (you, the skill, drive this loop — not the reviewer agent):
1. Dispatch the reviewer agent and read its findings
2. If Critical issues exist (confidence >= 90): fix them, then re-dispatch the reviewer
3. If only Important issues remain (80-89): fix them and proceed to Step 9
4. Stop re-dispatching when: no Critical/High issues remain, OR 5 review passes completed, OR only subjective issues remain

Refer to [shared/quality-checklist.md](shared/quality-checklist.md) for the full rubric and stop criteria.

### Step 9: Verify

- [ ] Project type correctly classified with ★ Insight rationale
- [ ] Quick Start section present (Docker for applications, mvn-only for libraries/frameworks)
- [ ] Learning Path section with Mermaid roadmap
- [ ] Technology mapping complete — every source tech mapped
- [ ] Deviation report complete — every gap documented with strong reasoning
- [ ] Module structure faithfully reproduces source architecture
- [ ] Feature sequence respects dependencies
- [ ] Mermaid diagrams use standardized color palette and have `<!-- diagram: slug -->` markers
- [ ] ★ Insight blocks have at minimum Why + Trade-off + Recommend
- [ ] Project skeleton compiles with `./mvnw compile` or `./gradlew build`
- [ ] Docker Compose starts (applications): `docker-compose up -d`
- [ ] Libraries/frameworks have zero Spring imports

## Output Summary

When done, tell the user:
1. Project type classification: Application/Library/Framework (with brief rationale)
2. Where the outline is: `enterprise-<project-name>/enterprise-outline.md`
3. How many features were identified and their build sequence
4. Key technology mappings and any notable deviations
5. How to proceed: "Review the outline and technology mapping first, then pick a feature and run `/enterprise-builder "<feature name>"`"
