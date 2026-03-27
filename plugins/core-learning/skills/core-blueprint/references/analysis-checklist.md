# Source Code Analysis Checklist (Core-First)

Use this checklist when analyzing a framework's source code to plan the outline. Not every item applies to every framework — skip what's irrelevant.

## 1. Build & Project Structure

- [ ] Read `pom.xml` / `build.gradle` for dependencies, Java version, module structure
- [ ] Identify multi-module structure (if any) — which module is the core?
- [ ] Note the base package name (e.g., `org.springframework.boot`)
- [ ] Check for `module-info.java` (Java module system usage)

## 2. Entry Points

- [ ] Find the main public API — what classes/interfaces do users interact with first?
- [ ] Identify factory classes, builders, or static entry points
- [ ] Look for `@API` annotations or `public` visibility patterns
- [ ] Read any "getting started" examples in the repo

## 3. Core Abstractions

- [ ] Map the key interfaces — what contracts does the framework define?
- [ ] Identify abstract base classes that provide default behavior
- [ ] Find the "God interface" or central abstraction everything revolves around
- [ ] Note generic type parameters and their bounds (they reveal design intent)

## 4. Implementation Classes

- [ ] Find the default/primary implementation of each core interface
- [ ] Look for inner classes (often hold mutable state for builders/specs)
- [ ] Identify the class hierarchy depth — deep hierarchies need more chapters
- [ ] Note which classes are `final` vs extensible

## 5. Lifecycle & State

- [ ] How are objects created? (constructors, factories, builders, DI)
- [ ] What state transitions do key objects go through?
- [ ] Are there initialization/shutdown hooks?
- [ ] Is there a container or registry that manages object lifecycle?

## 6. Extension Points

- [ ] Where can users plug in custom behavior? (interfaces, abstract classes, callbacks)
- [ ] What's the plugin/extension mechanism? (SPI, annotations, configuration)
- [ ] Which extension points are most commonly used?

## 7. Key Flows

- [ ] Trace 2-3 end-to-end operations (e.g., "handle an HTTP request", "send a message")
- [ ] Note where control passes between components
- [ ] Identify synchronous vs asynchronous boundaries
- [ ] Find where I/O or external system interaction happens

## 8. Design Patterns

- [ ] Which GoF / architectural patterns are used? (Factory, Strategy, Observer, etc.)
- [ ] Are there framework-specific patterns? (e.g., Spring's BeanPostProcessor)
- [ ] Note patterns that recur across multiple components

## 9. Configuration & Wiring

- [ ] How does the framework configure itself? (annotations, XML, properties, code)
- [ ] Is there auto-configuration or convention-over-configuration?
- [ ] How are dependencies between components resolved?

## 10. Simplification Opportunities

For each component, assess:
- [ ] Can it be reduced to a single class? (vs the real multi-class hierarchy)
- [ ] Can generic types be simplified? (e.g., `Map<String,String>` instead of `MultiValueMap`)
- [ ] Can I/O be faked or simplified? (in-memory instead of network/disk)
- [ ] Can configuration be hardcoded? (vs the real dynamic configuration)
- [ ] What's the minimum code that demonstrates the component's purpose?
