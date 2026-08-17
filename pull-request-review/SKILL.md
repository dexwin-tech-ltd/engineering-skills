---
name: pull-request-review
description: Review a GitHub pull request through the evidence-verified code-review pipeline, reconcile existing review threads against later commits, obtain user adjudication of new findings, and publish accepted inline comments with fix guidance. Use when the user asks to review a PR and wants verified findings managed as GitHub review conversations.
---

# Pull Request Review

Use `$code-review` as the sole analysis engine. On the Dexwin engineering server, use `code-review-dexwin`, its execution alias, as the same canonical analysis engine. Own the GitHub-specific workflow: resolve the pull request, reconcile existing threads, present new verified findings for user adjudication, publish accepted comments, and verify the resulting review state.

Do not duplicate or weaken `code-review` doctrine. If neither the canonical skill nor its platform alias is available, or one of its triggered engineering dependencies is unavailable, stop and name every missing skill.

## Boundary And Authorization

Keep analysis of new findings read-only. User acceptance of specific findings authorizes publication of only those findings to the resolved pull request. Never post rejected, unreviewed, `CONDITIONAL`, or `NEEDS_CONTEXT` candidates. Thread reconciliation may perform only the pre-authorized workflow-owned reply and resolution writes described below.

Treat thread reconciliation as part of every invocation; do not require a separate request. Automatically reply to and resolve only workflow-owned threads that are verified as fixed or obsolete. Inspect human-owned threads, but never resolve them automatically.

Do not implement fixes, push commits, dismiss human reviews, approve the pull request, or merge it unless the user separately authorizes that action.

## Workflow

### 1. Resolve The Pull Request

- Honor an explicit repository and PR number or URL.
- For "this PR" or the current branch, resolve the local repository, branch, remote, and associated PR.
- Record the repository, PR number, base branch, head branch, current head SHA, author, issue links, and requested reviewers.
- Use the configured GitHub connector for structured PR metadata and patch context when available. Use thread-aware GitHub GraphQL access when resolution state, outdated state, line anchors, replies, or thread mutation matters.
- Stop if the repository or PR remains ambiguous, authentication is missing, or the complete diff cannot be recovered.

### 2. Reconcile Existing Threads

Before finding new issues, inventory every unresolved review thread. Capture its thread ID, author, path, line, original claim, `isOutdated`, `isResolved`, `viewerCanResolve`, and, when recoverable, the head SHA against which it was last assessed.

Mark new workflow comments with a stable hidden marker:

```html
<!-- pull-request-review:finding=<stable-finding-id>;head=<reviewed-head-sha> -->
```

Treat a thread as workflow-owned only when that marker is present and was created by this workflow. Matching account identity alone is insufficient.

For each existing thread, reconstruct the original failure claim and verify it against the latest head using the normal `code-review` evidence standard. Run focused validation when it materially changes confidence. Assign exactly one reconciliation verdict:

- `FIXED`: the failure is no longer reachable and the required regression evidence exists.
- `PARTIALLY_FIXED`: part of the failure remains.
- `NOT_FIXED`: the original failure remains reachable.
- `OBSOLETE`: the relevant behavior was intentionally removed and no equivalent failure remains.
- `NEEDS_CONTEXT`: the current evidence cannot establish the result.

Never treat an outdated line, moved code, deleted hunk, reply saying "fixed", or new commit alone as proof.

For workflow-owned `FIXED` or verified `OBSOLETE` threads:

1. Reply with the fixing head or commit SHA, the evidence that refutes the old failure, and validation performed.
2. Resolve the thread only when GitHub reports `viewerCanResolve`.
3. Re-read the thread and verify that the reply exists and `isResolved` is true.

Leave every other workflow-owned thread open and report what remains. For human-owned threads, report the reconciliation verdict without replying or resolving unless the user separately authorizes those writes.

### 3. Run Code Review

Load and follow `$code-review`, or `code-review-dexwin` on the Dexwin engineering server, against the current PR head. Provide it the PR description, governing issue, complete diff, relevant repository contracts, validation evidence, and current unresolved-thread claims so it can avoid duplicates.

