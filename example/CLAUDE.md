# LLM Wiki — CLAUDE.md

This directory is a persistent, compounding knowledge base. The LLM incrementally builds and maintains a structured wiki of markdown files from raw sources. Knowledge is compiled once and kept current — not re-derived on every query.

---

## Domain Configuration

This section defines the domain-specific settings for THIS knowledge base. Copy and modify when creating a new knowledge base for a different domain.

### Domain

Embodied intelligence / 具身智能

### Coverage

sim-to-real, VLA models, robot learning, affordance, world models, manipulation, navigation, HRI, imitation learning, RL×robotics, LLM×robotics, datasets/benchmarks, simulation environments, hardware platforms, key labs/researchers.

### Wiki Structure

```
wiki/
  index.md          — Content catalog: every page with link + one-line summary, by category
  log.md            — Append-only chronological record of all operations
  overview.md       — High-level synthesis of embodied intelligence as a field
  concepts/         — Concept/topic pages (e.g., sim-to-real, affordance, world models)
  entities/         — Entity pages (e.g., labs, researchers, datasets, robot platforms)
  comparisons/      — Comparison tables/analyses from queries
  methods/          — Technical method pages (e.g., algorithms, training approaches)
  applications/     — Domain application pages (e.g., manipulation, navigation)
```

### Language

- **Chinese** (中文) by default
- Technical terms: Chinese + English (e.g., "仿真到现实迁移 (Sim-to-Real Transfer)")
- Source summaries may quote English originals

### Source IDs — derived from filename

`2304.13705v1` for `raw/2304.13705v1.pdf`. Used in cross-references and frontmatter.

### Index format — organized by category

```
## Concepts
- [[sim-to-real]] — 从仿真环境到真实世界的策略迁移方法

## Entities
- [[RT-2]] — Google DeepMind的视觉-语言-动作模型

```

---

## Universal Rules

These rules apply to ALL knowledge bases regardless of domain. Do not modify when creating a new knowledge base.

### Architecture

Three layers, strictly separated:

1. **`raw/`** — Immutable source documents. Read-only. Source of truth.
2. **`wiki/`** — LLM-generated markdown files. The LLM owns this layer entirely. User reads; LLM writes.
3. **`CLAUDE.md`** — Schema and conventions. Co-evolved over time.

### Page format — YAML frontmatter on every wiki page

```yaml
---
title: Page Title
tags: [tag1, tag2]
sources: [source-id1, source-id2]
updated: 2026-04-22
---
```

### Cross-references — Obsidian `[[wikilinks]]`

Enables graph view and backlink tracking.

### Log format

```
## [YYYY-MM-DD] operation_type | Description

Brief notes on what was done.
```

Operation types: `ingest`, `query`, `lint`, `update`

### Operations

Four operations drive the wiki:

- **Init** → `/wiki-init` — Bootstrap wiki from a batch of raw sources (cold start, one-time)
- **Ingest** → `/wiki-ingest` — Process a new source, integrate across wiki pages
- **Query** → `/wiki-query` — Answer questions from wiki, file valuable answers back
- **Lint** → `/wiki-lint` — Health-check wiki consistency, suggest improvements

### Rules

- **Never modify `raw/`** — sources immutable
- **Always update `index.md` and `log.md`** after any wiki change
- **Always add frontmatter** to new wiki pages
- **Always use `[[wikilinks]]`** for cross-references
- **Prefer integration over isolation** — update existing pages, not just standalone summaries
- **Ask before major structural changes** (new categories, reorganization)
- **Discuss interpretation with user** — they curate, you maintain