---
name: core-builder
version: 1.0.0
description: >
  Implements a specific feature from a core-blueprint outline, generating working Java source code,
  unit/integration tests, and a code-first tutorial guide — all verified to compile and pass. Uses
  the inside-out approach starting from the integration point. Requires a feature name argument
  matching an entry in core-outline.md. Triggers on "core-builder", "implement <feature name>", "build
  the <component> feature", "next feature", "next core feature", or any request to implement a
  component from a core-blueprint outline.
---

# Core Builder — Feature Implementation

## Philosophy: Integration Point First, Then Build Outward

Each feature starts at its **integration point** — the exact place where the new feature connects
to the existing system. The implementation follows the same direction a contributor would: "I see
where this feature plugs in, I want to understand what supporting code makes it work, so I trace
outward from the integration point."

This produces two artifacts, one truth:
1. **Real Java code** in `src/` — compiles, tests pass, is the source of truth
2. **Tutorial chapter** in `core-docs/` — reads back the actual source files, never diverges

## MANDATORY: Use Extended Thinking

**Use extended thinking before implementing.** The quality of the implementation depends on deep upfront reasoning about both the real framework and the simplified version. Before writing any code:

- Study the real framework's implementation of this feature end-to-end
- Identify the integration point — the exact seam where this feature connects to the existing system
- Plan the simplified class mapping — which real classes to keep, merge, skip, or hardcode
- Design the build sequence starting from the integration point outward
- Determine what to reuse and extend from prior features

Skipping extended thinking leads to implementations that miss the integration point, duplicate existing code, or produce a simplified version that doesn't capture the essential pattern.

## Input

The user provides a feature name matching an entry in `core-outline.md`:
```
/core-builder "Bean Lifecycle"
```

If the user says "next feature", "next core feature", or similar without specifying a name, the skill
automatically selects the next unimplemented feature from `core-outline.md`.

## Workflow

### Step 1: Locate and Validate

1. Find the `simple-<framework-name>/` project directory (look for `core-outline.md` in the current directory or immediate subdirectories)
2. **If a feature name is provided**, read `core-outline.md` to find the requested feature. If no match, report an error and list available features.
3. **If no feature name is provided** (e.g., user said "next feature" or "next core feature"), scan `core-outline.md` for the first feature still marked ⬜ whose dependencies are all ✅. Use that as the target feature. If all features are ✅, tell the user the outline is complete.
4. Determine the chapter number (`chNN`) based on the feature's position in the outline
5. **Check dependencies**: verify that all prerequisite features are implemented (look for ✅ markers or existing source files). If dependencies are missing, warn the user: "Feature X depends on [A, B] which haven't been implemented yet. Implement those first, or proceed at your own risk."

### Step 2: Study the Real Source Code (Agent-Powered)

**Dispatch a `core-learning:core-code-explorer` agent** to deeply analyze the real framework's implementation of this feature.

Example prompt:
```
Analyze the <feature-name> implementation in the framework at <framework-path>.
Focus on the files mapped in the outline: <maps-to list>
Trace the execution flow for the happy path.
Identify: key interfaces, implementation classes, design patterns, integration points.
Provide file:line references for the mapping table.
```

While the explorer agent runs, record the framework repo's commit hash:
```bash
git -C <framework-path> rev-parse --short HEAD
```

After the agent returns, read the essential files it identifies to build your own understanding.

### Step 3: Plan the Simplified Implementation (Agent-Powered)

**Dispatch a `core-learning:core-code-architect` agent** to design the simplified implementation based on the explorer's findings.

Example prompt:
```
Design a simplified implementation of <feature-name> for the simple-<framework> project.
Real framework analysis: <paste explorer findings>
Existing simplified code: <list prior features' key files>
Outline entry: <paste the feature's outline entry including integration point hint>
Produce: integration point design, class mapping table, build sequence, test plan.
```

