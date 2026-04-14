---
name: api-blueprint
version: 1.0.0
description: >
  "analyze this project's API", "outside-in outline for X", "API-first learning plan",
  "create an API-first outline for X", "trace the public API", "analyze the API surface",
  "re-implement this API in Java", "simplified Java API reimplementation of X", "api-blueprint" —
  this skill analyzes any source project (Python, Go, Rust, Node.js, C#, Ruby, Java, etc.)
  from the outside in and produces an API-first learning outline (api-outline.md) that maps
  the public API surface, traces each capability inward through implementation layers,
  generates Mermaid call-chain diagrams and architecture visualizations, and creates a
  buildable simplified Java project skeleton with ★ Insight blocks and a visual learning roadmap.
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

# API Blueprint — API-First Learning Outline

## Philosophy: Outside-In, API-First

The fastest way to learn a project's design and confidently re-implement it is to start where
the **user** starts — the public API. Instead of hunting for the project's internal
"heart" and building outward (v1 approach), this skill:

1. **Identifies the API surface** — the classes, functions, endpoints, and methods that project
   users actually import and call in their application code (in any language)
2. **Organizes features as vertical slices** — each feature represents one API capability,
   traced from the public method all the way down through internal layers
3. **Orders by usage tier** — most commonly used APIs first, power-user/advanced APIs last
4. **Produces a simplified Java reimplementation** — the output is always a buildable Java project,
   regardless of the source project's language
5. **Visualizes everything** — Mermaid diagrams for API surface maps, call chains, learning paths,
   and architecture overviews with ★ Insight blocks at every design decision

This approach works because:
- You're **productive immediately** — each feature gives you a working, callable Java API
- You understand **why** each internal piece exists — because you traced from a concrete API call
- You learn **cross-language design patterns** — seeing how Python/Go/Rust concepts map to Java
- You **see the big picture** — Mermaid diagrams show how features, layers, and call chains connect

## MANDATORY: Use Extended Thinking

**Use extended thinking for the analysis phase.** The quality of the outline depends entirely on deep upfront reasoning about the source project's API surface and internal architecture. Before writing any output:

- Identify the source project's language and technology stack
- Map the full public API surface and classify by usage tier (Tier 1/2/3)
- Trace Tier 1 API call chains from the public method inward through each depth layer
- Identify recurring depth layers across call chains and name them
- Determine vertical slice boundaries — which API methods group into a single feature
- Plan the usage-tier ordering and dependency flow between features
- Plan technology mapping: source language concepts to Java equivalents

This deep analysis cannot be done incrementally. Skipping extended thinking leads to outlines with poorly bounded features, missed shared internals, or API capabilities that can't be implemented in the stated order.

## Input

The user provides the source project repo path as an argument. The source project can be in any language (Python, Go, Rust, Node.js, C#, Ruby, Java, etc.). If omitted, use the current working directory. Examples:
```
/api-blueprint /path/to/javalin          # Java source
/api-blueprint /path/to/fastapi          # Python source
/api-blueprint /path/to/gin              # Go source
/api-blueprint /path/to/actix-web        # Rust source
```

## Workflow

### Step 1: Explore the API Surface (Agent-Powered)

**Dispatch 2-3 `api-learning:api-code-explorer` agents in parallel** to deeply analyze the source project from the outside in. The source project can be in any language. Each agent focuses on a different aspect:

- **Agent 1 — API Surface & Entry Points**: Public API classes/functions/endpoints, README examples, "Getting Started" patterns, published artifacts/packages, tier classification (Tier 1/2/3), build/package config (any language's build system), language version
- **Agent 2 — Call Chain Tracing**: For each Tier 1 API, trace the execution path from the public method/function inward through dispatch, processing, and infrastructure layers. Map shared internals across call chains.
- **Agent 3 — Configuration & Extension Model** (for medium/complex projects): Configuration API (builder, properties, decorators, annotations, config files), extension points (plugins, middleware, custom implementations), lifecycle hooks — these become Tier 2/3 API capabilities

Give each agent the source project repo path and a specific focus area. Example prompt for Agent 1:
```
Analyze the API surface of the project at <path>. Focus on:
- Every public class/function/endpoint that project users import and call
- README and "Getting Started" examples — which are Tier 1?
- Published artifacts (which package/module is the client module?)
- Tier classification: Tier 1 (essential), Tier 2 (common customization), Tier 3 (advanced)
- Build/package config for dependencies and language version
- Identify the source language and key technology choices
Use the outside-in approach: start from what clients see and use.
```

After agents return, **read the files they identify as essential** before proceeding. Synthesize their findings into a unified understanding of the API surface.

Read [references/analysis-checklist.md](references/analysis-checklist.md) and verify coverage against its checklist.

### Step 2: Trace Call Chains

Synthesize the explorer agents' findings. For each Tier 1 API, ask:
- What is the simplest API call a client can make?
- Which API method/function is the canonical "hello world"?
- What are the recurring depth layers across call chains?
- Which internal components are shared by multiple API methods/functions?

For each Tier 1 API, confirm the call chain trace (regardless of source language):

```
Client calls API method/function
  → Entry point (public method/function on API class/module)
    → Dispatch (routing, resolution, delegation)
      → Processing (core logic, transformation)
        → Infrastructure (I/O, caching, threading)
```

Name the recurring depth layers — they become the implementation layers in each feature's simplified Java vertical slice. Not every API call goes through all layers.

### Step 3: Architecture Analysis (Agent-Powered)

**Dispatch 1-2 `api-learning:api-code-architect` agents** to analyze the source project's architecture for simplified Java vertical slice planning:

- **Agent A — Vertical Slice Boundaries**: How to group API methods/functions into capabilities, what internal machinery each slice needs, which internals are shared across slices
- **Agent B — Simplification & Technology Mapping Strategy** (for medium/complex projects): At each depth layer, what can be simplified, merged, skipped, or hardcoded without breaking the API contract. How source language concepts map to Java equivalents.

Include the Mermaid color palette from [shared/diagram-standards.md](shared/diagram-standards.md) in each architect agent's prompt so diagrams follow the standardized palette.

Example prompt:
```
Analyze the architecture of <project> at <path> for API-first vertical slice planning.
Source language: <language>. Based on these explorer findings: <paste key findings>
For each API capability, determine: which depth layers are needed, what internals to build,
what to simplify at each layer. Map shared internals across capabilities.
Plan technology mapping: how source language concepts translate to simplified Java.
Every key design decision needs a ★ Insight block explaining WHY.
Mermaid color palette: Teal #4ecdc4 (API surface), Blue #45b7d1 (internal logic),
Purple #bb8fce (infrastructure), Orange #f39c12 (config), Red #ff6b6b (error),
Green #27ae60 (success), Yellow #f7dc6f (external/client).
```

### Step 4: Define Features as Vertical Slices

Each feature represents one API capability — a single thing a user can DO with the project, reimplemented as a simplified Java vertical slice from API surface to internals.

**Good v2 feature**: "Handle HTTP GET Requests" — includes the route registration API,
the dispatcher that matches routes, the handler invocation, and the response serialization.
A client can actually USE this after implementing it in the simplified Java version.

**Bad v2 feature (this is v1 thinking)**: "Request Router" — an internal component with
no directly callable API. The learner builds it but can't use it until other layers exist.

The test: after implementing a feature, can a client write Java code that calls the API and
gets a result? If yes, it's a good v2 feature. If no, it's too low-level — merge it into
the feature whose API it supports.

### Step 5: Order by Usage Tier and Dependency

1. **Start with Tier 1 APIs** — the "hello world" of the project
2. **Add Tier 2 APIs** — customization and configuration
3. **End with Tier 3 APIs** — advanced/extension capabilities

Within each tier, respect dependencies: if Feature B's implementation reuses internals
from Feature A, Feature A comes first. But always prefer the order that gives the learner
a working API at each step.

### Step 6: Decide Granularity

Based on project complexity:
- **Small project** (< 50 public API classes/functions): 5–8 features
- **Medium project** (50–200): 8–15 features
- **Large project** (200+): 15–25 features

Each feature should be implementable in a single focused session. If a feature requires
more than ~10 new classes, split it into sub-features (e.g., "Handle GET Requests" then
"Handle POST with Request Body").

### Step 7: Generate the Outline

Read [references/outline-template.md](references/outline-template.md) for the exact template structure. Refer to [shared/diagram-standards.md](shared/diagram-standards.md) for the Mermaid color palette and diagram rules. Refer to [shared/insight-format.md](shared/insight-format.md) for the ★ Insight block format.

The outline includes:
- **API Surface Map**: Mermaid flowchart showing which public API classes belong to which features
- **Call Chain Diagrams**: Mermaid sequence diagrams for each Tier 1 API, tracing from surface to internals
- **Learning Path**: Mermaid roadmap of the learning journey (feature dependency graph annotated with learning objectives), with links to all tutorial chapters in recommended reading order, prerequisite knowledge per chapter, and progress tracking with ⬜/✅ markers
- **Feature List**: ordered vertical slices with API contracts, depth layers, real source mappings, and ★ Insight blocks
- **Simplification Strategy**: what internal complexity is deferred vs. implemented

### Step 8: Generate Project Skeleton

Create a buildable Maven or Gradle project (the output is always simplified Java, regardless of the source project's language):

```
simple-<project-name>/
├── api-outline.md
├── .gitignore                  # target/, .gradle/, .idea/, *.class, etc.
├── mvnw (or gradlew)           # Build wrapper script
├── mvnw.cmd (or gradlew.bat)   # Windows wrapper
├── .mvn/ (or gradle/)          # Wrapper config
├── pom.xml (or build.gradle)
├── src/
│   ├── main/java/simple/<project>/
│   │   ├── api/          ← public API classes (what clients import)
│   │   └── internal/     ← implementation classes (what API classes delegate to)
│   └── test/java/simple/<project>/
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
- Any minimal dependencies needed for the simplified Java implementation (keep to a minimum — prefer JDK-only)

**Plugins:**
- `maven-compiler-plugin` / equivalent Gradle config
- `exec-maven-plugin` if demo execution is needed

#### Generate Maven Wrapper

Run `mvn wrapper:wrapper` if Maven is available, or create the wrapper files directly:
```bash
cd simple-<project-name> && mvn wrapper:wrapper
```
If Maven is not available, create a minimal `mvnw` script and `.mvn/wrapper/maven-wrapper.properties` pointing to a stable Maven version (3.9.9).

### Step 9: Quality Review (Agent-Powered)

**Dispatch an `api-learning:api-code-reviewer` agent** (use the Agent tool with `subagent_type="api-learning:api-code-reviewer"`) to validate the blueprint:

```
Review the API-first blueprint for <project-name> at <path>.
Source language: <language>. Check: API surface map completeness, call chain accuracy,
feature ordering, vertical slice boundaries, technology mapping accuracy,
Mermaid diagram validity, ★ Insight depth, outline completeness,
Learning Path coherence, skeleton compilation.
```

**Review loop orchestration** (you, the skill, drive this loop — not the reviewer agent):
1. Dispatch the reviewer agent and read its findings
2. If Critical issues exist (confidence >= 90): fix them, then re-dispatch the reviewer
3. If only Important issues remain (80-89): fix them and proceed to Step 10
4. Stop re-dispatching when: no Critical/High issues remain, OR 5 review passes completed, OR only subjective issues remain

Refer to [shared/quality-checklist.md](shared/quality-checklist.md) for the full rubric and stop criteria.

### Step 10: Verify

- [ ] Every feature has a clear API contract (public class/method the client calls)
- [ ] Features are ordered so each one produces a callable API
- [ ] No feature is purely internal — every feature traces from API to implementation
- [ ] Dependencies flow correctly (no feature depends on an unimplemented later feature)
- [ ] Learning Path section present with Mermaid roadmap
- [ ] Mermaid diagrams use standardized color palette and have `<!-- diagram: slug -->` markers
- [ ] ★ Insight blocks have at minimum Why + Trade-off + Recommend
- [ ] The project skeleton compiles with `./mvnw compile` or `./gradlew build`
- [ ] At least one Tier 1 API is in the first feature

## Output Summary

When done, tell the user:
1. Where the outline is: `simple-<project-name>/api-outline.md`
2. The source project's language and key technologies mapped to Java equivalents
3. How many features were identified and their build sequence
4. What the first API capability is (feature #1) and what a client can do after implementing it
5. How to proceed: "Review the outline and learning path first, then pick a feature and run `/api-builder "<feature name>"`"
