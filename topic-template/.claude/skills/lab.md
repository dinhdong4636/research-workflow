# /lab — Scaffold a New Lab/Scenario

When the user invokes `/lab <name>` (or `/lab` without a name), execute:

## Steps

1. **Get the lab name**:
   - If the user provided a name → normalize to kebab-case
   - If not → ask: "What use case is this lab about?"

2. **Confirm scope** (quick questions):
   - Docker environment needed? (single-broker / multi-broker / full-stack / none)
   - Difficulty: beginner / intermediate / advanced
   - Estimated duration in minutes?

3. **Determine sequence number**:
   - Count existing folders in `scenarios/`
   - Name: `scenarios/NN-<name>/`

4. **Create structure**:
   ```
   scenarios/NN-<name>/
   ├── README.md          ← Main lab note (from templates/lab-note.md)
   └── docker-compose.yml (if Docker env needed)
   ```

5. **Write README.md** with:
   - Full frontmatter from `templates/lab-note.md`
   - Sections: Problem Statement, Prerequisites, Architecture (ASCII), Steps, Verification, Takeaways
   - Docker commands if an env is selected

6. **Create docker-compose.yml** (if needed):
   - Copy from `docker/<env>/docker-compose.yml` if it exists
   - Otherwise create a minimal Kafka skeleton

7. **Update MOC** (`00-index/MOC.md`):
   - Add to the Labs section with link, env, and difficulty

8. **Confirm** with user and ask if they want to write the steps immediately.

## Fallback behavior

- No matching docker/ folder → create a minimal docker-compose.yml skeleton
- User doesn't want Docker → skip docker-compose.yml, add a note in README

## Output example

```
✓ Lab created: scenarios/06-schema-registry/
  ├── README.md
  └── docker-compose.yml (from full-stack env)

Config:
  env: full-stack
  difficulty: intermediate
  duration: 45 min

Would you like me to write the hands-on steps now?
```
