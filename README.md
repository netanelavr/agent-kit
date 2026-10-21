# agent-kit

Reusable Agent Skills for PRs, review, and shipping. Works with Cursor and other Agent Skills hosts.

Claude Code reads `AGENTS.md` for install and conventions when `CLAUDE.md` is absent. That does not load the always-on rules. Those stay in `rules/*.mdc`. Copy or point at them (see [How vendors consume the kit](#how-vendors-consume-the-kit)).

## Installation

Install all skills:
```bash
npx skills add netanelavr/agent-kit --all
```

Install a single skill:
```bash
npx skills add netanelavr/agent-kit/skills/kit-create-pr
```

Always-on rules are **not** installed by `npx skills add`. Copy `rules/*.mdc` if you want the always-on kit loop. Skills install does not do that. See [How vendors consume the kit](#how-vendors-consume-the-kit).

## Skills Catalog

| Skill | When to Use |
|-------|-------------|
| `kit-create-pr` | Commit changes and create a pull request with proper formatting |
| `kit-triage-comments` | Triage PR comments — batch or one-by-one before fixing |
| `kit-review` | Author self-check or reviewer second-pass (`author` / `reviewer`) |
| `kit-optimize-context` | Reduce always-on context across Cursor / Claude / Codex / etc. |
| `kit-debug-prod` | Production bugs via logs/traces/errors/metrics (needs team `wire-observability.md`) |
| `kit-challenge-design` | Stress-test a plan or design against domain language, docs, and code |
| `kit-debate-decision-tree` | Persona debate + wide, deep decision tree with simulated picks; writes plain decision sentences to new files |
| `kit-plan` | Distill the need first (don't-build can stop it); align a concept, with three readings when fuzzy; wait for approval |
| `kit-decisions` | Living log for one large feature: accepted, rejected, landmines, open |
| `kit-minimize` | Smallest useful path / minimal PR (not design grill) |
| `kit-strip-diff` | After the outcome works: drop unused additions, extra instruction lines, format-only hunks, and rare-case handling the outcome does not need |
| `kit-prove` | Live proof for “it works” claims — smoke or runtime path; probes stay temporary |
| `kit-explain` | Problem / Solution / How + one compact mermaid |
| `kit-learn-repo` | Ramp on a repo: layout, run/test, safe change points |
| `kit-check-blast` | What else could this change break — prove the safety fact |
| `kit-audit-metrics` | Honest aggregate analytics — define, query, provenance, no invented numbers |
| `kit-audit-skills` | After adding/renaming skills: overlaps, contradictions, stale names, catalog drift |
| `kit-capture-lesson` | Turn a steering/repeated miss into a short written rule; wait for approval |
| `kit-evolve` | Evolve and prune skills/rules from repeated corrections; keep or delete only after a measured with/without replay |

## Rules Catalog

Always-on rules (`rules/*.mdc`). Copy or point at them; do not rely on slash invoke. See [How vendors consume the kit](#how-vendors-consume-the-kit).

| Rule | When it applies |
|------|-----------------|
| `kit-comment-why` | WHY/landmine comments only — no WHAT narration |
| `kit-delegate` | Inline vs skill vs subagent; safe parallel fan-out |
| `kit-keep-diff` | always — minimal diffs, no unrelated reorders |
| `kit-humanize` | always — plain writing; no AI-slop in commits/PRs/docs |
| `kit-work-loop` | always — plan first, isolate noisy work, strip a fat diff, isolated prove, capture repeated fixes |
| `kit-evidence` | External tool, package, library, or source claims need a live URL and a liveness signal, or `unverified` |

Invoke a skill in Cursor with `/<skill-name>`.

## How vendors consume the kit

Skills install the same way on every host: `npx skills add`. Rules are separate.

| Vendor | Skills | Rules |
|--------|--------|-------|
| All Agent Skills hosts | `npx skills add netanelavr/agent-kit --all` (or a single skill path) | not installed by the CLI |
| Cursor | same | copy `rules/*.mdc` into `.cursor/rules/` (or user rules) |
| Claude Code | same | copy `rules/*.mdc` into the consuming repo, or point at them from root `AGENTS.md`. Claude Code reads `AGENTS.md` as a **fallback** only when `CLAUDE.md` is absent. If you already have a `CLAUDE.md`, delete it or reference `AGENTS.md` / the `.mdc` files from it |
| Codex | same | copy `rules/*.mdc`, or point at them from root `AGENTS.md` |

## License

MIT
