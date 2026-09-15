---
name: kit-audit-skills
description: Audit this repo’s skills and rules for overlap, contradiction, stale names, catalog drift, and employer/proprietary leakage. Use after adding, renaming, or removing a skill or rule, or when the kit feels too large.
disable-model-invocation: true
---

# Audit Skills

Treat the kit as a product. Every extra skill is a name the user must remember. Prefer fewer, sharper entries over a fat catalog.

**Read-only unless the user asks to apply cuts.** Recommend first; delete/merge only after approval.

## What this is NOT

- Not token/host context audit (`kit-optimize-context`)
- Not a code-diff review (`kit-review`)
- Not rewriting skill bodies for style unless a contradiction requires a one-line fix after they say go

## Inventory

Scan on disk (do not trust memory or git status alone):

- `skills/*/SKILL.md` — directory name, YAML `name`, `description`, `disable-model-invocation`
- `rules/*.mdc` — filename, `alwaysApply` / `globs`, description
- `README.md` — Skills Catalog, Rules Catalog, Usage slash list
- `AGENTS.md` — add/remove/rename instructions vs what exists

Build a table: **on disk / in catalog / in Usage / leftover alias dir**.

## Checks

### 1. Catalog drift

- Skill dir exists but missing from README catalog or Usage
- Catalog/Usage names a skill whose dir is gone
- YAML `name` ≠ directory name
- Old renamed folders still on disk next to the new name

### 2. Leftover aliases

Old renamed folders still on disk (`kit-self-review` next to `kit-review`, etc.) count as **duplicates**, not compatibility. Flag for delete unless the user explicitly wants a stub.

### 3. Same job, two names

Cluster by **user intent**, not title:

| Cluster | Smell |
|---|---|
| Think-before-code | Plan / reframe / grill / minimize overlapping “What this is NOT” |
| Prove it | Smoke vs runtime vs blast-radius with the same burden of proof |
| Review | Author vs reviewer as two skills instead of modes |
| Ship | Tiny git recipes that are four commands |
| Always-on habits | Comment style, delegate, “always ask” — these belong in `rules/`, not slash skills |

A skill is redundant if a user who forgets its name would still get the outcome from another skill.

### 4. Contradictions

- A says “use B first”; B says “not A” or points at a deleted name
- Two skills claim the same trigger in `description`
- A rule and a skill teach the same behavior (pick one surface)
- `What this is NOT` points at a skill that no longer exists
- Description “Replaces X and X” / self-replacements / duplicate paths

Grep all `kit-*` mentions across `skills/`, `rules/`, `README.md`, `AGENTS.md`. Every hit must resolve to a live skill or live rule (examples like `kit-new-skill` in AGENTS.md are fine).

### 5. Skill vs rule

Move to **rule** when the guidance must fire without a slash (comments, git hygiene, delegate). Keep a **skill** when it is a paced workflow the user opts into (plan, review, prove, create-pr).

### 6. Size bar

Slash-invoke kits go stale around **~12–14 skills**. Count live `SKILL.md` dirs. If over that, propose a cut list — do not only say “it’s a lot.”

### 7. Employer / proprietary leakage

Public kit must stay **generic** (see `AGENTS.md` — Keep skills generic).

Scan `skills/`, `rules/`, `README.md` (and examples under `**/references/`) for:

- Company or employer product brands
- Work emails, private hostnames, internal VPN/SaaS URLs
- Employer-only tool paths presented as the only way
- Comments that cite an employer as provenance (“from X’s rules”)

**Bar:** if a stranger at another company would need that name to use the skill, it is a leak.

Hits → **fix** (scrub or move to the consuming repo’s wiring file).

Policy lines that *forbid* employer details (e.g. “no employer hostnames”) are fine; concrete brand names are not.

## Verdicts (required)

For each live skill/rule, one of:

- **keep** — unique job, links are clean
- **merge into X** — same job; say which name survives
- **move to rule** — always-on habit
- **remove** — too small, unused, or leftover alias

Do not recommend merge and keep as separate without saying which name to type.

## Output

```md
## Kit audit

**Counts:** N skills, M rules (catalog says …)

### Drift
- …

### Overlaps
- {A} ∩ {B} — same job because …
- Recommend: merge / keep split (why)

### Contradictions / stale refs
- `file` → `kit-dead-name`

### Skill vs rule
- …

### Leakage (employer / proprietary)
- …

### Proposed catalog
- keep: …
- merge: …
- rules: …
- remove: …

### Apply?
Stop. Ask which verdicts to execute.
```

Cite paths. Do not invent invocation history — if you do not know what they used last month, say so and cut on overlap, not on guessed usage.

## After they approve cuts

Follow `AGENTS.md`: delete dirs, update README catalogs + Usage, fix remaining `kit-*` refs, **scrub employer/proprietary leaks**, keep names generic.
