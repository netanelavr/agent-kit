---
name: kit-evolve
description: Grow and prune the harness from repeated corrections. Propose one skill or rule edit at a time, and keep or delete text only after a measured with/without replay of saved tasks. Use for periodic harness maintenance, after sessions with repeated steering, or after a model change.
disable-model-invocation: true
---

# Evolve

Measure harness changes instead of trusting them. One success proves little: the same agent passes a task once and fails it the next time. Collect corrections from real sessions, test one edit at a time on replayed tasks with and without it, and keep it only on a real gain. Test existing skills and rules the same way before deleting them.

## What this is NOT

- Not a rule from one correction (`kit-capture-lesson`). A single miss goes there; this skill needs repeats and replays.
- Not catalog hygiene: overlap, stale names, drift (`kit-audit-skills`).
- Not trimming always-on context by host (`kit-optimize-context`).
- Not proving a code change works (`kit-prove`). This measures whether harness text changes agent behavior.
- Not auto-editing. The agent proposes and measures; you approve the plan. Nothing lands without your yes.

## The evidence store

Lives in the consuming repo, not in the kit: `.kit-evolve/`

- `lessons.md` - one line per correction: date, task, what the agent did, what it should have done. Duplicates stay; repeats are the signal.
- `tasks/<slug>/` - a replayable task: `request.txt` (the user's words, verbatim), `base` (commit where it was broken), `check.sh` (exits 0 when solved), `fix.patch` (proof it can be solved).
- `journal.md` - every cycle and verdict, append-only.
- `graveyard/<slug>.md` - killed candidates with the reason, so the same idea is not proposed again unchanged.

## 1. Collect (end of a session)

Ask two questions. Usually both are no; then record nothing.

1. Did the user correct the agent, even once? → one lesson line per correction.
2. Did something go from broken to working? → offer to save a replay task.

A task is a ruler only if its check separates broken from fixed. Before keeping one, run `check.sh` in three states: broken must fail; broken + fix must pass; broken + fix minus the fix's test edits must still pass (a replaying agent will not rewrite your test). Fail any state → quarantine the task, repair it once, delete it on a second fail.

Collection never edits skills or rules. That starts at step 2.

## 2. Find ONE theme

Bar: one lesson logged at least 3 times, or one cause behind at least 2 failing tasks, and no live skill or rule already covers it.

- Below the bar → stop. Record the cycle as barren. Run no replays.
- Check the graveyard first: a buried idea returns only revised, under a new name.
- One theme per cycle. Never batch.

## 3. Choose the surface

Measured findings behind this table: auto-loaded rules did most of the work. Skills helped only when offered next to the files the task touches. Skills offered on every turn were rarely opened.

| Where the fix lives | Surface |
|---|---|
| One folder or area | Rule with `globs` scoped there - it loads itself when the agent works on those files |
| Must fire no matter what | Always-on rule, kept short |
| Spread repo-wide, a paced workflow | Skill whose description names the areas and files it serves |
| One fact the agent breaks anywhere | One line in `AGENTS.md`, not a skill |
| Must run mechanically, every time | A hook or CI check, not text |

Default to a scoped rule. A new always-on skill is the last resort: its description costs context on every turn, and the agent still has to choose to open it.

## 4. Propose the one edit

Draft the exact diff for the candidate, under a new name. Show it with the measurement plan: the tasks, the runs per arm, the expected cost. Stop. Wait for yes.

## 5. Measure with/without

- Screen: replay the theme's tasks 2 times with the harness as-is (base) and 2 times with the candidate (cand), interleaved. No gain, or the candidate never reached the agent → kill it here; the confirm is not paid for.
- Confirm: replay every valid task 3 times per arm.
- Each run starts from a clean state (fresh clone or reset). The task's `check.sh` decides pass/fail - never your read of the transcript.
- Record exposure per run: rule loaded, skill listed, skill opened. Credit or blame the candidate only on tasks where it reached the agent.

## 6. Verdict (from the numbers, not vibes)

KEEP only when all four hold:

- Net win: runs gained minus runs lost >= 2 (a net of 1 is luck at k=3)
- No task collapses: a big drop on any one task → rerun that task once; kill if the drop repeats
- No task that never failed at base starts failing
- The candidate fired at least once (rule loaded or skill opened)

Anything else → KILL. Bury it in the graveyard with the gates it failed. A KEEP moves the candidate live, and its scores become the new baseline. Journal either verdict. After 2 cycles in a row without a KEEP, stop the loop.

## 7. Prune (monthly, and after every model change)

Same measurement in reverse. A stronger model may already do what an item teaches, so old verdicts expire.

1. Plan: the items to test (after a model change: all of them, largest always-on first), their tasks, runs, cost. Stop. Wait for approval.
2. One item at a time: replay its tasks 3 times with the harness and 3 times without it.
3. Verdicts:
   - **REJECT (keep it)**: removing it lost runs, collapsed a task, or broke a never-failing task.
   - **UNMEASURED (keep it)**: it never fired in the base runs. Text for a rare case looks like dead weight until the case arrives. Never delete what you never saw fire.
   - **ACCEPT (delete it)**: it fired, and removing it cost nothing measurable. Bury it in the graveyard.
4. Cheaper moves first, each its own measurement: narrow an always-on item used in one folder to `globs`; merge overlapping items.
5. Journal every verdict, and the model it ran on.

## Constraints

- One edit per cycle. The check decides, not impression.
- Never add or delete text the agent never saw. Unknown exposure blocks the verdict - a tooling failure is not dead weight.
- Always-on text is a budget: every line is paid on every turn. Prefer `globs`. If the always-on total grows past a screenful, prune before adding.
- Generic only: no employer names, private hosts, or work-only tools (`AGENTS.md`).

## Quality checklist

- [ ] Theme cleared the repeat bar and is not already covered
- [ ] Graveyard checked; candidate has a new name
- [ ] Surface chosen from the table (scoped rule by default)
- [ ] Every replay task's check validated in all three states
- [ ] Both arms replayed k times, interleaved, clean state per run
- [ ] Exposure recorded; the verdict reads only exposed tasks
- [ ] KEEP passed all four gates; KILL buried with reasons
- [ ] Journal appended

## Hand back

- Verdict, and the diff that landed (or the graveyard path)
- Sweep numbers: passes per arm per task, and where the item fired
- Journal path
- Next: the next theme, the prune due date, or "harness quiet - nothing to evolve"
