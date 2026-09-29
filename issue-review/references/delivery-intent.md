# Delivery Intent and Issue Ownership

## Delivery Intent And Issue Ownership

Classify the review before the final write:

- **Immediate delivery**: the initial request clearly asks to review or prepare
  the issue and then implement, deliver, build, fix, or carry it through.
- **Backlog only**: the initial request explicitly says review only, planning
  only, backlog capture, or not to implement.
- **Unresolved intent**: neither outcome is explicit. A request such as "review
  this issue" or "create this issue" does not settle what follows. Ask whether
  implementation will follow immediately. Do not silently choose a default.

When immediate delivery could refer to more than one resulting slice and the
initial request does not select them, ask which slices will begin now. Do not
create worktrees for every child merely because the parent pack is ready.

Do not infer immediate delivery from `Ready` status, priority, a roadmap
position, a complete Branch Contract, or the issue appearing implementation-
ready. A composing workflow that already declares its branch and delivery mode,
such as review-to-merge correction or deferred-follow-up capture, supplies this
intent explicitly; do not ask again.

For every Smallest Coherent Slice selected for immediate delivery:

1. Finish discovery, decomposition, decisions, cross-validation, and shared-
   understanding confirmation before mutating git state.
2. Verify the declared base ref and resolve its SHA. Inspect repository status
   and `git worktree list --porcelain`.
3. Create or verify the exact Branch Contract branch and its dedicated linked
   worktree. If the branch belongs to another worktree, contains ambiguous work,
   or cannot be created from the declared base without changing the contract,
   stop instead of inventing another branch.
4. Perform the final issue, roadmap, index, appendix, and required reference
   writes inside that worktree. Do not finalize them on a planning branch and
   transfer implementation elsewhere.
5. Record the runtime worktree path and resolved base SHA in the execution
   handoff, never in the portable issue.
6. Stage only the finalized issue and its required planning-surface updates,
   then create one coherent Approved Issue Commit on the implementation branch.
   Do not begin production-code changes before that commit exists. Do not push
   merely because issue review completed; publication remains separately
   authorized or owned by the composing delivery workflow. Under an Existing PR
   Correction Contract, create the commit before newly authorized correction
   edits; it need not predate the pull request's existing implementation.

For backlog-only work, do not create the eventual implementation worktree merely
because the issue is ready. Write through the repository's authorized planning
workflow. When implementation is requested later, its branch must start from a
verified base containing the exact Approved Issue Commit, and every later issue
update remains with that implementation branch.
