# Source Code Analysis Checklist (Core-First)

Use this checklist when analyzing a source project's code to plan the outline. The source project may be in any language. Not every item applies to every project — skip what's irrelevant.

## 0. Technology Identification

- [ ] Identify the source language and version (e.g., Python 3.11, Go 1.21, Rust 1.75, Java 17, Node.js 20, C# 12, Ruby 3.3)
- [ ] Identify the build system / package manager (pip/poetry, go mod, cargo, npm/yarn, maven/gradle, nuget, bundler, etc.)
- [ ] Note key language idioms in use (decorators, traits, goroutines, async/await, pattern matching, generators, etc.)
- [ ] Preliminary technology mapping: list source constructs that will need Java equivalents

## 1. Build & Project Structure

- [ ] Read the build file (`pom.xml`, `build.gradle`, `go.mod`, `Cargo.toml`, `package.json`, `pyproject.toml`, `*.csproj`, `Gemfile`, etc.) for dependencies, language version, module structure
- [ ] Identify multi-module/multi-package structure (if any) — which module is the core?
- [ ] Note the base package/module name (e.g., `org.springframework.boot`, `flask`, `gin`, `actix_web`)
- [ ] Check for module system usage (Java `module-info.java`, Go packages, Rust crates, Python packages, etc.)

## 2. Entry Points

- [ ] Find the main public API — what types/functions/classes do users interact with first?
- [ ] Identify factory functions, constructors, builders, or static entry points
- [ ] Look for visibility/export patterns (public classes, exported functions, `__all__`, `pub`, etc.)
- [ ] Read any "getting started" examples in the repo

## 3. Core Abstractions

- [ ] Map the key types — what contracts does the project define? (interfaces, traits, protocols, abstract classes, type hints)
- [ ] Identify base types that provide default behavior
- [ ] Find the central abstraction everything revolves around
- [ ] Note generic/template type parameters and their constraints (they reveal design intent)

## 4. Implementation Types

- [ ] Find the default/primary implementation of each core abstraction
- [ ] Look for nested types, closures, or inner classes (often hold mutable state)
- [ ] Identify the type hierarchy depth — deep hierarchies need more chapters
- [ ] Note which types are sealed, final, or extensible

## 5. Lifecycle & State

- [ ] How are objects/instances created? (constructors, factories, builders, DI, initialization functions)
- [ ] What state transitions do key objects go through?
- [ ] Are there initialization/shutdown hooks or lifecycle callbacks?
- [ ] Is there a container, registry, or runtime that manages object lifecycle?

## 6. Extension Points

- [ ] Where can users plug in custom behavior? (interfaces, traits, callbacks, middleware, hooks, decorators)
- [ ] What's the plugin/extension mechanism? (SPI, annotations, decorators, middleware registration, configuration)
- [ ] Which extension points are most commonly used?

## 7. Key Flows

- [ ] Trace 2-3 end-to-end operations (e.g., "handle an HTTP request", "send a message")
- [ ] Note where control passes between components
- [ ] Identify synchronous vs asynchronous boundaries
- [ ] Find where I/O or external system interaction happens

## 8. Design Patterns

- [ ] Which GoF / architectural patterns are used? (Factory, Strategy, Observer, etc.)
- [ ] Are there project-specific or language-specific patterns? (e.g., Spring's BeanPostProcessor, Go's middleware chains, Python's WSGI)
- [ ] Note patterns that recur across multiple components

## 9. Configuration & Wiring

- [ ] How does the project configure itself? (annotations, XML, properties, YAML, TOML, code, environment variables)
- [ ] Is there auto-configuration or convention-over-configuration?
- [ ] How are dependencies between components resolved?

## 10. Simplification Opportunities

For each component, assess:
- [ ] Can it be reduced to a single Java class? (vs the source's multi-type hierarchy)
- [ ] Can generic types be simplified? (e.g., `Map<String,String>` instead of custom collection types)
- [ ] Can I/O be faked or simplified? (in-memory instead of network/disk)
- [ ] Can configuration be hardcoded? (vs the real dynamic configuration)
- [ ] What's the minimum Java code that demonstrates the component's purpose?

## 10b. Technology Mapping Verification

For non-Java source projects, verify:
- [ ] Each major source language construct has a planned Java equivalent
- [ ] Concurrency model mapping is correct (goroutines → threads/executors, async/await → CompletableFuture, etc.)
- [ ] Type system differences are accounted for (duck typing → interfaces, traits → interfaces, sum types → sealed classes or enums)
- [ ] Idiomatic source patterns have Java counterparts (decorators → annotations/wrappers, middleware → filter chain, etc.)
- [ ] The mapping preserves architectural intent, not just syntax

## 11. Visualization & Insight Planning

### 11.1 Mermaid Diagram Planning
- [ ] Architecture overview diagram — what are the main components and how they relate?
- [ ] Component diagram — how subsystems connect (flowchart TD with subgraphs)?
- [ ] Dependency graph — which features MUST come before which?
- [ ] Learning roadmap — how do features depend on each other (for the Mermaid flowchart)?
- [ ] Integration point map — where each feature plugs into the core?
- [ ] Class hierarchy diagrams — key interface/class hierarchies?
- [ ] Lifecycle state diagrams — object state transitions?
- [ ] Apply standardized color palette per `shared/diagram-standards.md`

### 11.2 Insight Topic Identification
- [ ] Which integration point choices warrant ★ Insight blocks?
- [ ] Which simplification decisions need rationale documentation?
- [ ] Which design patterns are central and need explanation?
- [ ] Which progressive enhancements across chapters are most significant?
- [ ] All insights have minimum: **Why** + **Trade-off** + **Recommend** per `shared/insight-format.md`
