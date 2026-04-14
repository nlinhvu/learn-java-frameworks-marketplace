# Mermaid Diagram Standards

## Color Palette

Apply these `style` directives consistently across ALL Mermaid diagrams:

| Color | Hex | Use For |
| --- | --- | --- |
| Teal | `fill:#4ecdc4,color:#fff` | Core / heart components |
| Blue | `fill:#45b7d1,color:#fff` | Internal logic / processing |
| Purple | `fill:#bb8fce,color:#fff` | Extension points / SPI |
| Orange | `fill:#f39c12,color:#fff` | Configuration / wiring |
| Red | `fill:#ff6b6b,color:#fff` | Error / critical paths |
| Green | `fill:#27ae60,color:#fff` | Success / output / implemented features |
| Yellow | `fill:#f7dc6f,color:#333` | External systems / application code |

## Diagram Rules

1. **Title comment**: Place a descriptive HTML comment above EVERY Mermaid block: `<!-- diagram: slug_name -->`
2. **Subgraphs**: Use `subgraph` for logical grouping with descriptive labels
3. **Arrow labels**: Label EVERY arrow with the data type, protocol, or relationship
4. **Color-code**: Apply `style` directives to nodes matching the palette above
5. **GitHub compatibility**: Only use Mermaid features supported by GitHub's renderer
6. **Diagram types** to use:
   - `flowchart TD/LR` — architecture, component relationships, dependency graphs, integration points
   - `sequenceDiagram` — execution flows, runtime interactions, lifecycle events
   - `classDiagram` — class hierarchies, interface relationships
   - `stateDiagram-v2` — lifecycle flows, state transitions
   - `gantt` — implementation order / build sequence timelines

## Diagram Categories by Purpose

### Architecture Diagrams
- **Component Overview**: High-level component relationships (flowchart TD with subgraphs)
- **Integration Point Map**: Where each feature plugs into the core (flowchart showing seams)
- **Layer Diagram**: How subsystems stack and connect

### Execution Flow Diagrams
- **Happy Path Traces**: Sequence diagrams for primary operations
- **Lifecycle Flows**: State diagrams for object lifecycle (init, start, stop, destroy)

### Learning Path Diagrams
- **Feature Dependency Graph**: Mermaid flowchart showing feature ordering and dependencies
- **Learning Roadmap**: Annotated dependency graph with learning objectives
- **Progressive Enhancement**: How components grow across chapters

### Class Hierarchy Diagrams
- **Core Interfaces**: Key interface hierarchies and their implementations
- **Extension Model**: SPI and extension point class diagrams

## Emoji Markers for Visual Hierarchy

Use consistently across ALL documents:

| Emoji | Meaning |
| --- | --- |
| 🔑 | Essential concept — must understand before proceeding |
| ⚠️ | Pitfall — common source of bugs or confusion |
| 🚫 | Constraint — MUST NOT be violated |
| ★ | Insight block marker |
| 📊 | Data / metrics |
| 🔄 | Flow / process / execution path |
| 🏗️ | Architecture / structure |
| 🧩 | Component / subsystem |
| 🔗 | Integration point / dependency |

## When to Use Mermaid vs ASCII

- **Mermaid**: Architecture overviews, learning paths, dependency graphs, integration point maps, class hierarchies, lifecycle state machines
- **ASCII**: Inline execution flow traces within tutorial text where a quick visual aid suffices (supplement, not replace, Mermaid)
