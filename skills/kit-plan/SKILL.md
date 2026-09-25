---
name: kit-plan
description: Align on a buildable concept, then wait for explicit approval before coding. Use for complex or fuzzy work — includes three ticket readings when the ask is ambiguous.
disable-model-invocation: true
---

# Plan

Reach a **shared concept**, write a short decision log, and **stop**. Code only after explicit approval.

You stop plan-mode from inventing the wrong thing, and you stop coding from starting too early.

## What this is NOT

- Not grilling a finished design against glossary/ADRs (`kit-challenge-design`)
- Not the living log of a feature already in progress (`kit-decisions`)
- Not choosing the smallest path of an agreed outcome (`kit-minimize`)
- Not a PRD factory, issue tree, or future-proof architecture pass

If they already have a spec to stress-test → `kit-challenge-design`.
Once approved and they want the thinnest diff → `kit-minimize`.

## Effort

Effort = **how many consequential forks you open**, not how much trivia you ask.

Accept `kit-plan light|standard|deep`. If omitted, auto-calibrate and say the tier in one line.

| Tier | When | Forks |
|---|---|---|
| **light** | Local change, existing pattern, clear done | 0–2 |
| **standard** | Feature-shaped, fuzzy done, crosses a boundary | 3–7 batched |
| **deep** | New nouns, several valid architectures, or repeated misalignment | Walk the real forks |

Higher effort means more **scope / boundary / correctness / sequencing** questions — never more folder or button-copy questions.

## Question filter

Ask only if the answer would change at least one of:

- what we build vs skip
- system boundary or data model
- failure mode / correctness bar
- sequencing (what ships first)
- whether we are overbuilding for scale

Otherwise default from the codebase and say what you assumed.
Prefer 2–4 sharp options over serial ping-pong.

## Flow

### 1. Place the ask (three readings when fuzzy)

One sentence: local fix, workflow change, or new domain concept.

If the ask could still be three different tickets:

1. Write **Read A / B / C** — three distinct interpretations (goal, scope, non-goals). Keep each short.
2. **Compare** — shared intent vs divergence.
3. **Confidence**
   - High + shared → pick the reading; say why the others lose.
   - Low / unclear → **stop**. Ask 2–4 targeted questions. Do not invent a direction.

If you cannot name one shared outcome sentence that all three reads support, you are not ready to plan or code. Don’t collapse to one reading before compare.

### 2. Align the concept

Walk forks in dependency order. Capture as you go:

- **done** — one sentence
- **in / out of scope**
- **boundaries and interfaces**
- **defaults you assumed**
- **open risks**

Search the repo before proposing new files or layers. Reuse beats invention.
Stop when further questions no longer change the concept.

### 3. Scale-gate (always)

Any “yes we need fancy X” needs evidence.

- What limit are we near, and what proves it?
- Next order of magnitude, or fantasy scale?
- Specific problem, or a toolbox for hypothetical futures?
- Can we defer caches, new frameworks, and broad abstractions?

If the concept includes premature architecture, strip it or make the user accept the cost in writing.
Bias: specific > generic, today’s runway > status architecture.

### 4. Decision log (not a plan)

Default output is short:

- shared concept (5–10 lines)
- key decisions
- assumed defaults
- scale-gate outcome
- remaining non-guessables
- recommended next move (`go` / `lock it` / edit a fork)

**Do not** auto-write a PRD, task tree, or “comprehensive planning document.”
Only expand into a written plan/issues when they say to lock it.

### 5. Wait

Present the log. Ask for approval (`go` / `lock it` / numbered edits).
**Do not implement** until they explicitly approve.

### 6. After approval

Implement only the approved concept.
Prefer the smallest shape (`kit-minimize` if the path is still wide).
Prove claims with `kit-prove`. Do not add extensibility nobody approved.

## Do / Don't

| Do | Don't |
|---|---|
| "Auto → standard. Two forks: reuse Invoice vs new CreditNote; sync vs async. Rest I default from billing." | Forty questions about names, folders, labels |
| "Scale-gate fail: generic cache with no evidence we are near a limit. Defer." | Let sharding / “for later” ride along because it sounds responsible |
| Decision log, then wait | Start coding because the questions slowed down |
| On `light` for a null-check: confirm repro + acceptance, ask to go | Deep-grill a one-line fix |
| Three short readings when the ticket is ambiguous | Collapse to one story before comparing |
