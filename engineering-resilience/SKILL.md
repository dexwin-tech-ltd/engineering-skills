---
name: engineering-resilience
description: Explicit resilience doctrine for work that touches external calls, retries, mutation safety, concurrency, queues, cron tasks, webhooks, or other async processing.
---

# Engineering Resilience

Use this companion skill with `$engineering-for-certainty` when work touches integration resilience, async processing, concurrency, idempotency, or failure handling beyond ordinary route/service structure.

## Scope

Apply this skill when the work includes:

- outbound HTTP or third-party API calls
- queues, cron tasks, jobs, or webhooks
- retries, backoff, or cancellation behavior
- transactions, idempotency, or concurrency control
- upstream timeout, rate-limit, or service-availability handling

## Doctrine

- Every outbound HTTP, queue, or third-party call must define an explicit timeout.
- Propagate cancellation and `AbortSignal` semantics through adapters when the platform supports them.
- Retry only safe or idempotent operations.
- Retries must be bounded, use exponential backoff with jitter, and define a clear stop condition.
- Do not hide infinite retries, silent fallbacks, or background retry loops inside adapters.
- Timeouts, cancellations, rate limits, and upstream failures must be mapped into explicit infrastructure error variants instead of leaking transport-library errors.
- Multi-step writes that must succeed together require explicit transaction boundaries.
- Duplicate-prone externally triggered writes require idempotency protection.
- Concurrency behavior must be explicit. Use transactions, locks, optimistic concurrency, unique constraints, or equivalent safeguards instead of assuming single-writer behavior.
- Jobs, queues, cron tasks, and webhooks must validate payloads at their boundary with Zod.
- Async handlers must be idempotent where retries or duplicate delivery are possible.
- Retry policy, backoff strategy, and dead-letter behavior must be explicit for async work.
- Important async flows require integration or end-to-end coverage for processing and failure cases.

## Testing and Review

- Test timeout, retry, and error-mapping behavior for changed integrations.
- Test idempotency, duplicate-delivery handling, or concurrency protection where the change affects writes.
- Test retry/dead-letter behavior for changed async flows when a valid seam exists.
- Verify no silent infinite retry loops or fallback behavior were introduced.
