---
name: wiki-query
description: Use when the user asks a question that should be answered from the wiki knowledge base in the current project. Triggered by questions about concepts, comparisons, relationships, or any query where wiki pages exist — the domain scope is defined by the project's CLAUDE.md.
---

# Wiki Query

## Before Starting

Read the project's `CLAUDE.md` to understand domain configuration:
- Wiki Structure — what directories exist and what each contains
- Language conventions (default language, how to handle technical terms)
- Source ID derivation rules
- What each category in the Wiki Structure represents for this domain

This ensures your answer uses the correct terminology, categorization, and cross-reference style for the current knowledge base.

## Overview

Query answers questions from the wiki's accumulated knowledge, not by re-deriving from raw sources. When an answer produces a valuable new artifact (comparison, analysis, connection), it gets filed back into the wiki. **Good answers compound — they become new wiki pages.**

## When to Use

- User asks a question about concepts, methods, entities, etc. within the wiki's domain scope
- User wants a comparison, analysis, or synthesis across multiple topics
- User asks "compare X and Y", "what's the relationship between...", "summarize the state of..."

## Checklist

1. **Read index** — Start with `wiki/index.md` to find relevant pages. Don't guess — scan the catalog.
2. **Read relevant pages** — Drill into the pages identified from the index. Read full content.
3. **Go to raw sources if needed** — If wiki pages lack sufficient detail, read the original `raw/` documents referenced in their `sources` frontmatter.
4. **Synthesize answer** — Compose the answer in the language specified by CLAUDE.md with `[[wikilinks]]` citations pointing to source wiki pages. Present clearly to the user.
5. **File valuable answers back** — If the answer is a standalone artifact worth preserving:
   - Comparison → appropriate directory per CLAUDE.md's Wiki Structure
   - New topic analysis → appropriate directory per CLAUDE.md's Wiki Structure
   - New connection/synthesis → appropriate directory per CLAUDE.md's Wiki Structure
   - Add frontmatter, update `index.md`, append to `log.md`

## Decision: When to file back?

Not every answer becomes a wiki page. File back when:
- The answer synthesizes information across 3+ pages (not just paraphrasing one page)
- The answer creates a new comparison or relationship not already in the wiki
- The user explicitly asks to "save this" or "add this to the wiki"

Don't file back when:
- The answer is a simple lookup from a single page
- The information is already well-represented in existing pages
- The answer is ephemeral (e.g., "what does this term mean?")

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Jumping straight to raw sources without checking wiki | Wiki is the compiled knowledge — start there |
| Answering without citations | Always include `[[wikilinks]]` to source pages |
| Letting good answers evaporate into chat history | File valuable answers as new wiki pages |
| Answering in wrong language | Use the language specified in CLAUDE.md, with technical terms in their original language alongside |