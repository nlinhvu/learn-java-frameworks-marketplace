---
name: core-builder
version: 1.0.0
description: >
  "implement core feature <name>", "build the <component> core feature", "next core feature",
  "next core", "continue building core", "core-builder" — this skill implements a specific
  feature from a core-blueprint outline (requires core-outline.md in the project). Generates
  working simplified Java source code, unit/integration tests, and a 10-section tutorial chapter
  with Mermaid diagrams and ★ Insight blocks, all verified to compile and pass. The source
  project may be in any language — the output is always simplified Java. Uses the inside-out
  approach starting from the integration point.
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

# Core Builder — Feature Implementation

## Philosophy: Integration Point First, Then Build Outward

Each feature starts at its **integration point** — the exact place where the new feature connects
to the existing system. The source project may be in any language (Python, Go, Rust, Node.js, C#,
Ruby, Java, etc.) — the output is always simplified Java. The implementation follows the same
direction a contributor would: "I see where this feature plugs in, I want to understand what
supporting code makes it work, so I trace outward from the integration point."

This produces two artifacts, one truth:
1. **Working Java code** in `src/` — compiles, tests pass, is the source of truth
2. **Rich tutorial chapter** in `core-docs/` — reads back actual source files, never diverges, enriched with ★ Insight blocks and Mermaid diagrams at every design decision

Every integration point choice, simplification decision, technology mapping choice, and design pattern is a teaching moment. The learner doesn't just see WHAT was built — they understand WHY each choice was made, how the source language's idioms were translated to Java, and what alternatives were considered.

## MANDATORY: Use Extended Thinking

**Use extended thinking before implementing.** The quality of the implementation depends on deep upfront reasoning about both the real source project and the simplified Java version. Before writing any code:

- Study the source project's implementation of this feature end-to-end (in whatever language it's written in)
- Identify the integration point — the exact seam where this feature connects to the existing system
- Plan the technology mapping — how source language constructs translate to Java (e.g., goroutines to threads/executors, Python generators to Java iterators, Rust traits to Java interfaces)
- Plan the simplified class mapping — which source types to keep, merge, skip, or hardcode
- Design the build sequence starting from the integration point outward
- Determine what to reuse and extend from prior features

Skipping extended thinking leads to implementations that miss the integration point, duplicate existing code, or produce a simplified version that doesn't capture the essential pattern.

## Input

The user provides a feature name matching an entry in `core-outline.md`:
```
/core-builder "Bean Lifecycle"
```

If the user says "next core feature", "next core", or similar without specifying a name, the skill
automatically selects the next unimplemented feature from `core-outline.md`.

## Workflow

### Step 1: Locate and Validate

1. Find the `simple-<project-name>/` project directory (look for `core-outline.md` in the current directory or immediate subdirectories)
2. **If a feature name is provided**, read `core-outline.md` to find the requested feature. If no match, report an error and list available features.
3. **If no feature name is provided** (e.g., user said "next core feature" or "next core"), scan `core-outline.md` for the first feature still marked ⬜ whose dependencies are all ✅. Use that as the target feature. If all features are ✅, tell the user the outline is complete.
4. Determine the chapter number (`chNN`) based on the feature's position in the outline
5. **Check dependencies**: verify that all prerequisite features are implemented (look for ✅ markers or existing source files). If dependencies are missing, warn the user: "Feature X depends on [A, B] which haven't been implemented yet. Implement those first, or proceed at your own risk."

### Step 2: Study the Real Source Code (Agent-Powered)

**Dispatch a `core-learning:core-code-explorer` agent** to deeply analyze the source project's implementation of this feature. The source may be in any language — the agent adapts its analysis accordingly.

Example prompt:
```
Analyze the <feature-name> implementation in the source project at <project-path>.
Source language: <language>
Focus on the files mapped in the outline: <maps-to list>
Trace the execution flow for the happy path.
Identify: key types/interfaces/traits, implementation classes/structs, design patterns, integration points.
Note language-specific idioms that will need Java equivalents.
Provide file:line references for the mapping table.
```

While the explorer agent runs, record the source project repo's commit hash:
```bash
git -C <project-path> rev-parse --short HEAD
```

After the agent returns, read the essential files it identifies to build your own understanding.

### Step 3: Plan the Simplified Java Implementation (Agent-Powered)

**Dispatch a `core-learning:core-code-architect` agent** to design the simplified Java implementation based on the explorer's findings.

Example prompt:
```
Design a simplified Java implementation of <feature-name> for the simple-<project> project.
Source language: <language>
Source project analysis: <paste explorer findings>
Existing simplified code: <list prior features' key files>
Outline entry: <paste the feature's outline entry including integration point hint>
Produce: technology mapping (source → Java), integration point design, class mapping table, build sequence, test plan.
```

