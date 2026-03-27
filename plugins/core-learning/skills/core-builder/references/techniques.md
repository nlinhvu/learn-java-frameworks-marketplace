# Techniques Catalog

Proven patterns for creating effective code-first tutorials. Each technique includes what it is, why it matters, and a concrete example.

---

## 1. Integration Point First

**What**: Show the code where two subsystems connect BEFORE building the supporting components. The integration point is the heart of each feature — it reveals the direction and purpose of everything else.

**Why**: When learners see `converter.convert(value, targetType)` inside a resolver first, they immediately know WHY they're building a conversion service. When they see `chain.applyPreHandle()` in a dispatch method first, they understand the interceptor lifecycle before implementing a single interceptor. Starting with the integration point provides a mental roadmap — the learner knows where they're going before they start walking.

**How to identify the integration point:**
- Where does an existing class gain a new field or dependency?
- Where does a new method call appear in an existing pipeline?
- Where does a configuration step connect two independent subsystems?

**The 5 common locations** (observed across progressive framework rebuilds):
1. **Central dispatch pipeline** — features adding a step to the main processing loop (interceptors, exception resolvers, view rendering, session management)
2. **Adapter/invoker pipeline** — features changing how handlers are invoked or results written (argument resolution, return value handling, data binding)
3. **Resolver/converter internals** — features adding capabilities to existing resolvers (type conversion, validation)
4. **Registry/mapping** — features changing how inputs map to handlers (pattern matching, meta-annotations, filtering)
5. **Server/container infrastructure** — features connecting to the embedded server or container bootstrap (servlet mapping, component scanning, protocol support)

**Anti-pattern (bottom-up — wrong order):**
```markdown
## 5.1 The @Param Annotation
## 5.2 The ParameterMetadata Wrapper
## 5.3 The Resolver Interface
## 5.4 The ParamResolver Implementation
## 5.5 Wire into the Adapter  ← integration point buried last
```

**Correct (integration-point-first):**
```markdown
## 5.1 The Integration Point: Adapter gains resolveArguments()
[Show the modified handle() method — resolveArguments() before doInvoke()]
[Direction: "We need resolvers that extract values from requests."]
## 5.2 @Param annotation + ParameterMetadata
## 5.3 Resolver interface + composite + ParamResolver
## 5.4 Wire it all together
```

**Another example (interceptors feature):**
```markdown
## 10.1 The Integration Point: dispatch() wraps handler execution
[Show dispatch() calling chain.applyPreHandle() / applyPostHandle() / afterCompletion()]
[Direction: "We need an Interceptor interface and an ExecutionChain that manages the lifecycle."]
## 10.2 The Interceptor Interface
## 10.3 The ExecutionChain
```

**Exception — foundation chapters:** Chapter 1 (e.g., "Bean Container") has no prior system to integrate with. In this case, section N.1 shows the core data structure and states what future features will connect to it: "This container will be the glue that all future features retrieve their collaborators from."

---

## 2. Code-First Presentation

**What**: Present code BEFORE explanatory text. Always.

**Why**: Code is the truth. Prose is commentary. The reader should form their own understanding from the code, then have it confirmed and deepened by the explanation.

**Anti-pattern (explanation-first — avoid):**
```markdown
## 5.1 Understanding the Factory Pattern
The Factory Pattern is a creational design pattern that provides an interface
for creating objects without specifying their exact classes...
[3 paragraphs]
Now let's see the code:
```

**Correct (code-first):**
```markdown
## 5.1 Extract the Factory Interface
```java
public interface HttpRequestFactory {
    HttpRequest createRequest(URI uri, String method) throws IOException;
}
```‎
```

**Rules:**
- No explanatory paragraphs before code blocks in sections N.1–N.4
- Section headers describe what to BUILD, not what to learn ("Extract the Factory Interface" not "Understanding Factories")
- Minimal inline comments — just enough to orient, not explain the pattern

---

## 3. Build Challenge Framing

**What**: Table showing Current State | Limitation | Objective. ONE row only.

**Why**: Frames the chapter as solving a concrete limitation. The reader knows exactly what they can't do yet and what they'll be able to do after this chapter.

**Example:**
```markdown
## Build Challenge
| Current State | Limitation | Objective |
|---------------|------------|-----------|
| `body()` directly creates `HttpURLConnection` to make HTTP calls | Cannot swap HTTP libraries; cannot unit-test without a real network | Extract HTTP transport behind a factory interface so implementations are pluggable |
```

