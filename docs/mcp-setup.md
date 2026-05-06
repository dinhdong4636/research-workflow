# MCP Setup

Each MCP is installed once globally. Skills will auto-fallback if an MCP is unavailable.

---

## Must-have (install first)

### 1. Tavily — Web search + extract
Best-in-class research tool. Free tier available at [tavily.com](https://tavily.com).

```bash
# Get a free API key at tavily.com, then:
export TAVILY_API_KEY=tvly-xxxxxxxxxxxx

claude mcp add tavily -- npx -y tavily-mcp@latest
```

**Fallback:** Claude's built-in `WebSearch` + `WebFetch` tools.

---

### 2. Fetch — URL to clean markdown
Official MCP for fetching web pages without JS rendering issues.

```bash
# Requires: pip install uv (or uvx)
claude mcp add fetch -- uvx mcp-server-fetch
```

**Fallback:** `WebFetch` builtin.

---

### 3. Git — Structured git operations
```bash
claude mcp add git -- uvx mcp-server-git
```

**Fallback:** `git` commands via Bash (always available).

---

### 4. Filesystem — File access outside cwd
Allows Claude to read templates and other repos outside the current project folder.

```bash
# Replace ~/www/project with your root projects directory
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/www/project
```

**Fallback:** Claude's built-in `Read`/`Write` tools (work within cwd).

---

## Nice-to-have (install when needed)

### 5. Obsidian — Direct vault access
Requires the **Local REST API** community plugin in Obsidian.

```bash
# Step 1: In Obsidian → Settings → Community plugins → Browse → "Local REST API" → Install + Enable
# Step 2: Copy the API key from plugin settings
export OBSIDIAN_API_KEY=your-api-key-here

claude mcp add obsidian -- uvx mcp-obsidian
```

**Fallback:** `Write`/`Read` tools directly (Obsidian reads the same files).

---

### 6. GitHub — Issues, PRs, repo management
```bash
# Create a token at github.com/settings/tokens
export GITHUB_TOKEN=ghp_xxxxxxxxxxxx

claude mcp add github -- docker run -i --rm \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=$GITHUB_TOKEN \
  ghcr.io/github/github-mcp-server
```

**Fallback:** `gh` CLI via Bash.

---

### 7. Memory — Cross-session knowledge graph
Persists entities and relationships across Claude Code sessions.

```bash
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory
```

**Fallback:** Claude's built-in auto-memory system (`~/.claude/projects/*/memory/`).

---

## Verify installation

```bash
claude mcp list
```

Expected output:
```
tavily      ✓ connected
fetch       ✓ connected
git         ✓ connected
filesystem  ✓ connected
```
