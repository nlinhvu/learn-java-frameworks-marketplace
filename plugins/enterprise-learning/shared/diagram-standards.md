# Mermaid Diagram Standards

## Color Palette

Apply these `style` directives consistently across ALL Mermaid diagrams:

| Color | Hex | Use For |
| --- | --- | --- |
| Teal | `fill:#4ecdc4,color:#fff` | Input / UI layer |
| Blue | `fill:#45b7d1,color:#fff` | Business logic / core domain |
| Purple | `fill:#bb8fce,color:#fff` | Data layer / persistence |
| Orange | `fill:#f39c12,color:#fff` | Infrastructure / operations |
| Red | `fill:#ff6b6b,color:#fff` | Error / critical paths |
| Green | `fill:#27ae60,color:#fff` | Success / output / chosen option |
| Yellow | `fill:#f7dc6f,color:#333` | External systems / third-party |

## Diagram Rules

1. **Title comment**: Place a descriptive HTML comment above EVERY Mermaid block: `<!-- diagram: slug_name -->`
2. **Subgraphs**: Use `subgraph` for logical grouping with descriptive labels
3. **Arrow labels**: Label EVERY arrow with the data type, protocol, or relationship
4. **Color-code**: Apply `style` directives to nodes matching the palette above
5. **GitHub compatibility**: Only use Mermaid features supported by GitHub's renderer
6. **Auto-cluster**: For projects with 50+ modules, group modules by concern into subgraphs rather than showing all individually
7. **Diagram types** to use:
   - `flowchart TD/LR` — architecture, flows, dependencies, technology mapping comparisons
   - `sequenceDiagram` — runtime interactions, API calls, request processing
   - `stateDiagram-v2` — entity state machines, lifecycle flows
   - `erDiagram` — entity relationships, data models
   - `gantt` — implementation order / build sequence timelines
   - `classDiagram` — module structure, class hierarchies

## Diagram Categories by Purpose

### Architecture Diagrams
- **C4 System Context**: High-level system boundaries (flowchart TD with subgraphs)
- **C4 Container**: Services, databases, message queues within the system
- **C4 Component**: Internal components within a container/module

### Technology Mapping Diagrams
- **Source vs. Java comparison**: Side-by-side flowchart showing source technology stack mapped to Java equivalent
- **Framework concept mapping**: For Java-to-Java re-implementations, contrasting framework approaches

### Data Diagrams
- **ER diagrams**: Entity relationships for persistence layer
- **State machines**: Entity lifecycle flows (stateDiagram-v2)

### Flow Diagrams
- **Request flow**: Happy path + error path flowcharts
- **Sequence diagrams**: Multi-component runtime interactions
- **Module dependency graph**: Build order and inter-module dependencies

## Emoji Markers for Visual Hierarchy

Use consistently across ALL documents:

| Emoji | Meaning |
| --- | --- |
| 🔑 | Essential concept — must understand before proceeding |
| ⚠️ | Pitfall — common source of bugs or confusion |
| 🚫 | Constraint — MUST NOT be violated |
| ★ | Insight block marker |
| 📊 | Data / metrics |
| 🔄 | Flow / process |
| 🏗️ | Architecture / structure |
| 🧩 | Module / component |
| 🔗 | Dependency / connection |
| 🔀 | Technology substitution — source tech replaced by Java equivalent |
