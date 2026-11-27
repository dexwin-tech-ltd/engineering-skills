# Interactive Preview Review

Use this workflow for every interactive UI preview shared for human review,
including frontend-only work, the first handoff, and each feedback revision.

## Provide One Floating Toolbar

Put **Comments** and **Scenarios** side by side in that order in one floating
toolbar. **Comments** opens pinned feedback. Reviewers choose whether to leave
comments. **Scenarios** opens one on-page panel that serves as both the selector
and the guide. Show the exact active scenario name after a middle dot, for
example **Scenarios · Save failed**. Update the label when the active scenario
changes. At narrow widths, keep **Scenarios** visible and truncate only the
scenario name as needed. Keep the full label accessible and show the full name
in the panel.

Reuse the project's review tool when it meets this standard. Place the toolbar
where it is easy to find and clear of the content and primary actions. Support
keyboard, pointer, and touch at supported viewports.

Enable the controls and comment operations only through trusted review
configuration. Exclude them from production builds and routes. Use synthetic
data and the project's authorized preview access and storage. Follow
[Preview Commenting](preview-commenting.md) for pins, persistence, and resolution.

## Select And Test Scenarios In One Panel

- Keep the scenario table easy to scan. Mark the active row. Make each scenario
  name a stable deep link that selects its setup. Use the same visible names,
  order, and scope in the panel and handoff table. Each row describes what the
  scenario represents, the exact steps to test, and what to look for.
- Put preview controls alongside the steps in the rows that need them. Enable
  each control only while its scenario is active and its action is available.
  Name the controls in the test steps and distinguish them from actions in the
  app. For example, in **Slow settings save**, choose **Hold request** in the
  row, choose **Save** in the app, check that loading stays visible, then choose
  **Complete request** in the row to see the result.
- For a full round, cover every reviewer-facing scenario, including relevant
  loading, empty, error, denied, and recovery states. For a round focused on
  changes after feedback, show the same affected subset in the panel and
  handoff. Keep its relative order and retain the complete technical fixture
  list for automated checks.
- Before sharing, compare the names, order, and scope on the compiled review
  build with the handoff table. Select scenarios from the panel, open and
  refresh their deep links, and check that the setup, active row, and toolbar
  label agree. Exercise applicable row controls and verify they are disabled
  for inactive scenarios or unavailable actions. Check the toolbar at supported
  widths and complete the [commenting checks](preview-commenting.md#verify-the-shared-build).

## Hand Off And Record Approval

Share the working preview URL, exact revision or deployment build ID, and
scenario table together. Say where to find the toolbar and, for a focused
round, what changed. Name the screens and behaviors up for approval and record
the decision against that build and scenario scope.

Approval of a guide, site, or document covers that artifact. UI approval covers
the reviewed screens and behavior. Live integration acceptance and release
decisions each require their own evidence. Preserve comments and their
resolution history through replacement builds and closure. Retire the preview
according to the project's policy.
