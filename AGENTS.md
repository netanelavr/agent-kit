# agent-kit

Reusable Agent Skills and always-on rules for PRs, review, and shipping. Skills install through the Agent Skills CLI. Rule bodies live in `rules/*.mdc`.

Claude Code reads this file when `CLAUDE.md` is absent. Use it for install and conventions. This file does not apply the `.mdc` rules. Do not paste rule bodies here.

## Install

Skills (any Agent Skills host):

```bash
npx skills add netanelavr/agent-kit --all
```

One skill:

```bash
npx skills add netanelavr/agent-kit/skills/kit-create-pr
```

`npx skills add` does **not** install rules. Copy `rules/*.mdc` if you want the always-on kit loop. How each vendor picks up rules is in the README consumption matrix.

## Use

- Invoke a skill with `/<skill-name>` (Cursor) or the host’s skill command.
- Always-on rules are not slash skills. Bodies: `rules/*.mdc`. How each vendor attaches them: README section **How vendors consume the kit**.

## Kit conventions

- Skill directories and names use the `kit-` prefix (`skills/kit-<name>/`).
- Rule ids use the same prefix (`kit-comment-why`, …). Bodies live in `rules/kit-<name>.mdc`.
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
4. Run `kit-audit-skills` (overlaps, stale `kit-*` refs, catalog vs disk, `npx skills add . -l`) before treating the add as done

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

Rules live in `rules/` as `.mdc` files. They are always-on guidance (not slash skills). `npx skills add` does not install them. Keep `rules/AGENTS.md` as a short index only.

### Adding Rules
1. Create `rules/kit-<name>.mdc` with YAML frontmatter (`description`, `alwaysApply` and/or `globs`) and the rule body
2. Keep each rule to one concern and under ~50 lines
3. Add a row to `rules/AGENTS.md`
4. Update the Rules Catalog in `README.md`

### Removing or Renaming Rules
1. Delete or rename the `.mdc` file
2. Update `rules/AGENTS.md` and the Rules Catalog in `README.md`

## General Guidelines

- **No secrets**: Never commit credentials, tokens, private hostnames, or work emails
- **Keep skills generic**: Avoid employer names, internal tools, and proprietary details. Skills must work in any company.
- **Maintain catalog**: Always update `README.md` when changing skills or rules
- **Installation path**: Use `netanelavr/agent-kit` in all documentation examples
