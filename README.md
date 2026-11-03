# Spoke GitHub Action

Run CI through Spoke from a GitHub Actions workflow. The workflow generates work;
Spoke supplies isolated execution and managed build caches under an operator's
Terraform-managed governor. Your repository declares no worker fleet or capacity.

**Status: contract and workflow template only.** No executable action/reusable
workflow or hosted runner service is published yet. The example below shows the
proposed interface; its workflow path and commit placeholder are not runnable.

```yaml
jobs:
  check:
    uses: spoke-sh/spoke-github-action/.github/workflows/run.yml@<reviewed-commit-sha>
    permissions:
      contents: read
    with:
      command: just check
```

An organization authorizes the Spoke GitHub App and selected repositories once.
After that, a workflow call creates demand. Spoke resolves the authorized account,
admits work within shared limits, supplies a worker and records execution/usage.
The caller manages its command and GitHub permissions; it needs no Terraform,
machine size, runner count, governor ID, cloud credentials or cache resource.

The first integration will be a **job-level reusable workflow** so it can select
the execution environment and retain normal GitHub job logs and conclusions.
A step action runs after its enclosing runner has been allocated and cannot
move the surrounding job. See the [workflow contract](docs/workflow-contract.md)
and [non-executable template](templates/run.yml.example).

Platform owns the [demand/governor service proposal](https://github.com/spoke-sh/platform/blob/dcb40932b1e927b0525a3d6fa392142ed0b5044a/contracts/ci-demand-and-governor-v1.md),
admission, isolation, caches and billing. The
[Terraform provider](https://github.com/spoke-sh/terraform-provider-spoke/pull/19)
owns the operator governor adapter. This repository owns the caller experience,
workflow distribution and integration evidence. Delivery follows
[Platform's plan](https://github.com/spoke-sh/platform/blob/dcb40932b1e927b0525a3d6fa392142ed0b5044a/docs/roadmap/ci-runner-implementation.md)
and [tracker #56](https://github.com/spoke-sh/platform/issues/56), with Transit as
the first internal consumer.

The next step is to qualify workflow routing and demand authorization against
the governor contract. A single-organization pilot does not establish cross-org
availability. No machine was provisioned, benchmark run or speedup established by
this repository's initial contract.
