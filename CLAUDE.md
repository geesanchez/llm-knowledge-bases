# LLM Knowledge Base — Claude Code Instructions

This project is an LLM-powered personal knowledge base. You (Claude Code) are the engine that compiles, maintains, queries, and improves it. The user rarely edits the wiki directly — that's your job.

## Project Structure

- `raw/` — Source documents added by the user (articles, papers, images, notes). **Never modify raw/ files.**
- `wiki/` — The compiled wiki. **You write and maintain everything here.**
- `outputs/` — Q&A results, reports, and analyses filed back from your responses.
- `CLAUDE.md` — This file. Your operating instructions.

## Filename Conventions

- Raw articles: `YYYY-MM-DD-slug.md` (e.g., `2026-04-10-transformer-architecture.md`)
- Wiki articles: `concept-name.md` (e.g., `attention-mechanisms.md`)
- Output files: `YYYY-MM-DD-query-slug.md` (e.g., `2026-04-10-comparison-of-optimizers.md`)

## Index Files (Your Navigation Layer)

Three files in `wiki/` form your navigation layer. **Always read `_summary.md` first** when answering questions — it tells you which articles exist and what they cover.

### wiki/_index.md
Master index of all wiki articles, grouped by topic area. Each entry:
```
- [[article-name]] — one-line description (tags: #tag1, #tag2)
```

### wiki/_summary.md
One-line summary of every wiki article. This is your primary lookup table for Q&A. Format:
```
| Article | Summary | Sources |
|---------|---------|---------|
| [[article-name]] | Brief summary of key points | raw/articles/source.md |
```

### wiki/_backlinks.md
Cross-reference map showing which articles link to which. Format:
```
## [[article-name]]
Linked from: [[other-article]], [[another-article]]
Links to: [[concept-a]], [[concept-b]]
```

## Operating Modes

### 1. Compile ("compile the wiki")
Read new or changed files in `raw/` and compile them into wiki articles.

**Steps:**
1. List all files in `raw/` subdirectories
2. Compare against `wiki/_summary.md` to find new/unprocessed sources
3. **Before writing anything**, list what you plan to create or update, and confirm with the user
4. For each new source, either:
   - Create a new wiki article if it introduces a new concept
   - Update an existing wiki article if it adds to a known concept
5. After writing articles, update all three index files (`_index.md`, `_summary.md`, `_backlinks.md`)

**Wiki article template:**
```markdown
---
title: Article Title
date_compiled: YYYY-MM-DD
sources:
  - raw/articles/source-file.md
tags:
  - tag1
  - tag2
---

## Summary
2-3 sentence overview of this concept.

## Key Points
- Point 1
- Point 2
- Point 3

## Details
Extended explanation with context, examples, and nuance drawn from the source material.

## Related
- [[related-concept-1]] — how it connects
- [[related-concept-2]] — how it connects

## Sources
- [Source Title](raw/articles/source-file.md) — what this source contributed
```

### 2. Query (any question about the knowledge base)
Answer questions by reading the wiki.

**Steps:**
1. Read `wiki/_summary.md` to identify relevant articles
2. Read those articles in full
3. Synthesize an answer from the wiki content
4. Cite which wiki articles informed the answer
5. If the wiki doesn't cover the topic, say so — don't hallucinate beyond what's in the KB

### 3. File Back ("file this into the wiki")
Save a Q&A output or analysis back into the knowledge base.

**Steps:**
1. Save the output to `outputs/YYYY-MM-DD-query-slug.md`
2. If the output produced new knowledge worth preserving:
   - Create or update relevant wiki articles
   - Update index files
3. Add a backlink from the output to the wiki articles it references

### 4. Lint ("lint the wiki")
Run health checks on the wiki.

**Checks:**
- Orphaned articles (in `wiki/` but not in `_index.md`)
- Broken [[wikilinks]] (link to articles that don't exist)
- Missing summaries (articles not in `_summary.md`)
- Stale sources (raw files referenced by wiki articles that no longer exist)
- Missing backlinks (articles that reference each other but aren't in `_backlinks.md`)
- Duplicate or overlapping articles (concepts that should be merged)
- Inconsistent information across articles

**Output:** A report listing all issues found, grouped by severity.

### 5. Suggest ("what should I research next?")
Analyze the wiki for gaps and opportunities.

**Steps:**
1. Read `_index.md` and `_summary.md` to understand current coverage
2. Identify:
   - Concepts mentioned in [[wikilinks]] but with no article yet
   - Topics with thin coverage (few sources, short articles)
   - Interesting connections between distant concepts
   - Natural next questions the existing content raises
3. Output a ranked list of suggested topics to research

### 6. Index (automatic)
After any compile or file-back operation, update all three index files to reflect the current state of the wiki.

## Rules

1. **Never modify files in `raw/`.** That's the user's source-of-truth input.
2. **Always use [[wikilinks]]** for cross-references between wiki articles. This powers Obsidian's graph view.
3. **Read before writing.** Before creating a wiki article, check if one already exists for that concept.
4. **Confirm before large operations.** If a compile would create or update more than 3 articles, list your plan first.
5. **Cite sources.** Every wiki article must reference which raw/ files it was compiled from.
6. **Keep index files current.** After any write to `wiki/`, update `_index.md`, `_summary.md`, and `_backlinks.md`.
7. **Be conservative with merging.** Don't merge two articles unless they clearly cover the same concept.
8. **Preserve existing content.** When updating an article with new sources, add to it — don't overwrite previous content unless it's wrong.