The architect agent will deliver:
- **Integration point**: The exact seam, subsystems, direction, and code change
- **Class mapping**: Real framework classes → simplified classes with simplification rationale
- **Build sequence**: Implementation order (integration point first)
- **Test plan**: What to verify with unit and integration tests

Review the architect's blueprint before proceeding. Adjust if needed based on your understanding from step 2.

### Step 4: Implement the Code

Follow the architect's blueprint from step 3. **Start with the integration point** — write/modify the existing file where the new feature plugs in FIRST (e.g., modify the central dispatch method, or the adapter's invocation pipeline). This code will initially reference classes/interfaces that don't exist yet — that's expected. The compiler errors tell you exactly what to build next.

Then build the supporting pieces that the integration point requires, in dependency order per the architect's build sequence.

Write Java source files in `src/main/java/` following the project's package structure.

**When creating new files:** just write them.

**When modifying existing files** (from prior features): make the changes, and track exactly what changed — you'll need this for the tutorial's diff sections.

**Key principles:**
- Happy-path only — no edge case handling unless it's core to understanding the pattern
- Use standard JDK APIs where possible
- Keep each class focused and small
- Name things clearly — the code is educational

### Step 5: Write the Tests

Write tests in `src/test/java/` mirroring the source package structure.

**Unit tests** (`FeatureTest.java`): Test the feature's components in isolation.
**Integration tests** (`integration/FeatureIntegrationTest.java`): Test how this feature works with previously implemented features. Only create when this feature genuinely interacts with prior features.

**Test naming:** `should<ExpectedBehavior>_When<Condition>` — e.g.:
- `shouldResolveBean_WhenRegisteredByType`
- `shouldApplyPostProcessor_WhenBeanCreated`
- `shouldThrowException_WhenBeanNotFound`

Tests serve as living documentation. Someone reading only the test names should understand what the feature does.

### Step 6: Build and Verify

Run the full test suite — not just this feature's tests:
```bash
cd simple-<framework-name> && ./mvnw test
```
(or `./gradlew test` for Gradle projects)

**All tests must pass** — both the new tests and all prior features' tests. If something fails:
1. Read the error
2. Fix the code
3. Retry
4. Do not finalize until the build is green

**Java version handling:** If this feature requires Java 21+ capabilities, update `pom.xml` / `build.gradle` compiler settings, note the version change in the tutorial, and verify all prior features' tests still pass at the new version.

### Step 7: Quality Review (Agent-Powered)

**Dispatch a `core-learning:core-code-reviewer` agent** to review the implementation before writing the tutorial.

Example prompt:
```
Review the <feature-name> implementation (ch<NN>) in the simple-<framework> project at <path>.
Feature outline entry: <paste outline entry>
Real framework source: <framework-path>
Files created/modified: <list files>
Check: simplification correctness, integration point validity, test quality, progressive enhancement integrity.
Run ./mvnw test to verify all tests pass.
```

The reviewer will check:
- Simplified code correctly captures the essential pattern
- Integration point is correctly identified and implemented first
- Tests cover core behavior with proper naming conventions
- Prior features' tests still pass
- Code is ready for tutorial documentation

**If the reviewer finds critical issues (confidence >= 90):** fix them before proceeding.
**If only important issues (80-89):** fix what improves educational quality, note the rest.

### Step 8: Write the Tutorial

Generate `core-docs/chNN_<feature_name>.md` using the template in [references/tutorial-template.md](references/tutorial-template.md).

**Code-first rule:** Present code before explanations. The reader sees what to build, tries it, runs the tests, THEN reads why it works.

**Modification tracking:** When the feature modifies files from prior features, explicitly declare each change:
```markdown
**Modifying:** `src/main/java/simple/<framework>/Core.java`
**Change:** Add method `handleRequest()` after the existing `register()` method

```java
public void handleRequest(Request req) { ... }
```‎
```

