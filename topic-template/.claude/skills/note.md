# /note — Fetch a URL and Create a Note

When the user invokes `/note <url>` (or `/note` without a URL), execute:

## Steps

1. **Get the URL**:
   - If the user provided a URL → use it directly
   - If not → ask: "Which URL would you like to save as a note?"

2. **Fetch content** (fallback chain):
   - Primary: `tavily-extract` with the URL → returns clean markdown
   - Fallback 1: `mcp-server-fetch` → HTML → markdown
   - Fallback 2: `WebFetch` builtin
   - Fallback 3: notify the user "Cannot fetch — please paste the content here"

3. **Summarize and structure**:
   - Extract: title, author, date (if available)
   - Summarize key points (3–5 bullets)
   - Preserve important sections (quotes, code blocks, diagrams)
   - Strip nav, footer, ads, cookie banners

4. **Create note** with standard frontmatter:
   - Save to `01-inbox/YYYY-MM-DD-<slug>.md`
   - Use `templates/concept-note.md` as the base
   - Auto-generate tags from content
   - Set `sources` field to the original URL

5. **Write file** (fallback chain):
   - Primary: `mcp-obsidian` → `append_content` or create
   - Fallback: `Write` tool directly to the file path

6. **Confirm** with user:
   - Show frontmatter preview
   - Ask: "Move to docs/ now or keep in inbox?"
   - If moving to docs/: rename to `NN-<name>.md`, set `status: refining`

## Fallback behavior

- Tavily limit/down → fetch MCP → WebFetch → user paste (announce which fallback is active)
- Obsidian MCP down → Write tool directly (note: "Obsidian MCP unavailable, writing file directly")
- URL unreachable (paywall, JS-heavy) → notify user, offer to accept pasted excerpt

## Output example

```
✓ Fetched: https://martin.kleppmann.com/2015/05/27/logs-for-data-infrastructure.html
✓ Note created: 01-inbox/2026-05-06-logs-data-infrastructure.md

Preview:
  title: "Using logs for data infrastructure"
  author: Martin Kleppmann
  tags: [kafka/internals, distributed-systems]
  status: draft

Move to docs/? (y/n)
```
