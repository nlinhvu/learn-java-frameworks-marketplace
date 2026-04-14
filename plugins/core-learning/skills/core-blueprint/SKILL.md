---
name: core-blueprint
version: 1.0.0
description: >
  "analyze this project's internals", "inside-out outline for X", "core-first learning plan",
  "create a core-first outline for X", "map the core internals", "core-blueprint",
  "re-implement this core in Java", "simplified Java core reimplementation of X",
  "Java reimplementation of this project's internals" — this skill analyzes any project's source
  code (Python, Go, Rust, Node.js, C#, Ruby, Java, or any language) and produces a progressive
  learning outline (core-outline.md) for building a simplified Java reimplementation inside-out —
  starting from the internal core and building outward. Generates Mermaid dependency graphs,
  architecture diagrams, and integration point maps with ★ Insight blocks and a visual learning
  roadmap, plus a buildable Java project skeleton.
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

# Core Blueprint — Inside-Out Learning Outline

## Philosophy: Inside-Out, Core-First

The fastest way to understand any project's internals is to start at the **heart** — the minimal
core that everything else depends on. The source project can be in any language (Python, Go, Rust,
Node.js, C#, Ruby, Java, etc.) — the output is always a simplified Java reimplementation that
captures the essential architecture. Instead of starting from the public API surface and tracing
inward (API-first approach), this skill:

1. **Identifies the heart** — the minimal core component(s) that everything else depends on
2. **Builds outward progressively** — each feature plugs into the existing system at a clear
   integration point, extending the core's capabilities
3. **Orders by dependency** — foundations first, extensions later, ensuring each feature has
   a solid base to build on

This approach works because:
- Each feature has a clear **foundation** — you never build something that depends on something not yet built
- You understand **how** the core works before seeing how it's extended
- Core architectural patterns transcend language — analyzing a Go or Python project reveals the same design principles you'll reimplement in Java
- You **see the big picture** — Mermaid diagrams show integration points, component relationships, and the progressive build path

## MANDATORY: Use Extended Thinking

**Use extended thinking for the analysis phase.** The quality of the outline depends entirely on deep upfront reasoning about the source project's architecture. Before writing any output:

- Identify the source project's language and technology stack
- Map the full type/module/class hierarchy of core packages (adapting to the source language's conventions)
- Identify the "heart" — the minimal core that everything else depends on
- Trace key execution flows end-to-end
- Plan how source language constructs map to Java equivalents (e.g., Go interfaces to Java interfaces, Python decorators to Java annotations, Rust traits to Java interfaces)
- Determine which components can be meaningfully simplified vs which need near-full fidelity
- Plan the progressive enhancement order — each feature must build on previous ones

This deep analysis cannot be done incrementally. Skipping extended thinking leads to outlines that put the cart before the horse, miss critical dependencies, or produce features that can't be implemented in the stated order.

## Input

The user provides the source project repo path as an argument. The source project can be in any language. If omitted, use the current working directory. Examples:
```
/core-blueprint /path/to/spring-framework
/core-blueprint /path/to/flask
/core-blueprint /path/to/gin-gonic
/core-blueprint /path/to/actix-web
```

## Workflow

### Step 1: Explore the Source Project (Agent-Powered)

**Dispatch 2-3 `core-learning:core-code-explorer` agents in parallel** to deeply analyze the source project from different angles. The source may be in any language — agents must adapt their analysis to the source language's conventions. Each agent focuses on a different aspect:

- **Agent 1 — Core & Foundations**: Entry points, core abstractions, key types/interfaces/traits/protocols, package/module structure, build files (any build system: `pom.xml`, `build.gradle`, `go.mod`, `Cargo.toml`, `package.json`, `pyproject.toml`, `*.csproj`, `Gemfile`, etc.), language version
- **Agent 2 — Execution Flows**: Trace the primary happy-path operations end-to-end, map the central dispatch pipeline, identify lifecycle transitions
- **Agent 3 — Extension & Integration Model** (for medium/complex projects): Extension points, plugin mechanisms, middleware patterns, configuration model

Give each agent the source project repo path and a specific focus area. Example prompt for Agent 1:
```
Analyze the core foundations of the project at <path>. Focus on:
- Identify the source language and technology stack
- The minimal core (the "heart") that everything else depends on
- Core types, interfaces, traits, or protocols with their hierarchies
- Package/module structure and organization
- Build config for dependencies and language version
Use the inside-out approach: start from the innermost core and work outward.
Note: This source project may be in any language — adapt your analysis accordingly.
```

After agents return, **read the files they identify as essential** before proceeding. Synthesize their findings into a unified understanding.

Read [references/analysis-checklist.md](references/analysis-checklist.md) and verify coverage against its checklist.

### Step 2: Identify the Heart

Synthesize the explorer agents' findings to determine the minimal core — the component(s) that everything else depends on. This becomes feature #1 in the outline. Ask:
- What is the simplest thing this project does?
- If I stripped everything away, what would remain?
- What's the first thing a user interacts with?
- How does this core concept translate to Java? (e.g., a Go `struct` with methods becomes a Java class, a Python decorator becomes an annotation or wrapper pattern)

The core-code-explorer agents should have already identified this — verify and refine their finding.

### Step 3: Architecture Analysis (Agent-Powered)

**Dispatch 1-2 `core-learning:core-code-architect` agents** to analyze the source project's architecture for simplified Java reimplementation planning:

- **Agent A — Component Architecture**: How subsystems relate, what can be simplified vs. needs near-full fidelity, source-type-to-Java-class mapping opportunities, technology substitution decisions
- **Agent B — Integration Point Map** (for medium/complex projects): Map every subsystem's integration point — where each plugs into the core

Include the Mermaid color palette from [shared/diagram-standards.md](shared/diagram-standards.md) in each architect agent's prompt so diagrams follow the standardized palette.

Example prompt:
```
Analyze the architecture of <project> at <path> for simplified Java reimplementation planning.
Source language: <language identified in Step 1>
Based on these explorer findings: <paste key findings>
For each subsystem, determine: what to keep, merge, skip, or hardcode.
Map integration points — the exact function/method/type where each subsystem connects to the core.
Plan how source language constructs map to Java equivalents.
Every key design decision needs a ★ Insight block explaining WHY.
Mermaid color palette: Teal #4ecdc4 (core), Blue #45b7d1 (internal logic),
Purple #bb8fce (extension points), Orange #f39c12 (config), Red #ff6b6b (error),
Green #27ae60 (success), Yellow #f7dc6f (external/app code).
```

### Step 4: Map Dependencies and Integration Points

Synthesize the architect agents' findings. For each potential feature/component, confirm:
- What it depends on (must be implemented first)
- What depends on it (helps prioritize)
- Whether it can be meaningfully simplified
- **Where it plugs in (integration point):** The specific class/method where this feature connects to the existing system. For example: "plugs into `doDispatch()` pipeline" or "adds a new ArgumentResolver to the HandlerAdapter". This hint guides the `/core-builder` skill when implementing each feature — the integration point is where implementation starts.

### Step 5: Decide Granularity

The skill decides how many outline items to produce based on project complexity:

| Project Complexity | Items | Examples |
|---------------------|-------|---------|
| Simple library | 5–8 | Gson, Flask, Gin, clap |
| Medium framework | 8–15 | Retrofit, Express.js, FastAPI, Actix-web |
| Complex framework | 15–25 | Spring Boot, Django, Kubernetes client, Tokio |

Each item should be implementable in a single `/core-builder` session (roughly 1-3 Java classes + tests).

### Step 6: Generate the Outline

Write `core-outline.md` using the template in [references/outline-template.md](references/outline-template.md). Refer to [shared/diagram-standards.md](shared/diagram-standards.md) for the Mermaid color palette and diagram rules. Refer to [shared/insight-format.md](shared/insight-format.md) for the ★ Insight block format.

### Step 7: Generate the Project Skeleton

Create a buildable Maven or Gradle Wrapper Java project. The output project is always Java regardless of the source language. Choose build tool based on preference:
- If the source is a Java project using Gradle → generate Gradle Wrapper
- Otherwise → generate Maven Wrapper (default)

The skeleton must include:

```
simple-<project-name>/
├── core-outline.md
├── .gitignore                  # target/, .gradle/, .idea/, *.class, etc.
├── mvnw (or gradlew)           # Build wrapper script
├── mvnw.cmd (or gradlew.bat)   # Windows wrapper
├── .mvn/ (or gradle/)          # Wrapper config
├── pom.xml (or build.gradle)   # Build config
├── core-docs/                       # (populated by /core-builder — tutorial chapters go here)
└── src/
    ├── main/java/simple/<project>/   # (empty, implementation goes here)
    └── test/java/simple/<project>/   # (empty, tests go here)
```

#### Build Configuration

The `pom.xml` / `build.gradle` must include:

**Java version:** Minimum Java 17. If the outline identifies features requiring Java 21+ (virtual threads, pattern matching for switch, sequenced collections), note this per-feature in the outline and set the initial version to 17 — it will be upgraded when those features are implemented.

**Dependencies:**
- JUnit 5 (`junit-jupiter`) for testing
- AssertJ (`assertj-core`) for fluent assertions
- Any minimal dependencies needed for the Java reimplementation (keep to a minimum — prefer JDK-only)

**Plugins:**
- `maven-compiler-plugin` / equivalent Gradle config
- `exec-maven-plugin` if demo execution is needed

#### Generate Maven Wrapper

Run `mvn wrapper:wrapper` if Maven is available, or create the wrapper files directly:
```bash
cd simple-<project-name> && mvn wrapper:wrapper
```
If Maven is not available, create a minimal `mvnw` script and `.mvn/wrapper/maven-wrapper.properties` pointing to a stable Maven version (3.9.9).

### Step 8: Quality Review (Agent-Powered)

**Dispatch a `core-learning:core-code-reviewer` agent** (use the Agent tool with `subagent_type="core-learning:core-code-reviewer"`) to validate the blueprint:

```
Review the core-first blueprint for <project-name> at <path>.
Check: heart identification correctness, integration point mapping, feature ordering,
dependency graph accuracy, Mermaid diagram validity, ★ Insight depth, outline completeness,
Learning Path coherence, skeleton compilation.
```

**Review loop orchestration** (you, the skill, drive this loop — not the reviewer agent):
1. Dispatch the reviewer agent and read its findings
2. If Critical issues exist (confidence >= 90): fix them, then re-dispatch the reviewer
3. If only Important issues remain (80-89): fix them and proceed to Step 9
4. Stop re-dispatching when: no Critical/High issues remain, OR 5 review passes completed, OR only subjective issues remain

Refer to [shared/quality-checklist.md](shared/quality-checklist.md) for the full rubric and stop criteria.

### Step 9: Verify

Before finishing, verify:
- [ ] Outline starts with the project's heart
- [ ] Each feature's dependencies are listed and come earlier in the outline
- [ ] No circular dependencies exist
- [ ] Granularity is appropriate for the project's complexity
- [ ] Learning Path section present with Mermaid roadmap
- [ ] Mermaid diagrams use standardized color palette and have `<!-- diagram: slug -->` markers
- [ ] ★ Insight blocks have at minimum Why + Trade-off + Recommend
- [ ] The project skeleton builds: `./mvnw compile` (or `./gradlew compileJava`) succeeds
- [ ] Each feature maps to real source files/packages

## Output Summary

When done, tell the user:
1. Where the outline is: `simple-<project-name>/core-outline.md`
2. The source project language and key technology mappings to Java
3. How many features were identified and their build sequence
4. What the heart of the project is (feature #1)
5. How to proceed: "Review the outline and learning path first, then pick a feature and run `/core-builder "<feature name>"`"
