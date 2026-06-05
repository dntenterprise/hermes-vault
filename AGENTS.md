# Hermes Vault — Schema & Workflows

This document defines how the LLM maintains the Hermes knowledge base.
Read this before any vault operation.

## Three-Layer Architecture

```
Hermes Vault/
├── raw/              # Layer 1: Immutable source documents
├── wiki/             # Layer 2: LLM-maintained markdown wiki
│   ├── index.md      #   Content catalog
│   ├── log.md        #   Append-only chronological record
│   ├── entities/     #   Entity pages (people, companies, products)
│   ├── concepts/     #   Concept/theory pages
│   ├── sources/      #   Source summaries (articles, podcasts, videos)
│   └── system/       #   System documentation (architecture, config, workflows)
└── AGENTS.md         # Layer 3: This schema file
```

## Conventions

### File format
- All pages: markdown
- Frontmatter: `tags:`, `created:`, `updated:`, `sources:` (comma-separated)
- Wiki links: `[[wiki/entities/Name]]`, `[[wiki/concepts/Idea]]`
- Headers: `## Title Case`

### Naming
- Files: `kebab-case.md`
- Entities: capitalize proper nouns (`Dino-Valentino.md`)
- Concepts: lowercase (`dropshipping-strategy.md`)
- Sources: `YYYY-MM-DD_kebab-title.md`

### Cross-references
- Every entity/concept page MUST link to at least 2 other pages
- If you create a page, add a link to it from `wiki/index.md`
- Source summaries link back to entities and concepts they discuss

## Workflows

### Ingest
When user provides a source (URL, file, paste, TikTok link):
1. Save source to `raw/` (clip or download)
2. Read the source
3. Discuss key takeaways with the user
4. Create/update:
   - `wiki/sources/YYYY-MM-DD_kebab-title.md` (summary)
   - Relevant entity pages in `wiki/entities/`
   - Relevant concept pages in `wiki/concepts/`
   - Update `wiki/index.md` (add entry)
   - Append entry to `wiki/log.md`
5. Cross-link new pages with existing ones

### Query
When user asks a question:
1. Read `wiki/index.md` to find relevant pages
2. Read those pages
3. Synthesize answer with citations (`[[wiki/sources/file]]`)
4. If the answer is reference-worthy, file it as a new wiki page

### Lint
When user asks to lint:
1. Check for: contradictions, stale claims, orphan pages, missing cross-refs
2. Suggest new questions and sources to fill gaps
3. Update `wiki/index.md` and `wiki/log.md`

## Raw Sources

`raw/` is append-only — NEVER modify or delete files here.
Files are organized by type:
- `raw/articles/` — web articles, blog posts
- `raw/tiktok/` — TikTok video transcripts/notes
- `raw/docs/` — PDFs, documents, specs
- `raw/assets/` — images, attachments

## Wiki Index Format

```markdown
## Entities
| Page | Summary | Sources |
|------|---------|---------|
| [[wiki/entities/Name]] | One-line description | 3 |

## Concepts
...

## Sources
...
```

## Log Format

```markdown
## [2026-06-05] ingest | Title
- Raw: `raw/articles/file.md`
- Created: `wiki/sources/file.md`, updated: `wiki/entities/Name`
- Key takeaway: one sentence
```
