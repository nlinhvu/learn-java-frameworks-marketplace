# Framework Analysis Checklist (API-First)

Use this checklist to systematically analyze a framework's source code from the outside in.
The order matters — start with what clients see, then trace inward.

## Phase 1: The API Surface (what clients touch)

### 1.1 Published Artifacts
- [ ] What Maven/Gradle modules get published? Which is the "client" module?
- [ ] Is there a separate `-api` or `-core` artifact vs. `-impl`?
- [ ] What does `module-info.java` export? (Java 9+)

### 1.2 Public API Classes
- [ ] Which classes/interfaces are in the top-level or `api/` package?
- [ ] Which classes appear in README examples and "Getting Started" guides?
- [ ] Which classes have `@Public`, `@API`, or are documented as stable?
- [ ] Which classes are imported in integration tests and example projects?

### 1.3 Entry Points
- [ ] What's the first thing a user creates? (`new Builder()`, `static create()`, `@Annotation`)
- [ ] What's the "hello world" — the simplest working usage?
- [ ] What factory methods, builders, or bootstrap classes exist?

### 1.4 API Tiering
- [ ] **Tier 1 (Essential)**: Used in every application — what are they?
- [ ] **Tier 2 (Common)**: Used for customization by most users — what are they?
- [ ] **Tier 3 (Advanced)**: Extension points for power users — what are they?

## Phase 2: Call Chains (what happens when clients call the API)

### 2.1 Tier 1 Call Traces
For each Tier 1 API method, trace the execution:
- [ ] Entry point method → what does it call first?
- [ ] How many layers deep before real work happens?
- [ ] Where does delegation occur? (strategy, chain-of-responsibility, observer)
- [ ] Where does the "return value" get constructed?

### 2.2 Layer Identification
From the call traces, identify recurring layers:
- [ ] **API Layer**: Public classes that validate input and delegate
- [ ] **Dispatch Layer**: Resolution, routing, lookup (if exists)
- [ ] **Processing Layer**: Core business logic, transformation
- [ ] **Infrastructure Layer**: I/O, caching, threading, resource management

### 2.3 Shared Internals
- [ ] Which internal classes appear in multiple call chains?
- [ ] Are there shared registries, caches, or context objects?
- [ ] What's the "glue" that connects different API capabilities?

## Phase 3: Configuration & Extension (how clients customize)

### 3.1 Configuration API
- [ ] How do users configure the framework? (builder, properties, annotations, XML)
- [ ] What are the configuration entry points?
- [ ] What are sensible defaults vs. required configuration?

### 3.2 Extension Points
- [ ] What interfaces can users implement to extend behavior?
- [ ] Is there an SPI (Service Provider Interface)?
- [ ] What annotations trigger user-defined behavior?
- [ ] What listener/callback hooks exist?

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
