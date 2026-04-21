---
name: context-handoffs
description: Context scoping for agent spawns — use when deciding what context a spawned agent should receive, whether ephemeral reasoning should be materialized before handoff, and how much to pass. Poor context handoffs are the #1 cause of wasted agent work.
---

# Context Handoffs

- `/meridian-spawn` covers command mechanics.
- This skill covers scoping judgment.

## Choose Mechanism

- `-f`: default. Folders for structure, specific files for content.
- `--from`: use for reasoning/history not yet materialized.
- Materialize first when context is critical.

```bash
# Good: folder for map, specific files for content
meridian spawn -a coder -p "Implement auth middleware" \
  -f src/middleware/ \
  -f src/middleware/base.py \
  -f <work_dir>/phase-2.md

# Bad: broad dump
meridian spawn -a coder -p "Implement auth middleware" \
  -f <work_dir>/*.md -f src/**/*.py
```

```bash
# Good: reasoning context from prior spawn
meridian spawn -a reviewer --from p203 -p "Review against design intent"

# Materialize critical reasoning before spawn
meridian spawn -a coder -p "Implement event store" -f <work_dir>/approach.md
```

## Scope Tightly

- Pass overview plus task-specific files.
- Typical: 2-4 files.
- Six is heavy. Ten means poor handoff.
- Point to where more context lives instead of attaching everything.
- Prompt should name concrete files/sections and expected behavior.

## Cross-Phase Handoff

```bash
meridian spawn -a coder \
  --from p301 \
  -f src/auth/ \
  -f src/auth/tokens.py \
  -f <work_dir>/phase-2.md \
  -p "Implement token refresh, building on phase 1 token validation"
```

- Use `--from` for predecessor reasoning.
- Use `-f` folders for structure, specific files for content.
