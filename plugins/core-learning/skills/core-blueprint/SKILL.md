---
name: core-blueprint
version: 1.0.0
description: >
  This skill should be used when the user asks to "analyze this framework's internals", "inside-out outline for X",
  "core-first learning plan", "create a learning outline for X", "map the core internals", "core-blueprint",
  or any request to map a Java framework's core architecture for inside-out reimplementation. Analyzes a Java
  framework's source code and produces a progressive learning outline (core-outline.md) for building a simplified
  version inside-out — starting from the internal core and building outward. Generates a dependency graph,
  ASCII architecture diagrams, and a buildable project skeleton.
---

# Core Blueprint — Inside-Out Learning Outline

## Philosophy: Inside-Out, Core-First

The fastest way to understand a framework's internals and confidently contribute to it is to
start at the **heart** — the minimal core that everything else depends on. Instead of starting
from the public API surface and tracing inward (API-first approach), this skill:

1. **Identifies the heart** — the minimal core component(s) that everything else depends on
2. **Builds outward progressively** — each feature plugs into the existing system at a clear
   integration point, extending the core's capabilities
3. **Orders by dependency** — foundations first, extensions later, ensuring each feature has
   a solid base to build on

This approach works because:
- Each feature has a clear **foundation** — you never build something that depends on something not yet built
- You understand **how** the core works before seeing how it's extended
- You're **contribution-ready faster** — most framework internals follow the same integration-point pattern you've already practiced

## MANDATORY: Use Extended Thinking

**Use extended thinking for the analysis phase.** The quality of the outline depends entirely on deep upfront reasoning about the framework's architecture. Before writing any output:

- Map the full class/interface hierarchy of core packages
- Identify the "heart" — the minimal core that everything else depends on
- Trace key execution flows end-to-end
- Determine which components can be meaningfully simplified vs which need near-full fidelity
- Plan the progressive enhancement order — each feature must build on previous ones

This deep analysis cannot be done incrementally. Skipping extended thinking leads to outlines that put the cart before the horse, miss critical dependencies, or produce features that can't be implemented in the stated order.

## Input

The user provides the framework repo path as an argument. If omitted, use the current working directory. Example:
```
/core-blueprint /path/to/spring-framework
```

## Workflow

### Step 1: Explore the Framework (Agent-Powered)

**Dispatch 2-3 `core-learning:core-code-explorer` agents in parallel** to deeply analyze the framework from different angles. Each agent focuses on a different aspect:

- **Agent 1 — Core & Foundations**: Entry points, core abstractions, key interfaces, package structure, build files (`pom.xml`/`build.gradle`), Java version
- **Agent 2 — Execution Flows**: Trace the primary happy-path operations end-to-end, map the central dispatch pipeline, identify lifecycle transitions
- **Agent 3 — Extension & Integration Model** (for medium/complex frameworks): Extension points, plugin mechanisms, SPI patterns, configuration model

Give each agent the framework repo path and a specific focus area. Example prompt for Agent 1:
```
Analyze the core foundations of the framework at <path>. Focus on:
- The minimal core (the "heart") that everything else depends on
- Core interfaces and abstract classes with their hierarchies
- Package structure and module organization
- Build config (pom.xml/build.gradle) for dependencies and Java version
Use the inside-out approach: start from the innermost core and work outward.
```

After agents return, **read the files they identify as essential** before proceeding. Synthesize their findings into a unified understanding.

Also use the analysis checklist in [references/analysis-checklist.md](references/analysis-checklist.md) to verify coverage.

### Step 2: Identify the Heart

Synthesize the explorer agents' findings to determine the minimal core — the component(s) that everything else depends on. This becomes feature #1 in the outline. Ask:
- What is the simplest thing this framework does?
- If I stripped everything away, what would remain?
- What's the first thing a user interacts with?

The core-code-explorer agents should have already identified this — verify and refine their finding.

### Step 3: Architecture Analysis (Agent-Powered)

**Dispatch 1-2 `core-learning:core-code-architect` agents** to analyze the framework's architecture for simplification planning:

