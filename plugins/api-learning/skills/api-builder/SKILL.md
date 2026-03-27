---
name: api-builder
version: 1.0.0
description: >
  Implements a specific API capability from an api-blueprint outline, generating working Java source
  code, client-perspective and internal tests, and a code-first tutorial chapter — all verified to
  compile and pass. Requires a feature name argument matching an entry in api-outline.md. Triggers on
  "api-builder", "implement <feature name>", "build the <feature> API", "next feature", "next api",
  "next api feature", or any request to implement a vertical slice from an API-first learning outline.
---

# API Builder — API-First Implementation

## Philosophy: API Contract First, Then Implement Inward

Each feature is a **vertical slice** — a single API capability traced from the public surface
down through every internal layer needed to make it work. The implementation follows the
same direction a contributor would: "I see the API, I want to understand how it works, so
I trace the code from the public method inward."

This produces two artifacts, one truth:
1. **Real Java code** in `src/` — compiles, tests pass, is the source of truth
2. **Tutorial chapter** in `api-docs/` — reads back the actual source files, never diverges

## MANDATORY: Use Extended Thinking

**Use extended thinking before implementing.** The quality of the vertical slice depends on deep upfront reasoning about both the real framework and the simplified version. Before writing any code:

- Study the real framework's API contract and its full call chain from surface to internals
- Design the public API class/interface — method signatures, parameter types, return types
- Plan the depth layers — which layers this feature needs and what to simplify at each
- Determine what to reuse and extend from prior features' internals
- Design the client-perspective tests that will define the API's behavioral contract

Skipping extended thinking leads to vertical slices with incorrect API contracts, missing depth layers, or implementations that break prior features' tests.

## Input

The user provides a feature name matching an entry in `api-outline.md`:
```
/api-builder "Handle HTTP GET Requests"
```

If the user says "next feature", "next api", or similar without specifying a name, the skill
automatically selects the next unimplemented feature from `api-outline.md`.

## Workflow

### Step 1: Locate and Validate

1. Find `api-outline.md` in the project
2. **If a feature name is provided**, match it to an entry in the outline. If no match, report an error and list available features.
3. **If no feature name is provided** (e.g., user said "next feature" or "next api"), scan `api-outline.md` for the first feature still marked ⬜ whose dependencies are all ✅. Use that as the target feature. If all features are ✅, tell the user the outline is complete.
4. Check that all dependency features are marked ✅
5. If dependencies are missing, stop and tell the user which features must be implemented first

### Step 2: Study the Real Framework Source (Agent-Powered)

**Dispatch an `api-learning:api-code-explorer` agent** to deeply analyze the real framework's implementation of this API capability.

Example prompt:
```
Analyze the <feature-name> API capability in the framework at <framework-path>.
Focus on the files mapped in the outline: <maps-to list>
Start with the public API class — its method signatures, parameter types, return types.
Then trace the call chain inward through each depth layer listed in the outline.
Identify: API contract, dispatch logic, processing logic, infrastructure (if applicable).
Provide file:line references for the mapping table.
```

While the explorer agent runs, record the framework repo's commit hash:
```bash
git -C <framework-path> rev-parse --short HEAD
```

After the agent returns, read the essential files it identifies to build your own understanding.

### Step 3: Plan the Vertical Slice (Agent-Powered)

**Dispatch an `api-learning:api-code-architect` agent** to design the vertical slice implementation based on the explorer's findings.

Example prompt:
```
Design a vertical slice implementation of <feature-name> for the simple-<framework> project.
Real framework analysis: <paste explorer findings>
Existing simplified code: <list prior features' key files>
Outline entry: <paste the feature's outline entry including depth layers>
Produce: API contract, client test plan, depth layer implementations, build sequence (top-down).
```

The architect agent will deliver:
- **API contract**: The public class/interface with method signatures and usage example
- **Simplification decisions**: Per-layer choices (keep, merge, skip, hardcode)
- **Class mapping**: Real framework classes → simplified classes organized by depth layer
- **Build sequence**: Top-down implementation order (API contract → client tests → layers)
- **Test plan**: Client-perspective tests and internal unit tests

Review the architect's blueprint before proceeding. Adjust if needed based on your understanding from step 2.

