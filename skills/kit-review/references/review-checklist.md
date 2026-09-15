# Review Checklist

Generic review checklist (inspired by automated local/PR review prompts).  
Repo-specific rules (DTO decorators, package layout, test paths) belong in the **consuming** repo — not here.

Use with `kit-review`. Prefer **fewer, higher-severity** findings. Skip style nits tooling already covers.

## Calibration

For each finding ask: if this shipped as-is, would we regret missing it?

- Production break / leak / data loss → report
- “Maybe under rare conditions” → usually skip
- “Could be nicer” → skip unless it creates real maintenance pain

False positives erode trust. Aim for high confidence of real impact.

## Priority (fill slots from the top)

### P0 — Critical
- Security: secrets in source, injection, unsanitized dangerous sinks, authz/IDOR on user-supplied IDs
- Data loss/corruption: unbounded delete/overwrite without guardrails
- Error handling gaps that can crash the process or leak stack traces on critical paths

### P1 — High
- Significant performance (leaks, N+1, unbounded work on hot paths)
- Missing tests for new/modified **critical** paths
- Public API typed as `any` / missing types where callers depend on them

### P2 — Medium
- Maintainability issues that raise cognitive load
- Fragile React patterns (cascading `useEffect`, stale closures from missing deps)
- Magic strings duplicated 3+, type duplication vs package exports, needlessly complex conditionals
- Long functions doing multiple jobs (suggest extract with clear names)

### P3 — Low (only if higher slots are empty)
- Visual changes without screenshot/Loom note when the diff clearly changes UI
- Minor naming/formatting (if not enforced by lint)

## Pattern checks (scan the diff)

1. **Magic strings** — repeated literals → named constants
2. **Type duplication** — custom types that already exist in a dependency
3. **Conditionals** — nest/invert → early return
4. **Error handling** — async without handling; tests that never hit failure paths
5. **Long functions** — multi-concern blocks → helpers
6. **String building** — prefer template literals for multi-line/dynamic strings
7. **LLM / structured output** — prefer schema enums over “return success|error” in prose
8. **UI docs** — significant visual JSX/CSS changes → note evidence for PR (logic-only `.tsx` exempt)
9. **Prompts / agent instructions** — changes to system prompts or agent rules need regression care
10. **Listeners / timers** — arrow closures on long-lived listeners; `setTimeout`/`setInterval` without cleanup
11. **Schema / stored data** — renames/removals need legacy handling or migration
12. **Hook deps** — missing `useCallback`/`useEffect` dependencies → stale closures
13. **Null safety** — `Map.get` / `find` / optionals accessed without checks
14. **Prop drilling / hardcoded children** — 3+ unused forward layers; prefer composition/slots when it simplifies
15. **State ownership** — state only used by one child should live there

## Boy Scouts

Critical issues in **touched files** but **outside** the changed hunks: mark as Boy Scout cleanup opportunities — do not block the author self-check unless the user asked for drive-by fixes.

## Test expectations (generic)

- Prefer co-located unit/component tests for new logic
- Skip nagging for: pure types, config, migrations, generated files
- Consuming repo may override paths/conventions in its own wiring note

## Output shape

1. **Changed files** — short list
2. **Critical (must fix before publish)**
3. **Improvements** (optional)
4. **Missing tests** (critical paths only)
5. **Ready?** — one-line go / fix-first (author) or approve / request changes (reviewer)

Be brief. Specific fixes over essays.
