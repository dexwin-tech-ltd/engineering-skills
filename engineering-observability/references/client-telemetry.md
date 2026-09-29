# Client Telemetry

Read for frontend operational events, client spans or ingestion, or a direct crash-reporting SDK. Preserve backend authority and the common verification contract in `../SKILL.md`.

## Client Telemetry Ingestion

- Send frontend structured operational events and trace payloads through dedicated controlled backend endpoints. Never export those signals directly to Better Stack, an OpenTelemetry Collector, or another third-party sink.
- Treat each endpoint as an untrusted public telemetry boundary. Use a strict discriminated union for operational events and a closed, bounded client trace contract. Permit OTLP only when the receiver validates and reduces it to that restricted contract before export.
- Reject unknown fields, wrong content types, unsupported versions, oversized bodies or batches, excessive span or event counts, excessive attributes or links, excessive string lengths, and invalid or implausible timestamps.
- Allowlist accepted span names, kinds, resource fields, attributes, events, links, and baggage-derived fields. Reject or discard client-owned identity, tenant, environment, deployment, service authority, and exporter-routing metadata.
- Authenticate ingestion by default. If pre-auth telemetry is required, expose a reduced anonymous event set with lower limits and no client-provided identity.
- Apply rate limits and abuse protection before expensive decoding or forwarding when the framework permits. Do not persist or log raw rate-limit keys such as IP addresses.
- Attach server-owned ingestion time, request ID, verified actor or tenant context, environment, and deployment metadata after validation.
- Reject raw messages, stacks, breadcrumbs, console arguments, storage values, request bodies, form values, and URLs with query strings.
- Return a generic response. Invalid telemetry must not disclose validation internals or create another unsafe log containing the rejected payload.
- Prevent recursive ingestion: client or endpoint logging failures must not emit another frontend event through the same path.

### Direct Crash-Reporting Exception

A project may allow a dedicated frontend crash/error-reporting SDK to export
directly to its vendor only for unexpected or unhandled exception capture. This
is a separate advisory diagnostic signal, not an alternate route for structured
business operational events, metrics, traces, product analytics, or security-
or audit-relevant signals. Those signals keep their controlled-backend and
backend-authority requirements.

Allow the exception only when all of these conditions hold:

- The vendor's client key or token is explicitly designed for public client embedding: write-only, project-scoped, and vendor-rate-limited. Never embed a write credential for the project's own logging, tracing, or telemetry backend.
- Default PII capture and session replay are disabled unless separately reviewed. Restrict breadcrumbs and automatic capture to non-sensitive fields; never capture request or response bodies, headers, user-entered input, auth data, storage values, or URLs with query strings. Treat exception messages and stacks as crash-report payloads governed by this exception, never as Safe Log Events.
- Crash data remains advisory and diagnostic. Never use it as authoritative identity, security, authorization, or audit evidence.
- When an exception is causally associated with an outbound request, attach the same approved opaque execution reference to the request, corresponding backend log, and crash event. Prefer the active trace ID when tracing exists; otherwise follow the project's approved request or correlation convention. Treat a client-supplied identifier only as an advisory join key, and do not invent a backend relationship for a client-only crash.

Adopting this exception remains a project-level architectural decision that the
consuming project records for itself, such as in its own ADR. This skill defines
only the conditions under which that exception is permitted; it does not select
a vendor.
