# Working on Spoke's GitHub Actions integration

Make customer CI simple: a workflow describes work, Platform supplies execution,
and an operator governs supply through `terraform-provider-spoke`. Do not put
desired runner capacity, cloud provisioning or Terraform in customer workflows.

- Read the README, [workflow contract](docs/workflow-contract.md), and current
  [Platform tracking issue #56](https://github.com/spoke-sh/platform/issues/56).
  Platform owns the delivery sequence; do not create another milestone roadmap.
- Keep boundaries clear: this repository owns workflow inputs, permissions,
  checkout, logs/results, artifacts and distribution. Platform owns admission,
  account/budget authority, runner lifecycle, isolation, cache trust and billing.
  The provider owns the operator governor's HTTP adapter. Transit owns reusable
  native execution/state/effect primitives.
- Verify GitHub's current official workflow, routing, authorization and cancellation
  contracts before changing integration behavior. A step action cannot select its
  enclosing runner. A reusable workflow can own its jobs' `runs-on`.
- This initial repository contains a non-executable template, not a published
  `.github/workflows/run.yml` or `action.yml`. Before activating the entry point,
  qualify the installed service's demand authorization, actual assignment,
  bounded queue/refusal, stop/expiry and credential boundaries. Keep unsupported
  profiles explicit; never queue jobs indefinitely against an invented service.
- Preserve exact caller/source and called-workflow revisions, job attempts and
  artifacts. Pin released dependencies and workflow examples to reviewed immutable
  commits. Keep current caller permissions; do not inherit all secrets by default.
- Keep credentials out of repository files, logs and artifacts. Do not add cloud
  SDKs, privileged runner registration or a separate scheduler to this integration.
- During changes, read current Platform/provider progress and update #56 with
  verified source-specific evidence and the next proof. Re-read before editing
  and preserve other contributors' updates. Historical timings are not benchmarks
  for this integration; never skip qualification to claim a speedup.
- Use logical commits and review PRs. Validate documentation/examples for planning
  changes; run relevant workflow/integration tests when executable behavior changes.
  State implemented, qualified and proposed scope separately.
