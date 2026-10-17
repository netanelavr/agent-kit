---
name: kit-debate-decision-tree
description: Run a heated persona debate on a topic or design doc, build a wide and deep decision tree, simulate node constraints and the owner's picks, and write two new files - the debate plus tree, and plain "We decided to X because Y." sentences. Use when you want every fork of a design argued out and a decision draft, without touching the source doc.
disable-model-invocation: true
---

# Debate Decision Tree

Argue a topic or design doc from opposing seats. Turn the argument into a decision tree. Simulate what each branch costs and gains, simulate the owner's picks, and write the result as plain decision sentences.

## What this is NOT

- Not a one-question-at-a-time grilling with the user (`kit-challenge-design`). This runs on its own and simulates the answers.
- Not a pre-code concept alignment (`kit-plan`)
- Not the living log of real decisions on a feature (`kit-decisions`). Simulated picks go there only after the owner confirms them.
- Not a short explainer with one diagram (`kit-explain`)

## Input

`kit-debate-decision-tree [PATH_OR_TOPIC] [OUTPUT_DIR]`

- **PATH**: the design doc or spec. Read it. Never edit, rename, or reformat it.
- **TOPIC**: a short topic in chat. Treat the conversation as the source.
- **OUTPUT_DIR**: optional. Default is the input's directory, or the current directory for a topic.

If the target files already exist, do not overwrite them. Add a numeric suffix (`-2`, `-3`) and say so.

## 1. Read and extract facts

Read the whole input. List the facts the debate can cite: goals, constraints, numbers, named components, open questions, non-goals. Give each a short anchor (section heading or quote). Personas argue from these facts. A claim with no source is marked `(assumption)`.

## 2. Cast personas

Pick 6-10 personas whose interests really collide. Start from this pool and add domain seats the doc needs:

| Persona | Pushes for |
|---|---|
| Staff engineer | long-term architecture, fewer moving parts |
| Security | least privilege, threat surface, audit |
| Eng manager / cost | headcount, cloud spend, delivery date |
| Product | user value, speed to first release |
| SRE | operability, on-call load, rollback |
| QA | testability, repro, regression risk |
| Junior dev | onboarding cost, clarity, local dev |

Give each one line: what they want and what they fear. At least two pairs must be in direct conflict.

## 3. Heated debate

Go topic by topic, in the doc's order. For each topic:

- Opening positions, one per persona who cares
- At least two rounds of rebuttal that answer a named persona's point, not a strawman
- Every factual claim cites an anchor from step 1, or is marked `(assumption)`
- End the topic with the open forks it produced

Keep it sharp. Disagreement is the point. No persona wins by vote count.

## 4. Decision tree

Turn each fork into a node: `D<n>` with a question, 2-4 options, and child nodes the choice opens.

Targets (state the counts you hit):

- **Width**: at least 8 top-level decision areas, or every area the doc covers if fewer. 2-4 options per node.
- **Depth**: at least 4 levels on the main branches. Follow a branch until it hits a leaf: a concrete choice with nothing left to decide.

Write it twice:

- **Mermaid**: one overview `flowchart TD` of areas and root nodes, then one subtree diagram per area. Keep each diagram under about 30 nodes. Split further when bigger.
- **Indented outline**: the full tree with node IDs, so it reads without a renderer.

## 5. Simulate constraints and benefits

At each node, and on each option, add what happens if you take it: cost, risk, unlock, blocker, dependency on another node. Tag every line `[SIM]`. Note when one node's pick prunes or forces another (`D3.b forces D7.a`).

## 6. Simulate the owner's decisions

Pick an option at every node as the owner would.

- Source the owner's preferences from memory, a profile, or past decisions in the repo when available. Cite which.
- With no source, write the assumed preferences in a short list at the top and use only those.
- Tag every pick `[SIM]` with a one-line reason tied to a preference and a `[SIM]` constraint.
- When the preferences do not settle a node, mark it **Open** with the deciding question. Do not force a pick.

## 7. Write two new files

Name them from the input's base name. For a topic, use a short kebab-case slug.

**`<doc>-debate-tree.md`** (supporting file):

```md
# <Doc title> - debate and decision tree

Source: <path> (unchanged)
Simulated: personas, constraints, and owner picks are simulations, not real decisions.

## Facts used
- F1 <fact> (<anchor>)

## Personas
- <Persona> - wants ..., fears ...

## Debate
### <Topic>
**<Persona>:** ... (F2)
**<Persona>:** rebuts <Persona>: ... (assumption)
Open forks: D4, D5

## Owner preferences used
- <preference> (<source or "assumed">)

## Decision tree
Width: <n> areas, depth: <n> levels, nodes: <n>
### Overview
<mermaid>
### <Area> subtree
<mermaid>
### Outline
- D1 <question>
  - a) <option> [SIM] <constraint/benefit> -> D1.1
    - D1.1 <question>
      - ...
  - b) <option> [SIM] ...
  - Pick [SIM]: a, because ...
```

**`<doc>-decisions.md`** (the result):

```md
# <Doc title> - decisions (simulated)

Source: <path>. Full reasoning: <doc>-debate-tree.md.
These are simulated decisions. Confirm before acting on them.

## <Area>
- We decided to <X> because <Y>. (D1, D1.1)
- We decided not to <X> because <Y>. (D2)

## Open
- We have not decided <X>. It depends on <question>. (D9)
```

One sentence per decision, plain language, no jargon the doc does not use. Each line ends with the node IDs it comes from. Group by area, in the doc's order.

## Worked example

If `references/` in this skill holds a worked example, read it before step 2 and match its depth. Do not copy its content.

## Quality checklist

Check before you hand back:

- [ ] Input file unchanged (`git diff --stat <path>` is empty, or the mtime is the same)
- [ ] Two new files written; nothing existing overwritten
- [ ] 6-10 personas, at least two direct conflicts, at least two rebuttal rounds per topic
- [ ] Facts cite an anchor or say `(assumption)`
- [ ] Width and depth targets met, counts stated in the tree file
- [ ] Mermaid split into subtrees; the outline covers every node
- [ ] Every constraint, benefit, and owner pick is tagged `[SIM]`
- [ ] Every decision sentence maps to a tree node ID, and every leaf pick has a sentence or an **Open** line
- [ ] Owner preferences cited or listed as assumptions

## Hand back

- Paths of the two new files
- Personas, width, depth, and node counts
- Number of decisions and open items
- Which preferences were assumed
