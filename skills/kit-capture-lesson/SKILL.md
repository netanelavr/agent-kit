---
name: kit-capture-lesson
description: Turn a correction into a short written rule so it does not repeat. Use after steering, a repeated bug class, or “remember this”. Not domain glossary/ADRs (kit-challenge-design) and not shrinking a path (kit-minimize).
disable-model-invocation: true
---

# Capture Lesson

Write the smallest rule that would have blocked this mistake. Apply only after explicit approval.

## What this is NOT

- Not domain terms or hard-to-reverse architecture (`kit-challenge-design` → CONTEXT / ADR)
- Not choosing a thinner implementation (`kit-minimize`)
- Not a kit catalog audit (`kit-audit-skills`)
- Not a dump into CLAUDE.md / AGENTS.md / a new mega-skill

## Flow

1. **Failure** - one sentence: what the agent did or missed.
2. **Rule** - 1-5 lines that would have blocked it. Concrete trigger + action. No essays.
3. **Surface** - pick one:

| Put it here | When |
|---|---|
| Existing skill | The miss belongs to a paced workflow already in the catalog |
| Project `rules/` | It must fire without a slash (style, git, safety, loop gates) |
| New skill | Rare: a full opt-in flow that is not an always-on habit |
| User-global memory | Only if they ask for a personal habit across repos |

Prefer patching an existing skill or rule over adding a name.

4. Show the **exact patch**. Stop. Wait for yes.
5. After yes: smallest edit. If you add or rename a skill/rule, update `README.md` catalogs and run `kit-audit-skills`.

## Constraints

- Generic: no employer names, private hosts, or work-only tools (`AGENTS.md`)
- Do not duplicate `kit-plan`, `kit-prove`, `kit-minimize`, or `kit-delegate`
- Do not auto-apply. A proposed lesson that they reject is done.
