# Preview Commenting

## Offer it in the review handoff

Offer pinned comments whenever you share an interactive preview of a material
UI change for human review, including frontend-only work. Material changes
include new screens, substantial layouts, authoritative designs, and flows with
important states or interactions. Also offer them when explicitly requested.
Keep the normal review path for small changes without a commenting request.

Point the reviewer to the project's existing tool if it meets the need. If the
preview lacks one, offer to add it and get agreement before doing that work.
The reviewer can choose whether to use comments. Keep the planned preview and
review gates available either way.

## Make comments easy to find

- Provide a distinct comment mode. Let reviewers select text, controls, cards,
  or areas such as spacing around a section. While placing a pin, selecting a
  target should open the composer without triggering the underlying control.
  Provide keyboard and touch access to the commenting controls and list.
- Save the preview or build, route, named scenario when present, current screen
  or state, and target identity. A single URL can show several states. Record the
  relevant viewport or steps to reproduce a transient state when useful.
- Use stable element or section identifiers. Place area pins relative to a
  stable section when possible so they stay useful as the screen reflows.
  When a target disappears, keep the comment in the list with its original
  context and label the target as unavailable. Offer a screenshot when it would
  help preserve a transient state and the preview's data rules allow it.

## Show resolution consistently

- Drive the list and pins from the same saved status. Show **Resolved** in the
  list and a checkmark with a muted style and accessible status label on the
  pin. Keep resolved pins visible by default. Provide a **Hide resolved**
  control and a way to reopen a comment. Keep resolved comments readable.
- Follow the project's resolution ownership. Resolve a note after verifying the
  fix in the relevant preview or when the reviewer agrees to close it. Reopening
  returns it to the open state. Keep preview approval and its exact revision as
  a separate decision.

## Match storage and access to the preview

- For a local preview, a workspace file or the project's existing review store
  is enough when the reviewer and authorized agent can access it. Preserve the
  list and status across refreshes and preview process restarts.
- For a remote preview, save notes on persistent storage available to the
  remote preview service. Make each browser-facing preview URL configurable
  per environment and use those URLs for links between apps. Send comment
  requests to the same origin as the page. If two previews
  share a list, route their requests to the same store. Use the project's
  authenticated review access for the page and comment operations. A
  [Cloudflare Tunnel with Access](https://developers.cloudflare.com/cloudflare-one/access-controls/applications/http-apps/self-hosted-public-app/)
  is one way to do this. An authorized agent can read the remote store or use an
  export from the comment list.
- Keep the feature behind trusted review configuration and scoped access. Use
  synthetic preview data and keep notes and any screenshots in the authorized
  review store. Verify that release builds and routes exclude the review
  controls and comment operations.

Verify agreed behavior on the affected preview. Check an element pin, an area
pin, a moved or missing target, and resolution and reopening across refresh.
For a supported remote setup, check saving and reading from a browser on a
different machine. Use the existing runtime and access proof for the scope of
the review tooling.