**Guidelines:**
- ONE row only — if you need multiple rows, the chapter covers too much
- "Limitation" must be specific and testable, not vague
- "Objective" uses a verb phrase (BUILD something)

---

## 4. Try It Yourself Challenges

**What**: Collapsible `<details>` sections with challenges that extend the feature or internals.

**Why**: Active learning beats passive reading. Challenges should follow naturally from the chapter's feature. Since the actual code exists in `src/`, the user has options:
- Try implementing from the challenge description
- Peek at the solution in the `<details>` tag
- Look at the actual source files directly
- Delete the source files and rebuild from the tutorial

**Example:**
```markdown
## 3.3 Try It Yourself

<details>
<summary>Challenge: Create a type-safe bean resolver that finds beans by their interface type</summary>

Think about: How do you match a registration of `UserServiceImpl.class` when someone asks for `UserService.class`?

```java
public <T> T resolve(Class<T> type) {
    return registrations.entrySet().stream()
        .filter(e -> type.isAssignableFrom(e.getKey()))
        .map(e -> type.cast(e.getValue().get()))
        .findFirst()
        .orElseThrow(() -> new NoSuchBeanException(type));
}
```‎

</details>
```

---

## 5. Test-as-Documentation

**What**: Tests serve as living documentation — their names describe the feature's behavior.

**Why**: Someone reading only test names should understand what the feature does. Test naming IS the specification.

**Example:**
```java
class BeanContainerTest {

    @Test
    void shouldRegisterBean_WhenGivenClassAndSupplier() { ... }

    @Test
    void shouldResolveBean_WhenRegisteredByConcreteType() { ... }

    @Test
    void shouldResolveBean_WhenRegisteredByInterfaceType() { ... }

    @Test
    void shouldReturnSameInstance_WhenBeanIsSingleton() { ... }

    @Test
    void shouldReturnDifferentInstances_WhenBeanIsPrototype() { ... }

    @Test
    void shouldThrowNoSuchBeanException_WhenBeanNotRegistered() { ... }
}
```

**Test structure:** Use Arrange-Act-Assert (AAA) pattern consistently:
```java
@Test
void shouldResolveBean_WhenRegisteredByType() {
    // Arrange
    var container = new SimpleBeanContainer();
    container.register(UserService.class, SimpleUserService::new);

    // Act
    UserService service = container.resolve(UserService.class);

    // Assert
    assertThat(service).isInstanceOf(SimpleUserService.class);
}
```

---

## 6. Source Code Mapping Tables

**What**: Three levels of mapping between simplified code and real framework source.

**Why**: Connects the learner's simplified implementation to the real framework, showing where concepts align and where the real code adds complexity.

**Per-chapter mapping (most common):**
```markdown
| Simplified Code | Real Framework Code | File:Line | Key Difference |
|----------------|--------------------|-----------| ---------------|
| `SimpleBeanContainer` | `DefaultListableBeanFactory` | `DefaultListableBeanFactory.java:185` | Real handles circular deps, lazy init, scopes |
| `register()` | `registerBeanDefinition()` | `DefaultListableBeanFactory.java:892` | Real uses BeanDefinition metadata objects |
```

**Cross-chapter evolution (in later chapters):**
```markdown
| Concern | ch01 | ch03 | ch05 | Real Framework |
|---------|------|------|------|---------------|
| Bean storage | `Map<Class, Supplier>` | `Map<String, BeanDefinition>` | same + scope | `ConcurrentHashMap` + `BeanDefinitionRegistry` |
| Resolution | by type only | by name + type | + qualifier | `DependencyDescriptor` chain |
```

**Important:** Line references are verified at generation time against the recorded commit hash.

---

## 7. Enhancement Tracking Tables

**What**: "What We Enhanced" table, mandatory for ch02+, skip ch01.

**Why**: Shows the reader that simplifications are temporary. Each chapter enhances previous internals to support new capabilities. The "Real Framework" column shows what's left — the gap shrinks every chapter.

**Example:**
```markdown
## 5.6 What We Enhanced

| Aspect | Before (ch01) | Current (ch05) | Real Framework |
|--------|--------------|----------------|----------------|
| Bean storage | `Map<Class, Object>` — single instance per type | `Map<String, BeanDefinition>` — named beans with metadata | `ConcurrentHashMap<String, BeanDefinition>` with `BeanDefinitionRegistry` (`DefaultListableBeanFactory.java:185`) |
| Resolution | Direct type lookup | Name-based + type-based with precedence | `DependencyDescriptor` resolution chain with qualifiers |
```

