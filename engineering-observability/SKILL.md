---
name: engineering-observability
description: Explicit observability doctrine with clear logging, redaction, correlation ID, and ingestion rules for work that touches telemetry.
---

# Engineering Observability

Use this companion skill with `$engineering-for-certainty` when work touches logging or telemetry. Apply the doctrine in section order: logger setup first, then redaction/failure logging, then correlation/ingestion.

## Scope

Apply this skill when the work includes any of the following:

- centralized logger setup or changes
- Better Stack integration or logger configuration
- log redaction and sanitization
- correlation/request ID propagation
- frontend-to-backend log ingestion

## Doctrine

### Logger Setup

- Use one centralized logger abstraction for the app.
- When `NODE_ENV` is `production`, missing Better Stack configuration is a startup error. When `NODE_ENV` is `development`, default to console logging if Better Stack is not configured. When `NODE_ENV` is unset, treat it as development behavior only for local development environments.
- Inject the centralized logger into services and repositories as an explicit dependency; do not import logger singletons into domain code as hidden global state.
- If the centralized logger cannot write to its primary sink, fall back to a safe local stderr or console error path with sanitized context and surface the failure through the repo's existing alerting or health-check mechanism when one exists.

### Redaction and Failure Logging

- Never log secrets, API keys, tokens, cookies, auth headers, session identifiers, or raw request/response bodies.
- Redact unnecessary PII before anything reaches the centralized logger. Prefer shared redaction/sanitization helpers over ad hoc string cleanup.
- Output-validation failures and unexpected infrastructure failures must be logged through the centralized logger with sanitized context.

### Correlation and Ingestion

- Create or propagate correlation/request IDs at every external boundary.
- Preserve correlation/request IDs through route, service, repository, queue, cron, webhook, and logging flows.
- Prefer structured logs with queryable fields for correlation ID, operation name, and error class.
- Frontend apps must send logs through a dedicated backend logging endpoint. Do not send frontend logs directly to Better Stack or other third-party sinks from the client.
- Backend logging endpoints must validate incoming log payloads, require sanitized client context, and forward accepted logs through the centralized backend logger.

## Testing and Review

- Test startup behavior for Better Stack configuration in production vs development where a valid seam exists.
- Test correlation/request ID propagation on the changed path.
- Test redaction/sanitization behavior when logging changes.
- Verify frontend log ingestion validates payloads before forwarding.
