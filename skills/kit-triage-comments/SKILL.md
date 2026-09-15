---
name: kit-triage-comments
description: Triage PR review comments — choose batch (collect/classify/approve as a set) or one-by-one walkthrough with the user before fixing. Use after review feedback lands.
disable-model-invocation: true
---

# Triage Comments

Triage review comments, then fix only what was approved.

## What this is NOT

- Not a full PR code review (`kit-review`)
- Not silent auto-fix — triage first unless the user says “fix as we go” in walkthrough mode

## Input

`kit-triage-comments [PR_URL_OR_NUMBER] [batch|walkthrough]` (all optional)

- Full URL or number, or **no input** → auto-detect PR from current branch
- If mode omitted → ask once: **batch** or **walkthrough**

| Mode | Pace | Best when |
|---|---|---|
| **batch** | Full list → approve as a set | Speed / high trust |
| **walkthrough** | One comment → wait | Decide with the human |

## Shared: collect & classify

### 1. Resolve PR
```bash
PR_NUM=$(gh pr view --json number --jq '.number')
OWNER=$(gh pr view --json headRepositoryOwner --jq '.headRepositoryOwner.login')
REPO_NAME=$(gh pr view --json headRepository --jq '.headRepository.name')
REPO="$OWNER/$REPO_NAME"
```

### 2. Extract all comment types
```bash
# A. General PR discussion
gh api repos/$REPO/issues/$PR_NUM/comments

# B. Line-specific review comments
gh api repos/$REPO/pulls/$PR_NUM/comments

# C. Review submissions
gh api repos/$REPO/pulls/$PR_NUM/reviews
```

### 3. Drop resolved / outdated threads
```bash
gh api graphql -f query='query { repository(owner: "'$OWNER'", name: "'$REPO_NAME'") { pullRequest(number: '$PR_NUM') { reviewThreads(first: 100) { nodes { id isResolved isOutdated comments(first: 10) { nodes { id body author { login } }}}}}}}'
```
Skip `isResolved` or `isOutdated`.

### 4. Classify each remaining comment

**Severity:** Critical · Important · Minor · Noise  
**Recommendation:** Must Fix · Should Fix · Optional · Skip  

Guidelines:
- Human comments rank higher than bots
- Bot “Critical” → treat as Important until verified
- “Will this break production?” → Critical
- Preference-only → Minor/Noise

## Mode: batch

Present a structured list grouped by severity. For each item include: `#`, severity, recommendation, author, file:line, quoted comment, proposed solution, short reasoning.

Then ask:
> Which should I fix? Reply with numbers (`1, 3`), ranges (`1-5`), severity (`all critical`), `all` (Must Fix + Should Fix), or `none`.

## Mode: walkthrough

1. Present **only the next** comment (quote/paraphrase + link).
2. Propose severity + one-line action (fix / reply / defer / disagree).
3. **Wait** for the user’s decision; record it.
4. Continue until done.
5. Apply approved fixes as a small batch (or fix-as-you-go if they asked).

Rules: no silent fixes during triage; deferred needs a reason.

## After approval (both modes)

1. Implement only approved fixes; follow repo standards; lint.
2. Resolve addressed threads when appropriate:
```bash
gh api graphql -f query='mutation { resolveReviewThread(input: {threadId: "<ID>"}) { thread { isResolved }}}'
```
3. **Pause** — ask the user to commit/push and confirm before posting on the PR.
4. After confirm, post a short summary (`Fixed` / `Skipped` with reasons) via `gh pr comment` using `--body-file -`.

## Output

`Fixed X/Y approved comments | Lint clean | PR updated` (when summary posted)
