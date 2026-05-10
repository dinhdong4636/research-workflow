---
model: claude-haiku-4-5-20251001
description: Manage Memory MCP for the research-workflow project — track dev state, decisions, and roadmap
---

# /memory — Project Memory Management

Manages the knowledge graph for the **research-workflow project itself** — not for research topics.
Topic repos use the Memory MCP directly without a skill.

Two modes: `/memory load` (session start) and `/memory sync` (session end).

---

## Mode: load — Session Start

Query Memory MCP for the current development state of this project:

1. Pull entities:
   - Skills: done / in-progress / planned
   - Known bugs / open issues
   - Last worked-on area
   - Pending decisions

2. Return a compact summary:
   ```
   research-workflow — Dev State
   ──────────────────────────────
   Last session: <date> — worked on: <area>

   Skills:
     ✓ done:        /plan /note /lab /moc /commit /validate /release /issue /pr /memory
     📝 planned:    /deep-research (agent-based)

   Open issues: <count>
   Pending decisions: <list>

   Continue from: <last focus>?
   ```

3. Do NOT read source files unless needed — memory is the source of truth for dev state.

---

## Mode: sync — Session End

Save current development state to Memory MCP:

1. **Upsert skill entities** — one per skill file:
   ```
   entity: "/plan skill"
     location: topic-template
     model: sonnet
     status: done
     last-modified: <date>
   ```

2. **Upsert feature entities** — from CHANGELOG.md and open issues:
   ```
   entity: "agent-based /plan"
     type: feature
     status: planned
     priority: next
   ```

3. **Track decisions** — architectural choices made this session:
   ```
   entity: "memory skill placement"
     decision: moved from topic-template to dev skills
     reason: topic-template uses Memory MCP globally, no skill needed
     date: <date>
   ```

4. **Update root project entity**:
   ```
   entity: "research-workflow"
     version: 1.0.0
     skills_count: 10 (6 user + 4 dev)
     last_session: <date>
     next_priority: <what to work on next>
   ```

5. Report:
   ```
   ✓ Memory synced — research-workflow
     Entities updated: 14
     Decisions recorded: 1
     Next priority: agent-based /plan skill
   ```

---

## Fallback

- Memory MCP unavailable → append session summary to `scratch/session-log.md`
- First run → build from scratch by reading CHANGELOG.md + skill files

---

## What this tracks

| Tracked in memory | Not tracked (read directly) |
|---|---|
| Skill status (done/planned) | Skill file content |
| Architectural decisions | Code implementation details |
| Dev session continuity | Git history → use `git log` |
| Feature roadmap state | Full issue list → use `/issue` + GitHub MCP |
