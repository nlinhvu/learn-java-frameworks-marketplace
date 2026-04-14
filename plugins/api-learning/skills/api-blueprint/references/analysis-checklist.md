# Project Analysis Checklist (API-First)

Use this checklist to systematically analyze a source project's code from the outside in.
The source project can be in any language (Python, Go, Rust, Node.js, C#, Ruby, Java, etc.).
The order matters — start with what clients see, then trace inward.

## Phase 0: Technology Identification

### 0.1 Source Language & Stack
- [ ] What programming language is the project written in?
- [ ] What language version/runtime is required?
- [ ] What build/package system is used? (pip/poetry, go.mod, Cargo.toml, package.json, NuGet, Gemfile, Maven/Gradle, etc.)
- [ ] What key frameworks/libraries does it depend on?
- [ ] What language-specific patterns will need Java equivalents? (decorators, async/await, goroutines, traits, channels, generators, etc.)

### 0.2 Technology Mapping (Source → Java)
- [ ] Which source language concepts map directly to Java? (classes → classes, interfaces → interfaces)
- [ ] Which concepts need adaptation? (Python decorators → Java annotations, Go channels → Java queues, etc.)
- [ ] Which concepts have no direct Java equivalent and need creative solutions? (Rust ownership, Python generators, etc.)

## Phase 1: The API Surface (what clients touch)

### 1.1 Published Artifacts
- [ ] What packages/modules/crates/gems get published? Which is the "client" module?
- [ ] Is there a separate API/core artifact vs. implementation?
- [ ] What does the module system export? (Python `__init__.py`, Go package exports, Rust `pub`, Java `module-info.java`, etc.)

### 1.2 Public API Elements
- [ ] Which classes/functions/types are in the top-level or public package?
- [ ] Which elements appear in README examples and "Getting Started" guides?
- [ ] Which elements are documented as public/stable API?
- [ ] Which elements are imported in integration tests and example projects?

### 1.3 Entry Points
- [ ] What's the first thing a user creates? (constructor, factory, decorator, builder, CLI command)
- [ ] What's the "hello world" — the simplest working usage?
- [ ] What factory methods, builders, or bootstrap mechanisms exist?

### 1.4 API Tiering
- [ ] **Tier 1 (Essential)**: Used in every application — what are they?
- [ ] **Tier 2 (Common)**: Used for customization by most users — what are they?
- [ ] **Tier 3 (Advanced)**: Extension points for power users — what are they?

## Phase 2: Call Chains (what happens when clients call the API)

### 2.1 Tier 1 Call Traces
For each Tier 1 API method/function, trace the execution:
- [ ] Entry point method/function → what does it call first?
- [ ] How many layers deep before real work happens?
- [ ] Where does delegation occur? (strategy, chain-of-responsibility, observer, middleware, decorator)
- [ ] Where does the "return value" get constructed?

### 2.2 Layer Identification
From the call traces, identify recurring layers:
- [ ] **API Layer**: Public classes/functions that validate input and delegate
- [ ] **Dispatch Layer**: Resolution, routing, lookup (if exists)
- [ ] **Processing Layer**: Core business logic, transformation
- [ ] **Infrastructure Layer**: I/O, caching, threading, resource management

### 2.3 Shared Internals
- [ ] Which internal classes/modules appear in multiple call chains?
- [ ] Are there shared registries, caches, or context objects?
- [ ] What's the "glue" that connects different API capabilities?

## Phase 3: Configuration & Extension (how clients customize)

### 3.1 Configuration API
- [ ] How do users configure the project? (builder, properties, annotations, decorators, config files, env vars, CLI flags)
- [ ] What are the configuration entry points?
- [ ] What are sensible defaults vs. required configuration?

### 3.2 Extension Points
- [ ] What interfaces/protocols/traits can users implement to extend behavior?
- [ ] Is there a plugin system, SPI, or extension point mechanism?
- [ ] What annotations/decorators/attributes trigger user-defined behavior?
- [ ] What listener/callback/hook mechanisms exist?

### 3.3 Lifecycle
- [ ] What lifecycle events exist? (init, start, stop, destroy)
- [ ] Do users interact with lifecycle, or is it internal-only?
- [ ] What state transitions occur?

## Phase 4: Vertical Slice Planning

### 4.1 Feature Boundaries
For each candidate feature:
- [ ] Does it have a clear API contract (public method/class clients call)?
- [ ] Can a client USE this feature independently after it's implemented?
- [ ] How many depth layers does it touch?
- [ ] Does it introduce new internal classes or reuse existing ones?

### 4.2 Dependency Analysis
- [ ] Which features share internal components?
- [ ] What's the minimal set of internals needed for Feature 1?
- [ ] Where do later features EXTEND vs. REUSE internals from earlier features?

### 4.3 Simplification Decisions
For each internal component:
- [ ] Is this visible through the API? (if not, can it be drastically simplified?)
- [ ] What's the simplest implementation that makes the API work correctly?
- [ ] What can be hard-coded now and made configurable in a later feature?
- [ ] What error handling can be deferred? (happy path only for now)

## Phase 5: Design Patterns

### 5.1 Patterns Visible in the API
- [ ] Builder (fluent configuration)
- [ ] Factory Method (object creation)
- [ ] Strategy (pluggable behavior via interfaces)
- [ ] Observer/Listener (event-driven extension)

### 5.2 Patterns Hidden in Internals
- [ ] Chain of Responsibility (filter/interceptor chains)
- [ ] Template Method (hook methods in abstract classes)
- [ ] Proxy/Decorator (wrapping for cross-cutting concerns)
- [ ] Registry (mapping keys to handlers)
- [ ] Pipeline (staged processing)

### 5.3 Framework-Specific Idioms
- [ ] Any patterns unique to this framework?
- [ ] Any anti-patterns the framework explicitly avoids?

## Phase 6: Visualization & Insight Planning

### 6.1 Mermaid Diagram Planning
- [ ] Architecture overview diagram — what are the main components and how they relate?
- [ ] API Surface Map — which public classes belong to which features?
- [ ] Call chain diagrams — which Tier 1 APIs need sequence diagrams?
- [ ] Learning roadmap — how do features depend on each other (for the Mermaid flowchart)?
- [ ] Vertical slice diagrams — which features need layer visualizations?
- [ ] Apply standardized color palette per `shared/diagram-standards.md`

### 6.2 Insight Topic Identification
- [ ] Which API design decisions warrant ★ Insight blocks?
- [ ] Which simplification choices need rationale documentation?
- [ ] Which layer boundaries are non-obvious and need explanation?
- [ ] Which patterns are used differently from the source project and why?
- [ ] All insights have minimum: **Why** + **Trade-off** + **Recommend** per `shared/insight-format.md`
