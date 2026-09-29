# UI-First Review

For a feature with material UI and backend work, prepare the interactive
frontend stage first and obtain the user's review before backend integration.
Do not depend on the issue author to request this order explicitly.
Material work includes a new screen, a substantial layout change, work governed
by an authoritative design, or a flow with important states or interactions. A
small change within an established pattern keeps the normal focused frontend
check. Preserve stronger repository conventions.

## Plan The Review Surface

- Define the production-facing data and operation contracts before building the
  mock adapter. Record assumptions about pagination, permissions, content
  limits, and error outcomes that could change the UI during integration.
- Plan a feature-specific list of materially distinct scenarios before coding,
  including relevant loading, empty, sparse, dense or paginated, failure,
  recovery, permission, and content-boundary states. Do not require irrelevant
  states or a Cartesian product of every setting. Add a newly discovered state
  to the plan before treating it as reviewed.
- For an authoritative design, use the approved Design Reference Manifest,
  Evidence Bundle, and Design Audit Matrix from [Design Conformance And
  Audit](design-conformance.md). The UI-first stage does not weaken the source
  check, viewport coverage, accessibility proof, or independent audit.
- Name the first-stage UI acceptance, the mock adapter's integration blind spot,
  the user's review gate, and the later real-backend proof. Decide whether the
  staging build is available before merge by inspecting the deployment workflow
  and current staging process. An automatic pull-request preview is one route;
  an authorized deployment of an unmerged branch is another. If the exact
  candidate can reach staging before merge and the work is one coherent issue,
  use a frontend review checkpoint before the integration checkpoint. If
  opening a pull request is required to create that preview, publish the
  accepted frontend checkpoint as a draft pull request under the governing
  publication boundary, then review its deployed UI before integration. If
  staging receives only merged work, create an ordered feature pack with a
  complete frontend-only slice and its own branch and pull request first, then
  a separate integration slice that depends on the user's approval of the
  staged frontend. The first slice
  may include the minimal server or data-boundary scaffolding needed to run the
  mock-backed UI, but it does not implement the real backend behavior. The
  frontend slice can be done while the feature remains unreleased and not
  end-to-end complete. If neither staging path is feasible, surface the
  constraint for a user decision instead of silently switching to backend-first
  delivery.

## Build Interactive Scenarios

Implement the real route, flow, views, hooks, interactions, navigation, and
accessibility behavior against the same typed operation contracts that the
production adapter will implement. Follow [Frontend-First API
Mocks](frontend-first-api-mocks.md): mock only the backend-calling adapter or
the equivalent server-side data boundary in a full-stack app. Do not replace
flows or hooks with display-only state injection. Exercise transitions such as
retry and pagination as well as their visible results.

For web review builds, each named, deterministic scenario has a stable URL
identifier, such as `?uiScenario=orders.empty`. The floating selector updates
that identifier. Direct opening, refreshing, and browser back/forward navigation
must recreate the corresponding setup. The URL contains a scenario name, not
raw fixture data, personal data, or secrets. Reject unknown names with a safe
documented fallback or clear review error; never silently display a different
named scenario. Keep the selector keyboard accessible, labelled, and usable at
supported viewports without covering the content under review.

The URL selects a preset only after trusted build or deployment configuration
has enabled mock mode. It cannot enable mock mode by itself. On a scenario
change, cancel or isolate pending requests and reset affected flow and cache
state so results from the previous scenario cannot appear in the next one.
Provide a stable loading presentation when visual inspection needs it, and
separately verify real loading-to-result transitions. For native screens, use a
platform-appropriate deterministic review control and deep link where
supported; do not assume browser query parameters exist.

## Stage And Approve The UI

Run the assembled frontend on the project's normal staging environment when
deployment is automated or separately authorized. Do not infer deployment
authority from this workflow. Use synthetic data and the mock adapter; do not
connect review scenarios to production data or side-effecting services. Record
the exact deployed build or commit, environment, scenario links, relevant
viewports, runtime results, and design comparisons. A later staging deployment
invalidates any review evidence it could affect.

Engineering completes its frontend validation and independent review before
requesting the user's UI approval. The user's approval covers the demonstrated
appearance and client behavior at that revision. Record the approval or
requested changes against that revision and the scenario list. Do not begin
backend integration merely because automated checks or checkpoint review are
clean. A component harness, screenshot, or mock adapter alone is not running
staging proof.

Unfinished feature code may enter a production build only behind a default-off
gate that prevents direct access to its routes and any related operations. A
hidden navigation link or client-only flag is insufficient for that guarantee.
If the project cannot enforce the gate reliably, defer deploying the unfinished
code to production. Verify that the production deployment receives the
production artifact and configuration, never the staging mock build, and that
existing production routes remain healthy.
Production configuration rejects mock mode; the floating selector and
synthetic review controls cannot run there and should be excluded from
production bundles when the toolchain permits.

## Integrate And Recheck

After UI approval, connect the production adapter and backend while preserving
the approved frontend contract unless a new product decision changes it. Run
contract-boundary tests and the complete relevant Runtime Acceptance journeys
through the real backend, including important errors, pagination, permissions,
accessibility, and design states. Recheck affected Design Audit Matrix rows and
the integration seams; a mock-backed pass cannot satisfy live integration proof.

Compare the integrated screens with the approved staging UI. Return appearance
or behavior changes to the user for targeted review; unchanged screens keep
their earlier user approval when current engineering evidence confirms they
remain equivalent. Record the final integrated revision and evidence before
the separately authorized production release enables the feature gate.
