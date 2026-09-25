---
name: kit-minimize
description: Find the smallest useful path to an engineering outcome. Use for minimal PR, least diff, 80/20, or anti-over-engineering.
disable-model-invocation: true
---

# Minimize

Ship the outcome with the smallest change. Prefer reuse over invention.

## What this is NOT

- Not a full design challenge (`kit-challenge-design`)
- Not ticket readings or fuzzy alignment (`kit-plan`)

## Flow

1. **Outcome** — one sentence: what must be true when done (not the named implementation).
2. **2–3 paths** — smallest → largest. For each: rough files touched, risk, what you skip.
3. **Recommend the smallest** that still hits the outcome. Explicit **what we are not building**.
4. **Stop for approval** unless they already said “just do the minimal one”.
5. **Implement only the approved path.** Scope creep → pause and re-ask.

## Do / Don't

| Do | Don't |
|---|---|
| Reuse an existing helper/config | Add a new abstraction “for later” |
| Thin the outcome if the smallest path is still huge | Quietly expand into a platform refactor |
| Skip unrelated cleanup | “While we’re here” drive-bys |
| Tests that prove the outcome (or repo-required for the touch) | Speculative test matrix for untouched areas |

## Diff discipline

While implementing the chosen path, follow always-on `kit-keep-diff`: no unrelated reorders; only functional lines in the diff.

## Rules

- Prefer in-repo patterns over new frameworks.
- If nothing small works, say so — propose a thinner outcome, don’t fake minimal.
