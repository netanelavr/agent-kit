---
name: kit-trace-metrics
description: Keep aggregate data work honest — define the question, trace every number to a query, separate fact from interpretation. Use for counts, rates, trends, funnels, reports, or “why did metric X move”. Not for single-user prod debugging (use kit-debug-prod).
disable-model-invocation: true
---

# Trace Metrics

Anti-hallucination workflow for **aggregate** analytics. Source-agnostic (warehouse, logs, Mixpanel, BigQuery, …).

## Can it be generic?

Yes. The discipline is generic; connectors differ. Team wiring file may list default sources/exclusions — optional.

## What this is NOT

- Not single-session / single-user incident debug (`kit-debug-prod`)
- Not inventing charts without query provenance

## Phase 1 — Define (before querying)

Confirm with the user if missing:

- Question + why it matters
- Environment (default prod — confirm)
- Timeframe (never silently “all time”)
- Exclusions (internal/test users — ask)
- Sampling (full set default; ask before subsetting)

Write a short **ANALYSIS DEFINITION** block, then proceed.

## Phase 2 — Query with provenance

- Prefer primary source of truth for that metric
- Save/reproduce the exact query (or export path)
- Every number in the answer must point to a query/export artifact

## Phase 3 — Validate

- Spot-check totals / row counts
- Sanity vs a known baseline if available
- If an LLM categorizes rows, separate **model labels** from **raw facts**

## Phase 4 — Report

| Fact (with source) | Interpretation (labeled) |
|---|---|
| … | … |

State confidence and what was **not** checked. No fabricated metrics — if you lack access, say so (`kit-prove` mindset).
