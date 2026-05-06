---
model: claude-haiku-4-5-20251001
description: Validate topic-template integrity — check all required files, placeholders, and structure
---

# /validate — Validate Template Integrity

When the user invokes `/validate`, check the health of `topic-template/`.

## Checks to run

### 1. Required files present
Verify these files exist in `topic-template/`:
- `.claude/skills/plan.md`
- `.claude/skills/note.md`
- `.claude/skills/lab.md`
- `.claude/skills/moc.md`
- `.claude/skills/commit.md`
- `.claude/settings.json`
- `00-index/MOC.md`
- `00-index/progress.md`
- `00-index/questions.md`
- `00-index/glossary.md`
- `templates/concept-note.md`
- `templates/lab-note.md`
- `refs/papers.md`
- `refs/books.md`
- `refs/links.md`
- `CLAUDE.md`
- `.gitignore`

### 2. Placeholders intact
Each skill file must NOT contain hardcoded topic names — only `{{TOPIC}}`, `{{DATE}}`, `{{TITLE}}` placeholders.
Scan for accidental hardcoded values like "kafka", "aws", specific dates.

### 3. Model frontmatter present
Each skill file must have a valid `model:` field in frontmatter.
Valid values: `claude-haiku-4-5-20251001`, `claude-sonnet-4-6`, `claude-opus-4-7`

### 4. Frontmatter schema consistent
`templates/concept-note.md` must have: title, aliases, type, tags, status, difficulty, related, labs, sources, created, updated
`templates/lab-note.md` must have: title, aliases, type, tags, status, difficulty, duration, env, prerequisites, concepts, last-tested

### 5. Permissions structure valid
`.claude/settings.json` must have `allow` and `deny` arrays with at least:
- `Bash(git *)` in allow
- `Bash(rm -rf *)` in deny

### 6. Empty dirs have .gitkeep
Check: `01-inbox/`, `docs/`, `scenarios/`, `docker/`, `examples/`, `assets/`, `scratch/`

## Report format

```
Template Validation Report
──────────────────────────
✓ Required files: 17/17
✓ Placeholders: clean (no hardcoded topics)
✓ Model frontmatter: 5/5 skills
✓ Frontmatter schema: concept ✓, lab ✓
✓ Permissions: allow 14, deny 3
✓ .gitkeep: 7/7 empty dirs

Status: READY ✓

Issues found: 0
```

If issues found, list each with file path and what needs fixing.
