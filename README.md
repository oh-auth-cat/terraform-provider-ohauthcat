# terraform-provider-ohauthcat

The Terraform provider for [OhAuthCat](https://ohauthcat.com): manage organizations, domains, SSO connections (SAML and OIDC), SCIM directories, webhooks, sign-in policies, role assignment rules, portal profiles, branding, log streams and custom domains as code. It is a thin client over the public management API (`https://api.ohauthcat.com/v1`). Provider address: `registry.terraform.io/ohauthcat/ohauthcat` (not yet published).

## Status
Launch parity (TFP-001). Every resource and data source listed below is implemented and unit-tested against an in-memory fake of the API; it has not yet been run against a live environment. Not published to the Registry. See the handbook [STATUS.md](../ohauthcat-handbook/STATUS.md).

## Sibling repos

| Repo | Role |
|---|---|
| [ohauthcat-handbook](../ohauthcat-handbook) | Plan, decisions, status |
| [ohauthcat-core](../ohauthcat-core) | Go backend and the OpenAPI spec this provider pins |
| [ohauthcat-dashboard](../ohauthcat-dashboard) | Developer dashboard |
| [ohauthcat-portal](../ohauthcat-portal) | IT-admin setup portal |
| [ohauthcat-sdks](../ohauthcat-sdks) | Node and Python SDKs |
| [ohauthcat-cli](../ohauthcat-cli) | `ohauthcat` CLI |
| [ohauthcat-docs](../ohauthcat-docs) | Public docs site |
| [ohauthcat-landing](../ohauthcat-landing) | Marketing site |
| [ohauthcat-admin](../ohauthcat-admin) | Internal staff console |
| [ohauthcat-e2e](../ohauthcat-e2e) | Local end-to-end suite |

## Getting started
Prerequisites: Go 1.26 toolchain (fetched automatically from `go.mod`), Terraform ≥ 1.8 on `PATH` for tests.

```sh
make build                      # bin/terraform-provider-ohauthcat
make test PKG=./internal/...    # unit tests against the fake API
```

Using a local build:

```hcl
# ~/.terraformrc
provider_installation {
  dev_overrides { "ohauthcat/ohauthcat" = "/absolute/path/to/terraform-provider-ohauthcat/bin" }
  direct {}
}
```

```hcl
provider "ohauthcat" {}   # reads OHAUTHCAT_API_KEY

resource "ohauthcat_organization" "acme" {
  name        = "Acme"
  external_id = "acme"
}
```

## Commands

| Target | Does |
|---|---|
| `make build` | Builds `bin/terraform-provider-ohauthcat` |
| `make fmt` | `go fmt` + `goimports` |
| `make vet` | `go vet ./...` |
| `make lint` | `golangci-lint run` |
| `make vuln` | `govulncheck ./...` |
| `make test PKG=./internal/x/...` | Targeted tests; refuses to run without `PKG` |
| `make testacc PKG=…` | Acceptance tests (`TF_ACC=1`) against a local core stack only |
| `make docs` | Regenerates `docs/` with `tfplugindocs` |
| `make check-docs` | Regenerates and fails on drift |
| `make licenses` | Dependency licence check |
| `make third-party-licenses` | Writes `THIRD_PARTY_LICENSES` |
| `make check-spec` | Compares the pinned spec with `../ohauthcat-core` `dev` (local only) |

## Project layout
- `main.go`: provider server entry point.
- `internal/client/`: typed management API client.
- `internal/provider/`: provider, resources, data sources and their tests.
- `examples/`: HCL examples used by the docs generator.
- `docs/`: Registry documentation (generated).
- `spec/`: pinned OpenAPI spec and its source commit.
- `scripts/`: licence gate and spec pin check.
- `templates/`: docs generator templates.

## Configuration

| Setting | Env var | Default |
|---|---|---|
| `api_key` | `OHAUTHCAT_API_KEY` | required, sensitive |
| `base_url` | `OHAUTHCAT_BASE_URL` | `https://api.ohauthcat.com` |

## Tech stack
- Go 1.26 (`toolchain go1.26.10`).
- `terraform-plugin-framework`, `terraform-plugin-go`, `terraform-plugin-log` (MPL-2.0, owner review pending).
- `terraform-plugin-testing` for unit and acceptance tests; `terraform-plugin-docs` for docs (dev only).

## Testing and quality gates
CI runs format, vet, lint, govulncheck, licences, docs drift and targeted tests. Run one test: `make test PKG=./internal/provider/... RUN=TestOrganization`.

## Conventions
Branches `main` → `staging` → `dev`, feature branches from `dev`, fast-forward merges, Conventional Commits. See [AGENTS.md](AGENTS.md) and the [handbook](../ohauthcat-handbook/AGENTS.md).

## Publishing (owner)
See TFP-002 in [TASKS.md](TASKS.md).

## Licence
Open source per D32; the licence is not chosen yet, so no licence is granted until a `LICENSE` file is added.
