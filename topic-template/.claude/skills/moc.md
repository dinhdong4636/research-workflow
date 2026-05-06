---
model: claude-haiku-4-5-20251001
description: Scan vault files and regenerate MOC — structured, repetitive task
---

# /moc — Regenerate Map of Contents

When the user invokes `/moc`, execute:

## Steps

1. **Scan the vault**:
   - Read all `.md` files in `docs/` and `scenarios/*/README.md`
   - Parse frontmatter: `title`, `type`, `status`, `difficulty`, `tags`, `aliases`

2. **Classify**:
   - `type: concept` → group under Concepts
   - `type: lab` → group under Labs
   - Others → group under Other

3. **Sort**:
   - Concepts: by filename (NN- prefix handles order naturally)
   - Labs: by difficulty (beginner → advanced), then by name

4. **Generate MOC content**:
   - Each entry: `- [[filename|title]] — one-line description from frontmatter or first heading`
   - Preserve existing custom sections (do not delete them)
   - Append a Dataview query block at the bottom if Dataview plugin is detected

5. **Write** `00-index/MOC.md`:
   - Primary: `mcp-obsidian patch_content` to update specific sections
   - Fallback: `Write` tool to rewrite the entire file

6. **Also update** `00-index/progress.md`:
   - Completion % = notes with `status: done` / total notes
   - List notes still at `status: draft` or `wip`

7. **Show a diff** summary to the user: new entries added, entries updated.

## Fallback behavior

- Obsidian MCP unavailable → Write tool rewrites the entire file
- File has no frontmatter → use filename as title, skip tags
- MOC.md does not exist → create from skeleton template

## Output example

```
✓ Scanned: 11 files (6 concepts, 5 labs)
✓ MOC.md updated
✓ progress.md: 45% complete (5/11 notes done)

Changes:
  + Added: [[07-kafka-streams|Kafka Streams]]
  ~ Updated: [[04-consumer-groups]] (status: done)

Notes still in progress:
  - docs/07-kafka-streams.md (draft)
  - scenarios/06-schema-registry/README.md (wip)
```
