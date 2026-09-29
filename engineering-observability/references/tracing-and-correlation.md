# Tracing And Correlation

Read when tracing is triggered by the changed behavior or correlation ownership changes. Preserve the cross-signal privacy contract in `../SKILL.md`.

## Tracing Activation And Instrumentation

Require tracing when the work explicitly changes tracing; crosses services, processes, queues, webhooks, or other asynchronous boundaries; changes a critical multi-dependency path that logs and metrics cannot diagnose; or extends a path already traced by the repo. Do not add tracing to simple local work without one of these triggers.

- Preserve a compatible existing tracing stack. Otherwise use OpenTelemetry with W3C Trace Context.
- Prefer maintained framework and library instrumentation for supported HTTP, RPC, database, provider, and messaging boundaries. Add targeted manual spans for significant domain operations, unsupported integrations, or missing causal links; do not create one span per function.
- Create inbound server or consumer spans, outbound client or producer spans, and inject or extract context at every supported causal boundary on the affected path.
- Use parent context for direct causal continuation, links for fan-out, fan-in, batch, or otherwise non-parental relationships, and a root span for cron or background work with no valid upstream context.
- Let `$engineering-frontend` own where significant client spans begin. Activate browser or mobile tracing only for critical journeys that meet the same risk trigger.

## Trace Context And Span Contract

- Follow the applicable OpenTelemetry semantic convention for span names, kinds, attributes, and error status before defining custom fields.
- Keep span names stable and low-cardinality. Put approved execution identifiers in bounded attributes, never in span names, and namespace application-specific attributes.
- Review every automatic instrumentation's emitted attributes and capture settings. Allowlist or sanitize them before export; disable HTTP headers, query strings and userinfo, database statements and parameters, messaging payloads, RPC bodies, and provider request or response capture unless a narrower safe contract explicitly permits a field.
- Mark a span as `Error` with a predictable low-cardinality `error.type` when the instrumented operation throws, returns an error result, or otherwise fails its declared contract.
- Record a sanitized propagated exception at one owning boundary when it materially helps diagnosis. Never export raw messages, stacks, causes, bodies, query text, parameters, or third-party payloads.
- Represent handled expected domain outcomes with a bounded outcome attribute rather than error status. Leave successful status unset unless the applicable convention requires `Ok`.
- Treat incoming baggage as untrusted. Propagate only approved bounded keys, never put secrets or direct identity in baggage, and never use baggage for authentication or authorization.

## Correlation And Ownership

- Give each identifier one lifecycle: a request ID identifies one inbound request when the API convention needs it; trace and span IDs identify the connected execution and current operation; a separate correlation ID exists only for a business workflow spanning multiple traces or when an external contract requires it.
- Add active trace and span IDs to structured logs. Propagate trace context through causally connected boundaries; do not manufacture request IDs for queues, cron, or jobs.
- Derive actor, tenant, environment, and deployment context from trusted server state. Never trust a frontend payload to assert identity or authority.
- Generate authoritative security and audit events on the backend. Frontend events are advisory operational telemetry only.
