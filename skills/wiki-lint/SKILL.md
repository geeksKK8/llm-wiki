---
name: wiki-lint
description: Use when the user asks to health-check the wiki, or periodically when wiki grows large. Triggered by "lint", "检查", "健康检查", or when wiki has accumulated many pages and needs consistency review.
---

# Wiki Lint

## Before Starting

Read the project's `CLAUDE.md` to understand domain configuration:
- Wiki Structure — what directories exist and what each contains
- Language conventions
- What "stale" means in this domain (e.g., superseded papers vs. outdated analysis vs. deprecated APIs)
- Coverage scope — what topics the wiki should and shouldn't cover

This ensures your lint checks are calibrated to the domain's expectations and terminology.

## Overview

Lint health-checks the wiki for consistency gaps, stale content, and structural issues. It's proactive maintenance — finding problems before they degrade query quality. It also suggests new directions to explore.

## When to Use

- User asks to lint, check, or audit the wiki
- Wiki has grown beyond ~20 pages and hasn't been linted recently
- After a batch of ingests when consistency may have slipped

## Checklist

Read all wiki pages systematically. Check each dimension below and report findings with specific page references.

1. **Contradictions** — Claims on different pages that disagree (e.g., one page says method A is superior, another cites evidence against it). List each contradiction with `[[page A]]` vs `[[page B]]` and the conflicting claims.

2. **Stale claims** — Information superseded by newer sources but not updated. Check each page's `sources` frontmatter against newer ingests. Flag claims that may need revision.

3. **Orphan pages** — Pages with no inbound `[[wikilinks]]` from other wiki pages. These are disconnected from the knowledge graph. List them and suggest where cross-references should be added.

4. **Missing pages** — Concepts/entities frequently mentioned across the wiki but lacking their own dedicated page. These are gaps waiting to be filled. List each with count of mentions.

5. **Missing cross-references** — Pages that discuss related topics but don't link to each other. Suggest specific `[[wikilinks]]` to add.

6. **Data gaps** — Topics where the wiki's coverage is thin and could benefit from:
   - New source documents (suggest specific paper/article types)
   - Web search to fill factual gaps (suggest specific queries)

7. **Suggestions** — Based on the current wiki state, propose:
   - New questions worth exploring
   - New sources to seek out
   - Pages that could be generated from existing content (comparisons, syntheses)

## Output Format

Present lint results as structured markdown:

```
# Wiki Lint Report — [YYYY-MM-DD]

## Contradictions
- [[page A]] says X, but [[page B]] says Y

## Stale Claims
- [[page C]]: claim about Z may be outdated (source from 2023, newer 2025 source available)

## Orphan Pages
- [[page D]] — no inbound links, suggest adding links from [[page E]], [[page F]]

## Missing Pages
- "concept G" mentioned 8 times but has no dedicated page

## Missing Cross-References
- [[page H]] and [[page I]] cover related topics but don't link to each other

## Data Gaps
- Topic J: wiki lacks coverage of recent 2025-2026 papers on this subject

## Suggestions
- Generate comparison: [[method K]] vs [[method L]]
- New source to seek: 2025 survey paper on topic M
```

After presenting the report, ask the user which fixes to prioritize. Then execute fixes, updating all relevant pages, and append a lint entry to `wiki/log.md`.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Just listing problems without fixing them | Present report, then ask user which to fix and execute |
| Skipping the suggestions dimension | Proactive suggestions are the most valuable lint output |
| Not checking frontmatter consistency | Verify `sources`, `tags`, `updated` fields are current on every page |
| Ignoring the log update | Always append lint entry to `wiki/log.md` after completing |