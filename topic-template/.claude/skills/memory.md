---
model: claude-haiku-4-5-20251001
description: Sync knowledge graph with Memory MCP — load context at session start, save state at session end
---

# /memory — Knowledge Graph Sync

Two modes: `load` (session start) and `sync` (session end).
Call with `/memory load` or `/memory sync`. Default if no arg: `sync`.

---

## Mode: load — Session Start

When invoked as `/memory load` or at the beginning of a session:

1. **Query Memory MCP** for all entities in this topic:
   - Query: `"{{TOPIC}} research status"`
   - Pull: concept entities, lab entities, current focus, open questions

2. **Build a compact context summary** — do NOT read all files in docs/:
   ```
   Topic: {{TOPIC}}
   Last session: <date>
   Current focus: <entity last worked on>

   Concepts:
     ✓ done:    [list]
     🔄 wip:    [list]
     📝 draft:  [list]

   Labs:
     ✓ working: [list]
     🔄 wip:    [list]

   Open questions: <count> unanswered
   Next suggested: <entity with status draft or wip>
   ```

3. **Do not re-read files** unless user asks about a specific note.
   Memory MCP is the source of truth for state.

4. **Report** the summary and ask: "Continue from <last focus>?"

---

## Mode: sync — Session End

When invoked as `/memory sync` or `/memory`:

1. **Scan frontmatter** of all `.md` files in `docs/` and `scenarios/*/README.md`

2. **Upsert entities** in Memory MCP for each note:
   ```
   entity: "<title>"
     type: concept | lab
     status: draft | refining | done | wip | working
     difficulty: <value>
     file: <relative path>
     tags: [<tags>]
     last-updated: <date>
   ```

3. **Upsert relations** from frontmatter fields:
   - `related: ["[[X]]"]` → relation: `"<title>" relates_to "X"`
   - `labs: ["[[Y]]"]` → relation: `"<title>" has_lab "Y"`
   - `prerequisites: ["[[Z]]"]` → relation: `"Y" requires "Z"`

4. **Sync open questions** from `00-index/questions.md`:
   - Unanswered → entity with type `question`, status `open`
   - Answered → update status to `answered`, add relation to answer note

5. **Update session entity**:
   ```
   entity: "Session {{DATE}}"
     worked_on: [entities touched this session]
     notes_added: <count>
     notes_completed: <count>
   ```

6. **Report**:
   ```
   ✓ Memory synced: 11 entities updated
   ✓ Relations: 8 mapped
   ✓ Open questions: 3 tracked
   ```

---

## Fallback behavior

- Memory MCP unavailable → write compact summary to `00-index/progress.md` instead
- No entities in Memory MCP yet → run full sync from scratch (first-time setup)
- Partial sync (some files unreadable) → note which files were skipped

---

## Integration with other skills

- `/commit` calls `/memory sync` automatically after committing
- Session start: CLAUDE.md instructs Claude to run `/memory load` before anything else
- `/moc` can trigger `/memory sync` after regenerating the index

---

## Output example (sync)

```
Memory sync — {{TOPIC}}
────────────────────────
Entities upserted: 11
  Concepts: 6 (2 done, 3 draft, 1 refining)
  Labs:     5 (3 working, 2 wip)

Relations mapped: 8
  relates_to: 5
  has_lab:    2
  requires:   1

Open questions tracked: 3
Session entity created: Session 2026-05-07

✓ Memory MCP updated
Next suggested: docs/07-kafka-streams.md (draft)
```

## Output example (load)

```
Loading context from Memory MCP...
────────────────────────────────────
Topic: {{TOPIC}}
Last session: 2026-05-06

Concepts:
  ✓ done:   Core Concepts, Architecture, Partitioning, Consumer Groups
  🔄 wip:   Delivery Guarantees
  📝 draft: Kafka Streams, Performance

Labs:
  ✓ working: Event Streaming, Log Aggregation, Microservices
  🔄 wip:    Real-time Analytics, Fault Tolerance

Open questions: 3 unanswered

Next suggested: Delivery Guarantees (wip — continue from last session)

Continue from Delivery Guarantees? (y/n)
```
