---
name: engineering-observability
description: Explicit observability doctrine for backend and frontend operational telemetry, including safe structured logging, redaction, correlation IDs, database error metadata, frontend log ingestion, capacity protection, retention, and sink failure. Use when work touches logs, metrics, traces, telemetry, alerts, audits, request IDs, or frontend logging.
---

# Engineering Observability

Use this companion skill with `$engineering-for-certainty` when work touches logging or telemetry. Apply the doctrine in order: centralized boundary, safe event contract, source adapters, correlation, ingestion and delivery, then verification.

## Scope

Apply this skill to:

- centralized logger setup or configuration
- backend logs, metrics, trace attributes, alerts, and audit records
- redaction, sanitization, retention, and log access
- correlation or request ID propagation
- database, provider, SDK, queue, job, or infrastructure failure logging
- frontend operational event emission and frontend-to-backend log ingestion

## Centralized Boundary

- Use one centralized observability abstraction for the app. Every persisted log, metric attribute, trace attribute, and audit record must pass through it.
- Inject the abstraction into services and repositories; do not import logger singletons into domain code as hidden global state.
- Accept typed Safe Log Events, not arbitrary context bags, raw request objects, raw errors, or spread objects.
- Define a closed schema per event family. Reject unknown keys and bound every string, array, nesting level, and payload size before forwarding.
- Keep logging out of business transactions and critical request completion. Logging failure must not change a successful business outcome unless the event is an explicitly authoritative audit requirement.
- When `NODE_ENV` is `production`, missing Better Stack configuration is a startup error. In `development`, default to console logging when Better Stack is absent. Treat an unset `NODE_ENV` as development only in a verified local development environment.
- If the primary sink fails, use a bounded sanitized stderr or console fallback and surface safe aggregate health or alert signals when the repo supports them. Never recursively log a logger failure through the failing path.

## Safe Log Event Contract

Prefer controlled fields such as:

```text
event, level, service, component, operation, stage, outcome,
errorClass, errorCode, correlationId, requestId, durationMs,
retryCount, environment, deploymentVersion
```

- Use stable event and error codes instead of free-form explanations.
- Never log secrets, API keys, tokens, cookies, authorization headers, session identifiers, connection strings, encryption material, or raw request and response bodies.
- Never log direct personal data, user-entered values, form fields, file contents, URLs with query strings, console arguments, storage contents, or arbitrary third-party payloads.
- Do not persist raw `Error.message`, `stack`, `cause`, Zod issues, provider errors, or serialized error objects. Diagnose with controlled operation, stage, class, code, correlation, and deployment metadata.
- Normalize error class through an allowlist. Do not trust an arbitrary constructor name or driver property.
- Allow an approved opaque actor or record identifier only when the event cannot meet its operational or audit purpose without it. Prefer keyed pseudonymization when joinability is needed without direct identity, and document retention and access implications.
- Sanitize CR, LF, delimiters, and other log-injection characters in every remaining string field before encoding.
- Treat redaction as defense in depth, not as permission to accept arbitrary input.

## Source-Specific Failure Adapters

Create small allowlist-based adapters for database, HTTP/provider, queue, job, validation, and platform failures. Unknown fields never pass through automatically.

For a recognized PostgreSQL driver error, allow only bounded normalized fields when present:

```text
db.sqlstate
db.schema
db.table
db.column
db.constraint
```

- Read `code`, `schema`, `table`, `column`, and `constraint` from machine-readable driver properties and normalize their log field names.
- Never parse error prose to recover metadata.
- Never log `message`, `stack`, `detail`, `hint`, query text, parameters, values, internal query or context, positions, or unknown driver properties.
- Omit an identifier when the application permits database object names derived from tenant or user data.
- Keep missing metadata normal; do not widen the allowlist to fill a diagnostic gap.

Output-validation failures and unexpected infrastructure failures must still produce a Safe Log Event through their owning source adapter.

## Correlation And Ownership

- Create or propagate correlation and request IDs at every external boundary.
- Preserve them through route, service, repository, queue, cron, webhook, job, provider, and logging flows.
- Derive actor, tenant, environment, and deployment context from trusted server state. Never trust a frontend payload to assert identity or authority.
- Generate authoritative security and audit events on the backend. Frontend events are advisory operational telemetry only.

## Frontend Log Ingestion

- Send frontend operational events through a dedicated backend endpoint. Never send client logs directly to Better Stack or another third-party sink.
- Treat the endpoint as an untrusted public telemetry boundary. Use a strict discriminated union of allowed event types with closed per-event context schemas.
- Reject unknown fields, wrong content types, oversized bodies, oversized batches, excessive string lengths, invalid timestamps, and unsupported event versions.
- Authenticate ingestion by default. If pre-auth telemetry is required, expose a reduced anonymous event set with lower limits and no client-provided identity.
- Apply rate limits and abuse protection before expensive work when the framework permits. Do not persist or log raw rate-limit keys such as IP addresses.
- Attach server-owned ingestion time, request ID, verified actor or tenant context, environment, and deployment metadata after validation.
- Reject raw messages, stacks, breadcrumbs, console arguments, storage values, request bodies, and URLs with query strings.
- Return a generic response. Invalid telemetry must not disclose validation internals or create another unsafe log containing the rejected payload.
- Prevent recursive ingestion: client or endpoint logging failures must not emit another frontend event through the same path.

## Delivery, Capacity, And Retention

- Treat frontend telemetry as best-effort. Use a bounded in-memory or dedicated telemetry queue, batch delivery, explicit timeouts, and a circuit breaker around the sink.
- Apply `$engineering-resilience` to queues, retries, timeouts, backoff, circuit breaking, concurrency, and sink outages.
- Return after bounded validation and enqueue rather than waiting synchronously for the external sink. Do not write frontend telemetry through the application database or a business transaction.
- Define a Telemetry Budget from peak clients, maximum events per client, maximum event size, sustained rate, burst rate, queue capacity, and provider-outage duration.
- Under pressure, drop or sample low-priority telemetry, increment safe aggregate drop metrics, and preserve business traffic. Never create an unbounded queue or retry storm.
- Restrict log access by least privilege, audit access when required, and define retention and deletion from operational, contractual, and privacy needs. Do not keep telemetry indefinitely by default.

## Testing And Review

- Test production versus development startup behavior for the configured sink where a valid seam exists.
- Test event schemas, unknown-key rejection, length and size limits, log-injection sanitization, and correlation propagation.
- Seed sensitive values into raw errors, requests, database parameters, provider responses, and frontend payloads; prove that none reaches logs, metrics, traces, audits, or fallbacks.
- Test every source adapter's allowed and forbidden fields, including missing database metadata.
- Test frontend ingestion authentication, reduced anonymous events when present, forged identity, PII smuggling, batch and body limits, rate limits, and generic failures.
- Load-test sustained traffic, bursts, malicious clients, repeated frontend-error loops, a full queue, sink timeouts, and provider outages against the Telemetry Budget.
- Test logger failure and fallback behavior without recursion, unbounded memory growth, or business-operation failure.
- Verify retention, reader access, and authoritative audit ownership when the change affects them.
- Before completion, verify every triggered check or record its omission and alternative assurance in the `$engineering-for-certainty` handoff.
