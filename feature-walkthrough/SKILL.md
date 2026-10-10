---
name: feature-walkthrough
description: Create, check, publish, and preserve standalone HTML walkthroughs that explain implemented changes for human review. Use for PRs selected for human review or an explicitly requested feature explanation.
---

# Feature Walkthrough

Make an implemented change easy to understand through a rendered, self-contained
HTML page. The PR remains the authoritative review record; its prominent
**View feature walkthrough** link opens the explanation, not HTML source.
Use the same experience whether work was produced locally or on a remote server.

## Scope and authority

For delivery, use the selection criteria and authority in
[Delivery and Human Review](../engineering-for-certainty/references/delivery-and-human-review.md).
Every selected PR gets a walkthrough; routine work does not require one.
An explicit walkthrough request also qualifies. Explanation-only requests
authorize reading and local artifact creation, not PR writes or remote hosting.
The responsible delivery operator owns publication inside authorized delivery.

Reuse the repository's configured hosting and permitted audience. Creation of
new accounts, paid resources, DNS changes, expanded access, or changes to
repository protections requires its own authority; missing setup is publication
pending, not permission to improvise public hosting. Read
[Hosting and preservation](references/hosting-and-preservation.md) when choosing,
configuring, publishing, or repairing hosting. This skill does not authorize
feature changes, production demos, notifications, or human-review completion.

## Build from the actual change

Resolve the repository, approved intent, original diff, relevant code, verified
evidence, PR identity when available, and the exact code revision being
explained. For a merged PR, recover its original range rather than describing
today's entire default branch. Distinguish that code revision from the commit
containing the HTML and from the resulting merge/squash revision. Do not create
a self-referential commit-update loop.

For delivery, store the HTML using the repository's documentation convention. Otherwise use
`docs/feature-walkthroughs/<stable-change-id>/index.html`; use a descriptive ID
before the PR number exists, then record the PR without renaming unnecessarily.
Reuse the same artifact for updates to an open PR. Keep it in version control
with the change. A local-only explanation uses the requested output location or
an isolated local artifact directory; it does not require repository or Git
mutations. Record the hosting configuration separately from page content.

Write a layered walkthrough: a short opening establishes the problem, changed
behavior, and reason for human review. Let the reader expand relevant details:

- one concrete before/after scenario;
- how the changed path works, with a useful diagram, screenshot, or step-through;
- consequential choices and their tradeoffs;
- observed evidence, limits, material risks, and recovery boundaries;
- a few precise code and evidence links.

Scale the presentation to the change. A backend policy, migration, refactor,
and UI feature need different visual explanations. Explain technical terms at
first use. Show important warnings in the opening rather than hiding them in
collapsed details. Avoid a long report or mandatory empty sections.

Keep CSS, JavaScript, diagrams, and necessary images inside the single file.
Use inline SVG or embedded image data. No CDN scripts, remote fonts, analytics,
runtime fetches, build step, or application backend may be required to read the
core explanation. Code/evidence links may be external and remain authenticated
where appropriate. Prefer native HTML disclosure and navigation; add interactive
models only when they clarify behavior. Label illustrations and simulations;
never present them as captured application behavior or executed tests.

[The example page](assets/example.html) demonstrates a lightweight, responsive
layout with native disclosures and a before/after comparison. It is fictional
and contains no runtime proof. Adapt its presentation rather than copying its
claims. Escape untrusted text before inserting it into HTML; do not execute
repository or PR content as page code. Exclude secrets, customer data, raw logs,
and private machine paths, including embedded images and metadata.

## Check and open the rendered page

Verify factual claims against the diff and raw evidence. Include the explained
revision, evidence environments, PR link when known, and limitations. Keep
deployment state separate; a walkthrough is an explanation, not a live feature
demo or proof that production runs this revision.

Open the page in a browser and inspect a desktop and narrow viewport. Exercise
its disclosures, navigation, and any interactive model, including keyboard
access. Check readable contrast, focus, labels/alternative text, and overflow.
Confirm the core content renders without network resources. Source inspection
alone is not browser verification; if browser access is unavailable, report that
check as missing rather than claiming the page renders.

On a local machine, open the rendered file directly when the available browser
supports it. Otherwise serve only the artifact directory on loopback and open
that URL for the user. Do not leave them with raw tags or manual download steps
when the agent can open the page. On a remote server, prefer the durable hosted
URL; a protected temporary tunnel or existing port forwarding can support an
immediate preview, but never substitutes for the lasting PR link.

## Publish and hand off

Publish through the repository's configured static hosting. Verify the actual
rendered deployment and its audience as described in the hosting reference.
Put the verified URL near the top of the PR description and beside that PR's
entry in `PRS_PENDING_HUMAN_REVIEW.md`. Keep a compact problem/result summary
in the PR so it remains useful if hosting is interrupted. A source/blob/raw-file
link, expiring CI download, localhost URL, or temporary tunnel is not a delivered
walkthrough link.

While an open PR changes, keep the explanation and PR link current. Before
handoff, verify that the final candidate's material behavior is covered, not
just an earlier version. Include artifact commits in current-head checks and
required independent review. Pure explanation-only descendants can preserve
feature runtime evidence only after verifying they change no behavior or proof
contract; scripts, CI, hosting configuration, and substantive corrections need
their own affected checks.

After verified merge, freeze the delivered explanation and preserve its hosted
version. Record merge identity and known release state on the PR. The hosted
page and PR together must clearly identify the described revision; a later
deployment must not silently replace that explanation.

If publication or access verification fails, withhold an unsafe deployment/link
and report **Walkthrough publication pending** with the cause, checked local
artifact, durable repair owner/trigger, and tracking link. Continue safe repair
within scope. Otherwise-ready feature shipping can proceed under its existing
gates; never claim walkthrough delivery complete. Preparation or correctness
gaps are not hosting outages and require correction. This exception never
waives required feature proof or a separate explicit repository gate.

Return the artifact path, explained revision, browser checks, verified rendered
URL or publication-pending state, access verification, PR/index link status,
and remaining work. A human-review outcome belongs to
[$pull-request-review](../pull-request-review/references/post-merge-human-review.md).
