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
- auth middleware, guards, or backend auth boundaries

## Doctrine

- Prefer `httpOnly` secure cookies when the architecture allows it.
- Never store long-lived secrets or refresh tokens in insecure client storage.
- Make CSRF protection explicit when cookie-based auth is used.
- Session validation and refresh boundaries must be explicit and testable.
- Keep auth and permission checks explicit in both backend and UI. UI checks never replace backend authorization.
- Use shared permission constants or shared auth contracts where available; do not hard-code authorization literals when a shared package exists.
- In Fastify or similar backends, perform auth checks before parsing request bodies when parsing errors could leak information.
- Treat auth/session configuration as injected infrastructure, not hidden global state.

## Testing and Review

- Test login, logout, refresh, and expired-session behavior when touched.
- Test backend permission enforcement, not only UI gating.
- Test CSRF behavior whenever cookie-based auth changes.
- Verify secure storage and transport assumptions match the selected architecture.
