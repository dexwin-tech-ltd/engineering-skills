---
name: engineering-resilience
description: Explicit resilience doctrine. Use when work touches external calls, timeouts, retries, mutation safety, transactions, idempotency, concurrency, queues, cron tasks, webhooks, jobs, or other async processing.
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

### Timeouts and Cancellation

- Every outbound HTTP, queue, or third-party call must define an explicit timeout.
- Propagate cancellation and `AbortSignal` semantics through adapters when the platform supports them.

### Retry Rules

- Retry only operations that are idempotent or explicitly marked as safe, such as operations that do not introduce additional side effects on repeated execution.
- Retries must be bounded, use exponential backoff with jitter, and define a clear stop condition.
- Do not hide infinite retries, silent fallbacks, or background retry loops inside adapters.

### Error Mapping and Write Safety

- Timeouts, cancellations, rate limits, and upstream failures must be mapped into explicit infrastructure error variants instead of leaking transport-library errors.
- The boundary that performs an external call owns the timeout, cancellation, rate-limit, provider, and availability failures introduced by that call.
- Infrastructure error variants may be reusable atoms, but they are not an ambient union inherited by unrelated operations.
- A calling operation may widen its error contract only with downstream variants it deliberately exposes and failures introduced by its own orchestration.
- Operations that do not perform or depend on an external call must not claim that call's infrastructure failures in their result type.
- Multi-step writes that must succeed together require explicit transaction boundaries.
- Duplicate-prone externally triggered writes require idempotency protection.
- Concurrency behavior must be explicit. Use transactions, locks, optimistic concurrency, unique constraints, or equivalent safeguards instead of assuming single-writer behavior.

### Async Processing

- Jobs, queues, cron tasks, and webhooks must validate payloads at their boundary with Zod.
- Async handlers must be idempotent where retries or duplicate delivery are possible.
- Retry policy, backoff strategy, and dead-letter behavior must be explicit for async work.
- Important async flows require integration or end-to-end coverage for processing and failure cases.

## Testing and Review

- Test timeout, retry, and error-mapping behavior for changed integrations.
- Test idempotency, duplicate-delivery handling, or concurrency protection where the change affects writes.
- Test retry/dead-letter behavior for changed async flows when a valid seam exists.
- Verify no silent infinite retry loops or fallback behavior were introduced.
- Verify that each resilience error appears only in contracts whose execution path can introduce it.
- Before completion, verify every triggered check or record its omission and alternative assurance in the `$engineering-for-certainty` handoff.
