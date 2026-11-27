# Second Eye Contract

Optional vendor review tools plug into `$code-review` through this contract.
`$coderabbit-second-eye` is the v1 provider. A future vendor skill should satisfy
the same inputs, outputs, and failure rules so `$code-review` only renames the
soft-load target.

## When `$code-review` invokes a second eye

Run the installed provider after Phase 1's own finder packets complete and
before Phase 2 normalization when all of the following hold:

- Review effort is Medium, High, or Max, or the user explicitly requested
  independent review or a second-eye pass.
- The provider skill is installed, or the provider documents a CLI-only
  fallback the skill would have used.

Do not invoke a second eye on Low effort unless the user explicitly asked for
it. Do not call vendor CLIs from `$code-review` itself.

## Inputs the provider receives

- Review scope already resolved by Phase 0 (base, head, working-tree inclusion,
  file set, and any explicit directory or PR target).
- Chosen review effort.
- Whether the run is interactive or Composing Delivery Mode.

## Outputs the provider must return

A list of candidates already shaped for the Candidate Standard, plus:

```text
source          # stable vendor id, e.g. coderabbit
source_ref      # optional finding id, URL, or CLI record id
provisional_severity  # Critical | High | Medium | Low after the provider map
```

Every item is a **candidate**, never a confirmed finding. `$code-review` owns
normalize, verify, rank, and the Review Queue or Delivery Return Record.

Also return a short pass status:

- `ran` — vendor review completed (zero findings is still `ran`)
- `skipped` — not installed, not authenticated, not configured, or user declined
- `failed` — auth, network, credits, timeout, or tool error after an attempt

## Failure and skip rules

Auth failure, missing CLI, missing skill, network errors, credit limits,
timeouts, and vendor `review_skipped` results **skip the second eye only**.
They never fail the whole `$code-review` run.

When effort qualifies and the provider is missing or unusable, tell the
developer which skill or CLI is missing and why the second eye matters, then
continue. Do not install vendor CLIs or vendor skills from `$code-review` or
the provider. On Low effort, do not nag about a missing second eye unless the
user asked for it.

## Configuration

If the provider needs repository configuration and none exists, the provider
may analyze the repo and propose config through the vendor's official config
skill or CLI. Save only after explicit approval. Do not silently overwrite an
existing config file.

## Standalone invocation

A provider skill may be invoked directly. Standalone runs still return
candidates into `$code-review`'s verify and queue path unless the user asked
only for the vendor's own no-handoff skill.
