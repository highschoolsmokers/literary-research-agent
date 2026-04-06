# Lit Research Plugin

A literary research plugin for writers. Finds real published works, extracts verified quotes with proper citations, and builds structured reference material around themes, authors, and craft techniques.

Works standalone with manual archive verification, or connects to [literary-verity-mcp](https://github.com/highschoolsmokers/literary-verity-mcp) for machine-verified source grounding with an indexed corpus, claims database, and Paperless-ngx integration.

## What It Does

- **Source discovery** — Finds novels, stories, poems, essays, and criticism relevant to your topic, theme, or craft question
- **Quotation research** — Extracts real quotes from published texts with precise citations and confidence markers
- **Thematic analysis** — Traces how different authors handle the same theme, with side-by-side passages
- **Craft study** — Analyzes how published writers execute specific techniques, grounded in actual passages, with genre-specific vocabulary (poetry, fiction, essay, memoir, drama)
- **Comparative passage analysis** — Places passages side by side to show how different authors handle the same craft problem, with specific observations about what each approach gains and loses
- **Revision research** — Takes your draft passage and finds published models that handle the same craft problem — turns the plugin from a pre-writing tool into a revision companion
- **Translation-aware research** — Compares translations of the same passage, names translators in every citation, flags translation-sensitive passages, and recommends specific editions
- **Theory study** — Researches literary and critical theory (especially postmodern and poststructuralist), maps intellectual genealogies, and connects theoretical concepts to actual fiction and poetry
- **Intertextuality mapping** — Maps specific textual connections between works — epigraphs, allusions, structural borrowings, rewritings, shared sources — with passages from both sides
- **Reading path construction** — Sequences works pedagogically so you know what to read first and why, with alternate routes and time estimates
- **Bibliography building** — Maintains annotated, prioritized reading lists organized by your project's needs
- **Session logging** — Records what was searched, found, and came up empty each session, so you can pick up where you left off
- **Source health monitoring** — Checks that archived links are still live; flags dead URLs so you can find alternate editions

## What It Won't Do

- Fabricate quotes or invent sources
- Present paraphrases as exact quotations
- Cite works that don't exist
- Use web searches as verification — quotes are verified against original texts in digital archives and libraries

Every quote is tagged with a confidence level (`[verified — original accessed]`, `[verified — secondary]`, `[high confidence]`, `[paraphrase]`, `[unverified]`) so you always know what you're working with.

## Install

### As a Claude Code plugin

```bash
/plugin marketplace add highschoolsmokers/writer-utilities-marketplace
/plugin install lit-research-plugin
```

### Manual install

```bash
claude --agent agents/lit-research.md
```

Or copy into your global agents directory:

```bash
cp agents/lit-research.md ~/.claude/agents/lit-research.md
claude --agent lit-research
```

### As a system prompt

The `agents/lit-research.md` file can be used as a system prompt with any Claude API integration.

## MCP Integration (optional)

The plugin works standalone — no infrastructure required. But when connected to [literary-verity-mcp](https://github.com/highschoolsmokers/literary-verity-mcp), every verification step is backed by an indexed corpus with vector embeddings, a curated claims database, and automated bibliographic checks.

### What changes with MCP connected

| Without MCP | With MCP |
|---|---|
| Quotes verified by manually searching archives | `lit_search_texts` searches an indexed corpus with semantic similarity |
| Citations checked by searching catalogs | `lit_verify_citation` checks CrossRef, Semantic Scholar, OpenAlex |
| Theoretical positions characterized from memory | `lit_lookup_claims` returns documented claims with source passages |
| Paywalled sources require manual user download | `lit_procure_source` checks Paperless-ngx archive first, auto-ingests if found |
| No post-generation audit | `lit_verify_output` audits every citation in the agent's output |
| Confidence markers applied by the agent's judgment | Confidence backed by machine verification (embedding similarity scores, bibliographic hits) |

### Setup

1. Install and configure [literary-verity-mcp](https://github.com/highschoolsmokers/literary-verity-mcp) (requires Node.js, Neon Postgres, Voyage AI, and Anthropic API keys)

2. Add both the MCP server and this plugin to your `.mcp.json`:

```json
{
  "mcpServers": {
    "literary-verity": {
      "command": "node",
      "args": ["dist/index.js"],
      "cwd": "/path/to/literary-verity-mcp",
      "env": {
        "DATABASE_URL": "postgresql://...",
        "ANTHROPIC_API_KEY": "sk-ant-...",
        "VOYAGE_API_KEY": "pa-...",
        "PAPERLESS_URL": "http://localhost:8000",
        "PAPERLESS_TOKEN": "your-token"
      }
    }
  }
}
```

3. Start Claude Code with the agent:

```bash
claude --agent lit-research
```

The agent detects `lit_*` tools automatically and uses them when available. No configuration changes needed in the plugin itself.

### The procurement flow

When the agent encounters a paywalled source:

```
Agent finds a citation on JSTOR that needs verification
  → calls lit_procure_source(title: "...", author: "...")
  → checks your Paperless-ngx archive
    → found? Downloads, ingests into corpus, continues with [verified — corpus match]
    → not found? Returns access links, asks you to download
      → you download the PDF
      → agent calls lit_procure_source(file_path: "/path/to/paper.pdf")
      → ingests into corpus + archives to Paperless-ngx
      → continues with verified passages
```

Every source you provide gets indexed for future sessions *and* archived in Paperless-ngx for future retrieval.

### Paperless-ngx as your library

When MCP is connected, your Paperless-ngx archive doubles as the agent's knowledge of your library. At the start of a session, the agent discovers what you've already ingested and treats those documents as immediately available sources — no need to tell it what you own.

The agent organizes your library in three tiers:

| Tier | What it is | How the agent uses it |
|------|-----------|----------------------|
| **1 — Paperless-ngx / Corpus** | Documents you've ingested via MCP | Full-text searchable, instantly verifiable, auto-populated |
| **2 — Physical / Ebook** | Books you tell the agent you own | Agent asks you to check specific pages for verification |
| **3 — Institutional access** | JSTOR, university library, etc. | Agent asks you to retrieve the text when needed |

If `lit_list_library` is available, the agent catalogs your entire archive in one call. If not, it approximates by querying `lit_search_texts` broadly.

Documents in Paperless-ngx that aren't yet indexed for search will be flagged — the agent can ingest them on the spot via `lit_ingest_pdf`.

## Templates

The `templates/` directory contains starter templates for research entries:

| Template | Use For |
|----------|---------|
| `research-index.md` | Top-level index for all literary research on a project |
| `theme-study.md` | How a theme is handled across multiple works |
| `author-study.md` | Deep dive into one author's craft and relevant works |
| `quotes-collection.md` | Running collection of quotes organized by topic |
| `craft-study.md` | How a specific technique works, with examples from published fiction |
| `theory-study.md` | A theoretical concept, thinker, or school — with quotes, genealogy, and literary connections |
| `annotated-bibliography.md` | Prioritized reading list with annotations |
| `compare-passages.md` | Side-by-side analysis of how different authors handle the same craft problem |
| `reading-path.md` | Sequenced reading list with dependencies, alternate routes, and progress tracking |
| `intertextuality-map.md` | Map of specific textual connections — allusions, epigraphs, rewritings, shared sources |
| `session-log.md` | Record of what was searched, found, and came up empty in a research session |

### Using templates

Point the agent at a template when starting research:

```
"I want to study how published authors handle unreliable narrators.
Use the craft-study template."
```

The agent will research the topic, find real passages, verify what it can, and fill in the template.

## Example Usage

### Research a theme across literature

```
"I'm writing about grief after the death of a parent. Find me the best
passages in published fiction and poetry that handle this — I want to see
how different writers approach it. Real quotes only."
```

### Study an author's craft

```
"I want to learn from how Marilynne Robinson uses landscape and place
in her prose. Pull key passages from Housekeeping and Gilead, and find
what she's said about it in essays or interviews."
```

### Build a quotes collection

```
"I'm working on a novel about immigration. Start a quotes collection
of the best passages about displacement, belonging, and language from
published literature. Organize by subtopic."
```

### Analyze a craft technique

```
"How do published short story writers handle time compression — covering
years in a few paragraphs? Show me examples from Alice Munro, Edward P.
Jones, and anyone else who does this well."
```

### Build a bibliography

```
"I'm writing a novel set during the Partition of India. Build me an
annotated bibliography — fiction, poetry, memoir, and criticism — with
reading priorities. What should I read first?"
```

### Study a theoretical concept

```
"I want to understand Derrida's concept of hauntology and how it shows
up in contemporary fiction. Give me the key passages from Specters of Marx,
then show me novels and poetry that engage with it. Use the theory-study template."
```

### Explore postmodern narrative strategies

```
"Research how postmodern and poststructuralist ideas about the instability
of meaning actually show up in fiction — Barth, Pynchon, Markson, Danielewski.
What are they doing on the page and what theory is behind it?"
```

### Compare how authors handle the same moment

```
"Show me three different ways published novelists open a book with a death.
Put the passages side by side — I want to see the range of choices.
Use the compare-passages template."
```

### Get revision help from published models

```
"Here's a paragraph from my draft where I'm trying to compress ten years
into half a page. Who does this well in published fiction? Show me how
Munro, Edward P. Jones, or others handle the same move — I want to
see what I'm missing."
```

### Research translations

```
"I want to use a passage from Rilke's Duino Elegies in my epigraph.
Show me how different translators handle the opening of the First Elegy —
Mitchell, Poulin, Snow. Which translation should I use?"
```

### Build a reading path

```
"I want to understand magical realism — not just García Márquez but the
full tradition. Build me a reading path: what to read first, what to
read next, and why that order matters."
```

### Map intertextual connections

```
"I'm reading Cunningham's The Hours and want to understand all the ways
it connects to Mrs Dalloway — not just the plot parallels but the
specific textual echoes, structural borrowings, allusions. Use the
intertextuality-map template."
```

### Trace literary connections

```
"My story deals with memory and photography. What's the literary tradition
here? Start with Barthes and Sontag but go wider — fiction, poetry,
anything that connects these two subjects."
```

## Worked Examples

The `examples/` directory contains fully completed templates showing what finished research looks like:

| Example | Demonstrates |
|---------|-------------|
| `craft-study-time-compression.md` | A complete craft study on time compression in short fiction — Munro, Baldwin, Lahiri — with passages, craft analysis, pitfalls, and recommended reading |

Examples use the same confidence markers and citation format as real research. Study them to understand what a complete template should look like.

## Quick Reference

See [QUICK-REFERENCE.md](QUICK-REFERENCE.md) for a one-page cheat sheet of everything the plugin can do, all templates, confidence markers, and tips — designed to be glanced at, not read end-to-end.

## Tips

- **Tell the agent what you're writing.** Context about your project helps it find the most relevant sources.
- **Ask for real quotes.** The agent is built to find and cite actual passages, not summarize.
- **Check confidence levels.** `[verified — corpus match]` (with MCP) or `[verified — original accessed]` (standalone) means confirmed against source text; `[unverified]` means the original text was not accessed.
- **Request specific templates.** If you want a theme study vs. a quotes collection, say so.
- **Name authors you admire.** The agent can find connections between writers you already love and ones you haven't discovered yet.
- **Ask about translations.** When researching works from other languages, the agent will compare translations, name translators, and recommend specific editions.
- **Paste your draft for revision research.** The agent can take your own writing and find published passages that handle the same craft problem — useful during revision.
- **Ask for a reading path, not just a bibliography.** If order matters, request a reading path — it sequences works with dependencies and alternate routes.
- **Request comparisons.** "Show me three ways to open with a death scene" is more useful than "find passages about death." The compare-passages template forces specific observations.
- **Check session logs to pick up where you left off.** The agent generates a session log at the end of substantive research — use it to resume next time.
- **Tell the agent what books you own.** "I have Housekeeping on my shelf" lets the agent prioritize sources you can verify immediately and ask you to check specific pages.
- **Let the agent help you scope.** If your question is too broad ("grief in literature"), the agent will suggest ways to narrow it. If too narrow, it'll offer to widen the search.
- **Look at the worked examples.** The `examples/` directory shows what a completed template should look like — useful for understanding what to expect.
- **Provide PDFs when asked.** When the agent hits a paywall, giving it the downloaded PDF (especially with MCP connected) lets it index the source permanently for all future sessions.

## License

MIT
