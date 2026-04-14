---
name: enterprise-builder
version: 1.0.0
description: >
  "implement <feature name>", "build the <feature>", "next feature", "next enterprise feature",
  "implement the next feature", "continue building", "enterprise-builder" — this skill implements
  a specific feature from an enterprise-outline.md. Produces working enterprise Java source code
  (Spring Boot 4 for applications, plain Java 21+ for libraries/frameworks), tests (Testcontainers
  for applications, plain JUnit for libraries/frameworks), and a 10-section tutorial chapter with
  Mermaid diagrams and Insight blocks, all verified to compile and pass.
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

# Enterprise Builder — Feature Implementation with Rich Tutorials

## Philosophy: The Code IS the Curriculum

Each feature implementation produces two artifacts, one truth:
1. **Working enterprise Java code** in `src/` — compiles, tests pass, is the source of truth
2. **Rich tutorial chapter** in `enterprise-docs/` — reads back actual source files, never diverges, enriched with ★ Insight blocks and Mermaid diagrams at every design decision

Every technology choice, pattern selection, and architectural decision is a teaching moment. The learner doesn't just see WHAT was built — they understand WHY each choice was made and what alternatives were considered.

## MANDATORY: Use Extended Thinking

**Use extended thinking before implementing.** The quality of the implementation depends on deep upfront reasoning about both the source project and the enterprise Java version. Before writing any code:

- Study the source project's implementation of this feature (technologies, patterns, concurrency)
- Map every source technology used in this feature to its Java equivalent
- Design the Java classes, entities, and configuration
- Plan the test strategy (Testcontainers for applications, plain JUnit for libraries/frameworks)
- Identify ★ Insight topics for technology substitutions and design decisions
- Determine what to reuse and extend from prior features

Skipping extended thinking leads to implementations with wrong technology choices, missing insights, or broken prior features.

## Input

The user provides a feature name matching an entry in `enterprise-outline.md`:
```
/enterprise-builder "User Authentication"
```

If the user says "next feature" or similar without specifying a name, the skill automatically selects the next unimplemented feature from `enterprise-outline.md`.

## Workflow

### Step 1: Locate and Validate

1. Find `enterprise-outline.md` in the project
2. **If a feature name is provided**, match it to an entry in the outline. If no match, report an error and list available features.
3. **If no feature name is provided** (e.g., user said "next feature"), scan `enterprise-outline.md` for the first feature still marked ⬜ whose dependencies are all ✅. Use that as the target feature. If all features are ✅, tell the user the outline is complete.
4. Check that all dependency features are marked ✅
5. If dependencies are missing, stop and tell the user which features must be implemented first
6. Identify the project type (Application/Library/Framework) from the outline header
7. Read the source project path from the `enterprise-outline.md` header (recorded during blueprint phase)

### Step 2: Study the Source Project (Agent-Powered)

**Dispatch an `enterprise-learning:enterprise-code-explorer` agent** (use the Agent tool with `subagent_type="enterprise-learning:enterprise-code-explorer"`) to deeply analyze the source project's implementation of this specific feature.

Example prompt:
```
Analyze the <feature-name> in the source project at <source-path>.
Focus on:
- Which source files implement this feature?
- What technologies are used (database, cache, messaging, HTTP, etc.)?
- What is the data model / business logic?
- What is the concurrency pattern?
- What external systems does it interact with?
- What is the API surface for this feature (endpoints, public methods, etc.)?
Provide specific file paths and technology details for the Java mapping.
```

After the agent returns, read the essential files it identifies to build your own understanding.

### Step 3: Plan the Implementation (Agent-Powered)

**Dispatch an `enterprise-learning:enterprise-code-architect` agent** (use the Agent tool with `subagent_type="enterprise-learning:enterprise-code-architect"`) to design the enterprise Java implementation. Resolve the stack choice BEFORE dispatching: read the project type from the outline and specify "Spring Boot 4" or "Plain Java 21+" in the prompt. Consult [shared/technology-defaults.md](shared/technology-defaults.md) for the canonical stack per project type, concurrency rules, and infrastructure constraints.

Example prompt:
```
Design the enterprise Java implementation of <feature-name> for the enterprise-<project> project.
Project type: [Application/Library/Framework]
Source analysis: <paste explorer findings>
Existing enterprise code: <list prior features' key files>
Outline entry: <paste the feature's outline entry>
Produce: class design, technology choices with ★ Insights, test strategy, build sequence.
Stack: [Spring Boot 4 / Plain Java 21+]
```

The architect agent will deliver:
- **Technology choices**: Source tech → Java tech for this feature, with ★ Insight rationale
- **Class design**: Java classes with responsibilities, package placement, and dependencies
- **Test strategy**: What to test, how (Testcontainers vs. plain JUnit)
- **Build sequence**: Ordered implementation steps
- **Mermaid diagrams**: Architecture slice for this feature