The architect agent will deliver:
- **Integration point**: The exact seam, subsystems, direction, and code change
- **Technology mapping**: Source language constructs → Java equivalents (when source is not Java)
- **Class mapping**: Source project types → simplified Java classes with simplification rationale
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
cd simple-<project-name> && ./mvnw test
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
Review the <feature-name> implementation (ch<NN>) in the simple-<project> project at <path>.
Feature outline entry: <paste outline entry>
Source project: <project-path> (language: <source-language>)
Files created/modified: <list files>
Check: simplification correctness, integration point validity, test quality, progressive enhancement integrity.
Run ./mvnw test to verify all tests pass.
```

**Review loop orchestration** (you, the skill, drive this loop — not the reviewer agent):
1. Dispatch the reviewer agent and read its findings
2. If Critical issues exist (confidence >= 90): fix them, then re-dispatch the reviewer
3. If only Important issues remain (80-89): fix them and proceed to Step 8
4. Stop re-dispatching when: no Critical/High issues remain, OR 5 review passes completed, OR only subjective issues remain

For the full rubric and stop criteria, see [shared/quality-checklist.md](shared/quality-checklist.md).

### Step 8: Write the Tutorial

Generate `core-docs/chNN_<feature_name>.md` using the template in [references/tutorial-template.md](references/tutorial-template.md). Refer to [shared/diagram-standards.md](shared/diagram-standards.md) for Mermaid color palette and [shared/insight-format.md](shared/insight-format.md) for ★ Insight block format.

Write all **10 sections** following this structure:
1. **N.1 The Integration Point** — the exact code where this feature plugs into the existing system, with direction statement
2. **N.2 Supporting Components** — first piece the integration point requires
3. **N.3 More Components** — continue building outward from integration point
4. **N.4 Try It Yourself** — challenges in collapsible `<details>` blocks
5. **N.5 Tests** — unit tests and integration tests (if applicable)
6. **N.6 Why This Works** — ★ Insight blocks (1-3 per chapter, minimum Why + Trade-off + Recommend)
7. **N.7 What We Enhanced** — ch01: "Foundation established" summary; ch02+: mandatory enhancement table
8. **N.8 Connection to Source Project** — source mapping with file:line references, commit hash, and technology mapping (source language → Java)
9. **N.9 Architecture Visualization** — Mermaid diagrams for this feature's integration point and component relationships
10. **N.10 Complete Code** — read back every source file created or modified from actual `src/`; include full content; mark each file `[NEW]` or `[MODIFIED]`

**Code-first rule:** Present code before explanations. The reader sees what to build, tries it, runs the tests, THEN reads why it works.

**★ Insight blocks in implementation:** Add ★ Insight blocks at major design decision points in N.1-N.3, not just N.6.

End the chapter with a Summary table and "Next chapter" pointer per the tutorial template.

This is the "copy-paste guarantee" — if a reader copies all `[NEW]` files and
replaces all `[MODIFIED]` files, the project must compile with all tests passing.

### Step 9: Update the Outline

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
- Use JDK built-in types where the source project has custom types
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

- **All 10 sections** (N.1 through N.10) present in every chapter
- **Integration point comes FIRST** (N.1) — show the code where this feature plugs into the existing system, then state the direction
- **Code comes FIRST** (N.1–N.5) before ALL explanations (N.6+)
- **Build Challenge opens** every chapter — frames the task, not a reading assignment
- **★ Insight blocks in TWO places** — at design decision points in N.1-N.3, AND comprehensive insights in N.6
- **Tests before explanations** — the reader runs the tests to confirm it works, then reads why
- **"What We Enhanced" is mandatory** for all features after ch01
- **Architecture Visualization (N.9)** — at least one Mermaid diagram per chapter, using standardized color palette
- **Complete Code is mandatory** for every feature — shows ALL files in full

## Techniques

See [references/techniques.md](references/techniques.md) for detailed patterns and examples.

## Quality Checklist

Quick-reference checklist. For the full reviewer rubric with confidence scoring and review loop protocol, see [shared/quality-checklist.md](shared/quality-checklist.md).

Before finalizing, verify:

### Code
- [ ] All source files are in proper package under `src/main/java/`
- [ ] All test files are in proper package under `src/test/java/`
- [ ] `./mvnw test` passes (ALL tests, not just new ones)
- [ ] No edge case handling beyond what's needed to demonstrate the pattern
- [ ] Modified files from prior features are tracked

### Tutorial
- [ ] All 10 sections present (N.1 through N.10)
- [ ] Opens with Build Challenge table (Current State / Limitation / Objective)
- [ ] N.1 shows the integration point with "Direction" statement and ★ Insight blocks at key decisions
- [ ] N.2-N.3 have ★ Insight blocks at design decision points
- [ ] Code sections (N.1–N.5) appear BEFORE all explanation sections (N.6+)
- [ ] N.4 has collapsible `<details>` challenges
- [ ] N.5 has Tests section with unit tests (and integration tests if applicable)
- [ ] Test method names describe behavior: `should<X>_When<Y>`
- [ ] N.6 has 1-3 ★ Insight blocks with Why + Trade-off + Recommend
- [ ] N.7 has enhancement table (ch02+) or foundation summary (ch01)
- [ ] N.8 has Connection to Source Project with file:line references, commit hash, and technology mapping
- [ ] N.9 has Mermaid diagrams with standard color palette and `<!-- diagram: -->` markers
- [ ] N.10 has all files read from `src/`, marked `[NEW]`/`[MODIFIED]`
- [ ] Copy-paste guarantee: copying files produces a compiling, passing project
- [ ] All modifications to prior features' files are explicitly declared with diffs

## Output Summary

When done, tell the user:
1. Which feature was implemented and its chapter number
2. Test results — all tests passing (both new and prior features)
3. Where the tutorial is: `core-docs/chNN_<feature_name>.md`
4. Key design decisions made (brief)
5. Suggest the next feature from the outline: "Run `/core-builder "<next feature>"`"
