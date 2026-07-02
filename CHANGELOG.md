# Changelog

## Unreleased

### New
- `reader.html` — zero-build, dependency-free HTML page that renders all `.md` notes
  into one browsable site (client-side `marked.js`, auto-discovers files via directory
  listing, resolves Obsidian `[[wikilinks]]`, strips frontmatter). Shipped in both the
  repo root (`docs/`) and `topic-template/` (every generated topic).
- `/validate` now checks `reader.html` is present in the template.

## v1.0.0 — 2026-05-07

### New
- `topic-template/` — standard folder structure for any research topic
- 5 user skills: `/plan`, `/note`, `/lab`, `/moc`, `/commit`
- 4 dev skills: `/validate`, `/release`, `/issue`, `/pr`
- `scripts/research` CLI for bootstrapping new topics
- Frontmatter schema: `concept-note.md`, `lab-note.md`
- Graceful fallback system for all MCPs
- Model selection per skill (Haiku / Sonnet)
- `docs/`: getting-started, mcp-setup, obsidian-setup, workflow-overview
