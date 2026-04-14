# Mermaid Diagram Standards

## Color Palette

Apply these `style` directives consistently across ALL Mermaid diagrams:

| Color | Hex | Use For |
| --- | --- | --- |
| Teal | `fill:#4ecdc4,color:#fff` | API surface / public classes |
| Blue | `fill:#45b7d1,color:#fff` | Internal logic / dispatch & processing layers |
| Purple | `fill:#bb8fce,color:#fff` | Infrastructure / deep internals |
| Orange | `fill:#f39c12,color:#fff` | Configuration / extension points |
| Red | `fill:#ff6b6b,color:#fff` | Error / critical paths |
| Green | `fill:#27ae60,color:#fff` | Success / output / implemented features |
| Yellow | `fill:#f7dc6f,color:#333` | External systems / client code |

## Diagram Rules

1. **Title comment**: Place a descriptive HTML comment above EVERY Mermaid block: `<!-- diagram: slug_name -->`
2. **Subgraphs**: Use `subgraph` for logical grouping with descriptive labels
3. **Arrow labels**: Label EVERY arrow with the data type, protocol, or relationship
4. **Color-code**: Apply `style` directives to nodes matching the palette above
5. **GitHub compatibility**: Only use Mermaid features supported by GitHub's renderer
6. **Diagram types** to use:
   - `flowchart TD/LR` — architecture, vertical slices, API surface maps, dependency graphs
   - `sequenceDiagram` — call chain traces, runtime interactions
   - `classDiagram` — class hierarchies, API contracts
   - `gantt` — implementation order / build sequence timelines

## Diagram Categories by Purpose

### API Surface Diagrams
- **API Surface Map**: Which public classes/interfaces belong to which feature (flowchart TD with subgraphs)
- **Vertical Slice**: Layers from API → Dispatch → Processing → Infrastructure per feature

### Call Chain Diagrams
- **Tier 1 API Traces**: Sequence diagrams showing execution path from public method inward
- **Layer Interaction**: How API, dispatch, processing, and infrastructure layers connect

### Learning Path Diagrams
- **Feature Dependency Graph**: Mermaid flowchart showing feature ordering and dependencies
- **Learning Roadmap**: Annotated dependency graph with learning objectives

### Architecture Diagrams
- **Component Overview**: High-level component relationships (flowchart TD with subgraphs)
- **Package Structure**: `api/` vs `internal/` organization

## Emoji Markers for Visual Hierarchy

Use consistently across ALL documents:

| Emoji | Meaning |
| --- | --- |
| 🔑 | Essential concept — must understand before proceeding |
| ⚠️ | Pitfall — common source of bugs or confusion |
| 🚫 | Constraint — MUST NOT be violated |
| ★ | Insight block marker |
| 📊 | Data / metrics |
| 🔄 | Flow / process / call chain |
| 🏗️ | Architecture / structure |
| 🧩 | Component / layer |
| 🔗 | Dependency / connection |

## When to Use Mermaid vs ASCII

- **Mermaid**: Architecture overviews, learning paths, feature dependency graphs, vertical slice diagrams, component relationships
- **ASCII**: Inline call chain traces within tutorial text where a quick visual aid suffices (supplement, not replace, Mermaid)
