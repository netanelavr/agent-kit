---
name: kit-debug-prod
description: Investigate production bugs using logs, traces, error trackers, and metrics. Use when the user reports errors, user/session/trace IDs, timeframes, or status codes. Requires connected tools and/or a team `wire-observability.md`.
disable-model-invocation: true
---

# Debug Prod

**READ-ONLY** unless the user explicitly asks to mutate (close issues, change flags, etc.).

## Team wiring (`wire-observability.md`)

Before querying, look for team wiring next to this skill or in the consuming repo:

- `wire-observability.md`
- `references/wire-observability.md`
- host-specific notes the user points to

If present, read it for: product/service log filters, cloud projects, trace attribute names, default error-tracker project, MCP endpoints, and credential **locations** (never paste secrets into chat).

If absent: ask which sources are available, or proceed only with tools already connected — and say what you could not check.

See `references/wire-observability.example.md` for a template.

## Triggers

- Production bug / error, status codes (403, 500, …)
- User ID, session ID, trace ID, domain / tenant
- Timeframe (“last 2 hours”, “yesterday”)
- Links to logs, traces, or error-tracker issues

## Data sources (adapters)

Use what exists; skip the rest. Prefer the team’s primary backend source + one corroborating source.

| Category | Typical tools | Use for |
|---|---|---|
| AI / LLM traces | Langfuse CLI, host traces UI | Tool calls, model I/O, AI pipeline errors |
| Backend logs | Cloud logging CLI (e.g. `gcloud logging`), Datadog, … | API errors, request logs |
| Distributed traces | Cloud Trace, Jaeger, Tempo, … | Latency, span waterfall |
| Error tracker | Sentry MCP/UI, similar | Unhandled exceptions, stack traces |
| Tickets | Jira / Linear MCP | Existing bugs, severity |
| Product analytics | Mixpanel / Amplitude MCP | Scope, feature usage around the incident |
| Metrics | Prometheus/Grafana MCP | Rates, saturation, alerts |

## Investigation flow

**Always cross-reference ≥2 sources** unless the user names a single tool only.

1. **Extract context** — ids, timeframe, error pattern, environment (prod/staging).
2. **Query in parallel** — traces/logs + error tracker (and others as relevant).
3. **Cross-reference** — same user/tenant + overlapping timestamps.
4. **Quantify** — isolated (1 user) vs widespread (many).

### Cross-reference sketch

```text
1. Traces → error → user_id, timestamp, message
2. Logs → same id/domain near that timestamp
3. Distributed trace → waterfall / latency (if perf-related)
4. Traces/logs → same error pattern for other users?
5. Error tracker → related frontend/unhandled exceptions
```

## Logs (generic)

Always apply the **product/service filter** from the team wiring file on every query.

```bash
# Example shape (GCP); replace with your provider
gcloud logging read '<PRODUCT_FILTER> AND <YOUR_FILTERS>' \
  --project=<CLOUD_PROJECT> \
  --limit=50 \
  --format=json \
  --freshness=<TIME>
```

Freshness examples: `4h`, `24h`, `48h`, `7d`. Re-auth if the CLI says credentials expired.

## Patterns

| Signal | Likely direction |
|---|---|
| Auth / forbidden | Permission, session, token |
| Timeouts | Dependency / slow upstream |
| Quota / limit | Plan or rate limit |
| Client validation | Bad input / stale client |
| One user only | Config, cache, session, tenancy |
| Many users | Code bug or infra |
| One feature | Recent deploy / flag |

## Error trackers

Unhandled exceptions usually land in the tracker; handled errors often appear only as HTTP failures in logs. Org/project slugs come from the team wiring file. On 403: ask for access — do not invent data.

## Credentials

- Load secrets from env / `.env` files the user already uses — **never** echo secret values into the transcript.
- Prefer MCP OAuth over pasting tokens into commands.

## Output

```markdown
## Investigation Summary

**Context:** User / tenant, environment, timeframe

| Source | Finding |
|--------|---------|
| … | … |

**Root cause:** (High/Med/Low confidence) …
**Scope:** N users / isolated
**Immediate relief:** 1. … 2. …
**Suggested fix:** …
**Unproven / not checked:** …
```

## Troubleshooting

| Issue | Try |
|---|---|
| No log hits | Widen freshness; verify product filter |
| Auth expired | Re-login for that CLI/MCP |
| Huge CLI output | Write to a file, then grep |
| Tracker 403 | Request org/project access |

## Constraints

- No employer-specific project IDs or hostnames in this skill file — those belong in the team wiring file.
- If proof is incomplete, say **unproven** (pair with `kit-prove` when claiming a fix).
