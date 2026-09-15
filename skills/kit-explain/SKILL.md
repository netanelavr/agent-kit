---
name: kit-explain
description: Problem / Solution / How it works plus one compact mermaid for a PR or short HLD. Use to explain a change — not for ASCII brainstorm boards or full design grilling.
disable-model-invocation: true
---

# Explain

Reviewer-skim shape: **Problem / Solution / How it works** + one small mermaid.

## What this is NOT

- Not a substitute for `kit-challenge-design` or `kit-plan`
- Not a gallery of diagrams — **one** diagram unless the user asks for more

## Output shape

1. **Problem** — 2–4 sentences.
2. **Solution** — what we do and what we don’t.
3. **How it works** — short steps.
4. **Mermaid** — prefer `sequenceDiagram` for request/cred/flows; `flowchart` for branching decisions. Minimal nodes/labels.
5. Optional: open questions **with proposed options** (no bare open questions).

## Rules

- Diagram must match the prose — no decorative boxes.
- If the diagram exposes disagreement or a missing decision → **stop and ask** before more writing.
- For publish targets that don’t render mermaid, say so and offer image export — keep mermaid as source when in-repo.
- Generic names only in public examples (no employer hosts/paths).
