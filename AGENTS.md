# agent-kit

Reusable Agent Skills and always-on rules for PRs, review, and shipping. Skills install through the Agent Skills CLI. Rules live in plain markdown so Claude Code, Codex, Cursor, and other hosts can share one copy.

Works out of the box with Claude Code 2.1.277+ via `AGENTS.md` - one rules file for every coding agent, no per-vendor copies.

## Install

Skills (any Agent Skills host):

```bash
npx skills add netanelavr/agent-kit --all
```

One skill:

```bash
npx skills add netanelavr/agent-kit/skills/kit-create-pr
```

`npx skills add` does **not** install rules. How each vendor picks up rules is in the README consumption matrix.

## Use

- Invoke a skill with `/<skill-name>` (Cursor) or the host’s skill command.
- Always-on rules are not slash skills. Read and apply [`rules/AGENTS.md`](rules/AGENTS.md) in full. That file is the source of truth for `kit-comment-why`, `kit-delegate`, `kit-keep-diff`, `kit-humanize`, and `kit-work-loop`.
- Cursor: copy `rules/*.mdc` **and** `rules/AGENTS.md` into `.cursor/rules/` (or user rules). The `.mdc` files are wrappers; they do not carry a second copy of the bodies.
- Claude Code 2.1.277+: in the consuming repo, copy `rules/AGENTS.md` to repo-root `AGENTS.md`. That is the fallback when no `CLAUDE.md` exists. If you already have a `CLAUDE.md`, delete it or reference `AGENTS.md` from it. The fallback only fires when `CLAUDE.md` is absent.
- Codex: copy `rules/AGENTS.md` to repo-root `AGENTS.md`.

## Kit conventions

- Skill directories and names use the `kit-` prefix (`skills/kit-<name>/`).
- Rule ids use the same prefix (`kit-comment-why`, …). Canonical bodies live in `rules/AGENTS.md`. Cursor `.mdc` files are thin wrappers (frontmatter + pointer).
- Skills must stay generic: no employer names, internal tools, or proprietary details.
- Public examples use `netanelavr/agent-kit`.
- Catalogs in `README.md` must match what is on disk.

---

# Agent Maintenance Guide

Guidelines for maintaining agent-kit.

## Skill Management

### Adding New Skills
When adding a new skill:
1. Create a directory under `skills/` with the `kit-` prefix (e.g., `skills/kit-new-skill/`)
2. Add a `SKILL.md` file with YAML frontmatter:
   ```yaml
   ---
   name: kit-new-skill
   description: Clear description of when to use this skill
   disable-model-invocation: true
   ---
   ```
3. Update the Skills Catalog table in `README.md`
4. Run `kit-audit-skills` (overlaps, stale `kit-*` refs, catalog vs disk) before treating the add as done

### Removing Skills
When removing a skill:
1. Delete the skill directory from `skills/`
2. Remove the entry from the Skills Catalog table in `README.md`

### Renaming Skills
When renaming a skill:
1. Rename the directory (must maintain `kit-` prefix)
2. Update the `name` field in the YAML frontmatter
3. Update the Skills Catalog table in `README.md`

## Rule Management

Canonical rule content lives in `rules/AGENTS.md` (plain markdown). Cursor wrappers live in `rules/` as `kit-<name>.mdc`. They are always-on guidance (not slash skills). `npx skills add` does not install them.

### Adding Rules
1. Add a named section to `rules/AGENTS.md` (one concern, keep it short)
2. Add `rules/kit-<name>.mdc` with YAML frontmatter (`description`, `alwaysApply` and/or `globs`) and a pointer to that section - no forked body
3. Update the Rules Catalog in `README.md`

### Removing or Renaming Rules
1. Edit or delete the section in `rules/AGENTS.md`
2. Delete or rename the `.mdc` wrapper
3. Update the Rules Catalog in `README.md`

## General Guidelines

- **No secrets**: Never commit credentials, tokens, private hostnames, or work emails
- **Keep skills generic**: Avoid employer names, internal tools, and proprietary details. Skills must work in any company.
- **Maintain catalog**: Always update `README.md` when changing skills or rules
- **Installation path**: Use `netanelavr/agent-kit` in all documentation examples
