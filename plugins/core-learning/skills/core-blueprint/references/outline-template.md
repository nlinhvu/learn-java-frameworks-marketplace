# Outline Template (Core-First)

Use this template for `simple-<framework-name>/core-outline.md`. Adapt section content to the specific framework.

---

```markdown
# Outline: Simple [Framework Name]

> Learning [Framework Name] by building a simplified version from source.
> Generated from commit `<short-hash>` of the [Framework Name] repository.

## Quick Start

1. Browse the feature list below and pick one (start from #1 if this is your first time)
2. Run `/core-builder "<feature name>"`
3. The skill will generate:
   - A tutorial guide in `core-docs/chNN_<feature_name>.md`
   - Working Java code in `src/main/java/`
   - Tests in `src/test/java/`
4. Read the tutorial, explore the code, or delete it and rebuild from the tutorial

## Architecture Overview

[One paragraph describing what this framework does and its core philosophy]

**Real-world analogy:** [Analogy mapping to 3+ framework components]

### Component Diagram

```
[ASCII diagram showing how the outlined components relate to each other]
[Use boxes for components, arrows for dependencies]
[Example:]

┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│   Feature 1  │────>│   Feature 3   │────>│   Feature 5      │
│   (Core)     │     │  (Extension)  │     │  (Integration)   │
└──────┬───────┘     └──────────────┘     └─────────────────┘
       │
       ▼
┌─────────────┐     ┌──────────────┐
│   Feature 2  │────>│   Feature 4   │
│  (Support)   │     │  (Advanced)   │
└─────────────┘     └──────────────┘
```

### Dependency Graph

```
[ASCII showing which features MUST come before which]
[Example:]

1. Core Container ──┬──> 3. Bean Lifecycle
                    │
                    └──> 2. Configuration ──> 4. Auto-Configuration ──> 5. Starters
```

## Features

### 1. [Feature Name] ⬜
- **Description:** [One-line: what this component does in the framework]
- **Depends on:** None
- **Maps to:** `real/source/path/Package.java`, `real/source/path/Other.java`
- **Complexity:** Low / Medium / High
- **Java version:** 17 (or 21+ if needed, with reason)
- **What you'll build:** [Concrete deliverable — e.g., "A minimal IoC container that registers and resolves beans by type"]

### 2. [Feature Name] ⬜
- **Description:** [One-line]
- **Depends on:** Feature 1
- **Maps to:** `real/source/path/`
- **Complexity:** Low / Medium / High
- **Java version:** 17
- **What you'll build:** [Concrete deliverable]

[... continue for all features ...]

## Implementation Notes

### Simplification Strategy
[Brief description of what the simplified version will and will NOT cover]

**In scope (happy path):**
- [Core behavior 1]
- [Core behavior 2]

**Out of scope (edge cases / advanced):**
- [Advanced feature 1]
- [Advanced feature 2]

### Java Version
- **Baseline:** Java 17
- **Features requiring 21+:** [List any, or "None"]
```

---

## Feature Status Markers

When features are implemented via `/core-builder`, the ⬜ marker is updated:
- ⬜ = Not yet implemented
- ✅ = Implemented and tests passing
