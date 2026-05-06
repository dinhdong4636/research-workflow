---
model: claude-sonnet-4-6
description: Bump version, generate changelog, create git tag, push release
---

# /release — Create a New Release

When the user invokes `/release <version>` (e.g. `/release 1.1.0`), execute:

## Steps

1. **Validate first**
   - Run `/validate` internally
   - If validation fails → stop and report issues before releasing

2. **Determine version**
   - If user provided version → use it
   - If not → read current version from `CHANGELOG.md` (latest tag), suggest next semantic version
   - Ask user to confirm: patch / minor / major?

3. **Generate changelog entry**
   - Run `git log <last-tag>..HEAD --oneline`
   - Group commits by prefix:
     - `feat:` / `docs:` → New
     - `fix:` → Fixed
     - `chore:` / `refactor:` → Changed
   - Format as markdown section:
     ```
     ## v1.1.0 — 2026-05-07
     ### New
     - ...
     ### Fixed
     - ...
     ### Changed
     - ...
     ```
   - Prepend to `CHANGELOG.md`

4. **Commit changelog**
   - `git add CHANGELOG.md`
   - `git commit -m "chore: release v<version>"`

5. **Create git tag**
   - `git tag -a v<version> -m "Release v<version>"`

6. **Push**
   - `git push origin main`
   - `git push origin v<version>`

7. **Create GitHub release** (if GitHub MCP available):
   - Title: `v<version>`
   - Body: changelog entry from step 3
   - Fallback: print `gh release create` command for user to run

## Fallback behavior

- GitHub MCP unavailable → print `gh release create v<version> --notes "..."` for manual run
- No previous tag → treat as first release, include all commits in changelog

## Output example

```
✓ Validated: template ready
✓ Changelog updated: CHANGELOG.md
✓ Committed: chore: release v1.1.0
✓ Tagged: v1.1.0
✓ Pushed: main + tag
✓ GitHub release created: v1.1.0

Release URL: https://github.com/dinhdong4636/research-workflow/releases/tag/v1.1.0
```
