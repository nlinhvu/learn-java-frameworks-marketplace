# Learn Java Frameworks Marketplace

A curated collection of [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugins for learning Java by analyzing real-world projects in **any language** (Python, Go, Rust, Node.js, C#, Ruby, Java, etc.) and building simplified Java reimplementations. Each plugin combines specialized skills, agents, and reference templates to guide you through analyzing source code internals and reimplementing them as working, tested Java code with progressive, code-first tutorial documentation.

## ⚠️🔥 Warning: High Token Usage 🔥⚠️

These skills scan both your working repository and the reference framework repository extensively, which consumes a **massive** number of tokens. Each skill invocation typically takes **over 20 minutes** to complete.

If you are on a **Claude Pro subscription**, expect to run only **1-2 skill invocations per 5-hour usage window** depending on the framework size. Plan your sessions carefully and use `/clear` between invocations to free context.

## Installation

**Step 1 -- Add the marketplace:**                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                                  
```                                                     
/plugin marketplace add nlinhvu/learn-java-frameworks-marketplace
```

**Step 2 -- Install plugins:**                                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                       
```                                                                                                                                                                                                                                                                                                        
/plugin install api-learning@learn-java-frameworks-marketplace
/plugin install core-learning@learn-java-frameworks-marketplace
/plugin install enterprise-learning@learn-java-frameworks-marketplace
```                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                     
Or browse and install interactively via `/plugin` → **Discover** tab.

## Quick Start

### Outside-In (API-first)

```
# Analyze the framework's API surface
/api-blueprint /path/to/framework-repo

# Clear context to avoid context pollution
/clear

# Implement the first feature from the generated outline
/api-builder feature-1

# Clear context to avoid context pollution
/clear

# Implement the second feature from the generated outline
/api-builder feature-2
```

### Inside-Out (Core-first)

```
# Analyze the framework's internal core
/core-blueprint /path/to/framework-repo

# Clear context to avoid context pollution
/clear

# Implement the first feature from the generated outline
/core-builder feature-1

# Clear context to avoid context pollution
/clear

# Implement the second feature from the generated outline
/core-builder feature-2
```

### Enterprise Re-implementation (Any Language → Java)

```
# Analyze any source project (Go, Python, Rust, Node.js, etc.)
/enterprise-blueprint /path/to/source-project

# Clear context to avoid context pollution
/clear

# Implement features one at a time
/enterprise-builder "User Authentication"

# Clear context to avoid context pollution
/clear

/enterprise-builder "next feature"
```

## Prerequisites

- Java 17+ (Java 21+ recommended for enterprise-learning)
- Maven or Gradle
- A locally cloned source project repository (any language — Python, Go, Rust, Node.js, C#, Ruby, Java, etc.)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI

## Plugins

### API Learning -- Outside-In Approach

Learn by starting from the **public API surface** of any source project and tracing inward through implementation layers. You build vertical slices: pick an API capability (e.g., "Handle HTTP GET Requests"), understand how it's implemented in the source language, then reimplement it as simplified Java with tests and a tutorial.

**Skills:**

| Skill | Command | What It Does |
|-------|---------|--------------|
| `api-blueprint` | `/api-blueprint` | Analyzes a framework repo and generates an API-first learning outline (`api-outline.md`) with dependency graph, ASCII call-chain diagrams, and a buildable project skeleton |
| `api-builder` | `/api-builder` | Implements a single API capability as working Java code + unit/integration tests + tutorial chapter |

**Agents:**

| Agent | Role |
|-------|------|
| `api-code-explorer` | Traces API surfaces, call chains, and implementation layers in the real framework |
| `api-code-architect` | Designs vertical slice architectures and simplification strategies |
| `api-code-reviewer` | Validates API contracts, test quality, and tutorial accuracy |

### Core Learning -- Inside-Out Approach

Learn by starting from the **internal core** of any source project and building outward. You start from the project's "heart" (e.g., the IoC container in Spring, the Router in Gin) and progressively add layers until you reach the public API.

**Skills:**

| Skill | Command | What It Does |
|-------|---------|--------------|
| `core-blueprint` | `/core-blueprint` | Analyzes a framework repo and generates an inside-out learning outline (`core-outline.md`) with dependency graph, ASCII architecture diagrams, and a buildable project skeleton |
| `core-builder` | `/core-builder` | Implements a single feature/component as working Java code + unit/integration tests + tutorial chapter |

**Agents:**

| Agent | Role |
|-------|------|
| `core-code-explorer` | Traces internal hierarchies, integration points, and design patterns |
| `core-code-architect` | Designs simplified component architectures and integration strategies |
| `core-code-reviewer` | Validates correctness, test quality, and tutorial accuracy |

### Enterprise Learning -- Any Language → Enterprise Java

Re-implement any source project (Go, Python, Rust, Node.js, C#, Ruby, Java/Quarkus/Micronaut, etc.) into enterprise-grade Java. Every technology choice, architecture decision, and design trade-off is documented with rich Mermaid visualizations and Insight blocks explaining the **why** behind each choice.

**Skills:**

| Skill | Command | What It Does |
|-------|---------|--------------|
| `enterprise-blueprint` | `/enterprise-blueprint` | Analyzes a source project, classifies it (Application/Library/Framework), produces an outline with technology mapping, deviation report, and project skeleton |
| `enterprise-builder` | `/enterprise-builder` | Implements a single feature as working Java code + tests + 10-section tutorial chapter with Mermaid diagrams and Insight blocks |

**Agents:**

| Agent | Role |
|-------|------|
| `enterprise-code-explorer` | Multi-language source analysis, project type classification, technology identification |
| `enterprise-code-architect` | Enterprise Java architecture design, technology mapping, module structure |
| `enterprise-code-reviewer` | Validates correct stack, tutorial completeness, visualization quality, insight depth |

## How It Works

All three plugins follow a two-step workflow:

**Step 1 -- Blueprint.** Point the blueprint skill at a locally cloned source project (any language). It analyzes the source code using specialized explorer agents, produces an architecture overview, and generates a prioritized learning outline with a dependency graph.

**Step 2 -- Build.** Pick a feature from the outline and run the builder skill. It designs a simplified implementation using architect agents, writes Java source code and tests that compile and pass, generates a tutorial chapter, and validates everything with reviewer agents.

Each feature you build becomes a standalone, tested module in your simplified framework. The tutorials form a progressive guide you can follow from start to finish.

## Skills vs Agents

This marketplace uses two types of Claude Code plugin components:

- **Skills** are model-invoked capabilities defined in `SKILL.md` files with YAML frontmatter. They are triggered contextually based on user intent and orchestrate the full workflow (analysis, code generation, testing, documentation).

- **Agents** are specialized Claude instances defined in markdown files. Each agent has a focused mission (exploration, architecture, review) and is dispatched by skills as subprocesses to handle specific analysis or validation tasks.

## Repository Structure

```
.claude-plugin/
  marketplace.json              # Marketplace manifest listing all plugins
plugins/
  api-learning/                 # Outside-in learning plugin
    .claude-plugin/
      plugin.json               # Plugin manifest
    skills/
      api-blueprint/            # Outline generation skill
        SKILL.md
        references/             # Templates and checklists
      api-builder/              # Feature implementation skill
        SKILL.md
        references/             # Templates and techniques
    agents/
      api-code-explorer.md      # API surface analysis agent
      api-code-architect.md     # Vertical slice design agent
      api-code-reviewer.md      # Quality validation agent
    shared/                     # Cross-cutting standards
      diagram-standards.md      # Mermaid color palette and diagram rules
      insight-format.md         # Insight block template and rules
      quality-checklist.md      # Reviewer rubric and review loop protocol
      technology-defaults.md    # Default Java stack and conventions
    README.md
    LICENSE
  core-learning/                # Inside-out learning plugin
    .claude-plugin/
      plugin.json               # Plugin manifest
    skills/
      core-blueprint/           # Outline generation skill
        SKILL.md
        references/             # Templates and checklists
      core-builder/             # Feature implementation skill
        SKILL.md
        references/             # Templates and techniques
    agents/
      core-code-explorer.md     # Core analysis agent
      core-code-architect.md    # Component design agent
      core-code-reviewer.md     # Quality validation agent
    shared/                     # Cross-cutting standards
      diagram-standards.md      # Mermaid color palette and diagram rules
      insight-format.md         # Insight block template and rules
      quality-checklist.md      # Reviewer rubric and review loop protocol
      technology-defaults.md    # Default Java stack and conventions
    README.md
    LICENSE
  enterprise-learning/          # Any-language-to-Java learning plugin
    .claude-plugin/
      plugin.json               # Plugin manifest
    skills/
      enterprise-blueprint/     # Source project analysis skill
        SKILL.md
        references/             # Templates and checklists
      enterprise-builder/       # Feature implementation skill
        SKILL.md
        references/             # Templates and techniques
    agents/
      enterprise-code-explorer.md   # Multi-language source analysis agent
      enterprise-code-architect.md  # Enterprise architecture design agent
      enterprise-code-reviewer.md   # Quality validation agent
    shared/                     # Cross-cutting standards
      diagram-standards.md      # Mermaid color palette and diagram rules
      insight-format.md         # Insight block template and rules
      quality-checklist.md      # Reviewer rubric and review loop protocol
      technology-defaults.md    # Default Java stack per project type
    README.md
    LICENSE
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on submitting issues, pull requests, and new plugins.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release history.

## License

This project is licensed under the [Apache License 2.0](LICENSE).

```
Copyright 2026 nlinhvu

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