- **Agent A — Component Architecture**: How subsystems relate, what can be simplified vs. needs near-full fidelity, class-to-simplified-class mapping opportunities
- **Agent B — Integration Point Map** (for medium/complex frameworks): Map every subsystem's integration point — where each plugs into the core

Example prompt:
```
Analyze the architecture of <framework> at <path> for simplification planning.
Based on these explorer findings: <paste key findings>
For each subsystem, determine: what to keep, merge, skip, or hardcode.
Map integration points — the exact method/class where each subsystem connects to the core.
```

### Step 4: Map Dependencies and Integration Points

Synthesize the architect agents' findings. For each potential feature/component, confirm:
- What it depends on (must be implemented first)
- What depends on it (helps prioritize)
- Whether it can be meaningfully simplified
- **Where it plugs in (integration point):** The specific class/method where this feature connects to the existing system. For example: "plugs into `doDispatch()` pipeline" or "adds a new ArgumentResolver to the HandlerAdapter". This hint guides the `/core-builder` skill when implementing each feature — the integration point is where implementation starts.

### Step 5: Decide Granularity

The skill decides how many outline items to produce based on framework complexity:

| Framework Complexity | Items | Examples |
|---------------------|-------|---------|
| Simple library | 5–8 | Gson, Guava Cache, OkHttp |
| Medium framework | 8–15 | Retrofit, Caffeine, Resilience4j |
| Complex framework | 15–25 | Spring Boot, Netty, Kafka |

Each item should be implementable in a single `/core-builder` session (roughly 1-3 Java classes + tests).

### Step 6: Generate the Outline

Write `core-outline.md` using the template in [references/outline-template.md](references/outline-template.md).

### Step 7: Generate the Project Skeleton

Create a buildable Maven or Gradle Wrapper project. Choose based on what the target framework uses:
- Framework uses Gradle → generate Gradle Wrapper
- Framework uses Maven or unclear → generate Maven Wrapper (default)

The skeleton must include:

```
simple-<framework-name>/
├── core-outline.md
├── .gitignore                  # target/, .gradle/, .idea/, *.class, etc.
├── mvnw (or gradlew)           # Build wrapper script
├── mvnw.cmd (or gradlew.bat)   # Windows wrapper
├── .mvn/ (or gradle/)          # Wrapper config
├── pom.xml (or build.gradle)   # Build config
├── core-docs/                       # (populated by /core-builder — tutorial chapters go here)
└── src/
    ├── main/java/simple/<framework>/   # (empty, implementation goes here)
    └── test/java/simple/<framework>/   # (empty, tests go here)
```

#### Build Configuration

The `pom.xml` / `build.gradle` must include:

**Java version:** Minimum Java 17. If the outline identifies features requiring Java 21+ (virtual threads, pattern matching for switch, sequenced collections), note this per-feature in the outline and set the initial version to 17 — it will be upgraded when those features are implemented.

**Dependencies:**
- JUnit 5 (`junit-jupiter`) for testing
- AssertJ (`assertj-core`) for fluent assertions
- Any minimal framework-specific dependencies needed (keep to a minimum — prefer JDK-only)

**Plugins:**
- `maven-compiler-plugin` / equivalent Gradle config
- `exec-maven-plugin` if demo execution is needed

#### Generate Maven Wrapper

Run `mvn wrapper:wrapper` if Maven is available, or create the wrapper files directly:
```bash
cd simple-<framework-name> && mvn wrapper:wrapper
```
If Maven is not available, create a minimal `mvnw` script and `.mvn/wrapper/maven-wrapper.properties` pointing to a stable Maven version (3.9.9).

### Step 8: Verify

Before finishing, verify:
- [ ] Outline starts with the framework's heart
- [ ] Each feature's dependencies are listed and come earlier in the outline
- [ ] No circular dependencies exist
- [ ] Granularity is appropriate for the framework's complexity
- [ ] The project skeleton builds: `./mvnw compile` (or `./gradlew compileJava`) succeeds
- [ ] Each feature maps to real source files/packages

## Output Summary

When done, tell the user:
1. Where the outline is: `simple-<framework-name>/core-outline.md`
2. How many features were identified
3. What the heart of the framework is (feature #1)
4. How to proceed: "Pick a feature and run `/core-builder "<feature name>"`"
