# api-learning

Learn any project's API surface from the **outside in** — analyze source projects in **any language** (Python, Go, Rust, Node.js, C#, Ruby, Java, etc.) and build simplified Java reimplementations as vertical slices.

## Skills

| Skill | Purpose |
|-------|---------|
| `api-blueprint` | Analyzes any source project repo and produces an API-first learning outline (`api-outline.md`) |
| `api-builder` | Implements a single API capability as working Java code + tests + tutorial chapter |

## Agents

| Agent | Role |
|-------|------|
| `api-code-explorer` | Traces API surfaces, call chains, and implementation layers in any-language source projects |
| `api-code-architect` | Designs simplified Java vertical slice architectures and simplification strategies |
| `api-code-reviewer` | Validates API contracts, test quality, and tutorial accuracy of the Java output |

## Usage

```
# Step 1: Generate the learning outline from any source project
/api-blueprint /path/to/source-project

# Examples with different languages:
/api-blueprint /path/to/fastapi          # Python source → simplified Java
/api-blueprint /path/to/gin              # Go source → simplified Java
/api-blueprint /path/to/actix-web        # Rust source → simplified Java
/api-blueprint /path/to/javalin          # Java source → simplified Java

# Step 2: Implement features one by one (output is always simplified Java)
/api-builder "Handle HTTP GET Requests"
```

## Prerequisites

- Java 17+ (for the simplified output project)
- Maven or Gradle (for the simplified output project)
- A locally cloned project repository (any language — Python, Go, Rust, Node.js, C#, Ruby, Java, etc.)

## License

[Apache License 2.0](LICENSE)
