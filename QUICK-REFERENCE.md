# Lit Research Plugin — Quick Reference

## What You Can Ask For

| You want to... | Say something like... | Template |
|---|---|---|
| Find sources on a topic | "Find published fiction about [topic]" | — |
| Get real quotes | "Pull verified passages about [theme] from published literature" | `quotes-collection` |
| Study a theme across authors | "How do different authors handle [theme]? Show me passages." | `theme-study` |
| Deep-dive one author | "What can I learn from [Author]'s craft? Pull key passages." | `author-study` |
| Analyze a technique | "How do published writers handle [technique]? Show examples." | `craft-study` |
| Compare passages side by side | "Show me 3 ways different authors [do X]. Put them side by side." | `compare-passages` |
| Get revision help | "[paste your draft] — Who does this better? Show me published models." | `compare-passages` |
| Study theory | "Explain [concept] with passages from the original text." | `theory-study` |
| Map textual connections | "How does [Work A] connect to [Work B]? Show the specific echoes." | `intertextuality-map` |
| Build a reading list | "Build an annotated bibliography for [topic]." | `annotated-bibliography` |
| Get a reading order | "What should I read first to understand [tradition]? Sequence it." | `reading-path` |
| Compare translations | "Show me how different translators handle [passage]." | — |

## Confidence Markers

Every quote and interpretation comes with a confidence tag. Here's what they mean:

| Marker | What it means | Trust level |
|---|---|---|
| `[verified — corpus match]` | Found in indexed corpus (MCP) | Highest |
| `[verified — original accessed]` | Read in a digital archive this session | High |
| `[verified — bibliographic]` | Source confirmed real via catalog (MCP) | High |
| `[verified — secondary]` | Found in a scholarly secondary source | Good — check original |
| `[high confidence]` | Believed accurate, not verified this session | Moderate — verify before citing |
| `[paraphrase]` | Idea is right, wording may not be exact | Use with caution |
| `[unverified]` | Original text not accessed | Low — verify before using |
| `[⚠ FROM MEMORY]` | Produced from model memory, not a source | Treat as unverified |
| `[⚠ UNCORROBORATED]` | No source, no text — model memory only | Do not cite |

**Rule of thumb:** If it doesn't say "verified," verify it yourself before using it in your work.

## Templates

Request a specific template by name:

```
"Research [topic]. Use the theme-study template."
```

| Template | Best for |
|---|---|
| `research-index` | Master index for all research on a project |
| `theme-study` | One theme, multiple authors |
| `author-study` | One author, deep dive |
| `craft-study` | One technique, multiple examples |
| `compare-passages` | Side-by-side passage analysis |
| `theory-study` | Theoretical concept with source passages |
| `quotes-collection` | Running quote bank organized by topic |
| `annotated-bibliography` | Prioritized reading list |
| `reading-path` | Sequenced reading order with dependencies |
| `intertextuality-map` | Textual connections between works |
| `session-log` | Record of a research session |

## Tips

- **Name your project.** "I'm writing a novel about X" helps the agent prioritize.
- **Ask for real quotes.** The agent is built to find and cite actual passages.
- **Paste your draft.** For revision research, give the agent your own text.
- **Request comparisons.** "Show me 3 approaches" beats "find passages about X."
- **Ask about translations.** The agent will compare editions and recommend translators.
- **Check session logs.** They record what was found, what came up empty, and what's still open.
- **Tell the agent what you own.** "I have these books on my shelf" lets it prioritize sources you can verify immediately.

## Starting a New Project

```
"I'm starting research for [project description]. Create a research index
and help me figure out where to begin."
```

The agent will create a `research-index.md` and suggest first research directions.

## Picking Up Where You Left Off

```
"Read my latest session log and continue from where we stopped."
```
