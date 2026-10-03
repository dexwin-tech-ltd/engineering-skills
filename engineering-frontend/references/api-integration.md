# Frontend API Integration

In new Effect projects use Effect Schema decoding and explicit Effect adapter contracts; apply `$engineering-effect` for execution, defects, and interruption. The `.safeParse()` and Result-specific instructions below apply to established Zod/neverthrow adapters. The same boundary validation, exact operation errors, and layer ownership apply to both.
Read when changing a frontend API adapter or client boundary.

## API Integration

Default layer chain:

```text
contract -> API/client adapter -> hook -> flow -> view
```

Each arrow is a boundary. Nothing skips a layer.

Navigation loaders and prefetch boundaries may invoke the same query definition used by the hook to populate the cache before rendering; they must not call raw clients or duplicate adapter logic.

- API calls live in API/client adapter modules only. Components, screens, routes, stores, flows, views, and hooks must not call `fetch`, `axios`, SDK clients, or raw clients directly unless the repo explicitly makes that hook the adapter boundary.
- Preserve the repo's adapter location. If no convention exists, use domain-owned adapters and one adapter file per domain surface.
- Validate adapter input with `.safeParse()` before making the request. Return a typed validation error and do not call the API when request validation fails.
- Validate success responses against the endpoint success contract before returning data.
- Validate error responses against the endpoint error contract before mapping them into domain errors.
- Validate and parse the exact contract for the operation being called. Do not parse operation responses through a broader domain-wide or project-wide error schema.
- If response validation fails, map it to an explicit infrastructure failure such as `service_unavailable`; an unrecognized payload is not a domain error.
- Prefer combined per-endpoint error contracts when the repo supports them. Parse the error body once, then exhaustively match the parsed error discriminant instead of branching on HTTP status first with a broad fallback.
- Keep domain error unions as named top-level type aliases. Put domain error types and factories in the domain's frontend module, not inline inside adapter functions.
- Return the operation's exact error union plus only failures introduced by the adapter itself, such as client-side input validation, transport failure, SDK rejection, or malformed response data.
- Map transport, SDK, provider, and malformed-response failures to adapter-owned infrastructure variants such as `service_unavailable`; do not attribute them to the remote operation.
- No `try/catch` in adapters, hooks, or flows for expected failures. Use `Result`/`ResultAsync` and restrict `try/catch` to explicit bridges to throwing libraries.
