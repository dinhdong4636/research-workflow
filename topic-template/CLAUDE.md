# CLAUDE.md — {{TOPIC}}

This file is automatically read by Claude Code when opening this project.
It is the portable memory of this research topic — works on any machine, any account.

---

## What this project is

A **research + hands-on lab** environment for **{{TOPIC}}**.
Not production code — learning and exploration only.

**Philosophy:**
- Explain **why** before **how**
- ASCII diagrams for architecture
- Every command must run immediately via Docker
- Theory and practice in the same document

---

## Current status

```
00-index/
  MOC.md             — Map of Contents
  progress.md        — Completion tracking
  questions.md       — Open questions
  glossary.md        — Key terms

docs/               — Concept notes (add as you learn)

scenarios/          — Hands-on labs by use case

docker/             — Docker Compose environments

examples/           — Runnable code samples

refs/               — Papers, books, links
```

---

## Workflow

### Start a new session

```bash
git pull
claude
```

Memory is handled automatically by the Memory MCP installed globally.
No manual skill needed — Claude queries it when context is required.

### Add new content

```
New concept note  → /plan <subtopic>
Save a URL        → /note <url>
New lab           → /lab <name>
Update index      → /moc
Commit & push     → /commit
```

### End a session

```bash
/commit
```

---

## Available skills

| Skill | What it does |
|---|---|
| `/plan <subtopic>` | Research + outline + placeholder file |
| `/note <url>` | Fetch URL → note with frontmatter |
| `/lab <name>` | Scaffold lab/scenario folder |
| `/moc` | Regenerate Map of Contents |
| `/commit` | Smart git commit + push |

---

## Commit convention

```
docs:     add/edit concept notes
scenario: add/edit lab files
docker:   add/edit docker-compose
example:  add/edit code samples
fix:      correct errors (typo, wrong info)
chore:    update MOC, progress, frontmatter
```
