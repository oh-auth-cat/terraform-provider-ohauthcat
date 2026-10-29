# TASKS: terraform-provider-ohauthcat

Format: [handbook AGENTS §5](../ohauthcat-handbook/AGENTS.md#5-task-contract-format-used-in-every-tasksmd).

### TFP-001: Terraform provider, open source per D32
Status: in progress (agent, 2026-10-29)
Phase: launch parity   Depends on: —   Estimate: M
Goal: a Terraform provider covering the management API's configuration objects, ready for the owner to publish.
Inputs: handbook docs/31, docs/10 D32, docs/19, docs/07; `spec/openapi-v1.yaml`.
Steps: as in [PLAN.md](PLAN.md).
Acceptance criteria:
- Every resource in PLAN.md supports create, read, update (or replace), delete and import, and detects out-of-band drift; each has a unit test against the fake API.
- Data sources `ohauthcat_organization`, `ohauthcat_connection`, `ohauthcat_environment` have unit tests.
- The API key is sensitive and never logged.
- Acceptance-test scaffolding skips without `TF_ACC`.
- `docs/` (Registry layout) and `examples/` are present and `make docs` is drift-free.
Gates: `make fmt`, `make vet`, `make lint`, `make test PKG=./internal/...`, `make docs`, `make licenses`.
Out of scope: publishing to the Registry, creating remotes, choosing the licence (owner).

### TFP-002: Publish to the Terraform Registry
Status: blocked (owner: licence choice, GitHub repository, GPG signing key, Registry sign-in)
Phase: launch parity   Depends on: TFP-001   Estimate: S
Goal: the provider is installable as `ohauthcat/ohauthcat`.
Inputs: README "Publishing".
Steps: owner picks the licence; adds `LICENSE`; creates the public GitHub repository; adds the GoReleaser config and signing key; tags `v0.1.0`; publishes in the Registry.
Acceptance criteria: `terraform init` resolves `ohauthcat/ohauthcat`.
Gates: Registry shows the version with docs.
Out of scope: anything not owner-approved.
