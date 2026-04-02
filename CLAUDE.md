# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is the `.github` repository for the Brevis GitHub organization (`brevisdev`). It contains shared composite actions, default community health files (CODEOWNERS, Dependabot config), and organization-wide settings that apply across all Brevis repos.

## Architecture

### Composite Actions

Located in `.github/actions/`, these wrap upstream actions with pinned versions so all org repos share a single version source:

| Action | Wraps | Default |
|--------|-------|---------|
| `checkout` | `actions/checkout` | — |
| `setup-node` | `actions/setup-node` | Node.js 24.x |
| `setup-terraform` | `hashicorp/setup-terraform` | Terraform 1.14.3 |
| `configure-aws-credentials` | `aws-actions/configure-aws-credentials` | eu-north-1 |
| `docker-login` | `docker/login-action` | ghcr.io |
| `docker-build-push` | `docker/build-push-action` | — |
| `github-script` | `actions/github-script` | — |

Actions are consumed via: `brevisdev/.github/.github/actions/<name>@main`

All upstream action references use **pinned commit SHAs** with version comments (e.g., `actions/checkout@<sha> # v6.0.2`).

### Reusable Workflows

Located in `.github/workflows/`, these are shared workflows consumed via `workflow_call`:

| Workflow | Purpose |
|----------|---------|
| `deploy-dev-comment` | Triggers dev deployment from a "dev deploy" PR comment |
| `pr-auto-approve` | Auto-approves a PR when marked ready for review |

Workflows are consumed via: `brevisdev/.github/.github/workflows/<name>.yaml@main`

### Organization Defaults

- `.github/CODEOWNERS` — Default code owners (`@unknoundev/core`)
- `.github/dependabot.yml` — Monthly GitHub Actions version update checks (28th of each month)

## Conventions

- When adding or updating a composite action, pin the upstream action to a **full commit SHA** and add a version comment
- Each action lives in its own directory under `.github/actions/<action-name>/` with an `action.yaml` using `using: composite`
- After changing an action version here, all consuming repos automatically pick up the change on their next workflow run (they reference `@main`)
