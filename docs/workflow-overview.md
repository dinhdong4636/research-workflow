# Research Workflow — Overview

## The Idea

Most research ends up scattered — browser bookmarks, random notes, half-finished summaries across different apps. Nothing connects. Nothing is searchable. Six months later, you can't remember what you learned.

This workflow solves that by treating **research as a living knowledge base** instead of a pile of notes.

Every topic gets its own structured repository. Claude Code handles the mechanical work (fetching, formatting, indexing). You handle the thinking. Obsidian makes everything navigable. Git makes it permanent and portable.

```
You think → Claude structures → Obsidian visualizes → Git preserves
```

---

## How It Works

### The three-layer model

```
┌─────────────────────────────────────────────────────┐
│  Layer 1 — Input                                    │
│  URLs, ideas, questions, raw notes                  │
│                    ↓                                │
│  Layer 2 — Processing (Claude Code + Skills)        │
│  Fetch → Summarize → Structure → Link → Index      │
│                    ↓                                │
│  Layer 3 — Output                                   │
│  Obsidian vault  ←→  Git repository                 │
└─────────────────────────────────────────────────────┘
```

### What lives where

```
01-inbox/        Raw input — dump anything here first
docs/            Refined concept notes — the knowledge base
scenarios/       Hands-on labs — practice tied to theory
00-index/        Navigation layer — MOC, progress, glossary
refs/            Source tracking — papers, books, links
```

### The note lifecycle

```
URL or idea
    ↓
01-inbox/  (raw, unprocessed)
    ↓  /note or manual
docs/      (structured, frontmatter, linked)
    ↓  status: draft → refining → done
Knowledge graph (Obsidian graph view)
```

---

## The Five Skills

Each skill is a slash command that instructs Claude to run a specific workflow step. They are composable — use them in any order.

### `/plan <subtopic>`
**Purpose:** Start researching something new.

Claude searches the web from multiple angles, synthesizes an outline, and creates a structured placeholder file in `docs/`. The MOC is updated automatically.

Use this when: you want to learn a new concept and need a starting structure.

```
/plan kafka-streams
→ searches web
→ creates docs/07-kafka-streams.md with outline
→ updates MOC
```

### `/note <url>`
**Purpose:** Save an article, blog post, or documentation page as a structured note.

Claude fetches the page, strips noise, summarizes key points, adds frontmatter, and saves to `01-inbox/`. You decide whether to promote it to `docs/`.

Use this when: you find something worth keeping while browsing.

```
/note https://martin.kleppmann.com/2015/05/27/logs-for-data-infrastructure.html
→ fetches content
→ creates 01-inbox/2026-05-07-logs-data-infrastructure.md
→ asks: move to docs/?
```

### `/lab <name>`
**Purpose:** Scaffold a hands-on lab tied to a concept.

Claude creates a `scenarios/` folder with a structured README (problem statement, architecture diagram, step-by-step instructions) and an optional Docker Compose file.

Use this when: you want to practice what you just learned.

```
/lab schema-registry
→ creates scenarios/06-schema-registry/README.md
→ creates docker-compose.yml (if needed)
→ updates MOC under Labs section
```

### `/moc`
**Purpose:** Regenerate the Map of Contents.

Claude scans all notes in `docs/` and `scenarios/`, reads their frontmatter, and rebuilds `00-index/MOC.md` with proper Obsidian `[[links]]`. Also updates `progress.md` with completion percentage.

Use this when: after adding new notes, or at the end of a session.

```
/moc
→ scans 11 files
→ updates 00-index/MOC.md
→ updates 00-index/progress.md (45% done)
```

### `/commit`
**Purpose:** Commit and push everything cleanly.

Claude checks git status, shows what changed, generates a conventional commit message, stages only relevant files (never scratch/ or logs), and pushes.

Use this when: end of session, or after completing a note.

```
/commit
→ shows diff summary
→ generates: "docs: add kafka-streams outline"
→ confirms → commits → pushes
```

---

## Fallback System

Every skill degrades gracefully when an external service is unavailable.

