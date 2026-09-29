# Client Observability

Read when frontend code emits operational events or spans. Also apply `$engineering-observability`.

## Client Observability

Apply `$engineering-observability` whenever frontend code emits operational telemetry. It owns signal contracts, propagation, ingestion, export, privacy, retention, and capacity; this skill owns where and when meaningful client events and spans begin.

### Operational Events

Emit a Frontend Operational Event only at an owned boundary or meaningful transition:

- an application error boundary or unhandled client failure
- an API adapter transport failure after its bounded retry policy
- an API response that violates its declared contract
- an unexpected auth or session transition
- an unexpected failure of a critical user workflow
- an offline or online transition that materially changes an active workflow
- an aggregate client-logger health signal such as a dropped-event count

Do not log component renders, routine effects, keystrokes, form values, every request or response, every navigation or click, expected validation or domain failures without a named operational requirement, product analytics, raw errors, stacks, console arguments, storage values, arbitrary objects, or URLs with query strings.

- Emit once per failure occurrence or meaningful state transition, not once per render, retry, or observer.
- Deduplicate repeated signatures in a bounded window and aggregate repeated low-value events into counts.
- Disable debug events in production and sample informational events when complete collection is unnecessary.
- Use a bounded in-memory client queue, explicit event-count or byte thresholds, a maximum flush interval, and asynchronous non-blocking delivery.
- Bound retries with backoff and jitter. Drop telemetry when the queue is full; never block user actions, grow memory without limit, or recursively log delivery failure.
- Treat every severity as rate-limited. A critical label must not permit an event storm.
- Keep product analytics separate and generate authoritative security or audit truth on the backend.

### Client Tracing

- Trace only a critical user journey that meets `$engineering-observability`'s risk trigger; do not trace every render, effect, click, navigation, or request.
- Use supported automatic instrumentation plus targeted manual spans at meaningful journey and adapter boundaries.
- Propagate W3C trace context only to an explicit allowlist of first-party or trusted origins. Context propagation is not telemetry export.
- Export client span data only through the controlled backend trace endpoint. Never send it directly to an OpenTelemetry Collector, Better Stack, or another provider, and never ship exporter credentials to the client.
- Never attach auth data, form values, user-entered content, raw URLs or query strings, direct identity, or arbitrary baggage to client spans.
- Apply client-specific sampling, queue, payload, and privacy limits. Keep product analytics separate from operational traces.
