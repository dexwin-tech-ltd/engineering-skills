# Hosting and Preservation

Read when selecting a host, publishing a walkthrough, or repairing its link.
The HTML source stays with the repository. Static hosting keeps the rendered
PR link available independently of a laptop, development server, or deleted
feature branch. Hosting persistence still depends on retaining the project and
published versions; do not promise unconditional uptime or permanent service.

## Repository configuration

Inspect existing documentation hosting, CI, repository settings, and authorized
deployment procedures first. Reuse a suitable host without replacing an
existing application site or its publishing workflow. If a walkthrough shares
a documentation site, publish to a dedicated path and preserve unrelated pages.

Record portable settings in the established repository convention, or
`docs/feature-walkthroughs/hosting.md` when no convention exists:

- provider/project and rendered base URL;
- source directory, dedicated publish directory, and actual deploy command or CI workflow;
- publication trigger and the operator responsible for repair;
- public/private audience and how repository access maps to host access;
- deployment/alias origins and access-verification procedure;
- version paths, retention, and how to restore old pages after republishing.

Record credential references, never credentials. Configuration is evidence of
intent, not proof that deployment or authentication works. Do not require a new
configuration file when existing repository settings already resolve these
facts. New setup must stay inside the task's explicit infrastructure authority;
otherwise retain the local artifact and track missing setup.

## Choose a host

Visibility follows the repository: public repositories can use public hosting;
private/internal repositories require authentication for an audience contained
within their authorized readers. A separate allowlist does not automatically
track GitHub membership; document and verify its mapping and revocation process.
Unknown visibility or unresolved audience means no remote publication yet.

Prefer these defaults only when no suitable host is already configured:

- **Public repository:** GitHub Pages, using a dedicated path or site that does
  not overwrite existing Pages content.
- **Private/internal repository:** private GitHub Pages where an organization's
  Enterprise Cloud configuration supports it; otherwise Cloudflare Pages with
  Cloudflare Access protecting all origins serving the content.

GitHub Pages can publish from a private source without making the site private.
Do not infer site access from source visibility. Private Pages access control
has organization and plan requirements. Cloudflare's preview access setting
protects previews only; it does not by itself protect the production
`pages.dev` address or custom domains. Secure those routes or disable routes
that cannot be protected before any private content is uploaded. A secret URL,
`noindex`, or login screen implemented in client JavaScript is not access control.

Verify provider capabilities against current official documentation during
setup. Do not bake account-specific settings, plan assumptions, or obsolete CLI
flags into portable skills. Useful starting points:

- [GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages)
- [Private GitHub Pages](https://docs.github.com/en/enterprise-cloud@latest/pages/getting-started-with-github-pages/changing-the-visibility-of-your-github-pages-site)
- [Cloudflare Pages access and deployment URLs](https://developers.cloudflare.com/pages/configuration/preview-deployments/)
- [Cloudflare Pages Direct Upload](https://developers.cloudflare.com/pages/get-started/direct-upload/)
- [Cloudflare Quick Tunnels](https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/)

## Publish without exposing the working tree

Stage only the intended HTML files in the dedicated publish directory. Do not
serve or upload the repository root, `.git`, environment files, or unrelated
evidence. Preserve older published walkthroughs when the provider deploys a
whole replacement site; uploading only the newest page can delete old links.
Use a known serialized publishing workflow or the provider's appropriate
concurrency controls to avoid parallel PRs overwriting each other's archives.
Do not run deploys with secrets against untrusted fork code.

For private content, configure and verify access enforcement on every serving
origin first using a harmless probe. Then publish the private artifact. Check
deployment-specific URLs, production aliases, branch aliases, custom domains,
redirect targets, and direct asset URLs where applicable. Never upload private
content publicly and add protection afterward.

Verify the deployed page in a real browser, its described revision, and its
working links. For private pages, use an authorized reader to confirm access
and an unauthenticated or unauthorized session to confirm content denial.
An HTTP 200 from a login screen proves neither successful rendering nor private
content exposure; inspect the actual response/page. Record checks without
tokens or cookies. If the audience cannot be verified, publication is pending.

If access enforcement is absent or broken, withhold upload. If content is
already exposed, contain it through authorized access restriction or withdrawal
and report the exposure; merely removing the PR link does not protect the page.
Do not broaden access to get a green publishing result.

## Retain the original explanation

Publish a distinct version for each delivered PR. Prefer paths containing the
PR identity and explained code revision, or a retained deployment-specific URL.
Do not use a moving feature/branch alias as the sole historical PR link. Update
the PR to its final verified walkthrough URL, then preserve that target after
merge. The code revision need not equal the HTML-containing commit: record both
identities where needed rather than trying to embed a file's own commit SHA.

Retain hosted versions for the maintained lifetime of the repository. Human
review completion, branch deletion, and CI-artifact expiry are not cleanup
triggers. A later feature change gets a new walkthrough when selected for
review. A successor notice or factual correction may be added without silently
rewriting what the original PR implemented; keep the original version available.

Keep the HTML in repository history so it can be republished when hosting moves.
Verify old PR links after migrations; preserve URLs or authorized redirects,
and reconcile both PR descriptions and still-pending index entries when links
must change. Check the rendered content and access boundary again. Do not create
a periodic watcher unless explicitly requested.

## Temporary preview and failures

A local file or loopback server is sufficient for immediate local viewing.
Serve the artifact directory only, bind to loopback, and open the browser for
the user. On remote machines, reuse safe port forwarding or an authorized
protected preview tunnel when helpful. Quick Tunnels stop when their process
stops and may change hostname; label them temporary and keep them out of the
lasting PR link. A permanent tunnel still depends on the origin server staying
available, so it is not the default archive for a standalone document.

Hosting unavailable, missing authorized setup, or failed access checks leaves
walkthrough publication unfinished. Keep **Walkthrough publication pending**
in the PR and pending-review entry, link the source as a clearly labeled
fallback, and record a durable repair owner and trigger in the repository's
existing tracking convention. Continue otherwise-ready feature shipping where
its existing gates allow it. Repair the link when hosting is restored; follow
normal new-branch/linked-PR rules for source or index changes after merge.
