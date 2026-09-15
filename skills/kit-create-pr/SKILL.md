---
name: kit-create-pr
description: Commit changes and create a pull request with proper formatting. Use when ready to publish work for review.
disable-model-invocation: true
---

# Create PR

Commit changes and create a pull request with proper formatting.

## Flow

### 1. Optional tracker id

Read the current branch:
```bash
git rev-parse --abbrev-ref HEAD
```
If the branch or the user supplies a tracker id (Jira, Linear, GitHub issue, and similar), prefix commit and PR titles with it, for example `[ABC-123]`. If there is none, omit the prefix. Do not invent an id.

### 2. Commit Changes

**A. Check git status:**
```bash
git status
```

**B. Stage changes:**

1. Show `git status` / the file list to the user.
2. **Do not** run `git add -A` (or stage everything) unless they explicitly approve that full set.
3. Default: stage only the paths they named, or ask which paths to include.
```bash
git add path/to/file [...]
```

**C. Commit:**
```bash
git commit -m "Fix: <problem users experience>"
# or, if a tracker id exists:
git commit -m "[ABC-123] Fix: <problem users experience>"
```

**Commit message format:**
- Bug fix: `Fix: <problem users experience>`
- Feature: `Feature: <what users can now do>`
- Improvement: `Improve: <what's better for users>`
- Refactor: `Refactor: <description>`
- With tracker: `[ABC-123] Fix: <problem users experience>`

**Important:** 
- Describe the PROBLEM, not your solution
- Ask: "What did users experience that was wrong?"

### 3. Create Pull Request

**A. Push the branch:**
```bash
git push -u origin HEAD
```

**B. Create PR using gh CLI:**
```bash
gh pr create \
  --title "Fix: <problem users experience>" \
  --body "## Problem
<What users experienced - why this matters to them>

## Solution
<Brief explanation of how it's fixed>"
```

## Best Practices

### Commit Message
- **Format:** `Type: Description` (optional `[TRACKER-ID]` prefix)
- **Types:** Fix, Feature, Improve, Refactor
- **Description:** Describe the problem being fixed, not the code change
- **Length:** One line, < 72 chars if possible
- **Tense:** Imperative mood (Fix, Add, Update - not Fixed, Added, Updated)
- Bad: "Add null check to user handler" (what you did)
- Good: "Users see crash when opening empty profile" (what users experienced)

### PR Description
- **Problem:** What the user experienced (lead with this, make it clear why it matters)
- **Solution:** Brief explanation of how it's fixed (keep technical details minimal)
- Skip "Root Cause" and "Changes" sections - focus on user impact

### Branch Naming
- Use descriptive lowercase names with hyphens
- Examples: `fix-login-redirect`, `feature-chat-export`, `improve-loading-speed`

## Hygiene (before publish)

- **Review:** prefer `kit-review author` on the branch diff before publish.
- **Branch:** descriptive lowercase with hyphens (`fix-…`, `feature-…`). Prefer syncing with default branch before open if the branch is stale (`git fetch` + rebase/merge per repo norm).
- **Diff:** no secrets, no probe/TEMP validation junk, no unrelated drive-bys.
- **History:** do not force-push to the default branch. Force-push to your feature branch only if the user explicitly asks.
- **Remote:** push with `-u` on first publish; confirm `gh pr view` / URL after create.

## Troubleshooting

### Finding Current Branch PR
```bash
gh pr view --json number,url,title
```

## Example Complete Flow

```
1. Branch: fix-new-chat-reload (no tracker id)

2. Commit:
   git commit -m "Fix: New Chat button returns to previous chat after reload"
   
3. Push:
   git push -u origin fix-new-chat-reload

4. Create PR:
   gh pr create \
     --title "Fix: New Chat button returns to previous chat after reload" \
     --body "## Problem
   Users click New Chat expecting a fresh conversation, but after page reload they see the previous chat instead.
   
   ## Solution
   Clear chat state properly when starting a new conversation."
   
   → Result: PR #1780
```

## Output Format
After completion, provide user with (DO NOT wrap in code block - URLs must be clickable):

✅ **Commit**: `<Type>: <Description>`

✅ **PR**: [#NUMBER - <Title>](PR_URL)