**Try It Yourself sections:** Present challenges in `<details>` tags:
```markdown
<details><summary>Challenge: Implement the bean resolver</summary>

```java
// Solution code here
```‎

</details>
```

The user can attempt the challenge before looking at the answer. Since the actual code already exists in `src/`, they can also just look at the files directly.

### Step 9: Generate the Complete Code Chapter

This is the final section (N.9) of the tutorial. Generate it by **reading back the actual source files** that were written/modified in this feature:

1. List all files created or modified in this feature
2. Read each file's full content from the filesystem
3. Write them into the tutorial with headers indicating `[NEW]` or `[MODIFIED]`

This guarantees the Complete Code chapter and `src/` are always identical.

Include both production code AND test code in this chapter.

### Step 10: Update the Outline

Mark the feature as completed in `core-outline.md`:
- Change `⬜` to `✅` for this feature

## Key Principles

### The Integration Point Rule
Every feature starts at its integration point — the exact place where the new feature connects
to the existing system. Build from that seam outward. The integration point reveals the direction
and purpose of everything else in the feature.

### The Reuse Rule
Later features REUSE and EXTEND internals from earlier features. When Feature 3 needs a registry,
and Feature 1 already built one, Feature 3 extends it rather than building a new one. This mirrors
how real frameworks grow.

### The Simplification Rule
Implement the **minimum** that makes the feature work correctly:
- Happy-path only — no edge case handling unless core to understanding the pattern
- Hard-code what can be made configurable later
- Use JDK built-in types where the real framework has custom types
- Defer performance optimizations entirely

Each later feature can enhance earlier components. The "What We Enhanced" table tracks this
progression.

### Handling Modifications to Earlier Code
When a new feature modifies code from a previous feature (extending a registry,
adding a method to an existing class), always:
1. State which file is being modified
2. Show the specific change (not the whole file)
3. Explain why this feature requires this change to existing code
4. In the Complete Code section, show the full modified file marked `[MODIFIED]`

## Tutorial Structure

Each tutorial follows the structure in [references/tutorial-template.md](references/tutorial-template.md). Key structural rules:

- **Integration point comes FIRST** (N.1) — show the code where this feature plugs into the existing system, then state the direction
- **Code comes FIRST** (N.1–N.5) before ALL explanations (N.6+)
- **Build Challenge opens** every chapter — frames the task, not a reading assignment
- **Tests before explanations** — the reader runs the tests to confirm it works, then reads why
- **"What We Enhanced" is mandatory** for all features after ch01
- **Complete Code is mandatory** for every feature — shows ALL files in full
- **Insight blocks** (1-3 per chapter) appear only in "Why This Works" — focus on "why", include trade-offs

## Techniques

See [references/techniques.md](references/techniques.md) for detailed patterns and examples.

## Quality Checklist

Before finalizing, verify:

### Code
- [ ] All source files are in proper package under `src/main/java/`
- [ ] All test files are in proper package under `src/test/java/`
- [ ] `./mvnw test` passes (ALL tests, not just new ones)
- [ ] No edge case handling beyond what's needed to demonstrate the pattern
- [ ] Modified files from prior features are tracked

### Tutorial
- [ ] Opens with Build Challenge table (Current State / Limitation / Objective)
- [ ] N.1 shows the integration point — the exact code where this feature connects to the existing system (for foundation chapters: the core data structure that future features depend on)
- [ ] Integration point includes a "Direction" statement: what two subsystems connect and what supporting code needs to be built
- [ ] Key decisions at the integration point are explained (1-2 sentences)
- [ ] Code sections (N.1–N.5) appear BEFORE all explanation sections (N.6+)
- [ ] Has "Try It Yourself" section with `<details>` challenges
- [ ] Has Tests section with unit tests (and integration tests if applicable)
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
3. Where the tutorial is: `core-docs/chNN_<feature_name>.md`
4. Suggest the next feature from the outline: "Run `/core-builder "<next feature>"`"
