---
name: kit-strip-diff
description: Remove extras from a finished diff. Use when the outcome already works and the diff still has unused additions, extra instruction lines, format-only hunks, or rare-case handling the outcome does not need.
disable-model-invocation: true
---

# Strip diff

The outcome already works. Cut what this diff does not need. Ask before you delete or restore anything.

## What this is NOT

- Not choosing a smaller path before coding (`kit-minimize`)
- Not proving the change works (`kit-prove`)
- Not a bug or design review (`kit-review`)
- Not checking what else the change could break (`kit-check-blast`)
- Not a token audit of what loads before the user types (`kit-optimize-context`)
- Not moving working logic from a shared file to the one caller
- Not the always-on habit while typing (`kit-keep-diff`)

## Scope

Default: uncommitted changes, staged and unstaged. If they name a branch, a commit range, or a path, use that. If the boundary is unclear, ask once.

## What to cut

Read the outcome and the surrounding code. Flag a hunk only when you can show it is extra.

1. **Unused addition.** A symbol, type, or file this diff added that nothing calls. Deleting it leaves behavior the same.
2. **Extra instruction.** A new or edited line in a skill, rule, or agent doc that repeats a line already there, or that a competent model would do without being told. Cut that line only.
3. **Format-only hunk.** Order, wrap, or whitespace changed, and the meaning matches the base version. Restore the base text. If you cannot show the match, leave the hunk.
4. **Rare case.** A branch or helper this diff added for a case the agreed outcome does not need. Deleting it leaves that outcome the same.

Leave a change the outcome needs, even when it looks large. A clean hunk gets no finding.

## Flow

1. State the scope in one line.
2. List findings: file, what is extra, which of the four kinds, and the cut.
3. Stop. Wait for all, a subset, or no.
4. Apply only the approved cuts. Do not rewrite working behavior.
5. Re-run the check the repo already uses for the files you touched. If it fails, undo that cut.

## Hand back

- What you removed, by kind
- Lines removed and added
- Check result, or that you could not run one
