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

5. **Concrete**: Be specific — never "consider using a different approach". Instead: "The Router uses a `Map<String, Handler>` for O(1) exact-match lookups because the simplified version doesn't need regex pattern matching — the real framework uses a trie for path templates, but exact-match demonstrates the dispatch pattern with minimal complexity."
6. **Source references**: Use `→ src/path/File.java` format for simplified code, `→ real/path/File.java:line` for real framework references
7. **Placement**: Insight blocks appear in TWO places:
   - **N.3 (Implementation)**: At each major design decision point during the call chain build
   - **N.6 (Why This Works)**: 1-3 comprehensive insights per chapter

## Insight Categories

| Category | When to Use | Example Topic |
| --- | --- | --- |
| 🏗️ API Design Decision | Why the API is shaped this way | "Why fluent builder over constructor parameters" |
| 🧩 Layer Simplification | What was simplified at a depth layer | "Why hardcoded dispatch instead of full trie" |
| 🔑 Design Pattern | Pattern selection rationale | "Why Strategy pattern for handler resolution" |
| 🔄 Call Chain Decision | Why the call chain is structured this way | "Why dispatch before processing in the pipeline" |
| ⚠️ Simplification Warning | What the real framework handles that we skip | "Real framework handles async — we defer to ch07" |