Review the architect's blueprint before proceeding. Adjust if needed.

### Step 4: Define the Module Contract / API Contract

Before writing implementation code, define exactly what this feature provides. This contract definition becomes section **N.1** of the tutorial chapter and serves as the specification for writing tests in Step 5.

For **Applications**:
- REST endpoints (method, path, request/response DTOs)
- Service interfaces and their behavioral contracts
- Entity classes with JPA annotations (records where possible)
- Configuration properties

For **Libraries**:
- Public API classes/interfaces in the public package
- Method signatures, parameter types, return types
- Usage examples

For **Frameworks**:
- Extension points, interfaces users implement
- Annotations and their effects
- Lifecycle hooks

### Step 5: Write Client Tests FIRST

Before implementing anything, write tests from the **client's perspective**:

For **Applications** (Testcontainers integration tests):
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Testcontainers
class UserControllerTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    @Test
    void shouldCreateUser_WhenValidRequest() {
        // Test the REST API as a client would
    }
}
```

For **Libraries/Frameworks** (plain JUnit):
```java
class HttpClientTest {
    @Test
    void shouldSendGetRequest_WhenUrlProvided() {
        // Test the public API as a consumer would
    }
}
```

These tests serve two purposes:
- They **define** the feature's behavior (test-as-specification)
- They **verify** the implementation works (test-as-validation)

### Step 6: Implement the Feature

Now implement the feature, following the architect's build sequence:

#### For Applications (Spring Boot 4):
1. **Entity classes** — JPA entities with `jakarta.*` annotations, records for DTOs
2. **Repository interfaces** — Spring Data JPA repositories
3. **Service classes** — Business logic with `@Service`, virtual threads for concurrency
4. **Controller classes** — REST endpoints with `@RestController`
5. **Configuration** — `application.yml` additions, `@Configuration` classes
6. **Security** (if applicable) — Spring Security `SecurityFilterChain` beans

#### For Libraries (Plain Java 21+):
1. **Public API classes** — Interfaces and classes consumers import
2. **Internal implementation** — Classes that make the API work
3. **Configuration** — Builder pattern for library configuration (no Spring)

#### For Frameworks (Plain Java 21+):
1. **Extension interfaces** — What users implement
2. **Core engine** — Processing, routing, lifecycle management
3. **Default implementations** — Sensible defaults users can override

After each major component, run the client tests. They should progressively start passing.

### Step 7: Write Internal Unit Tests

After the feature works (client tests pass), add internal unit tests:

For **Applications**: Unit tests for services, edge cases for repositories (with Testcontainers)
For **Libraries/Frameworks**: Unit tests for internal components, edge cases

Test naming convention: `should<ExpectedBehavior>_When<Condition>`

### Step 8: Build and Verify

Run all tests — both new and existing:

```bash
./mvnw test
```

**Every test must pass** — old features must not break. If they do, you've changed a shared component in a way that breaks a previous feature. Fix it.

For applications, also verify Docker infrastructure is running:
```bash
docker-compose up -d
./mvnw verify
```

### Step 9: Quality Review (Agent-Powered)

**Dispatch an `enterprise-learning:enterprise-code-reviewer` agent** (use the Agent tool with `subagent_type="enterprise-learning:enterprise-code-reviewer"`) to review the implementation.

Example prompt:
```
Review the <feature-name> implementation (ch<NN>) in the enterprise-<project> project at <path>.
Project type: [Application/Library/Framework]
Feature outline entry: <paste outline entry>
Source project: <source-path>
Files created/modified: <list files>
Check: correct stack for project type, technology mapping accuracy, test quality,
tutorial completeness (all 10 sections), Mermaid diagram quality, ★ Insight depth.
Run ./mvnw test to verify all tests pass.
```

**Review loop orchestration** (you, the skill, drive this loop — not the reviewer agent):
1. Dispatch the reviewer agent and read its findings
2. If Critical issues exist (confidence >= 90): fix them, then re-dispatch the reviewer
3. If only Important issues remain (80-89): fix them and proceed to Step 10
4. Stop re-dispatching when: no Critical/High issues remain, OR 5 review passes completed, OR only subjective issues remain

For the full rubric and stop criteria, see [shared/quality-checklist.md](shared/quality-checklist.md).

### Step 10: Write the Tutorial Chapter

Read [references/tutorial-template.md](references/tutorial-template.md) and follow each section's rules. Refer to [shared/diagram-standards.md](shared/diagram-standards.md) for Mermaid color palette and [shared/insight-format.md](shared/insight-format.md) for ★ Insight block format.

Write all **10 sections** following this structure:
1. **N.1 Module Contract / API Contract** — what this feature provides (from Step 4)
2. **N.2 Client Tests** — behavior specification from consumer's perspective
3. **N.3 Implementation** — with ★ Insight blocks at each decision point
4. **N.4 Internal Tests** — Testcontainers for applications, plain JUnit for libraries/frameworks
5. **N.5 Try It Yourself** — challenges in collapsible `<details>` blocks
6. **N.6 Why This Works** — ★ Insight blocks (1-3 per chapter, minimum Why + Trade-off + Recommend)
7. **N.7 What We Enhanced** — ch01: "Foundation established" summary; ch02+: mandatory enhancement table
8. **N.8 Technology Mapping** — source tech → Java for this feature, with ★ Insights
9. **N.9 Architecture Visualization** — Mermaid diagrams for this feature's architecture slice
10. **N.10 Complete Code** — read back every source file created or modified from actual `src/`; include full content; mark each file `[NEW]` or `[MODIFIED]`

End the chapter with a Summary table and "Next chapter" pointer per the tutorial template.

This is the "copy-paste guarantee" — if a reader copies all `[NEW]` files and replaces all `[MODIFIED]` files:
- Applications: project compiles and all tests pass with Docker running
- Libraries/Frameworks: project compiles and all tests pass without Docker

### Step 11: Update the Outline

In `enterprise-outline.md`, change the feature's status from `⬜` to `✅`.

## Key Principles

### The Progressive Build Rule
Each feature builds on previous features. When Feature 3 needs a service that Feature 1 already built, Feature 3 extends it rather than duplicating. The "What We Enhanced" table tracks this growth.

### The Correct Stack Rule
Applications use Spring Boot 4 with real infrastructure. Libraries and frameworks use plain Java 21+. 🚫 Libraries MUST NOT have any Spring dependency.

### The Technology Transparency Rule
Every source technology → Java technology mapping is documented with a ★ Insight block. When no direct equivalent exists, the deviation is a teaching moment, not a silent substitution.

### The Virtual Threads Rule
All concurrency uses virtual threads. 🚫 Never Spring WebFlux, Reactive Streams, Mutiny, or RxJava.

### The Real Infrastructure Rule (Applications)
🚫 No H2, no in-memory maps, no embedded databases. Use PostgreSQL, Redis, Kafka, Neo4j as appropriate.

### The Jakarta Rule
All code uses `jakarta.*` namespace. 🚫 Never `javax.servlet`, `javax.persistence`, etc.

### Handling Modifications to Earlier Code

When a new feature modifies code from a previous feature:
1. State which file is being modified
2. Show the specific change (not the whole file)
3. Explain why this feature requires this change
4. In the Complete Code section, show the full modified file marked `[MODIFIED]`

## Techniques

See [references/techniques.md](references/techniques.md) for detailed teaching patterns and examples.

## Quality Checklist

Quick-reference checklist. For the full reviewer rubric with confidence scoring and review loop protocol, see [shared/quality-checklist.md](shared/quality-checklist.md).

Before finalizing, verify:

### Code
- [ ] Correct stack for project type (Spring for apps, plain Java for libs/frameworks)
- [ ] Java 21+ features used (records, sealed classes, pattern matching, virtual threads)
- [ ] Jakarta EE namespace (`jakarta.*`) — never `javax.*`
- [ ] No Spring WebFlux / Reactive Streams anywhere
- [ ] No H2 / in-memory databases (applications)
- [ ] `./mvnw test` passes (ALL tests, not just new ones)
- [ ] Modified files from prior features are tracked

### Tutorial
- [ ] All 10 sections present (N.1 through N.10)
- [ ] N.1 shows what this feature provides (contract/API)
- [ ] N.2 shows client tests before implementation
- [ ] N.3 has ★ Insight blocks at design decision points
- [ ] N.4 uses correct test infrastructure (Testcontainers for apps, JUnit for libs)
- [ ] N.5 has collapsible `<details>` challenges
- [ ] N.6 has 1-3 ★ Insight blocks with Why + Trade-off + Recommend
- [ ] N.7 has enhancement table (ch02+) or foundation summary (ch01)
- [ ] N.8 maps this feature's source tech → Java tech with ★ Insights
- [ ] N.9 has Mermaid diagrams with standard color palette and `<!-- diagram: -->` markers
- [ ] N.10 has all files read from `src/`, marked `[NEW]`/`[MODIFIED]`
- [ ] Copy-paste guarantee: copying files produces a compiling, passing project
- [ ] All modifications to prior features' files are explicitly declared

## Output Summary

When done, tell the user:
1. Which feature was implemented and its chapter number
2. Test results — all tests passing (both new and prior features)
3. Where the tutorial is: `enterprise-docs/chNN_<feature_name>.md`
4. Key technology mappings for this feature
5. Suggest the next feature from the outline: "Run `/enterprise-builder "<next feature>"`"
