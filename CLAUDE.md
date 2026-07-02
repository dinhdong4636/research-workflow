# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

This is **not an application** — it is a *workflow generator*. It ships a template
(`topic-template/`) and a CLI (`scripts/research`) that bootstrap a self-contained
research/learning environment for any tech topic. Each generated topic becomes its own
Git repo + Obsidian vault, carrying its own `CLAUDE.md`, skills, and structure.

There is no build, no test suite, no package manager. "Developing" here means editing
Bash, Markdown, and skill definitions, then validating template integrity.

## Common commands

```bash
# Generate a new topic environment (copies + templates topic-template/)
bash scripts/research new <topic-name> <target-dir>
bash scripts/research new kafka ~/www/project/kafka

# Validate template integrity before releasing (run the /validate skill)
/validate

# Cut a release (changelog + tag + GitHub release)
/release <version>
```

## Architecture — the two-layer skill system

The single most important thing to understand: **there are two independent sets of
skills**, and they live in different places and serve different audiences.

| Location | Audience | Skills | Purpose |
|---|---|---|---|
| `.claude/skills/` (repo root) | Maintainers of *this* repo | `/validate`, `/release`, `/issue`, `/pr`, `/memory` | Develop and release the workflow itself |
| `topic-template/.claude/skills/` | End users of a *generated* topic | `/plan`, `/note`, `/lab`, `/moc`, `/commit` | Research workflow inside a bootstrapped topic |

When you edit a skill, be deliberate about **which layer** you are touching. The
root-level skills never ship to users; the `topic-template/` skills are copied verbatim
into every new topic and must stay topic-agnostic (see templating below).

## Architecture — the templating mechanism

`scripts/research new` is the whole distribution pipeline. It:

1. `cp -rn topic-template/. <target>/` — copies the template (never overwrites existing files).
2. Runs `sed` over every `*.md` to substitute placeholders: **`{{TOPIC}}`** and **`{{DATE}}`** only.
3. `git init` + `branch -m main` if the target isn't already a repo.
4. Touches `.gitkeep` in known-empty dirs.

**Gotchas that require reading multiple files to catch:**

- Only `{{TOPIC}}` and `{{DATE}}` are substituted by the script. `validate.md` also
  references `{{TITLE}}` as a valid placeholder, but the CLI does **not** substitute it —
  any `{{TITLE}}` must be resolved at skill-runtime, not at generation time.
- Files in `topic-template/**/*.md` must therefore contain **no hardcoded topic names**
  (no "kafka", no real dates) — only placeholders. `/validate` enforces this.

## Architecture — the /validate contract

`.claude/skills/validate.md` hard-codes the expected contents of `topic-template/`:
the required file list, the frontmatter schema for `concept-note.md` / `lab-note.md`,
the set of valid `model:` values, and which dirs must carry `.gitkeep`.

**This is a coupling point.** If you add/remove/rename a file in `topic-template/`,
change a template's frontmatter fields, or introduce a new model, you must update
`validate.md` in the same change or `/validate` will report false failures.

## Conventions

- **Model-per-skill:** every skill declares a `model:` in its frontmatter, chosen by task
  weight — Haiku for mechanical/template work (`/commit`, `/moc`, `/issue`, `/memory`),
  Sonnet for synthesis/reasoning (`/plan`, `/note`, `/lab`, `/release`, `/pr`). Preserve
  this when adding skills; don't default everything to one model.
- **Graceful fallback:** skills assume MCPs (Tavily, Obsidian, Git, GitHub, Fetch, Memory)
  may be absent and degrade to a builtin, then to a manual step, without hard-stopping.
  Any new skill that touches an MCP should follow the same primary → fallback → manual chain.
- **Generated topics use conventional commits** with domain prefixes (`docs:`, `scenario:`,
  `docker:`, `example:`, `fix:`, `chore:`) — defined in `topic-template/CLAUDE.md`.
- **Permissions** live in `.claude/settings.json` (dev) and `topic-template/.claude/settings.json`
  (shipped to topics); both deny `rm -rf`, force-push, and hard-reset.

## Release process

`/release` reads `CHANGELOG.md`, bumps the version, tags, and publishes via GitHub.
Run `/validate` first — a release should never ship a template that fails validation.
