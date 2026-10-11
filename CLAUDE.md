# KubeOpenCode agent entry point

`AGENTS.md` is a symlink to this file; keep that link intact.

## Scope and runtime

KubeOpenCode is a Kubernetes-native controller for `Task`, `TaskTemplate`,
`Agent`, and optional `KubeOpenCodeConfig` resources. It uses Go, Kubernetes
controller-runtime, Helm, and one container image with `controller`,
`git-init`, `context-init`, and `url-fetch` subcommands. Preserve the
project's Kubernetes-native design and the absence of PostgreSQL/Redis.

The complete project reference, including API fields, examples, local and
remote cluster procedures, agent configuration, test recipes, merge semantics,
and documentation map, is [docs/agents/development-reference.md](docs/agents/development-reference.md).
Read only the relevant section before changing an unfamiliar area. For
OpenCode-specific operation, that document remains the standalone runtime
guide; in Codex, delegate only through native Codex task tools.

## Work contract

Before a nontrivial change, state the observable outcome, components to reuse,
the intended changes, and the smallest recipient-facing check. A small,
well-understood local change can proceed directly. Preserve unrelated changes,
avoid broad refactors, and stop after the accepted outcome and applicable
checks pass.

## Safety and delivery

- Treat cluster, registry, secret, RBAC, security, and deployment changes as
  high-impact. Inspect target cluster, namespace, image, rollout, and required
  permissions before acting.
- Keep local Kind E2E commands on local Kind clusters. For a remote cluster,
  use the documented build/push and rollout procedure only with task authority.
- Review the exact diff, target branch, required checks, and deployment effects
  before a low-risk merge. Escalate a material risk or unresolved product
  decision; do not treat a merge as a deployment authorization.
- Keep generated CRDs and deepcopy code in sync when an API change requires it.

## Verification

- Choose the smallest check that proves the changed behavior: focused unit or
  integration test for local controller behavior; the relevant build/lint check
  for a build or generated-code change.
- For any API field change, follow the complete API-change checklist in the
  reference: update type and markers, regenerate and verify, update
  documentation, integration tests, and E2E tests. API work is incomplete
  without all of them.
- When the requested work is E2E testing, run the documented three-step Kind
  flow (`e2e-teardown`, `e2e-setup`, `e2e-test`) or the documented iterative
  reload flow after a successful full flow in the same session. Do not use that
  requirement for unrelated edits.
- For agent-image changes, follow the reference checklist including the
  GitHub workflow path filters and the local image build.

## Communication

Report the observed result, version or environment, checks performed, and
material gaps. Distinguish local verification from remote CI or deployment.
