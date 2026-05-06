---
model: claude-haiku-4-5-20251001
description: Smart git commit and push — mechanical task, no deep reasoning needed
---

# /commit — Smart Git Commit + Push

When the user invokes `/commit` (optionally `/commit <message>`), execute:

## Steps

1. **Check status**:
   - Run `git status` to see changed files
   - If nothing has changed: report "Nothing to commit" and stop

2. **Show summary** to user:
   - New files: list
   - Modified files: list
   - Deleted files: list (warn if any)

3. **Generate commit message**:
   - If the user already provided a message → use it directly
   - Otherwise: analyze changed files and generate by convention:
     ```
     docs: add kafka-streams overview
     scenario: add schema-registry lab
     fix: update frontmatter in 03-partitioning
     chore: update MOC and progress
     ```
   - Show the message and ask the user to confirm

4. **Stage files**:
   - Stage only: `.md`, `.yml`, `.py`, `.sh`, `.json`
   - Never stage: `scratch/`, `*.log`, `node_modules/`, binary files

5. **Commit**:
   - Primary: `git` MCP commit
   - Fallback: `git add` + `git commit` via Bash

6. **Push**:
   - Ask user: "Push to remote?" (default: yes)
   - Primary: git push via MCP
   - Fallback: `git push` via Bash

7. **Auto-run `/moc`** after commit if `docs/` or `scenarios/` files changed.

8. **Auto-run `/memory sync`** after every commit to keep the knowledge graph current.

## Fallback behavior

- Git MCP fails → use Bash git commands (always available)
- No remote configured → notify "No remote set" + suggest `git remote add origin <url>`
- Push rejected (non-fast-forward) → notify user, suggest `git pull` first

## Commit convention

```
docs:     add/edit files in docs/
scenario: add/edit files in scenarios/
docker:   add/edit docker-compose files
example:  add/edit files in examples/
fix:      correct content errors (typo, wrong info)
chore:    update MOC, progress, frontmatter
init:     initialize a new topic
```

## Output example

```
Changes:
  + docs/07-kafka-streams.md (new)
  ~ 00-index/MOC.md (modified)
  ~ 00-index/progress.md (modified)

Commit message: "docs: add kafka-streams overview placeholder"
Confirm? (y/edit/n): y

✓ Committed: abc1234
✓ Pushed to origin/main
```
