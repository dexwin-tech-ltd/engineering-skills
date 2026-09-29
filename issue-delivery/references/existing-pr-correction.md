# Existing PR Correction

Read only when an outer review-to-merge workflow supplies an issue-approved Existing PR Correction Contract.

When an outer review-to-merge workflow supplies an issue-approved **Existing PR
Correction Contract**, use its exact published head and base as the branch
identity instead of requiring a conventional replacement branch. Verify the
current remote head SHA and push authority, then attach a dedicated linked
worktree to that branch before editing. Do not rename the branch, open a
replacement pull request, rewrite history, or proceed against a fork head that
cannot receive the correction. Verify the Approved Issue Commit is present on
that branch before applying any newly authorized correction.
