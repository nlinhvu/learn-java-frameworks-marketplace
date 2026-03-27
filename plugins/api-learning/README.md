# api-learning

Learn Java frameworks from the **outside in** by analyzing their public API surface and building simplified reimplementations as vertical slices.

## Skills

| Skill | Purpose |
|-------|---------|
| `api-blueprint` | Analyzes a framework repo and produces an API-first learning outline (`api-outline.md`) |
| `api-builder` | Implements a single API capability as working code + tests + tutorial chapter |

## Agents

| Agent | Role |
|-------|------|
| `api-code-explorer` | Traces API surfaces, call chains, and implementation layers |
| `api-code-architect` | Designs vertical slice architectures and simplification strategies |
| `api-code-reviewer` | Validates API contracts, test quality, and tutorial accuracy |

## Usage

```
# Step 1: Generate the learning outline
/api-blueprint /path/to/framework-repo

# Step 2: Implement features one by one
/api-builder "Handle HTTP GET Requests"
```

## Prerequisites

- Java 17+
- Maven or Gradle
- A locally cloned Java framework/library repository

## License

[Apache License 2.0](LICENSE)
