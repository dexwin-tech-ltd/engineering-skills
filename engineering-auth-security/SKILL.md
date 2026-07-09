---
name: engineering-auth-security
description: Explicit auth and session security doctrine for work that touches cookies, tokens, sessions, CSRF, auth boundaries, or permission enforcement.
---

# Engineering Auth Security

Use this companion skill with `$engineering-for-certainty` when work touches authentication, authorization, token or session handling, cookie policy, or permission boundaries.

## Scope

Apply this skill when the work includes:

- login, logout, refresh, or session lifecycle changes
- cookie or token storage decisions
- CSRF handling
- permission checks or authorization rules
- roles, permissions, or policy registry changes
- auth middleware, guards, or backend auth boundaries

## Doctrine

### Session and Token Handling

- Prefer `httpOnly` secure cookies unless all of the following are true: the requirement is recorded in repo docs or an ADR, the feature needs direct client-side cookie reads, and a server-managed session alternative was evaluated and rejected for a documented technical reason.
- Never store long-lived secrets or refresh tokens in insecure client storage.
- Make CSRF protection explicit when cookie-based auth is used.
- Session validation and refresh boundaries must be explicit and testable.
- Treat auth/session configuration as injected infrastructure, not hidden global state.

### Authorization Model

Apply this subsection in order: shared permission source first, registry shape second, role modeling third, and boundary enforcement throughout.

- Keep auth and permission checks explicit in both backend and UI. UI checks never replace backend authorization.
- Use shared permission constants or shared auth contracts where available; do not hard-code authorization literals when a shared package exists.
- Model authorization with granular string permission keys rather than coarse
  role-name checks or boolean capability flags.
- Define permission keys in a shared package so backend, frontend, and shared
  contracts can depend on the same source of truth.
- In repos with a dedicated permissions package, define permissions as a nested
  resource/action registry in the shape
  `{ resource: { action: "resource.action" } }`.
- Keep the top-level resource key, nested action key, and string literal
  aligned so the registry stays self-describing and easy to audit. Prefer
  `role: { create: "role.create" }`, not flattened maps, renamed aliases, or
  values that drift from the canonical permission name.
- Export shared permission registries as `as const` so callers get stable
  literal types from the same source of truth.
- Example:
  ```ts
  export const PERMISSIONS = {
    role: { create: "role.create", update: "role.update" },
    user: { create: "user.create" },
  } as const;
  ```
- Prefer seeded system roles plus explicit custom roles when the product needs
  role management. Seeded roles should stay system-managed and immutable unless
  the repo intentionally models a controlled cloning or migration flow.
- Keep role provenance and scope explicit in the model. Do not infer protected
  or platform-level roles from naming conventions alone.

### Policy and Backend Boundaries

- Centralize policy definitions in a server-side registry referenced by routes
  and services.
- Policy definitions should support `all_of` and `any_of` semantics when the
  authorization model needs composition.
- Clients should not send authorization policy definitions in normal business
  requests; the server resolves required policy from trusted route or service
  wiring.
- Unknown policy keys, invalid policy configuration, and unknown role or
  permission keys should fail fast as configuration defects rather than falling
  through to implicit allow or deny behavior.
- Authorization failures should return structured, stable error codes that can
  be mapped exhaustively at the boundary.
- In Fastify or similar backends, perform auth checks before parsing request bodies when parsing errors could leak information.

## Testing and Review

- Test login, logout, refresh, and expired-session behavior when touched.
- Test backend permission enforcement, not only UI gating.
- Test seeded-role protection, custom-role behavior, and shared permission-key
  and registry-shape validation when role management or permission-registry
  changes.
- Test policy-registry resolution and both `all_of` and `any_of` policy
  evaluation when authorization rules change.
- Test CSRF behavior whenever cookie-based auth changes.
- Verify secure storage and transport assumptions match the selected architecture.
