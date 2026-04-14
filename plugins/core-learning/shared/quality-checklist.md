# Quality Review Checklist

## Reviewer Rubric

The `core-code-reviewer` agent evaluates every skill output against this rubric:

| Criterion | Check | Severity |
| --- | --- | --- |
| Completeness | All required sections present per skill spec? All 10 tutorial sections present? | Critical |
| Accuracy | Code compiles? Tests pass? Mermaid diagrams match actual architecture? | Critical |
| Integration Point | Feature starts at the correct integration point? Seam identified correctly? | Critical |
| Progressive Build | Later features reuse and extend earlier internals? No duplication? | Critical |
| Markdown table format | All tables have correct header / `\| --- \|` separator / data rows with matching column counts and leading+trailing pipes? | Critical |
| Insight quality | All ★ Insight blocks have Why + Trade-off + Recommend at minimum? Insights in both implementation and N.6? | High |
| Mermaid validity | All diagrams syntactically valid for GitHub rendering? Color palette applied? `<!-- diagram: slug -->` markers present? | High |
| Tutorial structure | All 10 sections (N.1 through N.10) present in every chapter? | High |
| Test quality | Unit tests cover core behavior? Integration tests for cross-feature interactions? | High |
| Simplification correctness | Simplified code captures essential pattern? No over-simplification? | High |
| Cross-references | Links between tutorial chapters? Connection to real framework with commit hash? | Medium |
| Emoji markers | 🔑/⚠️/🚫/★/🔄/🔗 used correctly per diagram-standards? | Medium |
| Diagram markers | `<!-- diagram: slug -->` comment above each Mermaid block? | Medium |
| Impact ordering | Insights ordered most-impactful first? | Low |

## Confidence Scoring (0-100)

The reviewer assigns a confidence score to each finding:

| Score | Meaning | Example |
| --- | --- | --- |
| 0 | Not an issue | — |
| 25 | Minor style | Slightly inconsistent emoji usage |
| 50 | Moderate | Tutorial section present but shallow |
| 75 | Significant | Missing ★ Insight on a key design decision |
| 90 | Critical | Integration point wrong — feature plugs into wrong seam |
| 100 | Critical | Failing tests, broken compilation, missing integration point |

**Only report issues with confidence >= 80.** Group as:
- **Critical (>= 90)**: Must fix before proceeding
- **Important (80-89)**: Should fix to improve educational quality

## Review Loop Protocol

1. Reviewer evaluates output against rubric above
2. Produces structured feedback per finding: `{ criterion, severity, confidence, finding, fix_suggestion }`
3. **Critical issues found (confidence >= 90)** → skill applies fixes → re-submit to reviewer (next pass)
4. **Only Important issues (80-89)** → skill applies fixes → write final output (no re-review needed)
5. **No Critical/High issues remain** → stop (success)
6. **5 passes reached with unresolved issues** → write output with inline `<!-- REVIEWER: [issue] -->` markers
7. **Only subjective/style issues remain** → stop (success)

## Stop Criteria

Stop the review loop when ANY of these conditions are met:
- No Critical/High issues remain
- Max 5 passes reached
- Only subjective/style issues remain

Reviewer appends quality summary to end of document:

```markdown
---
## Quality Review Summary
- **Review passes:** N/5
- **Status:** All critical/high issues resolved | N unresolved issues remain
- **Remaining notes:**
  - (Severity) Description of remaining issue
```

## Markdown Table Format Rules

Tables MUST follow this exact format to render correctly:

```markdown
| Column A | Column B | Column C |
| --- | --- | --- |
| data 1 | data 2 | data 3 |
```

**Rules:**
- Header row: column names separated by `|`, with leading and trailing `|`
- Separator row: `| --- |` pattern, one `---` per column, with leading and trailing `|`
- Data rows: matching column count, with leading and trailing `|`
- NO missing separators, NO mismatched column counts, NO omitted pipes
