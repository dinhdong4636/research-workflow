---
model: claude-sonnet-4-6
description: Create a pull request with structured description from current branch changes
---

# /pr — Create a Pull Request

When the user invokes `/pr` (or `/pr <title>`), execute:

## Steps

1. **Check current branch**
   - If on `main` → warn: "You're on main. Create a feature branch first."
   - If on feature branch → proceed

2. **Analyze changes**
   - `git log main..HEAD --oneline` — list commits
   - `git diff main...HEAD --stat` — files changed
   - Identify: which skills changed? which docs? template structure?

3. **Generate PR description**:
   ```
   ## Summary
   <what this PR does in 2-3 bullets>

   ## Changes
   - Skills: ...
   - Template: ...
   - Docs: ...

   ## Testing
   - [ ] /validate passes
   - [ ] Tested on a topic repo (specify which)
   - [ ] Fallback behavior verified

   ## Notes
   <anything reviewers should know>
   ```

4. **Create PR** (GitHub MCP preferred):
   - Base: `main`
   - Title from user or generated from commit summary
   - Fallback: print `gh pr create` command

5. **Confirm** with PR URL.

## Fallback

- GitHub MCP unavailable → generate full `gh pr create` command with body
- No remote → advise setting up remote first

## Output example

```
✓ PR created: #8 — feat: add /deep-research skill with agent support

URL: https://github.com/dinhdong4636/research-workflow/pull/8
Base: main ← feature/deep-research
Files: 3 changed
```
