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

- Prefer `httpOnly` secure cookies when the architecture allows it.
- Never store long-lived secrets or refresh tokens in insecure client storage.
- Make CSRF protection explicit when cookie-based auth is used.
- Session validation and refresh boundaries must be explicit and testable.
- Keep auth and permission checks explicit in both backend and UI. UI checks never replace backend authorization.
- Use shared permission constants or shared auth contracts where available; do not hard-code authorization literals when a shared package exists.
- Model authorization with granular string permission keys rather than coarse
  role-name checks or boolean capability flags.
- Define permission keys in a shared package so backend, frontend, and shared
  contracts can depend on the same source of truth.
- Prefer seeded system roles plus explicit custom roles when the product needs
  role management. Seeded roles should stay system-managed and immutable unless
  the repo intentionally models a controlled cloning or migration flow.
- Keep role provenance and scope explicit in the model. Do not infer protected
  or platform-level roles from naming conventions alone.
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
- Treat auth/session configuration as injected infrastructure, not hidden global state.

## Testing and Review

- Test login, logout, refresh, and expired-session behavior when touched.
- Test backend permission enforcement, not only UI gating.
- Test seeded-role protection, custom-role behavior, and shared permission-key
  validation when role management changes.
- Test policy-registry resolution and both `all_of` and `any_of` policy
  evaluation when authorization rules change.
- Test CSRF behavior whenever cookie-based auth changes.
- Verify secure storage and transport assumptions match the selected architecture.
