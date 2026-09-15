# wire-observability.md (example — copy into the consuming repo)

Fill in your team values. Do not commit production secrets here; point at env var **names** or secret stores.

## Product filter

- Log filter field/value: e.g. `resource.labels.service_name="my-api"`
- Default environment: `production`

## Cloud

- Logs project: `my-gcp-project`
- Trace console / project: `…`
- Useful trace attributes: `enduser.id`, `http.route`, …

## AI traces (if any)

- Host / project
- CLI: e.g. `npx langfuse-cli` with env vars `…_PUBLIC_KEY`, `…_SECRET_KEY`, `…_HOST`
- How to select production vs staging keys

## Error tracker

- Org / region / default project
- MCP or UI entrypoint

## Analytics / metrics (optional)

- Mixpanel/Amplitude project
- Grafana datasource / common PromQL starters

## MCP notes

List which MCP servers this repo expects (Sentry, Atlassian, …) and where the host config lives (`~/.cursor/mcp.json`, Claude MCP, …).
