---
name: api-blueprint
version: 1.0.0
description: >
  This skill should be used when the user asks to "analyze this framework's API", "outside-in outline for X",
  "API-first learning plan", "create a learning outline for X", "trace the public API", "analyze the API surface",
  "api-blueprint", or any request to map a Java framework's API surface for outside-in reimplementation. Analyzes
  a Java framework's source code and produces an API-first learning outline (api-outline.md) that maps the public
  API surface, traces each capability inward through implementation layers, generates ASCII call-chain diagrams,
  and creates a buildable project skeleton.
---

# API Blueprint — API-First Learning Outline

## Philosophy: Outside-In, API-First

The fastest way to learn a framework and confidently contribute to it is to start where
the **user** starts — the public API. Instead of hunting for the framework's internal
"heart" and building outward (v1 approach), this skill:

1. **Identifies the API surface** — the classes, interfaces, and methods that framework users
   actually import and call in their application code
2. **Organizes features as vertical slices** — each feature represents one API capability,
   traced from the public method all the way down through internal layers
3. **Orders by usage tier** — most commonly used APIs first, power-user/advanced APIs last

This approach works because:
- You're **productive immediately** — each feature gives you a working, callable API
- You understand **why** each internal piece exists — because you traced from a concrete API call
- You're **contribution-ready faster** — most open-source contributions start with "I use API X
  and it doesn't do Y" — this teaches you exactly how API X is implemented

## MANDATORY: Use Extended Thinking

**Use extended thinking for the analysis phase.** The quality of the outline depends entirely on deep upfront reasoning about the framework's API surface and internal architecture. Before writing any output:

- Map the full public API surface and classify by usage tier (Tier 1/2/3)
- Trace Tier 1 API call chains from the public method inward through each depth layer
- Identify recurring depth layers across call chains and name them
- Determine vertical slice boundaries — which API methods group into a single feature
- Plan the usage-tier ordering and dependency flow between features

This deep analysis cannot be done incrementally. Skipping extended thinking leads to outlines with poorly bounded features, missed shared internals, or API capabilities that can't be implemented in the stated order.

## Input

The user provides the framework repo path as an argument. If omitted, use the current working directory. Example:
```
/api-blueprint /path/to/javalin
```

## Workflow

### Step 1: Explore the API Surface (Agent-Powered)

**Dispatch 2-3 `api-learning:api-code-explorer` agents in parallel** to deeply analyze the framework from the outside in. Each agent focuses on a different aspect:

- **Agent 1 — API Surface & Entry Points**: Public API classes, README examples, "Getting Started" patterns, published artifacts, tier classification (Tier 1/2/3), build files (`pom.xml`/`build.gradle`), Java version
- **Agent 2 — Call Chain Tracing**: For each Tier 1 API, trace the execution path from the public method inward through dispatch, processing, and infrastructure layers. Map shared internals across call chains.
- **Agent 3 — Configuration & Extension Model** (for medium/complex frameworks): Configuration API (builder, properties, annotations), extension points (SPI, custom implementations), lifecycle hooks — these become Tier 2/3 API capabilities

Give each agent the framework repo path and a specific focus area. Example prompt for Agent 1:
```
Analyze the API surface of the framework at <path>. Focus on:
- Every public class/interface that framework users import and call
- README and "Getting Started" examples — which classes are Tier 1?
- Published artifacts (which module is the client module?)
- Tier classification: Tier 1 (essential), Tier 2 (common customization), Tier 3 (advanced)
- Build config (pom.xml/build.gradle) for dependencies and Java version
Use the outside-in approach: start from what clients see and use.
```

After agents return, **read the files they identify as essential** before proceeding. Synthesize their findings into a unified understanding of the API surface.

Also use the analysis checklist in [references/analysis-checklist.md](references/analysis-checklist.md) to verify coverage.

### Step 2: Trace Call Chains

Synthesize the explorer agents' findings. For each Tier 1 API, ask:
- What is the simplest API call a client can make?
- Which API method is the canonical "hello world"?
- What are the recurring depth layers across call chains?
- Which internal components are shared by multiple API methods?

For each Tier 1 API, confirm the call chain trace:

```
Client calls API method
  → Entry point (public method on API class)
    → Dispatch (routing, resolution, delegation)
      → Processing (core logic, transformation)
        → Infrastructure (I/O, caching, threading)
```

Name the recurring depth layers — they become the implementation layers in each feature's vertical slice. Not every API call goes through all layers.

