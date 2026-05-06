# Getting Started

## Prerequisites

- Claude Code CLI installed
- Docker (for labs with hands-on environments)
- Obsidian desktop app
- Git + GitHub CLI (`gh`)

---

## Step 1 — Install MCPs (one-time)

See [mcp-setup.md](mcp-setup.md) for full instructions.

At minimum, install:
```bash
claude mcp add tavily -- npx -y tavily-mcp@latest
claude mcp add fetch -- uvx mcp-server-fetch
claude mcp add git -- uvx mcp-server-git
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/www/project
```

---

## Step 2 — Create your first topic

```bash
bash ~/www/project/research-workflow/scripts/research new kafka ~/www/project/kafka
```

This will:
- Copy the topic template into the target folder
- Replace `{{TOPIC}}` placeholders with your topic name
- Initialize a git repo
- Print next steps

---

## Step 3 — Open in Obsidian

1. Open Obsidian
2. Click **Open folder as vault**
3. Select the target folder (e.g. `~/www/project/kafka`)
4. Install recommended plugins (see [obsidian-setup.md](obsidian-setup.md))

---

## Step 4 — Start researching

```bash
cd ~/www/project/kafka
claude
```

Inside Claude Code:

```
/plan core-concepts        # Research and create your first note
/note https://some-url     # Save an article as a note
/moc                       # Generate the Map of Contents
/commit                    # Commit and push
```

---

## Folder structure (after bootstrapping)

```
<topic>/
├── 00-index/          ← MOC, progress, questions, glossary
├── 01-inbox/          ← Raw notes, unprocessed
├── docs/              ← Refined concept notes
├── scenarios/         ← Hands-on labs
├── docker/            ← Docker Compose environments
├── examples/          ← Runnable code
├── refs/              ← Papers, books, links
├── assets/            ← Images and diagrams
├── templates/         ← Note templates
├── scratch/           ← Temporary work (gitignored)
├── CLAUDE.md          ← Project memory for Claude
└── README.md
```

---

## Daily session workflow

```
git pull
↓
claude (CLAUDE.md auto-loads)
↓
/plan <subtopic>   or   /note <url>
↓
Review in Obsidian, add personal insights
↓
/commit
```
