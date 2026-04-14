# Tutorial Template (Enterprise Java — 10-Section Structure)

Each chapter is `enterprise-docs/chNN_<feature_name>.md`. Replace `N` with the chapter number
and `{{placeholders}}` with actual content.

**IMPORTANT**: Every chapter MUST have all 10 sections. If a section doesn't apply to this
feature, include it with: "N/A — this feature does not involve [topic]" and a brief explanation.

---

```markdown
# Chapter N: {{Feature Name}}

## N.1 Module Contract / API Contract

**Build Challenge:**

| Current State         | Limitation                     | Objective                                  |
|-----------------------|--------------------------------|--------------------------------------------|
| {{what exists now}}   | {{what can't be done yet}}     | {{what will be possible after this chapter}}|

{{FOR APPLICATIONS:}}
After this chapter, the application will:
- {{endpoint or capability 1}}
- {{endpoint or capability 2}}

**REST API** (if applicable):
```
{{METHOD}} /{{path}}
Request:  {{request body or parameters}}
Response: {{response body}}
```

**Service Contract**:
```java
// {{ServiceName}}.java — behavioral contract
{{service interface or class with method signatures}}
```

{{FOR LIBRARIES:}}
After this chapter, consumers can:
```java
{{3-8 lines showing the API in use}}
```

**Public API**:
```java
// {{ClassName}}.java — what consumers import
{{public class/interface with method signatures}}
```

{{FOR FRAMEWORKS:}}
After this chapter, users can:
```java
{{3-8 lines showing the programming model in use}}
```

**Extension Interface**:
```java
// {{InterfaceName}}.java — what users implement
{{interface or abstract class}}
```

## N.2 Client Tests — Proving the Contract

Before implementing anything, these tests define what "correct" means:

{{FOR APPLICATIONS (Testcontainers):}}
```java
// {{FeatureName}}Test.java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Testcontainers
class {{FeatureName}}Test {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:16");

    {{2-4 tests covering the feature's behavioral contract}}
}
```

{{FOR LIBRARIES/FRAMEWORKS (plain JUnit):}}
```java
// {{FeatureName}}Test.java
class {{FeatureName}}Test {

