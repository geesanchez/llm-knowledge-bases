# LLM Knowledge Base

A personal knowledge base powered by Claude Code. You add raw source material — articles, papers, notes, images — and Claude compiles, cross-references, and maintains a structured wiki from it.

Unlike RAG (which re-derives answers on every query), this system compiles knowledge once into persistent wiki articles that compound over time. Every question you ask can be filed back into the wiki, so your explorations enrich future queries.

## How It Works

```
raw/          You add source material here
  articles/   Web articles, blog posts
  papers/     Academic papers (PDF)
  notes/      Personal notes, READMEs, transcripts
  images/     Screenshots, diagrams

wiki/         Claude compiles and maintains this
  _index.md      Master index grouped by topic
  _summary.md    One-line lookup table for every article
  _backlinks.md  Cross-reference map (powers Obsidian graph view)
  *.md           Individual wiki articles

outputs/      Q&A results and analyses filed back from queries
```

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed and authenticated
- [Obsidian](https://obsidian.md/) (optional, for browsing the wiki with graph view and wikilinks)

## Setup

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/llm-knowledge-bases.git
   cd llm-knowledge-bases
   ```

2. Open a Claude Code session in the project root:
   ```bash
   claude
   ```

3. Add source material to `raw/` — drop articles into `raw/articles/`, papers into `raw/papers/`, etc.

4. Tell Claude to compile:
   ```
   compile the wiki
   ```

Claude will read your sources, create wiki articles, and build the index files automatically.

## Commands

These are natural-language instructions you give Claude inside a session:

| Command | What it does |
|---------|-------------|
| `compile the wiki` | Process new/changed files in `raw/` into wiki articles |
| _Ask any question_ | Claude reads the wiki and answers from your knowledge base |
| `file this into the wiki` | Save a Q&A output back into the knowledge base |
| `lint the wiki` | Health check: orphaned articles, broken links, stale sources |
| `suggest what to research next` | Analyze gaps and recommend topics to add |

## Obsidian Integration

Open this repo as an Obsidian vault. The wiki uses `[[wikilinks]]` for cross-references, so Obsidian's graph view will show you how your knowledge connects. The `_index.md` file works as a home page.

## Rules

- **`raw/` is read-only** — Claude never modifies your source files
- **`wiki/` is Claude's domain** — Claude writes and maintains everything here
- **Sources are always cited** — every wiki article links back to the raw files it was compiled from
- **Index files stay current** — `_index.md`, `_summary.md`, and `_backlinks.md` update automatically after every compile

## License

MIT
