---
name: kit-review
description: Review a branch or PR — author self-check (changed files only) or reviewer second-pass (architecture + diff). Use before publish or when reviewing someone else’s PR. Modes: author | reviewer.
disable-model-invocation: true
---

# Review

Valuable, trustworthy feedback — not a wall of nits.

**Before reviewing, read** `references/review-checklist.md`.

Accept `kit-review author|reviewer`. If omitted: **author** when the user owns the uncommitted/branch diff; **reviewer** when they asked for a second pass, a PR URL, or “review this PR”.

## What this is NOT

- Not “comment on every line” — prefer fewer, higher-severity findings
- Not a license to rewrite unrelated code (Boy Scout only when asked)
- Not proof that it works (`kit-prove`) and not blast-radius (`kit-check-blast`)

## Scope

| Mode | Scope |
|---|---|
| **author** | Only the provided changes (diff / modified files). Unchanged code only if the change directly breaks it. |
| **reviewer** | Diff vs default branch **plus** a light approach check. Prefer fewer findings. Full domain/ADR grill → `kit-challenge-design`. |

If nothing meaningful: say the diff is acceptable.

## Approach

1. **Big picture** — intent + system impact
2. **Flows** — main paths (happy + failure)
3. **Drill down** — checklist P0 → P3
4. **Alternatives** — only if they clearly improve simplicity/correctness/reuse
5. **Reviewer-only** — light approach check (why this path; simpler option; failure modes). Deep domain/ADR grilling is `kit-challenge-design`. Skip hypothetical 10× scale unless the change is actually about scale.

## Philosophy

- Impact over certainty
- Raise issues likely to cause: maintenance burden, fragile behavior, hidden bugs, security/data risk
- Engineering judgment, not lint echo
- Flag speculative generality, single-use abstractions, and new modules that duplicate in-repo behavior

## Response structure

### 1. Big Picture Summary
- Intent / system impact / key trade-offs

### 2. What / Where
- Concise change list + files

### 3. Flows & Logic
- Per main flow: entry, happy path, failure/edges, side effects

### 4. Findings (checklist order)
- Critical (must fix)
- Improvements
- Missing tests (critical paths)
- Boy Scout (optional)

### 5. Could this be done better?
- Alternative + why + trade-offs — only if meaningful

### 6. Ready?
- **author:** publish / fix-first
- **reviewer:** approve / request changes + the bar that was not met

## Output guidelines

- Clear and direct
- Prefer fewer, higher-value comments
- No generic advice or hypothetical edge cases
- Pair “it works” claims with `kit-prove` if validation is in scope
