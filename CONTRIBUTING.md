# Contributing to Learn Java Frameworks Marketplace

Thank you for your interest in contributing! This marketplace helps developers learn Java frameworks by building simplified versions from source code, and we welcome contributions that improve the learning experience.

## How to Contribute

### Reporting Issues

- Use [GitHub Issues](https://github.com/nlinhvu/learn-java-frameworks-marketplace/issues) to report bugs or suggest improvements
- Include which plugin (`api-learning` or `core-learning`) is affected
- Provide the framework you were analyzing and the steps to reproduce

### Submitting Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Test your changes against a real Java framework repository
5. Commit with a clear message (`git commit -m "Add: description of change"`)
6. Push to your fork (`git push origin feature/your-feature`)
7. Open a Pull Request

### What to Contribute

**High-impact contributions:**

- Improving skill prompts for better outline or tutorial generation
- Adding reference templates for new tutorial patterns
- Enhancing agent prompts for deeper framework analysis
- Fixing issues with generated code compilation or test failures
- Adding support for new build tools (e.g., Bazel, sbt)

**New plugins:**

If you want to add an entirely new learning approach:

1. Create a new directory under `plugins/`
2. Follow the existing plugin structure (see `plugins/api-learning/` as a reference)
3. Include a `plugin.json`, `README.md`, `LICENSE`, skills, and agents
4. Register the plugin in `.claude-plugin/marketplace.json`

### Plugin Structure

Each plugin follows this structure:

```
plugins/your-plugin/
  .claude-plugin/
    plugin.json              # Plugin manifest
  skills/
    your-skill/
      SKILL.md               # Skill definition with YAML frontmatter
      references/             # Reference templates and checklists
  agents/
    your-agent.md            # Agent definition
  README.md
  LICENSE
```

### Skill Guidelines

- Skills are defined in `SKILL.md` files with YAML frontmatter (`name`, `version`, `description`)
- Keep skill descriptions specific enough for accurate triggering -- include natural trigger phrases
- Include reference templates in the `references/` subdirectory
- Test skills against multiple Java frameworks (e.g., Spring, Guava, Jackson)

### Agent Guidelines

- Agents are specialized Claude instances defined in markdown files with YAML frontmatter (`name`, `description`, `tools`, `model`, `color`)
- Each agent should have a focused mission (exploration, architecture, review)
- Include `<example>` blocks in the description to illustrate when the agent should be dispatched
- Document what the agent analyzes and how it's used by skills

### Code Style

- Markdown files: use ATX-style headings (`#`), fenced code blocks
- YAML frontmatter: use consistent field ordering as defined in each component's guidelines
- Keep line lengths reasonable (no hard wrap required, but break at logical points)
- Use tables for structured comparisons

## License

By contributing, you agree that your contributions will be licensed under the [Apache License 2.0](LICENSE).
