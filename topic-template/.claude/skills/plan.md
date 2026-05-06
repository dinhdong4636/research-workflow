---
model: claude-sonnet-4-6
description: Research topic and create outline — needs web search synthesis and reasoning
---

# /plan — Research & Plan a Subtopic

When the user invokes `/plan <subtopic>`, execute this workflow:

## Steps

1. **Clarify scope** with the user if the subtopic is ambiguous.

2. **Research** (priority order, fallback on failure):
   - Primary: `tavily-search` MCP → search "`<subtopic>` <TOPIC> tutorial overview"
   - Fallback 1: `WebSearch` builtin with the same query
   - Fallback 2: ask the user to provide a URL or a brief description

3. **Build an outline** from research results:
   - 5–7 main sections
   - One-line description per section
   - Note reference sources

4. **Create a placeholder file** in `docs/`:
   - Name: `NN-<subtopic-kebab>.md` (NN = next available number)
   - Add full frontmatter from `templates/concept-note.md`
   - Add outline as headings (`## Section 1`, `## Section 2`...)
   - Set `status: draft`

5. **Update MOC** (`00-index/MOC.md`):
   - Add link `[[NN-subtopic|Subtopic Name]]` under the correct section
   - Create a new section if none fits

6. **Report** to the user:
   - File created: path
   - Outline preview
   - Ask: "Which section would you like me to write first?"

## Fallback behavior

- Tavily rate-limited → switch to WebSearch automatically, no interruption
- WebSearch also fails → ask the user for a manual topic description
- No information found → create file with basic outline and `<!-- TODO: needs more research -->`

## Output example

```
✓ Research complete: 8 sources found
✓ File created: docs/07-kafka-streams.md
✓ MOC updated

Outline:
  1. What is Kafka Streams?
  2. Kafka Streams vs Flink vs Spark
  3. KStream vs KTable
  4. Windowing operations
  5. Joins
  6. Hands-on lab

Which section should I write first?
```