Keep finder and verifier contexts logically independent. Never convert an existing comment, reviewer opinion, or subagent claim directly into a finding without verification.

### 4. Work Through The Review Queue

Receive the complete confirmed, deduplicated, ranked finding queue from `code-review` before presenting its first item. Do not limit broad investigation to the current conversation item.

Present one finding at a time. Include its stable ID, the full normal review evidence, a preview of the proposed GitHub comment, and `**Progress: Finding <position> of <total> - <remaining> remain after this**` on every response that presents or continues it. Calculate position and total using prior dispositions plus the active queue; do not show a remaining count alone. Ask the user to accept, reject, revise, defer, discuss, or request named evidence for the current finding. Do not present the next finding until the current one has an explicit disposition, then recompute the queue for dependencies or duplicates. If the total changes, state `**Queue revised: <old> -> <new>.** <reason>` before the next finding; never silently change the denominator or stable finding IDs.

Use a batch of at most ten only when the user explicitly requests batch adjudication or a complete report. A generic request to review a pull request is not a request for batching. Treat `agree to all` as acceptance of the current explicit batch only.

Do not publish unadjudicated findings. Preserve accepted, rejected, revised, and deferred state across the queue, and never re-propose a rejected finding unless new evidence materially changes the claim.

### 5. Publish Accepted Findings

Deduplicate against all existing threads immediately before publication. Attach each accepted finding to the closest causal changed line. If the causal line cannot accept an inline comment, use a file-level thread or the nearest changed line and explain the anchor.

Use this comment structure:

```markdown
@responsible-engineer **[Severity] Finding summary**

**Problem:** <precise defect>

**Failure scenario:** <input/state -> executed path -> material consequence>

**Evidence and impact:** <repository or runtime evidence>

**Required fix:** <what must change and why>

**Proposed code:**
<exact suggestion block or typed illustrative snippet>

**Regression proof:** <exact test or verification required>

After pushing the correction and regression evidence, leave this thread open, apply the `pending-review` label to the pull request, and reply here tagging the reviewer to request another look.

<!-- pull-request-review:finding=<stable-finding-id>;head=<reviewed-head-sha> -->
```

Use a GitHub suggestion block only when the replacement is exact, complete, and safe to apply at that line. For cross-file or architectural corrections, label the snippet as illustrative and name every affected owner rather than presenting an unsafe one-click patch.

Resolve the Responsible Engineer from the issue's explicit GitHub implementer, falling back to the PR author. Never infer ownership from `git blame`. Allow a user-supplied override. Skip a redundant tag when the reviewing and responsible accounts are the same.

Treat `pending-review` only as a reviewer-attention signal. Do not tell the Responsible Engineer to resolve workflow-owned threads; this workflow retains verification and resolution ownership. If the label is unavailable, instruct the engineer to report that in the reply and still notify the reviewer rather than creating or substituting a label.

Submit `REQUEST_CHANGES` when at least one accepted finding is explicitly blocking. Otherwise submit a comment review. Do not let severity alone silently decide whether the review blocks merging; state blocking status during adjudication.

### 6. Verify Publication

Re-read the submitted review and threads. Verify:

- every accepted finding was posted exactly once;
- no rejected or unadjudicated finding was posted;
- anchors, responsible-engineer tags, severity, evidence, fix guidance, code, and regression proof are present;
- every correction request tells the Responsible Engineer to leave the thread open, apply `pending-review`, and notify the reviewer after pushing the correction and regression evidence;
- the review event matches the accepted blocking status;
- resolved workflow-owned threads remain resolved at the current head;
- limitations such as outdated anchors or missing permissions are explicit.

Report posted thread links or IDs, reconciled threads, unresolved human-owned threads, validation performed, and any verified findings still awaiting adjudication.

## Failure Handling

- If GitHub thread state is unavailable through a flat comment API, use thread-aware GraphQL rather than guessing from comments.
- If a comment cannot be anchored after the head changes, refresh the diff once and retry with the new line. Do not publish to an unrelated line.
- If authentication, permissions, rate limits, or API failures interrupt publication, stop and report exactly which accepted findings were and were not posted.
- Never mark a review successful from the mutation response alone; verify the resulting GitHub state.
