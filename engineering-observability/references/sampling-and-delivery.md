# Sampling And Delivery

Read for tracing or changed sink, exporter, queue, sampling, capacity, timeout, or retention behavior. Preserve the common signal-safety and verification rules in `../SKILL.md`.

## Sampling And Export

- Configure sampling explicitly by environment. Development and tests may sample every trace at bounded volume; production must declare and justify its root policy from measured traffic, cost, and diagnostic needs.
- Use parent-based sampling so child spans respect the upstream decision. Use Collector-side tail sampling for slow or failed traces only when the deployment supports it and the policy is documented.
- Keep all structured operational-event and trace exporter credentials and routing on the controlled backend. The only permitted client key exception is a dedicated crash-reporting SDK that satisfies [the direct crash-reporting conditions](client-telemetry.md#direct-crash-reporting-exception). Preserve a compatible server-owned exporter; otherwise prefer OTLP through an OpenTelemetry Collector, while allowing direct backend-to-provider export when the same safety and capacity rules hold.
- Treat missing or structurally invalid required production tracing configuration as a startup failure. After successful startup, tracing and exporter failures must fail open and never fail a business operation.
- Export asynchronously through bounded queues and batches with explicit timeouts. Bound flush and shutdown; drop spans after capacity is exhausted and surface only safe aggregate failure and drop signals.

## Delivery, Capacity, And Retention

- Treat telemetry as best-effort except when an audit event is explicitly authoritative. Use bounded in-memory or dedicated telemetry queues, batch delivery, explicit timeouts, and a circuit breaker around external sinks.
- Apply `$engineering-resilience` to queues, retries, timeouts, backoff, circuit breaking, concurrency, and sink outages.
- Return after bounded validation and enqueue rather than waiting synchronously for the external sink. Do not write frontend telemetry through the application database or a business transaction.
- Define one observability budget with signal-specific limits. For traces include root sampling, sampled throughput, span attributes/events/links/baggage, attribute cardinality and length, queue and batch capacity, flush interval, export timeout, provider-outage duration, and dropped-span behavior.
- For frontend events include peak clients, maximum events and bytes per client, sustained and burst rate, queue capacity, and provider-outage duration.
- Under pressure, drop or sample low-priority telemetry, increment safe aggregate drop metrics, and preserve business traffic. Never create an unbounded queue or retry storm.
- Give authorized project engineering/support investigators routine searchable, read-only log access within their project scope; authorized agents use approved tools under the same scope. Do not require incident-by-incident elevated access merely to read rich diagnostics. Keep access administration, deletion, and collection configuration separate. Audit access when required. Log access does not authorize copying customer information into other services; shared findings retain only necessary evidence.
- Default operational-log retention to **one year**, including diagnostic details and permitted personal identifiers, not just event codes/metadata. Projects may document a different concrete duration based on delayed reports, periodic workflows, investigation lookback, volume/cost, and applicable constraints. Verify the actual provider settings and deletion behavior; this policy does not itself configure retention or establish affordability.
- Apply the declared operational-log lifetime to searchable copies, archives, and exports, measured from the original event rather than the last copy or move. Moving evidence into cheaper storage does not restart retention. Define per-signal retention for metrics, traces, authoritative audits, and dedicated crash reporting; the one-year log default does not silently override those contracts or special short-lived Test Message Sinks. If another signal is used to store the operational log's diagnostic copy, it follows that log's declared lifetime.
