---
name: kit-optimize-context
description: Analyze always-on agent context across coding hosts (Cursor, Claude Code, Codex, Gemini CLI, etc.) and suggest token reductions. Use when sessions feel heavy before you type anything.
disable-model-invocation: true
---

# Optimize Context

Map what loads into an agent session **before the user types**, then recommend cuts. Host-agnostic: detect or ask which host(s) to audit.

## What this is NOT

- Not a rewrite of skills/rules content (edit those files directly)
- Not installing/uninstalling tools — **read-only** audit

## 0. Scope the host(s)

Ask once if unclear: which host to audit — **Cursor**, **Claude Code**, **Codex**, **Gemini CLI**, **OpenCode / other**, or **all present**.

Default: audit every host that has a detectable config/skills dir on this machine, plus the current repo’s agent folders.

Token heuristic: **chars ÷ 4** unless you can tokenize accurately. Mark estimates.

## 1. Always-on instructions (per host)

For each in-scope host, find always-on instruction surfaces **that exist**:

| Host | Typical paths / surfaces |
|---|---|
| Cursor | `<repo>/.cursor/rules/` (`alwaysApply` / requestable), user rules in Cursor settings |
| Claude Code | `CLAUDE.md`, `<repo>/.claude/`, `~/.claude/` settings / memory |
| Codex | `AGENTS.md`, `<repo>/.codex/`, `~/.codex/` |
| Gemini CLI | `GEMINI.md` / `.gemini/` if present |
| Shared / other | `AGENTS.md`, `.agents/`, host docs the user names |

For each always-on file/rule:

- filename/path
- ~chars / ~tokens
- always-on vs on-demand (how this host decides)
- heaviest items
- recommendation: keep always-on, split, or move to a skill / on-demand rule

Skip missing paths; say what you checked.

## 2. Skills / commands metadata

Count discrete skills (`SKILL.md` roots or declared entries) and **metadata size visible before a skill body is opened**:

| Source | Notes |
|---|---|
| `<repo>/.agents/skills/`, `<repo>/.cursor/skills/`, `<repo>/skills/` | repo-local |
| `~/.agents/skills/`, `~/.cursor/skills/`, `~/.claude/skills/`, `~/.codex/skills/`, `~/.gemini/skills/` | user-global |
| Host-bundled / plugin skill roots | only if discoverable on disk |

Also note slash-command / prompt folders if the host keeps them separate (e.g. legacy `commands/`).

Per existing source: count, combined metadata ~tokens, whether `disable-model-invocation` (or host equivalent) keeps bodies out of default context.

## 3. Plugins / MCP / bridges

Where present and readable without secrets:

- Enabled plugins / marketplaces (Claude `settings.json`, Cursor extensions, etc.)
- MCP servers advertised into context (titles/descriptions only — **redact URLs/tokens**)
- Flag plugins unlikely to matter for **this repo’s domain**

State limits when metadata is opaque.

## 4. Index / VCS noise

After **explicit permission** to run git (or skip if withheld):

- `git status --short` line count
- Untracked large dirs escaping ignore
- Host ignore files if present (`.cursorignore`, `.claudeignore`, etc.) and what large paths they hide

## 5. Summary table

| Component | Host | ~Chars | ~Tokens | Reducible? | Recommended action |
|---|---|---|---|---|---|

One row per major bucket (always-on packs, heavy files, skill metadata by source, plugins/MCP, ignore/git noise).

## 6. Top 3 quick wins

Highest impact for **fresh sessions**, ordered by estimated tokens saved. Each: savings bracket, how to do it, trade-off.

## Constraints

- **Read-only** — do not edit settings, ignores, rules, or plugins in this run
- **Redact** secrets, tokens, private hostnames, employer-private plugin names (describe by category)
- State methodology: hosts checked, paths missing, chars/4 vs tokenizer
