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

### Removing Skills
When removing a skill:
1. Delete the skill directory from `skills/`
2. Remove the entry from the Skills Catalog table in `README.md`

### Renaming Skills
When renaming a skill:
1. Rename the directory (must maintain `kit-` prefix)
2. Update the `name` field in the YAML frontmatter
3. Update the Skills Catalog table in `README.md`

## Rules Management

### Adding Rules
Rules in `rules/` should be:
- Always-on behavioral guidelines (not workflows)
- Generic enough to apply across projects
- Free of employer-specific or proprietary information

When adding a rule:
1. Use `.mdc` extension
2. Include clear frontmatter with `description` and `alwaysApply: true`
3. Document the rule in the Rules section of `README.md`

### Converting Rules to Skills
If a rule describes a multi-step workflow:
1. Create a new skill under `skills/kit-<name>/`
2. Set `alwaysApply: false` or `agentRequestable: true` in the rule
3. Consider removing the rule entirely if it's purely procedural

## General Guidelines

- **No secrets**: Never commit credentials or sensitive information
- **Keep skills generic**: Avoid employer-specific terminology or proprietary details
- **Maintain catalog**: Always update `README.md` when changing skills
- **Installation path**: Use `netanelavr/agent-kit` in all documentation examples
