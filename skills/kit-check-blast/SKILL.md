---
name: kit-check-blast
description: Find what a change could break beyond the diff, and prove the one fact that makes it safe by running real code. Use before shipping a small diff you don’t fully trust.
disable-model-invocation: true
---

# Check Blast

What else breaks if this ships? Grep alone is not enough.

## What this is NOT

- Not a full PR review (`kit-review`)
- Not “list every caller” — that’s trivial; focus on breakage grep misses
- Companion to `kit-prove`: safety claims need live proof

## Flow

1. **Read the change** — symbols added/changed/deleted; behavior the diff doesn’t spell out.
2. **Find the one safety fact** — the single assumption that clears most risk if true (e.g. “this only drops already-dead cache entries”).
3. **Look where grep stops** — library source + pinned version, timing/teardown, wire formats, flags, other languages/services reading the same bytes.
4. **Rank risks** — real chance × real cost. Keep confirmed risks; list cleared checks separately. Cite `file:line`; never invent callers.
5. **Prove the safety fact** — small script/test that calls the real code and fails loud if wrong. Paste output. If you can’t reach a run cheaply, mark **unproven**.

## Hand back

- Safety fact + proof artifact (or unproven)
- Confirmed risks (chance/cost + citation)
- Cleared checks
- Recommended next step (ship / narrow / add test / ask human)
