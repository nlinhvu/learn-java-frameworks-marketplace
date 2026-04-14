# Insight Block Format

## Standard Format

Every architectural decision, design choice, simplification rationale, or non-obvious trade-off MUST get an insight block:

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

5. **Concrete**: Be specific — never "consider using a different approach". Instead: "The SimpleBeanContainer uses a `Map<Class<?>, Supplier<?>>` for lazy instantiation because the real framework's BeanDefinition metadata is a premature abstraction at this stage — Feature 3 will introduce BeanDefinition when we need named beans and scopes."
6. **Source references**: Use `→ src/path/File.java` format for simplified code, `→ real/path/File.java:line` for real framework references
7. **Placement**: Insight blocks appear in TWO places:
   - **N.1-N.3 (Implementation)**: At each major design decision point during the build
   - **N.6 (Why This Works)**: 1-3 comprehensive insights per chapter

## Insight Categories

| Category | When to Use | Example Topic |
| --- | --- | --- |
| 🔗 Integration Point Decision | Why this seam was chosen for the feature | "Why dispatch pipeline over separate handler chain" |
| 🏗️ Architecture Decision | Structural design choice | "Why Registry pattern for bean storage" |
| 🧩 Simplification Rationale | What was simplified and why it's safe | "Why Map replaces BeanDefinitionRegistry" |
| 🔑 Design Pattern | Pattern selection rationale | "Why Strategy for dependency resolution" |
| ⚠️ Simplification Warning | What the real framework handles that we skip | "Real framework handles circular deps — we defer to ch06" |
| 🔄 Progressive Enhancement | Why this enhancement was triggered by the new feature | "Why bean storage evolved from Map<Class,Object> to named beans" |
