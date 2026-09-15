---
name: kit-learn-repo
description: Learn an unfamiliar codebase — purpose, layout, how to run/test, safe change points. Use when joining a repo, switching projects, or before the first change in a new area.
disable-model-invocation: true
---

# Learn Repo

Build a practical mental model fast — not an encyclopedia.

## What this is NOT

- Not a full design challenge (`kit-challenge-design`)
- Not implementing yet (`kit-plan` / `kit-minimize` if you still need a path)

## Flow

1. **Basics** — README / docs: what it does, runtime, package manager, how to run and test locally.
2. **Layout** — entry points (apps, services, CLIs), major domains, one vertical slice (one API or user flow).
3. **Tooling** — relevant MCP/extensions for this repo (names only; configure secrets outside chat).
4. **Recent signal** — recent commits/PRs in the area you’ll touch (direction, not ownership gospel).
5. **Warnings** — fragile zones: generated code, migrations, hidden coupling, custom deploy assumptions.

## Output

Short brief:

- What it does
- How it’s laid out
- How to run / test
- Where to change safely for the user’s ask
- Warnings + open questions + what to read next
