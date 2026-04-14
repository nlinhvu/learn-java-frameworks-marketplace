# core-learning

Learn any project's internals from the **inside out** — analyze source code in any language (Python, Go, Rust, Node.js, C#, Ruby, Java, etc.) and build simplified Java reimplementations that extend outward from the core.

## Skills

| Skill | Purpose |
|-------|---------|
| `core-blueprint` | Analyzes any project repo (any language) and produces an inside-out learning outline (`core-outline.md`) for simplified Java reimplementation |
| `core-builder` | Implements a single feature as working code + tests + tutorial chapter |

## Agents

| Agent | Role |
|-------|------|
| `core-code-explorer` | Traces internal hierarchies, integration points, and code patterns in any-language source projects |
| `core-code-architect` | Designs simplified Java component architectures from any-language source analysis |
| `core-code-reviewer` | Validates correctness, test quality, and tutorial accuracy |

## Usage

```
# Step 1: Generate the learning outline (source can be any language)
/core-blueprint /path/to/source-project

# Step 2: Implement features one by one (output is simplified Java)
/core-builder "Bean Lifecycle"
```

**Examples with different source languages:**
```
# Java source → simplified Java reimplementation
/core-blueprint /path/to/spring-framework

# Python source → simplified Java reimplementation
/core-blueprint /path/to/flask

# Go source → simplified Java reimplementation
/core-blueprint /path/to/gin

# Rust source → simplified Java reimplementation
/core-blueprint /path/to/actix-web
```

## Prerequisites

- Java 17+ (for the output project)
- Maven or Gradle (for building the output)
- A locally cloned source project repository (any language)

## License

[Apache License 2.0](LICENSE)
