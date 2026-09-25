---
name: kit-decisions
description: Keep a short living decision log for one large feature. Use while building it, before continuing the work, or when a reviewer needs the history.
disable-model-invocation: true
---

# Decisions

Keep one short file for a large feature so a later agent can see why the code looks like this.

Two jobs:

- **Capture** — record a closed fork or a landmine
- **Brief** — read the file before review or more work on that feature

Accept `kit-decisions capture|brief`. If omitted: **capture** when this conversation closed a fork or hit a landmine; **brief** when they ask what was already decided, or before changing the feature.

## What this is NOT

- Not the pre-code concept log (`kit-plan`). That log lives in chat and stops at approval.
- Not a glossary or sparse ADRs (`kit-challenge-design`). Promote only when the ADR bar is met.
- Not a reusable rule after a repeated miss (`kit-capture-lesson`).
- Not a review of the diff (`kit-review`). Brief states what the log says. Review still owns findings.

## Where the file lives

Default: `docs/features/<feature>/decisions.md`

If a `decisions.md` for this feature already exists, use that path. Do not move it.

The file stays as long as the feature lives. Do not delete it on merge.

Create the file only when the first real entry exists.

## What to record

Four sections. One short bullet each. Edit in place. No chat transcript.

| Section | Write when |
|---|---|
| **Accepted** | A direction was chosen. Include why, and what must not be undone without new evidence. |
| **Rejected** | A direction was tried or seriously considered, and lost. Include why, and when it may be revisited. |
| **Landmines** | Something looked trivial and broke, or a small change would undo a hard-won fix. |
| **Open** | A real fork is still undecided. |

Skip obvious choices, easy-to-reverse trivia, and glossary terms.

When an accepted decision is replaced, move the old bullet to **Rejected** and add the new evidence. Keep the original why.

## Format

```md
# Decisions: {feature}

Living log for this feature. Hard-to-reverse decisions that meet the ADR bar belong in `docs/adr/` — leave a pointer here.

## Accepted
- **{decision}** — {why}. Do not undo without {new evidence}.

## Rejected
- **{option}** — {why it lost}. Revisit only if {condition}.

## Landmines
- **{looks trivial}** — {what broke, and why the obvious fix fails}.

## Open
- **{fork}** — {what is still unknown}.
```

Leave a heading in place when its section is empty.

## Flow

### Capture

1. Name the feature. Find the file, or create it at the default path on the first entry.
2. Distill this conversation into one or two bullets. Do not paste the transcript.
3. Update the matching section in place. Move **Open** items when they close.
4. If an **Accepted** item is hard to reverse, surprising without this file, and had a real alternative, offer an ADR through `kit-challenge-design`. Write the ADR only after they say yes. Then add a one-line pointer under **Accepted**.
5. Show the bullets that changed. Stop.

### Brief

1. Find the file. If it is missing, say so. Do not invent history.
2. List only the bullets this change can touch.
3. **Before review:** re-proposing a **Rejected** option needs new evidence that beats the recorded why. Undoing an **Accepted** decision without that evidence is a finding for `kit-review`.
4. **Before more work:** state the landmines and accepted constraints the edit can hit. Continue only if the change respects them, or the user accepts the new evidence.

## Do / Don't

| Do | Don't |
|---|---|
| "Rejected sync retry — duplicates under timeout. Revisit only with an idempotency key." | A paragraph retelling the thread |
| One bullet when a fork closes | An entry after every message |
| A pointer to ADR-0007 after they accept the promotion | A copy of the whole ADR in this file |
| "No decisions.md for billing. Nothing recorded." | A guessed history |

Generic only: no employer names, private hosts, or work-only tools.
