# CLAUDE.md — Research Workflow

This repo contains the **standard workflow** for researching any tech topic.
Claude Code reads this file automatically when opening the project.

---

## What this repo contains

```
topic-template/     ← Template folder to bootstrap a new topic
scripts/research    ← CLI: research new <topic>
docs/               ← Setup and usage guides
```

## Usage

### Create a new topic
```bash
bash scripts/research new <topic-name> <target-dir>
# Examples:
bash scripts/research new kafka ~/www/project/kafka
bash scripts/research new aws ~/www/project/aws
```

### Available skills after applying to a topic
```
/plan <subtopic>    → Research + generate outline + create placeholder file
/note <url>         → Fetch URL → structured note with frontmatter
/lab <name>         → Scaffold a new lab/scenario
/moc                → Regenerate Map of Contents
/commit             → Smart commit + push
```

## MCPs to install (one-time setup)

See docs/mcp-setup.md for full details.

```bash
# Web search + extract
claude mcp add tavily -- npx -y tavily-mcp@latest

# Obsidian vault access
claude mcp add obsidian -- uvx mcp-obsidian

# Git operations
claude mcp add git -- uvx mcp-server-git

# GitHub integration (requires GITHUB_TOKEN — see docs/mcp-setup.md)

# URL fetching (fallback)
claude mcp add fetch -- uvx mcp-server-fetch

# Cross-session memory
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory

# File access outside cwd
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/www/project
```

## Dev skills (for developing this project)

```
/validate       → check topic-template integrity before release
/release <ver>  → changelog + git tag + GitHub release
/issue          → create GitHub issue (bug / feature / improvement)
/pr             → create pull request with structured description
```

MCPs needed for dev:
- `github` MCP — issues, PRs, releases (see docs/mcp-setup.md)
- `git` MCP — already in standard setup

## Philosophy

- Each topic = one Git repo + one Obsidian vault
- Skills auto-fallback when an MCP is unavailable
- CLAUDE.md inside each topic is its "portable memory"
- Inbox first, refine later — no need to be perfect immediately
