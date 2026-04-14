# Technology Defaults for Core-First Reimplementation

## Scope

Core-first reimplementations produce **simplified Java versions** of source projects, built inside-out from the heart. The goal is learning internal architecture, not production deployment. Technology choices prioritize clarity and minimal dependencies.

## Java Stack

| Category | Technology | Version | Why This Choice |
| --- | --- | --- | --- |
| Language | Java | 17+ (upgrade to 21+ when needed) | Records, sealed classes; virtual threads when concurrency features are reached |
| Framework | None (plain Java) | — | Simplified reimplementations must be framework-agnostic to focus on patterns |
| HTTP | java.net.http.HttpClient | Java 11+ | Standard library, no external dependency |
| Concurrency | java.util.concurrent + Virtual Threads | Java 21+ | Standard library; upgrade from 17 when first concurrency feature is reached |
| JSON | Jackson or Gson | Latest | Most adopted JSON libraries in Java |
| Build | Maven (preferred) | Latest | Standard artifact building and wrapper support |
| Testing | JUnit 5 + AssertJ | Latest | Modern Java testing stack |

## Technology Substitution Rules

When mapping source technologies to simplified Java equivalents:

1. **Prefer JDK built-in types** over external libraries
2. **Hard-code** what can be made configurable later
3. **Skip** infrastructure (databases, caches, messaging) — use in-memory data structures
4. **Simplify** concurrency — use simple synchronous code until a feature specifically teaches concurrency patterns

Each technology mapping decision is a teaching moment — document with a ★ Insight block.

## Concurrency Rules

| Source Pattern | Java Equivalent |
| --- | --- |
| Goroutines (Go) | Virtual Threads (when feature requires it) |
| async/await (JS/C#/Python) | Virtual Threads or synchronous (if concurrency is not the lesson) |
| Tokio tasks (Rust) | Virtual Threads |
| Event loop (Node.js) | Virtual Threads or synchronous |

For simplified reimplementations, default to **synchronous code** unless the feature being taught is specifically about concurrency. Upgrade to virtual threads when the learning objective requires it.

## Package Structure

Source project package/module structures vary by language. The simplified Java reimplementation uses a flat structure:

```
src/main/java/simple/<project>/
    (classes organized by feature, not by layer)
src/test/java/simple/<project>/
    (mirrors source structure)
```

Organize classes by feature proximity rather than by architectural layer, since the inside-out approach builds feature by feature from the core outward.
