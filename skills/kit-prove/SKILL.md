---
name: kit-prove
description: Require live proof for any “works / tested / validated” claim — CLI, raw JSON/logs, timestamps, or a real runtime path. Use when verifying a fix, smoke, or “validate it works”. Not for dead-code cleanup or design debate.
disable-model-invocation: true
---

# Prove

**Inverted burden of proof:** a claim is guilty until there is a live artifact.

## What this is NOT

- Not code cleanup after AI iteration (prune the diff separately)
- Not “I think this approach is right” without a run — that’s opinion, not proof
- Not a substitute for `kit-review` judgment
- Not production incident work (`kit-debug-prod`)
- Not leaving probes / TEMP bypasses in git

## Claim → proof

| Claim | Acceptable proof |
|---|---|
| Command / test passed | Full command + exit + relevant stdout/stderr |
| API behaves | Raw request/response JSON (redact secrets) |
| UI fixed | Real UI screenshot of the actual run, or recorded steps + DOM/network evidence |
| Reproduced bug | Steps + failing artifact before fix |
| “Deployed / published” | Registry/CI URL or CLI output with version + timestamp |
| Feature works at runtime | Exercised real path (adapter below) + artifact — tests alone are not enough when the change is risky |

## Bugfix before / after

When the claim is a **bug fix**, a diff or a new passing test alone is not enough.

1. **Before** — reproduce the failure on the real path; save the artifact (HTTP/JSON, logs, UI, CLI).
2. **After** — same steps, same environment, against the fix; save the matching artifact.
3. Show both side by side in the summary so the delta is obvious.
4. If the tree is already fixed, temporarily restore the broken path long enough to recapture before, then restore and recapture after — or say clearly what you could not reproduce.

Same endpoint / query / UI flow / log line for both captures.

## Rejected as proof

- Mockups, HTML→PNG “screenshots”, reconstructed diagrams sold as authentic
- “I ran it” with no artifact
- Summaries without the underlying command + output

## Flow

1. List claims that need proof.
2. **Isolated checker (prefer for multi-step or risky changes):** spawn a **new** subagent. Give it only claims, done criteria, how to run proof, and paths in scope. Do **not** pass the build chat, your rationale, or “it should pass.” That checker runs this skill and reports artifacts or **unproven**. Tiny obvious fixes may stay inline; say so.
3. **Depth:** Tiny change + strong tests + obvious path → you may **skip deep probes** (extra adapters/instrumentation). Say why. This is not permission to claim “it works” without an artifact.
4. For each claim you still assert: run or request the real path. Pick an adapter when the user asked to validate/verify a feature (not only a one-line smoke).
5. Paste/attach the **raw** artifact (keep timestamps).
6. If only a derived visual exists: label **reconstruction** and still attach the raw source.
7. **Cleanup** — remove probes; `git diff` + search for TEMP/debug markers must be clean before push.
8. Never commit validation junk under `docs/` (or similar); keep proofs in chat/PR comments.

## Adapters (pick one when you need a runtime path)

Discover ports from compose/env/`package.json` / listening processes — never assume defaults.

| Situation | Approach |
|---|---|
| Local HTTP API | Hit real endpoints; assert status/body |
| Hard-to-reach backend path | Temporary checkpoints → local collector / logs (remove before commit) |
| UI / browser | Drive the real UI or attach to an existing session |
| User must click | Prep harness first, then instruct — don’t ask them to set up |

## Output

- Claims vs artifacts
- What was exercised (if a runtime path)
- Gaps / **unproven**
- Confirm probes removed

## Rules

- Missing artifact → say **unproven**. Do not soften to “likely works”.
- Early stop only skips **deep probes** — any “it works” / “validated” claim still needs an artifact, or mark **unproven**.
- Prefer short reproducible commands over long narrative.
