# Tutorial Chapter Template

Use this template for each `core-docs/chNN_<feature_name>.md`. Adapt content to the specific feature.

---

```markdown
# Chapter N: [Feature Name]

> Line references based on commit `<short-hash>` of the [Framework Name] repository.

## Build Challenge

| Current State                                      | Limitation                                 | Objective                                          |
|----------------------------------------------------|--------------------------------------------|----------------------------------------------------|
| [What the code looks like after previous chapters]  | [A concrete problem or missing capability] | [What the reader will build in this chapter]       |

---

## N.1 The Integration Point

[Show the EXACT code where this feature plugs into the existing system FIRST.
This is typically a modification to an existing file — the place where two
subsystems connect. Show the code before any supporting pieces exist.]

**Modifying:** `src/main/java/com/simple/<framework>/<path>/ExistingClass.java`
**Change:** [Brief description of the integration — e.g., "Add argument resolution before handler invocation"]

```java
// The modified method showing the new integration
// This references classes/interfaces that don't exist yet — that's intentional
```

[After the code, state the DIRECTION — what this integration point tells us we need to build:]

Two key decisions here:

1. [Why was this integration point chosen over alternatives?]
2. [What design constraint does it introduce?]

This [connects Subsystem A to Subsystem B]. To make it work, we need to build:
- [Component X — what it does]
- [Component Y — what it does]
- [Component Z — what it does]

[**For foundation chapters (ch01)** with no prior system to plug into: show the core data
structure instead, and state what future features will connect to it.]

## N.2 [First Supporting Component]

[Build the first piece that the integration point requires.]

[Code-first. For NEW files, just show the code:]

**New file:** `src/main/java/com/simple/<framework>/<path>/NewClass.java`

```java
package com.simple.<framework>.<path>;

public class NewClass {
    // implementation
}
```

## N.3 [Second Supporting Component]

[Continue building. Each sub-section adds one piece of the solution.]

## N.4 Try It Yourself

[Present 1-2 challenges before showing the full solution. The code already exists in src/, so the user can peek if needed — or delete and try from scratch.]

<details>
<summary>Challenge: [Descriptive challenge title]</summary>

[Hint or starting point]

```java
// Solution code
```

</details>

## N.5 Tests

### Unit Tests

**New file:** `src/test/java/com/simple/<framework>/<path>/FeatureTest.java`

```java
package com.simple.<framework>.<path>;

import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.*;

class FeatureTest {

    @Test
    void shouldDoExpectedBehavior_WhenCondition() {
        // Arrange
        ...

        // Act
        ...

        // Assert
        assertThat(result).isEqualTo(expected);
    }

    @Test
    void shouldHandleAnotherCase_WhenDifferentCondition() {
        ...
    }
}
```

### Integration Tests

[Only include if this feature interacts with prior features]

**New file:** `src/test/java/com/simple/<framework>/integration/FeatureIntegrationTest.java`

```java
package com.simple.<framework>.integration;

import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.*;

class FeatureIntegrationTest {

    @Test
    void shouldIntegrateWithPriorFeature_WhenUsedTogether() {
        ...
    }
}
```

**Run:** `./mvnw test` — expected: all tests pass (including prior features' tests)

---

## N.6 Why This Works

[Explain the design decisions AFTER the reader has seen and run the code.]

> ★ **Insight** ─────────────────────────────────
> - [Why this design decision is a best practice]
> - [What alternatives were considered and why they fall short]
> - [When this pattern should NOT be used]
> ─────────────────────────────────────────────────

[Optional: additional insight blocks, up to 3 total per chapter]

## N.7 What We Enhanced

[Mandatory for all chapters after ch01. Skip for ch01.]

| Aspect    | Before (chNN)             | Current (this chapter)      | Real Framework                                |
|-----------|---------------------------|-----------------------------|-----------------------------------------------|
| [Concern] | [Previous simplification] | [What this chapter improved] | [Production approach with `File.java:line`]  |

## N.8 Connection to Real Framework

| Simplified Code  | Real Framework Code | File:Line           | Key Difference                  |
|------------------|---------------------|---------------------|---------------------------------|
| `SimpleClass`    | `RealClass`         | `RealClass.java:42` | [What the real version adds]    |
| `simpleMethod()` | `realMethod()`      | `RealClass.java:108`| [What the real version handles] |

## N.9 Complete Code

[ALL files created or modified in this chapter, shown in full.]
[This section is generated by READING BACK the actual source files from src/.]
[Include both production code AND test code.]

### Production Code

#### File: `src/main/java/com/simple/<framework>/<path>/NewClass.java` [NEW]

```java
[Full file content — read from the actual file]
```

#### File: `src/main/java/com/simple/<framework>/<path>/ModifiedClass.java` [MODIFIED]

```java
[Full file content — read from the actual file, showing the complete file after modification]
```

### Test Code

#### File: `src/test/java/com/simple/<framework>/<path>/FeatureTest.java` [NEW]

```java
[Full file content — read from the actual file]
```

#### File: `src/test/java/com/simple/<framework>/integration/FeatureIntegrationTest.java` [NEW]

```java
[Full file content — read from the actual file]
```

---

## Summary

| Concept              | What It Means           |
|----------------------|-------------------------|
| **[Term 1]**         | [One-line explanation]   |
| **[Term 2]**         | [One-line explanation]   |
| **[Pattern name]**   | [One-line explanation]   |

**Next: Chapter N+1 — [Next Feature Name]** — [One-sentence preview of what comes next and why it's needed]
```

---

## Template Usage Notes

### Build Challenge Guidelines
- **Current State**: Refer to the concrete output of the previous chapter (or "No simplified framework exists yet" for ch01)
- **Limitation**: State a specific, testable problem — not a vague learning goal
- **Objective**: Use a verb phrase describing what the reader will BUILD
- **Keep it to ONE row** — if you need multiple rows, the chapter covers too much

### Code Modification Tracking
When modifying files from prior features, ALWAYS:
1. State which file is being modified
2. Describe the change briefly
3. Show the new/changed code
4. In the Complete Code chapter, show the FULL file (not just the diff)

### Complete Code Chapter Rules
- Generated by reading actual files from `src/`, not written independently
- Must include EVERY file that was created or modified in this chapter
- Each file is marked `[NEW]` or `[MODIFIED]`
- Files are shown in full — no partial snippets
- Includes both `src/main/java/` and `src/test/java/` files
- If the reader copies all `[NEW]` files and replaces all `[MODIFIED]` files, the project must compile and all tests must pass

### Insight Block Guidelines
- **Placement:** ONLY in the "Why This Works" section
- **Frequency:** 1-3 per chapter
- **Focus:** "Why" not "what" — the chapter already shows what
- **Include trade-offs:** Mention when the pattern is NOT the right choice
- **Connect to real world:** Reference how the actual framework uses the same principle
