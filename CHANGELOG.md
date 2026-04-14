# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-03-31

### Added

- **api-learning plugin** -- outside-in, API-first approach to learning Java frameworks
  - `api-blueprint` skill for generating API-first learning outlines
  - `api-builder` skill for implementing vertical slices with code, tests, and tutorials
  - `api-code-explorer` agent for tracing API surfaces and call chains
  - `api-code-architect` agent for designing vertical slice architectures
  - `api-code-reviewer` agent for validating API contracts and tutorial accuracy
- **core-learning plugin** -- inside-out approach to learning Java frameworks
  - `core-blueprint` skill for generating core-first learning outlines
  - `core-builder` skill for implementing features with code, tests, and tutorials
  - `core-code-explorer` agent for tracing internal hierarchies and execution flows
  - `core-code-architect` agent for designing simplified component architectures
  - `core-code-reviewer` agent for validating correctness and tutorial accuracy
- **enterprise-learning plugin** -- re-implement any source project (any language) into enterprise-grade Java
  - `enterprise-blueprint` skill for analyzing source projects, classifying type, and generating outlines with technology mappings
  - `enterprise-builder` skill for implementing features with rich tutorials, Mermaid visualizations, and Insight blocks
  - `enterprise-code-explorer` agent for multi-language source analysis and project type classification
  - `enterprise-code-architect` agent for enterprise Java architecture design and technology mapping
  - `enterprise-code-reviewer` agent for validating correct stack usage, tutorial completeness, and visualization quality
  - Shared standards for diagrams, insights, quality checklists, and technology defaults
- Marketplace manifest with plugin registry
- Reference templates for outlines and tutorials
- Analysis checklists for framework exploration
- Apache 2.0 licensing

[1.0.0]: https://github.com/nlinhvu/learn-java-frameworks-marketplace/releases/tag/v1.0.0
