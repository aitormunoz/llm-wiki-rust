# Rust LLM Wiki — schema

This folder is an Obsidian vault and a git repo. It holds a personal wiki about learning Rust,
**written and maintained entirely by you, the LLM**. I add sources and ask questions; you keep
the wiki correct, linked and up to date.

## Layers

- `raw/` — source documents. **Immutable**: read them, never edit, move or delete them.
- `wiki/` — the compiled knowledge. You own it: create, rewrite, split and merge pages freely.
- `CLAUDE.md` — this schema. Propose changes to it, but don't edit it without being asked.

## Wiki layout

- `wiki/index.md` — the catalog. One line per page: `- [[page-name]] — one-line summary`, grouped
  by section (Sources, Concepts, Answers). Read it first for every operation.
- `wiki/log.md` — append-only history. Never rewrite past entries.
- `wiki/sources/<source-file-name>.md` — one summary page per raw source.
- `wiki/concepts/<concept>.md` — one page per concept (`ownership`, `borrowing`, `lifetimes`,
  `rc`, `send-and-sync`...). This is where most of the knowledge lives.
- `wiki/answers/<question-slug>.md` — answers to my questions that are worth keeping.

## Page conventions

- File names: lowercase kebab-case. Link pages with Obsidian wikilinks: `[[borrowing]]`.
- Every page starts with frontmatter:

  ```yaml
  ---
  type: source | concept | answer
  sources: [raw/ch04-understanding-ownership.md]
  updated: YYYY-MM-DD
  ---
  ```

- Concept pages: a 2–3 sentence definition, then the key rules, a minimal Rust example, how it
  relates to other concepts (with links), and common mistakes.
- **Every non-obvious claim cites its source** inline: `(src: [[ch04-understanding-ownership]])`.
- Keep pages short and focused. Rewrite sections to integrate new information; **don't just
  append** a new paragraph at the bottom of a page.

## Operations

### Ingest (`ingest raw/<file>`)

1. Read the source in full, then `wiki/index.md`.
2. Write its summary page in `wiki/sources/`.
3. Create or **update** every concept page the source touches. Prefer updating an existing page
   over creating a near-duplicate.
4. If the source contradicts what the wiki already says, don't silently overwrite. Add an
   Obsidian callout on the affected page and resolve it if the sources make the answer clear:

   ```markdown
   > [!warning] Conflict
   > What each source says, which one is current and why.
   ```

5. Update `wiki/index.md` and append to `wiki/log.md`:
   `## [YYYY-MM-DD] ingest | <source title>` followed by a bullet list of pages created and updated.

### Query (`query: <question>`)

1. Read `wiki/index.md`, then the relevant pages. Use `raw/` only when the wiki falls short,
   and if it does, fix the wiki too.
2. Answer with citations to wiki pages.
3. If the answer is worth keeping, save it in `wiki/answers/`, link it from the related concept
   pages and the index, and log it: `## [YYYY-MM-DD] query | <question>`.

### Lint (`lint`)

Health-check the whole wiki and report, by severity:

- contradictions between pages, or between a page and its sources;
- stale claims superseded by a newer source;
- orphan pages (no inbound links) and broken wikilinks;
- concepts mentioned on several pages that have no page of their own;
- claims without a citation.

Only report unless I say `lint --fix`. Log it: `## [YYYY-MM-DD] lint | <n> issues`.
