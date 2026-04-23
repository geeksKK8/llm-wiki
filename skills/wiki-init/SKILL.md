---
name: wiki-init
description: Use when initializing the wiki from an existing collection of raw sources. Triggered when the user has multiple files in raw/ and wants to build the wiki from scratch, or when they say "初始化", "init", "setup", "建立知识库", "从零开始", "批量处理所有源", or when wiki/ is empty or minimal while raw/ has 5+ files. Also use when a user says they have a bunch of papers/articles they want to process all at once, even if they don't explicitly say "init".
---

# Wiki Init

## Before Starting

Read the project's `CLAUDE.md` to understand domain configuration:
- Wiki Structure — what directories exist and what each contains
- Language conventions (default language, how to handle technical terms)
- Source ID derivation rules
- Domain coverage scope
- What each category in the Wiki Structure represents for this domain

This ensures you create pages in the correct directories and use the right terminology from the start.

## Overview

Init bootstraps the wiki from a collection of raw sources — not one-by-one like ingest, but by scanning the full landscape first, then building the knowledge network in one coordinated pass. The goal is **establishing a connected skeleton**, not exhaustively detailing every source. After init, individual sources should still be deepened via `wiki-ingest`.

Init is a one-time operation. It produces the directory structure, the overview, the index, a batch of initial pages, and their cross-references. It's the cold-start that makes subsequent ingest and query operations meaningful.

## When NOT to Use

- Only 1-2 raw sources — just use `wiki-ingest` directly
- Wiki already has substantial content (5+ pages with linked topic pages) — the wiki is past cold-start; continue with ingest
- User wants to add a single new source to an existing wiki — that's `wiki-ingest`
- User wants to lint or fix the wiki — that's `wiki-lint`
- User is asking a question — that's `wiki-query`

## When to Use

- `raw/` has 5+ files and wiki is empty or minimal
- User explicitly asks to initialize, setup, or batch-process all sources
- User says they've collected a bunch of sources and want to "get the wiki going"
- User asks to "process everything" or "build the whole wiki at once"

## Checklist

Every init MUST complete ALL steps below. Use TaskCreate to track progress per step.

### Phase 1: Scan

1. **List all raw sources** — Enumerate every file in `raw/` with filename and type (PDF, markdown, image, etc.). Present the full list to the user.

2. **Quick-read each source** — For each raw document, do a shallow scan to extract:
   - Title and authors
   - 3-5 key topics/terms mentioned
   - Key entities relevant to this domain
   - Domain/application area
   - Relationship to other sources (if apparent from references)

   For PDFs: read title page and abstract/first few pages. Don't attempt full deep reading at this stage — you're building a map, not writing every page in detail. For images: view them and note what they depict.

3. **Compile a landscape map** — Present the user with a structured summary of what you found, organized by the categories defined in `CLAUDE.md`:
   ```
   ## Raw Sources Landscape

   | # | Source ID | Title | Key Topics | Key Entities | Domain |
   |---|-----------|-------|------------|--------------|--------|
   | 1 | ... | ... | ... | ... | ... |

   ## Topic Frequency
   (populated from scan — use categories from CLAUDE.md)

   ## Suggested Initial Pages
   (organized by CLAUDE.md Wiki Structure)
   ```

### Phase 2: Discuss

4. **Discuss with user** — Ask:
   - Which topics are most important to them? (This guides page depth)
   - Should any topics be merged or split differently than suggested?
   - What's the narrative frame for the overview? (e.g., "historical evolution", "current methods landscape", "practical applications focus")
   - Are there sources they consider low-priority or want to skip?
   - Any language preferences beyond what's specified in CLAUDE.md?

   The user curates direction. Don't assume emphasis. Their answers determine what pages get created and how deep each goes.

### Phase 3: Build

5. **Create directory structure** — Ensure all wiki subdirectories exist per `CLAUDE.md`'s Wiki Structure definition, plus the standard files:
   ```
   wiki/
     index.md
     log.md
     overview.md
     (subdirectories as defined in CLAUDE.md)
   ```

6. **Create category pages** — For each page identified in Phase 1+2, create it in the directory defined by `CLAUDE.md` for its category:
   - Write initial content that synthesizes information from ALL sources that mention this topic (not just one source). This is init's key advantage over sequential ingest — pages are born connected.
   - Add frontmatter with all relevant source IDs in `sources` field. Format per CLAUDE.md:
     ```yaml
     ---
     title: Page Title
     tags: [tag1, tag2]
     sources: [source-id1, source-id2]
     updated: YYYY-MM-DD
     ---
     ```
   - Use `[[wikilinks]]` to connect to related pages
   - Pages should be substantive enough to be useful (not stubs with just a definition), but not as detailed as they'll become after later ingests

   Prioritize by frequency and user emphasis — use category labels from `CLAUDE.md`.

7. **Write overview** — Compose `wiki/overview.md` as a high-level synthesis based on all scanned sources. This is the panoramic view that individual pages zoom into. Structure:
   - Domain definition and scope
   - Core problems/challenges
   - Key trends and directions
   - Notable entities and their contributions
   - Cross-references to key `[[wikilinks]]`

   The overview's narrative frame should follow the user's preference from Phase 2.

8. **Build index** — Write `wiki/index.md` listing all pages created, organized by category with one-line summaries (language per CLAUDE.md) and wikilinks. Every page must appear in the index.

9. **Append log** — Add entry to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] init | Batch initialization from N sources

   Scanned N sources in raw/, discussed priorities with user, created:
   - K topic pages across categories defined in CLAUDE.md
   - overview.md, index.md
   ```

### Phase 4: Verify

10. **Consistency check** — Quickly verify:
    - Every page has frontmatter with title, tags, sources, updated
    - Every page is listed in index.md
    - Key wikilinks reference pages that actually exist
    - No orphan pages (every page is linked from at least one other page)
    - Source IDs in frontmatter match actual raw filenames

    Fix any issues found.

11. **Present results to user** — Show what was created and suggest next steps:
    - Which sources deserve deeper ingest treatment
    - Which pages are thin and could benefit from more detail
    - What new sources would fill gaps
    - Suggest running `wiki-lint` after a few more ingests

## Init vs Ingest: Key Differences

| Aspect | Init | Ingest |
|--------|------|--------|
| Scope | All sources at once | One source at a time |
| Depth per source | Shallow scan + key findings in log | Deep reading + discussion |
| Page creation | Born connected, synthesizing across sources | Added to existing network |
| Overview | Written from scratch | Incrementally updated |
| Interaction | One landscape discussion upfront | Per-source discussion |
| Frequency | Once (cold start) | Every new source |
| Follows | Nothing (it's first) | Init or previous ingest |

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Trying to do full deep reading of every source in Phase 2 | Init scans; ingest deepens. Keep Phase 2 shallow — you're mapping, not mining |
| Creating stub pages with just a one-line definition | Pages should synthesize across sources, not just define. Each topic page should mention which sources discuss it, what they say, and link to related topics |
| Processing sources sequentially instead of by topic | Init's power is cross-source synthesis. Write topic pages that pull from ALL relevant sources at once, not one-by-one |
| Skipping the landscape discussion | User curates which topics matter most — this determines page priority and depth |
| Writing the overview before discussing narrative frame | Ask the user what angle they want first |
| Not linking pages to each other | Every new page should have multiple [[wikilinks]] — an isolated page defeats init's purpose |
| Forgetting to update frontmatter sources field | Every topic page must list ALL source IDs that contributed to it |
| Creating pages in wrong language | Use the language specified in CLAUDE.md, with technical terms in their original language alongside |