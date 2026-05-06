# Obsidian Setup

## Open a topic as a vault

1. Open Obsidian
2. **Open folder as vault** → select `~/www/project/<topic>`
3. Trust the author if prompted

Each topic is its own vault — the graph view stays clean and focused.

---

## Recommended plugins

Install via: **Settings → Community plugins → Browse**

| Plugin | Why |
|---|---|
| **Dataview** | Query frontmatter into tables inside MOC.md |
| **Templater** | Insert note templates with `/plan` and `/lab` |
| **Git** | Auto-commit + push from inside Obsidian |
| **Local REST API** | Required for `mcp-obsidian` — lets Claude write to vault directly |

---

## Settings to configure

### Files & Links
- **Default location for new notes:** `01-inbox`
- **New link format:** Shortest path when possible
- **Use `[[Wikilinks]]`:** On

### Templates (Templater plugin)
- **Template folder:** `templates`

### Attachments
- **Default attachment folder:** `assets`

---

## Git plugin config (if using Obsidian Git)

- **Auto pull interval:** 10 minutes
- **Auto push on commit:** On
- **Commit message:** `chore: obsidian auto-save {{date}}`

> Note: The `/commit` skill handles git from Claude Code — Obsidian Git is optional for in-app saves.

---

## Graph view tips

- **Filters:** Hide `type: meta` nodes (progress, questions) to see only content
- **Groups:** Color by `type` field (concept = blue, lab = green, moc = yellow)
- **Center node:** Open `00-index/MOC.md` as the starting point

The graph becomes meaningful after ~10 linked notes. Keep writing!