```
┌──────────────────┬──────────────────┬──────────────────┐
│ Primary          │ Fallback 1       │ Fallback 2       │
├──────────────────┼──────────────────┼──────────────────┤
│ Tavily search    │ WebSearch builtin│ User describes   │
│ Tavily extract   │ Fetch MCP        │ WebFetch builtin │
│ Obsidian MCP     │ Write tool       │ —                │
│ Git MCP          │ git via Bash     │ —                │
│ GitHub MCP       │ gh CLI           │ —                │
│ Memory MCP       │ Auto-memory      │ progress.md      │
└──────────────────┴──────────────────┴──────────────────┘
```

The workflow never hard-stops. If Tavily hits its rate limit, Claude switches to WebSearch silently and continues. You only get notified if all options fail.

---

## Model Strategy

Different tasks require different reasoning levels. Using a capable model for everything wastes tokens and slows you down.

```
/commit  → Haiku   — read git diff, generate message (mechanical)
/moc     → Haiku   — scan files, format links (repetitive)
/note    → Sonnet  — summarize and understand content
/lab     → Sonnet  — structured writing with reasoning
/plan    → Sonnet  — synthesize research from multiple sources
```

Upgrade `/plan` to Opus when the topic requires deep cross-domain reasoning or brainstorming. Keep Haiku for anything that is pattern-matching or template-filling.

---

## What Makes This Different

### 1. Inbox-first, refine-later
Nothing needs to be perfect immediately. Dump a URL into `01-inbox/`, process it when you have time. The structure guides without blocking.

### 2. Bidirectional linking
Every concept note links to its labs. Every lab links back to the concepts it requires. Obsidian's graph view makes these connections visual.

```
[[03-partitioning]] ←→ [[scenarios/02-log-aggregation/README]]
```

### 3. Frontmatter as metadata
Every note carries structured metadata — status, difficulty, related notes, sources. This powers Obsidian's Dataview plugin to generate live tables and dashboards without manual updates.

```yaml
status: done
difficulty: 2
related: ["[[02-architecture]]"]
labs: ["[[scenarios/03-fault-tolerance/README]]"]
```

### 4. Portable memory
`CLAUDE.md` in each topic folder is the project's memory. It tells Claude exactly what this project is, what workflow to follow, and what conventions to use — on any machine, any account, any session.

### 5. Multi-topic, one system
Each topic is an independent repo with its own vault. The same workflow, same skills, same structure — whether it's Kafka, AWS, algorithms, or system design.

```
~/www/project/
├── kafka/         ← vault 1
├── aws/           ← vault 2
├── algorithms/    ← vault 3
└── system-design/ ← vault 4
```

---

## How to Use It for Research

### Starting a new topic

```bash
bash ~/www/project/research-workflow/scripts/research new <topic> <target-dir>
cd <target-dir>
claude
```

First commands inside Claude Code:
```
/plan <first-concept>   # get oriented, create first note
/moc                    # initialize the index
/commit                 # save the starting point
```

### During a research session

```
Read something interesting?     → /note <url>
Want to go deeper on a concept? → /plan <subtopic>
Ready to practice?              → /lab <name>
End of session?                 → /moc → /commit
```

### Building depth over time

```
Week 1:  /plan core concepts × 5         → 5 draft notes
Week 2:  /note <urls> × 10              → inbox → promote to docs
Week 3:  /lab <scenarios> × 3           → hands-on practice
Week 4:  edit notes directly in Obsidian → add personal insights
         /moc                           → graph connects everything
```

### Reading in Obsidian

- Open `00-index/MOC.md` as the entry point
- Use graph view to explore connections
- Filter by `status: done` to review what's complete
- Filter by `status: draft` to see what needs work
- Use `questions.md` to log things you don't understand yet

---

## Roadmap

### Current (v1) — Skill-based
All steps are sequential, controlled by the user. Good for learning the workflow and validating output quality.

### Next (v2) — Agent-based `/plan`
`/plan` spawns parallel research agents (one per angle), synthesizes results. Faster and broader research coverage.

### Future (v3) — Deep research mode
New `/deep-research` skill using Opus as orchestrator. Spawns agents for multiple sources, cross-references, identifies gaps, generates a comprehensive report automatically.
