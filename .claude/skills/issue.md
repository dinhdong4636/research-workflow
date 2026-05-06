---
model: claude-haiku-4-5-20251001
description: Create a GitHub issue for a bug, feature request, or improvement
---

# /issue — Create a GitHub Issue

When the user invokes `/issue` (or `/issue <title>`), execute:

## Steps

1. **Gather info** (ask if not provided):
   - Title: one-line summary
   - Type: `bug` / `feature` / `improvement` / `question`
   - Description: what's the problem or idea?
   - Affected area: which skill, which file, which doc?

2. **Format issue body**:
   ```
   ## Summary
   <one paragraph>

   ## Details
   <what, why, expected behavior>

   ## Affected
   - File/skill: ...
   - Workflow step: ...

   ## Proposed solution (if any)
   ...
   ```

3. **Create issue** (GitHub MCP preferred):
   - Labels: auto-assign based on type (`bug`, `enhancement`, `docs`)
   - Fallback: print `gh issue create` command for user to run manually

4. **Confirm** with issue URL.

## Fallback

- GitHub MCP unavailable → generate `gh issue create --title "..." --body "..." --label "..."` command
- No remote set → save issue to `scratch/issues/YYYY-MM-DD-<slug>.md` locally

## Output example

```
✓ Issue created: #12 — /plan skill: fallback not triggered when Tavily 429

URL: https://github.com/dinhdong4636/research-workflow/issues/12
Labels: bug
```
