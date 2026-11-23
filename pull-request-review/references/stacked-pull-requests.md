# Stacked Pull Requests

Use this procedure for an approved stack with real dependencies. Read the
declared order and expected bases. Keep monitoring read-only. A request to
observe or wait grants no merge, retarget, rewrite, or deletion authority.

## Pick the Next Pull Request

Begin with the lowest unresolved dependency. Inspect the provider's state,
expected base, and exact head. If it is blocked, report that PR and its
blocker. If it is waiting on a gate or queued, continue observation only
within the requested wait. An auto-merge request, queue entry, or a successful merge
call alone does not establish a merge. Verify provider-reported `MERGED`,
the intended base, the merged head and resulting commit before assessing its
dependent. A closed unmerged or superseded predecessor needs reconciliation.

If merge is authorized, apply all [Merge Gates](review-to-merge.md#merge-gates)
and the expected-head guard. After the provider confirms the merge, retarget a
dependent only within existing retarget authority and verify its new base.
Compare old and new base/head, effective diff and relevant consumers. Patch
equivalence alone leaves changed base behavior unproved. Rerun affected
acceptance and integration checks, apply required exact-head CI, and update
review for the current candidate. Carry unaffected evidence only when its
inputs still apply. Use [Guarded Branch Cleanup](review-to-merge.md#guarded-branch-cleanup).
If retargeting alone cannot yield a correct diff, preserve the branch and
route any downstream rewrite as a separate decision.

## Observe One Exact Candidate

Set `repository` and `number` from the approved stack. Run every provider
call through a process runner with a 10-second cap, including standalone
observations, every check page and the final read. For a requested wait,
shorten the cap to the remaining total budget. This command is the first
and last provider read of a stable observation.

```sh
gh pr view "$number" --repo "$repository" \
  --json number,headRefOid,baseRefName,baseRefOid,state,mergedAt,mergeCommit,mergeStateStatus,reviewDecision,isDraft,autoMergeRequest
```

Verify the returned PR number and identity. Save its head SHA as `head` and the
base ref and SHA from that first read. Fetch checks for the pinned head by
pull-request number. Set `owner` and `name` from `repository`. Omit the cursor
for the first page, then supply each returned end cursor until the last page.

```sh
query=$(cat <<'GRAPHQL'
query($owner: String!, $name: String!, $head: GitObjectID!, $number: Int!, $cursor: String) {
  repository(owner: $owner, name: $name) {
    object(oid: $head) {
      ... on Commit {
        oid
        statusCheckRollup {
          contexts(first: 50, after: $cursor) {
            pageInfo { hasNextPage endCursor }
            nodes {
              __typename
              ... on CheckRun {
                name status conclusion
                checkSuite { app { databaseId } }
                isRequired(pullRequestNumber: $number)
              }
              ... on StatusContext {
                context state
                isRequired(pullRequestNumber: $number)
              }
            }
          }
        }
      }
    }
  }
}
GRAPHQL
)
if [ -n "${cursor:-}" ]; then
  gh api graphql -f query="$query" -f owner="$owner" -f name="$name" \
    -f head="$head" -F number="$number" -f cursor="$cursor"
else
  gh api graphql -f query="$query" -f owner="$owner" -f name="$name" \
    -f head="$head" -F number="$number"
fi
```

Require a successful response with no GraphQL errors and the same commit OID
on every page. Reject missing, repeated, or incomplete pagination and
ambiguous context identity. Reconcile required check names and app identities
where applicable against the live repository gates. All required contexts
must have an acceptable terminal result under those gates. Missing,
ambiguous, or unrepresented required evidence stays unverified. A missing or
empty rollup supports an empty requirement set only after separate live
confirmation from the repository gates. `gh pr checks --required` can aid diagnosis. Its response alone
cannot certify a head SHA.

Re-read the PR after fetching checks and before returning a status verdict.
Discard the observation if the head SHA, base ref or base SHA changes.
Use the latest provider state for open/draft/queue status, review conditions,
and merge outcome. A passing check observation is only one merge gate.

## Bound the Wait

For a requested wait, set one monotonic 120-second deadline and run at most
eight observation passes. Between pending passes wait up to 15 seconds, capped
by the remaining budget. Apply an independent timeout to each provider call,
including each check page and the final PR read. Use the lesser of 10 seconds
and the remaining total budget. Check the budget before every call and wait.

Enforce the timeout on the local provider process. On timeout or cancellation
terminate the call and any owned subprocesses and reap them. A subprocess
runner with a per-call timeout can enforce this. A deadline tested only
between blocking calls cannot. Return distinct results for pending at the
pass limit, timeout, rate limit, unavailable provider, cancellation, and
identity drift. Stop earlier for any terminal provider state or provider
error. Report the last verified state and stop reason. Ready requires a
complete current observation and all applicable merge gates.