    {{2-4 tests covering the API's behavioral contract}}
}
```

These tests won't compile yet — we haven't implemented the feature. That's intentional.
Each test represents a promise the feature makes to its clients.

{{Explain each test briefly — what contract does it verify?}}

## N.3 Implementation

Now we build each component to make those tests pass.

### N.3.1 {{Component Name}} — {{purpose}}

{{FOR APPLICATIONS — entities:}}
```java
// {{EntityName}}.java [NEW]
@Entity
@Table(name = "{{table_name}}")
public class {{EntityName}} {
    {{entity fields with jakarta.persistence annotations}}
}
```

> ★ **Insight** -------------------------------------------
> - **Why {{design decision}}?** {{Rationale}}
> - **Trade-off:** {{What was sacrificed}}
> - **Recommend:** {{When to use this approach}}
> -----------------------------------------------------------

{{FOR LIBRARIES — implementation:}}
```java
// {{ClassName}}.java [NEW]
{{implementation code}}
```

{{Explain what this component does and how it fits in the feature}}

### N.3.2 {{Next Component}} — {{purpose}}

```java
// {{ClassName}}.java [NEW] or [MODIFIED]
{{implementation code}}
```

{{For NEW files: show complete code}}
{{For MODIFIED files: state the file, describe what changed, show the diff}}

> ★ **Insight** -------------------------------------------
> - **Why {{technology or pattern choice}}?** {{Rationale with alternatives}}
> - **Trade-off:** {{Downsides, when this might be wrong}}
> - **Recommend:** {{Guidance for real projects}}
> -----------------------------------------------------------

{{Continue for each component...}}

**Run the client tests now — they should all pass.**

## N.4 Internal Tests

With the feature working, add targeted tests for internal components:

{{FOR APPLICATIONS:}}
```java
// {{ServiceName}}Test.java [NEW]
@SpringBootTest
@Testcontainers
class {{ServiceName}}Test {
    {{unit tests for service logic, edge cases, error handling}}
}
```

{{FOR LIBRARIES/FRAMEWORKS:}}
```java
// {{InternalClass}}Test.java [NEW]
class {{InternalClass}}Test {
    {{unit tests for internal components}}
}
```

## N.5 Try It Yourself

<details>
<summary>Challenge 1: {{description of what to extend or modify}}</summary>

**Hint**: {{which component to modify and what to add}}

**Solution**:
```java
{{solution code}}
```
</details>

<details>
<summary>Challenge 2: {{description}}</summary>

**Hint**: {{hint}}

**Solution**:
```java
{{solution code}}
```
</details>

## N.6 Why This Works

> ★ **Insight** -------------------------------------------
> - **Why {{core design decision for this feature}}?** {{Deep rationale with alternatives considered}}
> - **Trade-off:** {{What was sacrificed, downsides, when this choice might be wrong}}
> - **Recommend:** {{For the learner: when to use this approach in real enterprise projects}}
> -----------------------------------------------------------

{{1-3 insight blocks. Focus on WHY the design works, not WHAT the code does.
Topics should cover the most impactful design decisions in this feature:
- Technology substitution rationale
- Pattern choice justification
- Architecture decision reasoning
- Enterprise Java convention explanations}}

## N.7 What We Enhanced

{{FOR CHAPTER 1:}}

**Foundation established.** This chapter created the core infrastructure:

| Component | What Was Created | Purpose |
|-----------|-----------------|---------|
| {{component}} | {{what was built}} | {{why it exists}} |

{{FOR CHAPTER 2+: This table is MANDATORY.}}

| Component | Before (Ch {{N-1}}) | Current (Ch {{N}}) | Source Project |
|-----------|--------------------|--------------------|----------------|
| {{component}} | {{previous state}} | {{current state}}  | {{what source does}} |

{{At least one row per chapter. Show how the enterprise code grows to handle new capabilities.}}

## N.8 Technology Mapping

Source technologies used in this feature and their Java equivalents:

| Source Technology | Java Equivalent | Role in This Feature |
|-------------------|-----------------|---------------------|
| {{source tech}}   | {{java tech}}   | {{what it does here}} |

{{For each mapping that involves a non-trivial substitution:}}

> ★ **Insight** -------------------------------------------
> - **Why {{java tech}} replaces {{source tech}}?** {{Specific rationale for this feature's context}}
> - **Trade-off:** {{Capabilities different or lost}}
> - **Recommend:** {{When each approach is better}}
> -----------------------------------------------------------

{{For Java-to-Java: contrast framework approaches instead of technology substitution}}

See [technology-mapping.md](../technology-mapping.md) for the complete mapping.

## N.9 Architecture Visualization

<!-- diagram: ch{{NN}}_{{feature_slug}}_architecture -->
```mermaid
flowchart TD
    subgraph "{{Feature Name}}"
        {{component diagram for this feature}}
    end
    style {{nodes}} fill:#45b7d1,color:#fff
```

{{Additional diagrams as needed:}}

{{FOR APPLICATIONS with data model:}}
<!-- diagram: ch{{NN}}_{{feature_slug}}_data_model -->
```mermaid
erDiagram
    {{entity relationships for this feature}}
```

{{FOR features with complex flows:}}
<!-- diagram: ch{{NN}}_{{feature_slug}}_flow -->
```mermaid
sequenceDiagram
    {{request flow for this feature}}
```

## N.10 Complete Code

All files created or modified in this chapter. Copy all `[NEW]` files and replace
all `[MODIFIED]` files to get a compiling project with all tests passing.

{{FOR APPLICATIONS: Ensure Docker is running: `docker-compose up -d`}}

### Production Code

#### `src/main/java/{{package}}/{{ClassName}}.java` [NEW]
```java
{{FULL file content — read from actual src/ file, do NOT write from memory}}
```

#### `src/main/java/{{package}}/{{ClassName}}.java` [MODIFIED]
```java
{{FULL file content — read from actual src/ file}}
```

{{Include ALL files created or modified, in dependency order}}

### Test Code

#### `src/test/java/{{package}}/{{ClassName}}Test.java` [NEW]
```java
{{FULL file content — read from actual src/ file}}
```

### Configuration

{{FOR APPLICATIONS:}}
#### `src/main/resources/application.yml` [NEW] or [MODIFIED]
```yaml
{{configuration content}}
```

## Summary

| Concept       | What You Built                              |
|---------------|---------------------------------------------|
| {{concept 1}} | {{what it does in the enterprise version}}  |
| {{concept 2}} | {{what it does}}                            |

**Next chapter**: {{Feature N+1 name}} — {{one sentence about what capability comes next
and what new technology mappings or design decisions it will introduce}}
```

---

## Template Rules

1. **All 10 sections**: Every chapter has N.1 through N.10. Mark inapplicable sections "N/A — this feature does not involve [topic]" rather than omitting.
2. **Module/API Contract first**: Show what clients/consumers get before any implementation.
3. **Client tests before implementation**: Tests define the contract, implementation fulfills it.
4. **★ Insight blocks at decision points**: In N.3 (implementation) AND N.6 (Why This Works), with minimum Why + Trade-off + Recommend.
5. **Code before explanation**: Show the code, then explain what it does and why.
6. **File annotations**: Every code block header states the file path and `[NEW]` or `[MODIFIED]`.
7. **Modified files**: State the file, describe the change, show the specific change.
8. **Complete Code**: Generated by reading actual `src/` files. Must match reality exactly.
9. **Technology Mapping (N.8)**: Maps source tech → Java for THIS feature, with ★ Insights.
10. **Architecture Visualization (N.9)**: At least one Mermaid diagram per chapter, using standardized color palette and `<!-- diagram: slug -->` markers.
11. **What We Enhanced (N.7)**: SKIP content for ch01 (show foundation table). MANDATORY enhancement table for ch02+.
12. **Copy-paste guarantee**: If reader copies `[NEW]` files and replaces `[MODIFIED]` files, the project compiles and all tests pass.
13. **Mermaid diagrams**: Use standardized 7-color palette, `<!-- diagram: slug -->` comment markers, labeled arrows, subgraphs for logical grouping.
14. **Emoji markers**: 🔑 Essential, ⚠️ Pitfall, 🚫 Constraint, ★ Insight, 🔀 Technology substitution — used consistently.
