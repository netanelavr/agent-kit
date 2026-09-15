# agent-kit

Personal reusable Agent Skills for shipping code: PR workflows, code review, and context optimization. Works with Cursor and other Agent Skills hosts.

## Installation

Install all skills:
```bash
npx skills add netanelavr/agent-kit --all
```

Install a single skill:
```bash
npx skills add netanelavr/agent-kit/skills/kit-create-pr
```

## Skills Catalog

| Skill | When to Use |
|-------|-------------|
| `kit-create-pr` | Commit changes and create a pull request with proper formatting |
| `kit-handle-pr-comments` | Fetch PR comments, classify by severity, and present for approval before acting |
| `kit-self-review` | Review only changed files in this branch before submitting |
| `kit-optimize-context` | Analyze Cursor base context and suggest optimizations |
| `kit-back-to-main` | Cleanup current branch and switch to default branch with latest changes |
| `kit-design-review` | Stress-test a plan or design against domain language, docs, and code |
| `kit-feature-implementation` | Structured approach for complex feature implementation with mandatory planning |
| `kit-pr-review` | Comprehensive code-review analysis before publishing a PR |

## Usage

Invoke skills by name in Cursor:
```
/kit-create-pr
/kit-self-review
/kit-optimize-context
```

## Rules

The `rules/` directory contains always-on Cursor rules that are automatically applied when copied into a Cursor project:

- `ask-before-acting.mdc` — Ask clarifying questions before starting any task
- `git-safety.mdc` — Require explicit permission for all git operations
- `docs-location.mdc` — Documentation location conventions
- `mcp-tool-definition.mdc` — Guidelines for defining MCP server tools (on-demand)

Copy these rules to your project's `.cursor/rules/` directory to enable them.

## License

MIT
