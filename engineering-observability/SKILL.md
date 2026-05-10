---
name: engineering-observability
description: Explicit observability and logging doctrine for coding work that touches centralized logging, Better Stack integration, correlation IDs, redaction, or frontend log ingestion.
---

# Engineering Observability

Use this companion skill with `$engineering-for-certainty` when work touches logging, telemetry, correlation/request IDs, Better Stack integration, or frontend log ingestion.

## Scope

Apply this skill when the work includes any of the following:

- centralized logger setup or changes
- Better Stack integration or logger configuration
- log redaction and sanitization
- correlation/request ID propagation
- frontend-to-backend log ingestion

## Doctrine

- Use one centralized logger abstraction for the app.
- In production, missing Better Stack configuration is a startup error. In development, default to console logging when Better Stack is not configured.
- Inject the centralized logger into services and repositories as an explicit dependency; do not import logger singletons into domain code as hidden global state.
- Never log secrets, API keys, tokens, cookies, auth headers, session identifiers, or raw request/response bodies.
- Redact unnecessary PII before anything reaches the centralized logger. Prefer shared redaction/sanitization helpers over ad hoc string cleanup.
- Output-validation failures and unexpected infrastructure failures must be logged through the centralized logger with sanitized context.
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