### Step 4: Define the API Contract

Using the architect's blueprint from Step 3 as a starting point, refine and finalize the API contract.
This is the HEART of the implementation. Before writing any implementation code, define exactly what the
client sees:

1. **The public class/interface** — what the client imports from `api/`
2. **Method signatures** — parameters, return types, exceptions
3. **Usage example** — 3-8 lines showing how a client calls this API
4. **Behavioral contract** — what the API promises (not how it delivers)

Write this as a Java interface or class with method signatures and Javadoc.
This goes in `src/main/java/simple/<framework>/api/`.

### Step 5: Write Client-Perspective Tests FIRST

Before implementing anything, write tests from the **client's perspective** — tests that
use the API exactly as a framework user would:

```java
// This test represents how a real user would use the API
@Test
void shouldReturnGreeting_WhenHandlerRegistered() {
    // Arrange — client setup code
    var app = SimpleFramework.create();
    app.get("/hello", ctx -> ctx.text("Hello World"));

    // Act — client triggers the API
    var response = app.handleRequest("GET", "/hello");

    // Assert — client checks the result
    assertEquals(200, response.status());
    assertEquals("Hello World", response.body());
}
```

These tests serve two purposes:
- They **define** the API's behavior (test-as-specification)
- They **verify** the implementation works (test-as-validation)

Place client-perspective tests in `src/test/java/simple/<framework>/api/`.

### Step 6: Implement the Vertical Slice (Top-Down)

Now implement each depth layer, starting from the API surface and working inward.
The order follows the call chain:

#### 6a. API Layer (the entry point)
Write the public class that clients import. It should:
- Accept the client's input
- Validate and normalize it
- Delegate to the next layer
- Return the result to the client

This class lives in `api/` and is the only class clients import.

#### 6b. Dispatch Layer (if applicable)
Write the routing/resolution logic:
- How does the framework decide which handler/processor to use?
- Registry lookups, pattern matching, annotation scanning

This class lives in `internal/`.

#### 6c. Processing Layer
Write the core logic:
- The actual work the framework does on behalf of the client
- Transformation, execution, computation

This class lives in `internal/`.

#### 6d. Infrastructure Layer (if applicable)
Write supporting infrastructure:
- I/O, caching, threading, resource management
- Only if this feature's call chain reaches this deep

This class lives in `internal/`.

After each layer, run the client-perspective tests. They should progressively start
passing as you build deeper.

### Step 7: Write Internal Unit Tests

After the vertical slice works (client tests pass), add internal unit tests for
non-trivial internal components:

- Test edge cases in dispatch logic (pattern matching, fallback behavior)
- Test processing logic in isolation
- Test error handling paths

Place internal tests in `src/test/java/simple/<framework>/internal/`.

Test naming convention: `should<ExpectedBehavior>_When<Condition>`

### Step 8: Build and Verify

Run all tests — both new and existing:

```bash
./mvnw test
```

or

```bash
./gradlew test
```

**Every test must pass** — old features must not break. If they do, you've changed a
shared internal component in a way that breaks a previous API's contract. Fix it.

### Step 9: Quality Review (Agent-Powered)

**Dispatch an `api-learning:api-code-reviewer` agent** to review the implementation before writing the tutorial.

Example prompt:
```
Review the <feature-name> implementation (ch<NN>) in the simple-<framework> project at <path>.
Feature outline entry: <paste outline entry>
Real framework source: <framework-path>
Files created/modified: <list files>
Check: API contract correctness, vertical slice completeness, client test quality,
internal test coverage, progressive enhancement integrity.
Run ./mvnw test to verify all tests pass.
```

The reviewer will check:
- API contract mirrors the real framework's public interface
- Vertical slice is complete — every depth layer is implemented
- Client-perspective tests cover the behavioral contract
- Internal tests cover non-trivial logic
- Prior features' tests still pass
- Code is ready for tutorial documentation

**If the reviewer finds critical issues (confidence >= 90):** fix them before proceeding.
**If only important issues (80-89):** fix what improves educational quality, note the rest.

### Step 10: Write the Tutorial Chapter

Read [references/tutorial-template.md](references/tutorial-template.md) for the exact template.

