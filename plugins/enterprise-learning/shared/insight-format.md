# Insight Block Format

## Standard Format

Every architectural decision, technology substitution, design choice, or non-obvious trade-off MUST get an insight block:

```markdown
> ★ **Insight** -------------------------------------------
> - **Why [topic]?** [Rationale with alternatives considered. What problem this uniquely solves that other approaches don't.]
> - **Trade-off:** [What was sacrificed. Downsides. When this choice might be wrong.]
> - **Recommend:** [For the learner: when to use this approach vs. alternatives in real projects.]
> - **Where:** [→ src/path/File.java — methodName]
> - **When:** [During init? Runtime? Under load? At scale?]
> - **How to verify:** [How the learner confirms their understanding — a test, a log output, a metric]
> -----------------------------------------------------------
```

## Rules

1. **Ordering**: Insight blocks MUST be ordered by impact — most impactful first within each document section
2. **Minimum fields**: Every insight MUST have at least: **Why**, **Trade-off**, **Recommend**
3. **Full fields**: When information is available, include all 6 fields (Why, Trade-off, Recommend, Where, When, How to verify)
4. **Lower-impact insights**: Use collapsible `<details>` blocks:

```markdown
<details>
<summary>★ Insight: [brief topic]</summary>

> ★ **Insight** -------------------------------------------
> - **Why [topic]?** [explanation]
> - **Trade-off:** [details]
> - **Recommend:** [guidance]
> -----------------------------------------------------------

</details>
```

5. **Concrete**: Be specific — never "consider using a different approach". Instead: "PostgreSQL replaces SQLite because enterprise applications need concurrent write access, ACID transactions across connections, and connection pooling — SQLite's single-writer lock becomes a bottleneck above ~10 concurrent users."
6. **Source references**: Use `→ src/path/File.java` format for Java implementations, `→ source/path/file.xx` for source project references
7. **Technology substitution insights** must include:
   - Source technology name and what it does
   - Chosen Java alternative and why
   - Capabilities lost or different
   - Alternatives considered and why rejected

## Insight Categories

| Category | When to Use | Example Topic |
| --- | --- | --- |
| 🔀 Technology Substitution | Source tech → Java equivalent | "Why PostgreSQL replaces SQLite" |
| 🏗️ Architecture Decision | Structural design choice | "Why virtual threads replace goroutines" |
| 🧩 Design Pattern | Pattern selection rationale | "Why Builder pattern for configuration" |
| ⚠️ Deviation Warning | No direct Java equivalent exists | "Java lacks Leiden Algorithm — using Louvain via JGraphT" |
| 🔑 Framework Contrast | Java-to-Java re-implementation | "Why @Component replaces @ApplicationScoped" |
