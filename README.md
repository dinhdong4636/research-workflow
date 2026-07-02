# Research Workflow

> A standard workflow for researching any tech topic — powered by Claude Code, Obsidian, and Git.

## Features

- **Topic template** — standard folder structure, Obsidian-ready from day one
- **5 slash commands** — `/plan`, `/note`, `/lab`, `/moc`, `/commit`
- **Web reader** — `reader.html` renders all notes into one browsable page, no build step
- **Graceful fallback** — MCP unavailable? The workflow still runs
- **Multi-topic** — Kafka, AWS, DSA, System Design... each gets its own repo

## Quick Start

```bash
# 1. Clone this repo
git clone https://github.com/dinhdong4636/research-workflow ~/www/project/research-workflow

# 2. Install MCPs (one-time setup)
# See docs/mcp-setup.md

# 3. Create a new topic
bash ~/www/project/research-workflow/scripts/research new kafka ~/www/project/kafka

# 4. Open Obsidian → Open folder as vault → point to ~/www/project/kafka
```

## Slash Commands

| Command | What it does |
|---|---|
| `/plan <subtopic>` | Web search → outline → placeholder file |
| `/note <url>` | Fetch URL → structured note with frontmatter |
| `/lab <name>` | Scaffold a lab/scenario folder |
| `/moc` | Regenerate Map of Contents |
| `/commit` | Smart git commit + push |

## MCPs Used

| MCP | Purpose | Fallback |
|---|---|---|
| Tavily | Web search + extract | WebSearch builtin |
| mcp-obsidian | Read/write vault | Write tool directly |
| mcp-server-git | Git operations | git via Bash |
| github MCP | Issues, PRs | gh CLI |
| mcp-server-fetch | URL → markdown | WebFetch builtin |
| mcp-server-memory | Cross-session memory | Auto-memory builtin |

## Demo

Kafka topic: [github.com/dinhdong4636/kafka-learning-lab](https://github.com/dinhdong4636/kafka-learning-lab)

## Docs

- [Getting Started](docs/getting-started.md)
- [MCP Setup](docs/mcp-setup.md)
- [Obsidian Setup](docs/obsidian-setup.md)
