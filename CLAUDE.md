# Lit Research Plugin — Development Guide

## What This Is

A Claude Code plugin that turns Claude into a literary research assistant for writers. It finds verified quotes from published texts, builds citations, analyzes craft and theory, and produces structured research files.

The core principle is **verification-first**: every quote, citation, and theoretical interpretation must be traceable to a real published source. When verification isn't possible, the output says so — loudly.

## Repository Structure

```
.claude-plugin/plugin.json   — Plugin metadata and version
agents/lit-research.md        — The agent system prompt (this IS the plugin)
templates/                    — Markdown templates for research output
examples/                     — Worked examples showing completed templates
QUICK-REFERENCE.md            — One-page cheat sheet for writers
README.md                     — Installation, usage, and documentation
```

## Key Design Decisions

- **The agent file is the product.** `agents/lit-research.md` is the system prompt that defines all behavior. Changes to plugin functionality happen there.
- **Templates are scaffolding, not forms.** They define structure and expectations, but the agent adapts them to the specific research. A filled template should read like a useful document, not a filled-in form.
- **MCP is optional.** The plugin must work fully without literary-verity-mcp. MCP tools add machine verification but are never required.
- **Confidence markers are non-negotiable.** Every quote and interpretation gets a confidence marker. This is the plugin's primary defense against hallucination and cannot be relaxed.

## Contributing

### Editing the agent prompt

The agent prompt in `agents/lit-research.md` follows this structure:

1. **Identity & MCP integration** — Who the agent is, how it uses MCP tools
2. **Core Capabilities** (numbered sections) — What the agent can do
3. **Research Standards** — Non-negotiable verification rules
4. **Hallucination Safeguards** — Failure modes and countermeasures
5. **Workflow** — Step-by-step process for handling requests
6. **Presentation** — How to structure and scope responses
7. **Scope & Boundaries** — What the plugin handles and what it doesn't
8. **Tone** — Voice and manner

When adding a capability, add a numbered section under Core Capabilities. When adding a rule, add it to Research Standards. Don't mix the two — capabilities say what the agent *can do*, standards say what it *must always do*.

### Adding templates

Templates live in `templates/`. Every template should:

- Start with a title, metadata block (date, purpose), and overview section
- Use the standard citation format: `> "[Quote]"\n>\n> — Author, *Title*, Location. Year. **[confidence level]**`
- Include a Sources section at the bottom
- End with a `*See also:*` cross-reference line
- Include `**Accessed at:**` fields for every source excerpt
- Be useful as a blank scaffold AND readable as a finished document

When adding a new template, also:
- Add it to the templates table in `README.md`
- Add a corresponding section to `templates/research-index.md`
- Add an example workflow to the README's Example Usage section

### Adding examples

Worked examples live in `examples/`. They show what a completed template looks like with real literary content. Examples should:

- Use real authors, real titles, and real (or clearly marked as illustrative) quotes
- Follow the same confidence-marking rules as the agent
- Demonstrate the template's structure fully — no empty sections
- Be genuinely useful as reference material, not just structural demos

## Version Bumping

The version lives in `.claude-plugin/plugin.json`. Bump it when:
- **Patch** (1.2.x): Template fixes, typo corrections, minor prompt tweaks
- **Minor** (1.x.0): New capabilities, new templates, significant prompt changes
- **Major** (x.0.0): Breaking changes to template format or fundamental agent behavior

## Testing Changes

There's no automated test suite — the plugin is a prompt, not code. To test changes:

1. Run `claude --agent agents/lit-research.md`
2. Try the workflows described in the README
3. Verify that confidence markers appear on every quote and interpretation
4. Verify that the agent uses templates correctly
5. Check that MCP-dependent features degrade gracefully when MCP is unavailable

## What Not to Do

- Don't add capabilities that bypass verification. The whole point is verified research.
- Don't make templates more complex than the content requires. A writer should be able to glance at a template and understand what goes where.
- Don't add features that require infrastructure beyond Claude Code + optional MCP. The plugin must work with `claude --agent` and nothing else.
- Don't weaken confidence markers or make them optional. They exist to protect the writer from hallucinated research.
