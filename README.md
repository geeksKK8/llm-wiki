# LLM Wiki

A pattern for building personal, compounding knowledge bases with LLMs.

Inspired by [Andrej Karpathy](https://twitter.com/karpathy)'s vision of using LLMs as persistent knowledge compilers — not just retrieval engines, but systems that read, synthesize, cross-reference, and maintain a living wiki that grows richer with every source you add.

> Most people's experience with LLMs and documents looks like RAG: upload files, retrieve chunks, generate an answer. The LLM rediscover knowledge from scratch on every question. There's no accumulation.
>
> The idea here is different. The LLM **incrementally builds and maintains a persistent wiki** — a structured, interlinked collection of markdown files. Knowledge is compiled once and *kept current*, not re-derived on every query.

## 核心理念

受 [Andrej Karpathy](https://twitter.com/karpathy) "LLM 作为知识编译器" 的启发——不是 RAG 式的每次检索从头推导，而是让 LLM 持续构建和维护一个增量式 wiki：每加入一个新源，就提取关键信息、更新已有页面、标记矛盾、强化或修正已有论断。知识编译一次，然后持续保持更新。

**关键区别：wiki 是一个持续积累、不断复利的产物。** 交叉引用已经建立，矛盾已经标记，综合已经反映所有已读内容。wiki 随每个新源和每次提问变得更加丰富。

## How It Works

### Three Layers

| Layer | Description | Who owns it |
|-------|-------------|-------------|
| `raw/` | Immutable source documents (papers, articles, images) | You — read-only for LLM |
| `wiki/` | LLM-generated markdown pages (summaries, concepts, entities, comparisons) | LLM — you read, LLM writes |
| `CLAUDE.md` | Schema & conventions for the wiki structure and operations | You + LLM co-evolve |

### Four Operations

| Operation | Skill | Purpose |
|-----------|-------|---------|
| **Init** | `/wiki-init` | Bootstrap wiki from a batch of raw sources (one-time cold start) |
| **Ingest** | `/wiki-ingest` | Process a new source, integrate across 5-15 wiki pages |
| **Query** | `/wiki-query` | Answer questions from wiki, file valuable answers back as new pages |
| **Lint** | `/wiki-lint` | Health-check wiki for contradictions, stale claims, orphans, gaps |

### Key Files

- **`wiki/index.md`** — Content catalog of every wiki page, organized by category. The LLM reads this first to find relevant pages.
- **`wiki/log.md`** — Append-only chronological record of all operations. Parseable with simple unix tools.
- **`wiki/overview.md`** — High-level synthesis of the entire domain.

## Quick Start

1. **Install the skills** — Copy the `skills/` directory into your LLM agent's skill configuration (e.g., `.claude/skills/` for Claude Code, or the equivalent for your agent).

2. **Configure your domain** — Copy `example/CLAUDE.md` into your project root and modify the Domain Configuration section:
   - Set your domain and coverage scope
   - Define your wiki directory structure
   - Choose your default language
   - Define source ID derivation rules

3. **Add sources** — Drop documents into `raw/` (PDFs, markdown, images, etc.).

4. **Run `/wiki-init`** — The LLM scans all sources, discusses priorities with you, and builds the initial wiki in one coordinated pass.

5. **Continue with `/wiki-ingest`** for each new source, `/wiki-query` to ask questions, and `/wiki-lint` to health-check.

## Project Structure

```
llm-wiki/
  llm_wiki.md           — The original idea document (the pattern description)
  README.md             — This file
  example/
    CLAUDE.md           — Example schema (domain: embodied intelligence / 具身智能)
  skills/
    wiki-init/SKILL.md  — Bootstrap skill for cold-start
    wiki-ingest/SKILL.md — Per-source integration skill
    wiki-query/SKILL.md — Query & answer skill
    wiki-lint/SKILL.md  — Health-check skill
```

In your actual knowledge base project, the structure would look like:

```
your-project/
  CLAUDE.md             — Your domain-specific schema
  raw/                  — Your source documents (immutable)
  wiki/
    index.md            — Content catalog
    log.md              — Operation log
    overview.md         — Domain synthesis
    concepts/           — Concept/topic pages
    entities/           — Entity pages (people, models, datasets, etc.)
    sources/            — Per-source summaries
    comparisons/        — Comparison analyses
    methods/            — Technical method pages
    applications/       — Application domain pages
```

## Tips

- **Obsidian** is the best viewer — graph view, backlinks, Dataview queries all work with the `[[wikilinks]]` format.
- **Obsidian Web Clipper** converts web articles to markdown for easy ingestion.
- The wiki is just markdown + git — you get version history and collaboration for free.
- Good answers from `/wiki-query` can be filed back as new wiki pages — knowledge compounds.
- At moderate scale (~100 sources, ~hundreds of pages), the `index.md` file works well for navigation without needing embedding-based RAG.

## 适用场景

- **个人成长** — 记录目标、健康、心理，结构化地积累自我认知
- **深度研究** — 几周到几个月地追踪论文和报告，构建有演化论点的综合 wiki
- **读书笔记** — 每章归档，构建人物、主题、情节的互联 wiki
- **团队知识库** — 由 LLM 维护内部 wiki，源自 Slack、会议纪要、项目文档
- **竞品分析、尽职调查、旅行规划、课程笔记、爱好深研** — 任何需要长期积累和组织的知识

## Philosophy

The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping. Updating cross-references, keeping summaries current, noting contradictions, maintaining consistency across dozens of pages. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget a cross-reference, and can touch 15 files in one pass. The wiki stays maintained because the cost of maintenance is near zero.

Your job: curate sources, direct analysis, ask good questions. The LLM's job: everything else.

Related in spirit to Vannevar Bush's [Memex](https://en.wikipedia.org/wiki/Memex) (1945) — a personal, curated knowledge store with associative trails. Bush couldn't solve who does the maintenance. The LLM handles that.

## License

MIT