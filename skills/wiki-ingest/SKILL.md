---
name: wiki-ingest
description: Use when adding a new source document to the raw directory and processing it into the wiki. Triggered by user dropping a file into raw/ or asking to ingest/process a source. Source types are defined by the project's CLAUDE.md.
---

# Wiki Ingest

## Before Starting

Read the project's `CLAUDE.md` to understand domain configuration:
- Wiki Structure — what directories exist and what each contains
- Language conventions (default language, how to handle technical terms)
- Source ID derivation rules
- What each category in the Wiki Structure represents for this domain

This ensures you create/update pages in the correct directories and use the right terminology.

## Overview

Ingest integrates a new raw source into the existing wiki — not just summarizing it, but updating multiple pages across the knowledge base. A single source typically touches 5-15 wiki pages. The goal is **integration, not isolation**.

## When to Use

- User adds a file to `raw/` and asks to process it
- User says "ingest", "处理", "摄入", or refers to a specific source by name
- User asks you to read a paper/article and add it to the knowledge base

## Checklist

Every ingest MUST complete ALL steps below. Use TaskCreate to track progress per step.

1. **Read source** — Read the full document from `raw/`. For PDFs, read page ranges as needed. For images in sources, read them separately after text.
2. **Discuss with user** — Summarize key takeaways, ask what to emphasize, clarify ambiguous points. The user curates direction.
3. **Update wiki pages** — Identify key topics from the source. For each:
   - If page exists: update with new information, add source ID to `sources` field, update `updated` date
   - If page doesn't exist: create it with full frontmatter and initial content
   - Pages go to directories per CLAUDE.md's Wiki Structure
5. **Update overview** — If the source affects the high-level synthesis, update `wiki/overview.md`.
6. **Update index** — Add all new/changed pages to `wiki/index.md` under appropriate categories with one-line summaries (language per CLAUDE.md).
7. **Append log** — Add entry to `wiki/log.md`: `## [YYYY-MM-DD] ingest | Source Title`, noting which pages were created/updated and a brief summary of key findings from this source.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Creating only a standalone page for the source, not touching other pages | Step 3 is the core value — always update existing pages across the wiki |
| Skipping the discussion step | User curates direction; don't assume emphasis |
| Forgetting to update frontmatter `sources` field | Every touched page must list the source ID |
| Writing wiki pages in wrong language | Use the language specified in CLAUDE.md, with technical terms in their original language alongside |
| Missing wikilinks between related pages | Every mention of another wiki concept should be `[[linked]]` |