# agent-kit

Reusable Agent Skills for PRs, review, and shipping. Works with Cursor and other Agent Skills hosts.

Works out of the box with Claude Code 2.1.277+ via AGENTS.md - one rules file for every coding agent, no per-vendor copies.

## Installation

Install all skills:
```bash
npx skills add netanelavr/agent-kit --all
```

Install a single skill:
```bash
npx skills add netanelavr/agent-kit/skills/kit-create-pr
```

Always-on rules are **not** installed by `npx skills add`. See [How vendors consume the kit](#how-vendors-consume-the-kit).

## Skills Catalog

| Skill | When to Use |
|-------|-------------|
| `kit-create-pr` | Commit changes and create a pull request with proper formatting |
| `kit-triage-comments` | Triage PR comments — batch or one-by-one before fixing |
| `kit-review` | Author self-check or reviewer second-pass (`author` / `reviewer`) |
| `kit-optimize-context` | Reduce always-on context across Cursor / Claude / Codex / etc. |
| `kit-debug-prod` | Production bugs via logs/traces/errors/metrics (needs team `wire-observability.md`) |
| `kit-challenge-design` | Stress-test a plan or design against domain language, docs, and code |
| `kit-plan` | Align on a buildable concept (includes three readings when the ask is fuzzy); wait for approval |
| `kit-minimize` | Smallest useful path / minimal PR (not design grill) |
| `kit-prove` | Live proof for “it works” claims — smoke or runtime path; probes stay temporary |
| `kit-explain` | Problem / Solution / How + one compact mermaid |
| `kit-learn-repo` | Ramp on a repo: layout, run/test, safe change points |
| `kit-check-blast` | What else could this change break — prove the safety fact |
| `kit-audit-metrics` | Honest aggregate analytics — define, query, provenance, no invented numbers |
| `kit-audit-skills` | After adding/renaming skills: overlaps, contradictions, stale names, catalog drift |
| `kit-capture-lesson` | Turn a steering/repeated miss into a short written rule; wait for approval |

## Rules Catalog

Always-on rules. Bodies live in `rules/AGENTS.md`. Cursor wrappers are `rules/*.mdc`. Copy into the consuming repo; do not rely on slash invoke.

| Rule | When it applies |
|------|-----------------|
| `kit-comment-why` | WHY/landmine comments only — no WHAT narration |
| `kit-delegate` | Inline vs skill vs subagent; safe parallel fan-out |
| `kit-keep-diff` | always — minimal diffs, no unrelated reorders |
| `kit-humanize` | always — plain writing; no AI-slop in commits/PRs/docs |
| `kit-work-loop` | always — plan first, isolate noisy work, isolated prove, capture repeated fixes |

Invoke a skill in Cursor with `/<skill-name>`.

## How vendors consume the kit

Skills install the same way on every host: `npx skills add`. Rules are separate.

| Vendor | Skills | Rules |
|--------|--------|-------|
| All Agent Skills hosts | `npx skills add netanelavr/agent-kit --all` (or a single skill path) | not installed by the CLI |
| Cursor | same | copy `rules/*.mdc` and `rules/AGENTS.md` into `.cursor/rules/` (or user rules). The `.mdc` files point at `AGENTS.md`; they are not a second copy of the bodies |
| Claude Code 2.1.277+ | same | copy `rules/AGENTS.md` to repo-root `AGENTS.md`. Claude Code reads that file as a **fallback**. Fallback only fires when `CLAUDE.md` is absent. If you already have a `CLAUDE.md`, delete it or reference `AGENTS.md` from it (for example `@AGENTS.md`) |
| Codex | same | copy `rules/AGENTS.md` to repo-root `AGENTS.md` |

## License

MIT
