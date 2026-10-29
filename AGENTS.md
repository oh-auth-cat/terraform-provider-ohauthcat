# AGENTS.md: terraform-provider-ohauthcat

**Read [`../ohauthcat-handbook/AGENTS.md`](../ohauthcat-handbook/AGENTS.md) first.** It holds the hard rules, the owner-gated actions, how to pick a task and the definition of done. This file only adds what is specific to this repo.

## What this repo is
The Terraform provider for OhAuthCat (`registry.terraform.io/ohauthcat/ohauthcat`), built on the HashiCorp Terraform Plugin Framework. It is a thin client over the public management API (`/v1/*`) and never contains server logic.

- **Design:** [PLAN.md](PLAN.md).
- **Work items:** [TASKS.md](TASKS.md).
- **Global order:** [backlog](../ohauthcat-handbook/docs/25-backlog.md).
- **Current state:** [STATUS](../ohauthcat-handbook/STATUS.md).

## Repo-specific rules
- **The single open-source exception (D32).** This is the only OhAuthCat repo that is published as open source. Its licence is chosen by the owner; until then the repo carries no `LICENSE` file. Never copy code from another OhAuthCat repo into this one: everything here becomes public.
- Talks only to the public management API through `internal/client`, using the pinned spec in `spec/openapi-v1.yaml`. A test checks that every route the client calls exists in the pinned spec.
- The API key is sensitive: it comes from `OHAUTHCAT_API_KEY` or the provider block and is never logged, never written to state outside the provider configuration and never put in error messages.
- Write-only API values (IdP client secrets, SAML metadata, SCIM tokens, webhook secrets, log stream credentials) are marked `Sensitive` in the schema.
- Unit tests run against the in-memory fake API in `internal/provider/fakeapi_test.go`. Acceptance tests run only with `TF_ACC=1` against a **local** core stack; never against any cloud environment.
- Publishing to the Terraform Registry and creating the GitHub repository are owner-gated.

## Before you commit
- `make fmt`, `make vet`, `make lint`, targeted `make test PKG=...`, `make docs` (docs drift is a failure).
- Update `TASKS.md` (status), the handbook `STATUS.md` and the backlog tick.
- Never push (the owner pushes).