### Step 3: Architecture Analysis (Agent-Powered)

**Dispatch 1-2 `api-learning:api-code-architect` agents** to analyze the framework's architecture for vertical slice planning:

- **Agent A — Vertical Slice Boundaries**: How to group API methods into capabilities, what internal machinery each slice needs, which internals are shared across slices
- **Agent B — Simplification Strategy** (for medium/complex frameworks): At each depth layer, what can be simplified, merged, skipped, or hardcoded without breaking the API contract

Example prompt:
```
Analyze the architecture of <framework> at <path> for API-first vertical slice planning.
Based on these explorer findings: <paste key findings>
For each API capability, determine: which depth layers are needed, what internals to build,
what to simplify at each layer. Map shared internals across capabilities.
```

### Step 4: Define Features as Vertical Slices

Each feature represents one API capability — a single thing a user can DO with the framework, implemented as a vertical slice from API surface to internals.

**Good v2 feature**: "Handle HTTP GET Requests" — includes the route annotation API,
the dispatcher that matches routes, the handler invocation, and the response serialization.
A client can actually USE this after implementing it.

**Bad v2 feature (this is v1 thinking)**: "Request Router" — an internal component with
no directly callable API. The learner builds it but can't use it until other layers exist.

The test: after implementing a feature, can a client write code that calls the API and
gets a result? If yes, it's a good v2 feature. If no, it's too low-level — merge it into
the feature whose API it supports.

### Step 5: Order by Usage Tier and Dependency

1. **Start with Tier 1 APIs** — the "hello world" of the framework
2. **Add Tier 2 APIs** — customization and configuration
3. **End with Tier 3 APIs** — advanced/extension capabilities

Within each tier, respect dependencies: if Feature B's implementation reuses internals
from Feature A, Feature A comes first. But always prefer the order that gives the learner
a working API at each step.

### Step 6: Decide Granularity

Based on framework complexity:
- **Small framework** (< 50 public API classes): 5–8 features
- **Medium framework** (50–200): 8–15 features
- **Large framework** (200+): 15–25 features

Each feature should be implementable in a single focused session. If a feature requires
more than ~10 new classes, split it into sub-features (e.g., "Handle GET Requests" then
"Handle POST with Request Body").

### Step 7: Generate the Outline

Read [references/outline-template.md](references/outline-template.md) for the exact template structure.

The outline includes:
- **API Surface Map**: visual overview of which APIs belong to which features
- **Call Chain Diagrams**: for each Tier 1 API, the trace from surface to internals
- **Feature List**: ordered vertical slices with API contracts, depth layers, and real source mappings
- **Simplification Strategy**: what internal complexity is deferred vs. implemented

### Step 8: Generate Project Skeleton

Create a buildable Maven or Gradle project:

```
simple-<framework-name>/
├── api-outline.md
├── .gitignore                  # target/, .gradle/, .idea/, *.class, etc.
├── mvnw (or gradlew)           # Build wrapper script
├── mvnw.cmd (or gradlew.bat)   # Windows wrapper
├── .mvn/ (or gradle/)          # Wrapper config
├── pom.xml (or build.gradle)
├── src/
│   ├── main/java/simple/<framework>/
│   │   ├── api/          ← public API classes (what clients import)
│   │   └── internal/     ← implementation classes (what API classes delegate to)
│   └── test/java/simple/<framework>/
│       ├── api/          ← client-perspective tests
│       └── internal/     ← unit tests for internals
└── api-docs/
    └── (populated by /api-builder — tutorial chapters go here)
```

The `api/` vs `internal/` split is deliberate — it mirrors the API-first philosophy.
Clients import from `api/`; the `internal/` package contains the machinery.

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

### Step 9: Verify

- [ ] Every feature has a clear API contract (public class/method the client calls)
- [ ] Features are ordered so each one produces a callable API
- [ ] No feature is purely internal — every feature traces from API to implementation
- [ ] Dependencies flow correctly (no feature depends on an unimplemented later feature)
- [ ] The project skeleton compiles with `./mvnw compile` or `./gradlew build`
- [ ] At least one Tier 1 API is in the first feature

## Output Summary

When done, tell the user:
1. Where the outline is: `simple-<framework-name>/api-outline.md`
2. How many features were identified
3. What the first API capability is (feature #1) and what a client can do after implementing it
4. How to proceed: "Pick a feature and run `/api-builder "<feature name>"`"