The chapter follows the API-first flow:
1. The API Contract — what the client sees
2. Client Usage & Tests — how to use it and verify it works
3. Implementing the Call Chain — building each layer top-down
4. Internal Tests — targeted tests for internal components
5. Try It Yourself — challenges
6. Why This Works — insights
7. What We Enhanced — progression tracking
8. Connection to Real Framework — source mapping
9. Complete Code — ALL files, read back from `src/`

### Step 11: Generate Complete Code Section

Read back every source file created or modified in this feature from `src/`.
Include the full content. Mark each file `[NEW]` or `[MODIFIED]`.

This is the "copy-paste guarantee" — if a reader copies all `[NEW]` files and
replaces all `[MODIFIED]` files, the project must compile with all tests passing.

### Step 12: Update the Outline

In `api-outline.md`, change the feature's status from `⬜` to `✅`.

## Key Principles

### The Vertical Slice Rule
Every feature must produce a callable API. After implementing Feature N, a client can
write code that calls the Feature N API and gets correct results. No feature exists
solely as internal infrastructure.

### The Reuse Rule
Later features REUSE and EXTEND internals from earlier features. When Feature 3's call
chain needs a dispatcher, and Feature 1 already built one, Feature 3 extends it rather
than building a new one. This mirrors how real frameworks grow.

### The Depth Rule
Not every feature needs every layer. Some APIs are thin (API → Processing, no dispatch).
Some are deep (API → Dispatch → Processing → Infrastructure). Let the real framework's
call chain determine the depth.

### The Simplification Rule
At each depth layer, implement the **minimum** that makes the API work correctly:
- Hard-code what can be made configurable later
- Support only the happy path
- Use JDK built-in types where the real framework has custom types
- Defer performance optimizations entirely

Each later feature can enhance earlier layers. The "What We Enhanced" table tracks this
progression.

### Handling Modifications to Earlier Code

When a new feature modifies code from a previous feature (extending a dispatcher,
adding a method to an existing API class), always:
1. State which file is being modified
2. Show the specific change (not the whole file)
3. Explain why this API capability requires this change to existing internals
4. In the Complete Code section, show the full modified file marked `[MODIFIED]`

## Techniques

See [references/techniques.md](references/techniques.md) for detailed patterns and examples.

## Quality Checklist

Before finalizing, verify:

### Code
- [ ] All API classes are in `api/` package under `src/main/java/`
- [ ] All internal classes are in `internal/` package under `src/main/java/`
- [ ] Client-perspective tests are in `api/` package under `src/test/java/`
- [ ] Internal unit tests are in `internal/` package under `src/test/java/`
- [ ] `./mvnw test` passes (ALL tests, not just new ones)
- [ ] No edge case handling beyond what's needed to demonstrate the pattern
- [ ] Modified files from prior features are tracked

### Tutorial
- [ ] Opens with Build Challenge table (Current State / Limitation / Objective)
- [ ] N.1 shows the API Contract — what clients will call, with usage example
- [ ] N.2 shows Client Tests — test-as-specification, written before implementation
- [ ] N.3 implements the Call Chain top-down (API → Dispatch → Processing → Infrastructure)
- [ ] Code sections (N.1–N.4) appear BEFORE all explanation sections (N.5+)
- [ ] Has "Try It Yourself" section with `<details>` challenges
- [ ] Has Internal Tests section (N.4) for non-trivial internal components
- [ ] Test method names describe behavior: `should<X>_When<Y>`
- [ ] Has "Why This Works" with 1-3 `★ Insight` blocks
- [ ] Has "What We Enhanced" table (if not ch01) with at least one enhancement row
- [ ] Has "Connection to Real Framework" with file:line references and commit hash
- [ ] Has "Complete Code" chapter showing ALL files (production + test), matching `src/` exactly
- [ ] Summary table and "Next chapter" preview at bottom
- [ ] All modifications to prior features' files are explicitly declared with diffs
- [ ] Line references verified against the actual source at the recorded commit

## Output Summary

When done, tell the user:
1. Which feature was implemented and its chapter number
2. Test results — all tests passing (both new and prior features)
3. Where the tutorial is: `api-docs/chNN_<feature_name>.md`
4. Suggest the next feature from the outline: "Run `/api-builder "<next feature>"`"
