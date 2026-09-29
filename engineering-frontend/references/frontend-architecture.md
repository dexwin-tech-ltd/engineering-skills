# Frontend Architecture

Read when changing route or screen ownership, flows, views, components, reducers, handlers, or navigation-time data loading.

## Structure

- Organize frontend code by domain modules, not broad file-type buckets, where the repo structure allows it.
- Keep routes/pages/screens thin. They render one flow/container component and avoid branching orchestration logic.
- Flows own UI orchestration: consuming server state, coordinating transitions, reducer dispatch, refresh behavior, mutations, navigation side effects, and exhaustive state matching.
- In React and React Native flows, views, components, routes, and screens, prefer named local handlers immediately before the JSX `return` when a callback prop branches, has multiple statements, awaits or coordinates work, dispatches state, navigates, invokes a mutation, or makes another meaningful transition. Pass it by identifier on one line, such as `onConfirmLeave={handleConfirmLeave}`; keep direct references unchanged, such as `onRefresh={detail.refresh}`. A short, single-expression callback may stay inline when it only forwards, binds, or makes a minor UI event/value adaptation. Do not add `useCallback` solely for readability; use it only when a documented consumer or dependency contract independently requires referential stability. Naming a handler does not transfer orchestration ownership: flows retain it, and presentational views keep local logic minimal.
- When the router or framework provides a suitable data-loading boundary, it owns navigation-timed loading and prefetching for route- or screen-critical data.
- Reserve `views/` for full-screen presentational surfaces that represent one complete navigable screen. Views receive explicit props, communicate through callbacks, and keep local logic minimal.
- Put presentational UI that does not represent one complete navigable screen in `components/`.
- Put a component used only by one domain or feature module in that module's `components/` folder. Put a shared component at the narrowest module, domain, or application boundary that owns all of its uses; use application-level `components/` only when its uses span unrelated domains, and preserve an existing shared UI or design-system package for generic primitives.
- Prefer reducers over multiple related `useState` calls, especially for transition-heavy flows or two or more related state values.
- Put non-trivial reducers in a `reducers/` folder inside the relevant domain module, flow, or feature module instead of colocating reducer transition logic inside route, screen, or view files.
- Cross-domain UI or app workflows should live in explicit flow or process modules rather than being buried inside a single domain component or screen.
- Use exhaustive matching for non-boolean discriminants. Avoid nested ternaries; use reducer transitions or pattern matching.
- In React and React Native hooks or components, add a brief comment above every `useEffect` explaining what that effect does and why it exists.