**Rules:**
- At least one row per chapter (except ch01)
- "Before" column references a specific chapter
- "Real Framework" references production code with file:line
- Enhancement must be real — don't claim enhancement if the code didn't change

---

## 8. Before/After Code Comparison

**What**: When modifying code from a previous chapter, show what changed.

**Why**: Explicit diffs prevent confusion about what's new vs. what existed before. The reader can see exactly how the feature evolves across chapters.

**Table format (precise):**
```markdown
| What | Before (ch03) | After (ch05) |
|------|---------------|--------------|
| Bean registration | `container.register(Foo.class, new Foo())` | `container.register("foo", Foo.class, Foo::new)` |
| Resolution | `container.get(Foo.class)` | `container.getBean("foo", Foo.class)` |
```

**Diff format (for file modifications):**
```markdown
**Modifying:** `SimpleBeanContainer.java`

Before:
```java
private final Map<Class<?>, Object> beans = new HashMap<>();
```‎

After:
```java
private final Map<String, BeanDefinition> definitions = new HashMap<>();
private final Map<String, Object> singletons = new HashMap<>();
```‎
```

---

## 9. ASCII Diagrams

**What**: Visual diagrams for execution flows, class hierarchies, and component relationships.

**Why**: A diagram communicates structure faster than prose. Use for any structure with 3+ components.

**Class/interface hierarchy:**
```
BeanFactory                              ← getBean()
├── ListableBeanFactory                  ← getBeanNamesForType()
│   └── ApplicationContext               ← extends both + environment, events
│       └── ConfigurableApplicationContext ← refresh(), close()
└── HierarchicalBeanFactory              ← getParentBeanFactory()
```

**Execution flow:**
```
container.getBean("userService")
    │
    ├──> Check singleton cache
    │    └── Found? → return
    │
    ├──> Get BeanDefinition
    │    └── Not found? → throw NoSuchBeanException
    │
    ├──> Resolve dependencies (recursive getBean)
    │
    ├──> Create instance (constructor injection)
    │
    ├──> Apply BeanPostProcessors
    │
    └──> Cache as singleton → return
```

**Layer diagram:**
```
┌──────────────────────────────────────┐
│          Application Code            │
├──────────────────────────────────────┤
│     ApplicationContext (ch05)        │
├──────────────────────────────────────┤
│     BeanFactory + Container (ch01)   │
├──────────────────────────────────────┤
│     BeanDefinition Registry (ch03)   │
└──────────────────────────────────────┘
```

---

## 10. Pattern Naming

**What**: Name and number each design pattern as introduced. Accumulate across chapters.

**Why**: Explicit pattern naming builds the learner's design vocabulary and creates a reference index for the entire learning path.

```markdown
| # | Pattern | Why It Exists | Introduced |
|---|---------|---------------|------------|
| 1 | **Registry** | Central storage for bean definitions | ch01 |
| 2 | **Factory** | Decouple bean creation from usage | ch02 |
| 3 | **Strategy** | Pluggable dependency resolution | ch04 |
| 4 | **Observer** | Event-driven lifecycle hooks | ch06 |
```

---

## 11. Insight Blocks

**What**: Educational blocks explaining WHY design decisions work.

**Why**: Insights transform "I copied this pattern" into "I understand why this pattern exists and when to use it." Always in the "Why This Works" section, AFTER code.

**Format:**
```markdown
> ★ **Insight** ─────────────────────────────────
> - **Why Factory over direct instantiation?** The Factory Pattern follows the Dependency Inversion Principle — high-level modules don't depend on low-level implementations. This is why Spring's entire DI container works.
> - **When to skip it:** If your component will only ever have one implementation, a factory adds indirection without benefit. Not everything needs to be abstracted.
> - **Real-world parallel:** Spring's `BeanFactory` is literally this pattern. `DefaultListableBeanFactory` is the one implementation that handles 99% of use cases.
> ─────────────────────────────────────────────────
```

**Guidelines:**
- Focus on "why" not "what"
- Include trade-offs: when NOT to use this pattern
- Connect to the real framework: how does the actual codebase use this same idea?
- Keep to 2-3 bullet points per block
