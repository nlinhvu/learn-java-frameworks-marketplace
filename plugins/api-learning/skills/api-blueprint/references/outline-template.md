# Outline Template (API-First)

Use this template for the `api-outline.md` file. Replace all `{{placeholders}}`.

---

```markdown
# Simple {{Framework Name}} — API-First Learning Outline

> Learn {{framework}} by building a simplified version from the **outside in** — start with
> the APIs that clients actually call, then implement the internal machinery behind each one.

## Quick Start

```bash
# After implementing Feature 1, a client can already do:
{{minimal usage example — 3-5 lines showing the simplest API call}}
```

## Architecture Overview

{{One paragraph: what does this framework do for its users? Not what it IS internally,
but what it DOES — what problem does a client solve by using it?}}

**Real-world analogy**: {{Analogy framed from the user's perspective — e.g., "Using Spring MVC
is like having a receptionist (DispatcherServlet) who takes every visitor's request, looks up
which department handles it, and routes them there."}}

## API Surface Map

Shows which public API classes/interfaces belong to each feature:

```
┌─────────────────────────────────────────────────────────────┐
│                      CLIENT CODE                            │
│  uses: {{TopLevelAPI}}, {{ConfigAPI}}, {{AdvancedAPI}}       │
└──────────────┬──────────────┬───────────────┬───────────────┘
               │              │               │
    ┌──────────▼──────┐ ┌────▼────────┐ ┌────▼────────────┐
    │   Feature 1     │ │  Feature 2  │ │   Feature 3     │
    │ {{API class}}   │ │ {{API cls}} │ │  {{API class}}  │
    │ {{API method}}  │ │ {{method}}  │ │  {{method}}     │
    └──────────┬──────┘ └────┬────────┘ └────┬────────────┘
               │              │               │
    ┌──────────▼──────────────▼───────────────▼───────────────┐
    │                 SHARED INTERNALS                         │
    │  {{internal components shared across features}}          │
    └─────────────────────────────────────────────────────────┘
```

## Call Chain Diagrams

For each Tier 1 API, trace the execution path:

### {{API Method Name}} Call Chain
```
Client calls: {{api.method(args)}}
  │
  ├─► [API Layer] {{PublicClass.method()}}
  │     Validates input, creates internal request
  │
  ├─► [Dispatch Layer] {{Dispatcher.route()}}
  │     Resolves which handler/processor to use
  │
  ├─► [Processing Layer] {{Processor.execute()}}
  │     Core logic: {{what it does}}
  │
  └─► [Infrastructure Layer] {{if applicable — I/O, caching, etc.}}
        {{what it does}}
```

{{Repeat for each Tier 1 API}}

## Features

### Feature 1: {{API Capability Name}} {{Tier: 1}}
⬜ Not started

**API Contract** (what clients call):
```java
// Client writes this:
{{code showing how a client uses this API — 3-8 lines}}
```

**What it does**: {{One sentence describing the capability from the user's perspective}}

**Depth layers** (what gets built to support this API):

| Layer          | Simplified Class | Real Framework Class | Purpose                |
|----------------|------------------|----------------------|------------------------|
| API            | `Simple{{Name}}` | `{{RealClass}}`      | {{what client imports}} |
| Dispatch       | `{{Name}}`       | `{{RealClass}}`      | {{routing/resolution}} |
| Processing     | `{{Name}}`       | `{{RealClass}}`      | {{core logic}}         |
| Infrastructure | `{{Name}}`       | `{{RealClass}}`      | {{if needed}}          |

**Real source files**: {{comma-separated list of key source files in the real framework}}

**Depends on**: None (first feature)
**Complexity**: {{Low / Medium / High}} · {{estimated new classes}} new classes
**Java version**: {{8 / 11 / 17 / 21}}

**Concrete deliverables**:
- [ ] `api/{{ClassName}}.java` — {{what it provides}}
- [ ] `internal/{{ClassName}}.java` — {{what it does}}
- [ ] `api/{{ClassName}}Test.java` — client-perspective test
- [ ] `internal/{{ClassName}}Test.java` — internal unit test
- [ ] `api-docs/ch01_{{feature_name}}.md` — tutorial chapter

---

### Feature 2: {{API Capability Name}} {{Tier: 1|2}}
⬜ Not started

**API Contract** (what clients call):
```java
{{client usage code}}
```

**What it does**: {{one sentence, user perspective}}

**Depth layers**:

| Layer | Simplified Class | Real Framework Class | Purpose |
|-------|------------------|----------------------|---------|
| API   | ...              | ...                  | ...     |
| ...   | ...              | ...                  | ...     |

**Real source files**: {{...}}

**Depends on**: Feature 1 (reuses {{specific internal component}})
**Complexity**: {{...}} · {{N}} new classes, {{M}} modified
**Java version**: {{...}}

**Concrete deliverables**:
- [ ] ...

---

{{Continue for all features...}}

## Implementation Notes

### Simplification Strategy

| Real Framework Concept | Simplified Version | Why Simplified                            |
|------------------------|--------------------|-------------------------------------------|
| {{complex thing}}      | {{simple version}} | {{reason — what learning goal it serves}} |
| {{complex thing}}      | Deferred           | {{not needed to understand the API}}      |

### In Scope
- {{API capabilities being implemented}}

### Out of Scope
- {{Internal optimizations not visible through the API}}
- {{Edge cases that don't affect the learning path}}

### Java Version
- Base: Java {{version}}
- Features using newer APIs: noted per-feature

## Status Markers
- ⬜ = not started
- ✅ = implemented, tests passing, tutorial written
```
