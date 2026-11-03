# Workflow demand contract — proposed v1

Customers call a reusable workflow. Their repository describes work; supply is
owned by a separately authorized Platform governor. Neither a pool resource nor
a cache resource is required in the consuming repository.

## Caller interface

The proposed entry is `.github/workflows/run.yml@<immutable-commit>`, called at
`jobs.<job>.uses`. It has no executable release yet. The initial template is
Linux/x86_64; optional compatibility classes can follow their own service proof.

| Input or result | Contract |
| --- | --- |
| `command` | Required Bash command for the checked-out caller revision; bounded input with explicit shell/working-directory semantics |
| `artifact-path` | Optional caller-workspace output path; uploaded to that GitHub run under bounded size/retention policy; no Platform private-cache credentials |
| Permissions / secrets | Explicit caller job permissions; contents read is sufficient for the initial checkout. No inherited secrets or cloud credentials by default |
| Result | Normal GitHub job success/failure/cancellation and step logs; command exit status is preserved |
| Usage | Platform evidence attributed to the authorized account and exact GitHub run/attempt/job; caller output cannot certify billing |

The integration checks out the accepted caller SHA and uses a reviewed immutable
checkout action with persisted Git credentials disabled. Treat command text as
intentional workload code, but pass it through an environment/file boundary,
not direct expression interpolation into generated shell. Run it inside the
isolated worker. Logs and artifacts remain subject to caller authorization and
secret handling. Artifact failures must not turn a failed command green.

Workflow permissions cannot increase through a reusable call. Resolve exact
GitHub event/ref behavior, nested calls, inputs/outputs, artifact exclusions,
size limits, shell flags, signal propagation and cancellation grace during the
executable contract slice. An arbitrary shell command does not promise every
existing workflow's job containers/services or arbitrary action steps; add only
independently supported profiles. No fake passing fallback is allowed.

## Demand and authority

GitHub creates queued jobs when the called workflow selects Spoke's internal
routing target. A scoped GitHub App/scale-set channel supplies authenticated demand
before execution. Job OIDC cannot bootstrap a job waiting for its first runner.
Labels, runtime hints and caller-supplied tenant IDs are not spending authority.

Platform binds installation, repository, caller/called-workflow revision and
run/attempt/job to current account entitlements. It applies operator-wide and
tenant limits across repositories before reserving supply. Temporary shortages,
policy refusal and expiration remain distinct and visible. Qualification must
prove the real GitHub status/cancellation APIs; do not assume a queued job can be
independently canceled or that a demand event reserves the next runner.

Runner-group repository/workflow access and actual assignment must prevent a
competing unauthorized job from consuming the reservation. Verify account-tier
and cross-organization restrictions for the selected GitHub topology. A public
workflow file alone does not make customers in other organizations eligible.
The initial pilot is limited to the internal organization and approved code.

## Release and qualification

Keep the [template](../templates/run.yml.example) outside `.github/workflows/`
until there is a supported service profile. It records intended mechanics, not
an action customers should copy into production today.

Before publishing an executable workflow:

1. Qualify a caller repository with no Terraform or supply credentials. Preserve
   exact checkout, command result, logs and optional artifacts on one real worker.
2. Prove no-demand means no worker, cancellation/expiry terminate the worker,
   late/duplicate events preserve attempt identity, and GitHub outages remain bounded.
3. Use two repositories under one account and two accounts to prove aggregate
   quota, current authorization, cache isolation and refusal reporting.
4. Pin workflow/action dependencies and test actual repository/group access,
   supported input limits, secret permissions, artifact handling and distribution.
5. Change operator supply policy without changing customer workflow files; keep
   an independently provisioned GitHub-hosted recovery path for Spoke itself.

Paid usage, ARM64, public forks and cross-organization availability are separate
qualification gates. Platform's [tracker](https://github.com/spoke-sh/platform/issues/56)
owns implementation order and measured performance evidence.

GitHub sources reviewed for this proposal:
[workflow reuse versus actions](https://docs.github.com/en/actions/concepts/workflows-and-actions/reusing-workflow-configurations),
[self-hosted routing](https://docs.github.com/en/actions/reference/runners/self-hosted-runners),
[runner-group access](https://docs.github.com/en/enterprise-cloud@latest/actions/how-tos/manage-runners/self-hosted-runners/manage-access).
