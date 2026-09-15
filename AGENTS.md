# Agent Maintenance Guide

This document provides guidelines for maintaining agent-kit.

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

Rules live in `rules/` as `.mdc` files. They are always-on guidance (not slash skills). `npx skills add` does not install them.

### Adding Rules
1. Create `rules/kit-<name>.mdc` with YAML frontmatter (`description`, `alwaysApply` and/or `globs`)
2. Keep each rule to one concern and under ~50 lines
3. Update the Rules Catalog in `README.md`

### Removing or Renaming Rules
1. Delete or rename the `.mdc` file
2. Update the Rules Catalog in `README.md`

## General Guidelines

- **No secrets**: Never commit credentials, tokens, private hostnames, or work emails
- **Keep skills generic**: Avoid employer names, internal tools, and proprietary details. Skills must work in any company.
- **Maintain catalog**: Always update `README.md` when changing skills or rules
- **Installation path**: Use `netanelavr/agent-kit` in all documentation examples
