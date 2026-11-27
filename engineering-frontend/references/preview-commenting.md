# Preview Commenting

## Open Feedback From The Review Toolbar

Make pinned feedback available through **Comments**, the first control in the
[interactive preview review toolbar](interactive-preview-review.md#provide-one-floating-toolbar).
Reviewers can open the comment list or enter pin mode.

## Make comments easy to find

- Provide a distinct comment mode. Let reviewers select text, controls, cards,
  or areas such as spacing around a section. While placing a pin, selecting a
  target should open the composer without triggering the underlying control.
  Provide keyboard and touch access to the commenting controls and list.
- Save the exact preview build, route, named scenario, current screen or state,
  and target identity. A single URL can show several states. Record the
  relevant viewport or steps to reproduce a transient state when useful.
- Use stable element or section identifiers. Place area pins relative to a
  stable section when possible so they stay useful as the screen reflows. Keep
  notes for moved or missing targets in the list with their original context.
  Label missing targets as unavailable. Offer a screenshot when it would help
  preserve a transient state and the preview's data rules allow it.

## Show resolution consistently

- Drive the list and pins from one saved **Open** or **Resolved** status. Show
  the status in the list and on the pin. Give resolved pins a checkmark, muted
  styling, and an accessible status label. Keep them visible by default. Provide
  **Hide resolved** and **Reopen** and keep resolved notes readable.
- Follow the project's resolution ownership. Verify an applied fix in the
  relevant scenario before resolving the note. Record any alternate
  disposition the reviewer accepts. Reopening returns the note to **Open**.

## Match storage and access to the preview

- For a local preview, a workspace file or the project's existing review store
  is enough when the reviewer and authorized agent can access it. Preserve the
  notes and resolution history across refreshes, preview process restarts, and
  replacement builds of the same review.
- For a remote preview, save notes on persistent storage available to the
  remote preview service. Make each browser-facing preview URL configurable
  per environment and use those URLs for links between apps. Send comment
  requests to the same origin as the page. If two previews
  share a list, route their requests to the same store. Use the project's
  authenticated review access for the page and comment operations. A
  [Cloudflare Tunnel with Access](https://developers.cloudflare.com/cloudflare-one/access-controls/applications/http-apps/self-hosted-public-app/)
  is one way to do this. An authorized agent can read the remote store or use an
  export from the comment list.
- Keep notes and any screenshots in the authorized review store. Keep the same
  review's notes available when a build is replaced. Verify that production
  builds and routes exclude the review controls and comment operations.

## Verify The Shared Build

Check an element pin and an area pin, including that placing a pin on a control
leaves it inactive. Check that saved notes remain findable when targets move or
disappear. Verify resolution and reopening, persistence across reload, and
read-back from a replacement build of the same review when one is available.
For a remote preview, save a note in one browser and read it from a separate
authorized browser. Use the existing runtime and access proof for the scope of
the review tooling.
