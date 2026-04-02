# .github

Shared GitHub configuration for the Brevis organization. Contains composite actions, default community health files, and organization-wide settings.

## Composite Actions

Centralized action wrappers so action versions are managed in one place. Update the version here and all repos pick it up.

### checkout

Wraps `actions/checkout`.

```yaml
- uses: brevisdev/.github/.github/actions/checkout@main
  with:
    ref: ${{ inputs.branch }}  # optional
```

### setup-node

Wraps `actions/setup-node`. Defaults to Node.js 24.x.

```yaml
- uses: brevisdev/.github/.github/actions/setup-node@main

# or with a specific version
- uses: brevisdev/.github/.github/actions/setup-node@main
  with:
    node-version: '22.x'
```

### github-script

Wraps `actions/github-script`. Passes through `result` output.

```yaml
- uses: brevisdev/.github/.github/actions/github-script@main
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    script: |
      console.log('Hello from script')
```

### configure-aws-credentials

Wraps `aws-actions/configure-aws-credentials` via OIDC. Defaults to `eu-north-1`.

```yaml
- uses: brevisdev/.github/.github/actions/configure-aws-credentials@main
  with:
    role-to-assume: ${{ vars.AWS_DEPLOYMENT_ROLE_ARN }}
```

### docker-login

Wraps `docker/login-action`. Defaults to `ghcr.io`.

```yaml
- uses: brevisdev/.github/.github/actions/docker-login@main
  with:
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}
```

### docker-build-push

Wraps `docker/build-push-action`.

```yaml
- uses: brevisdev/.github/.github/actions/docker-build-push@main
  with:
    tags: ghcr.io/${{ github.repository }}:latest
```

### setup-terraform

Wraps `hashicorp/setup-terraform`. Defaults to Terraform 1.14.3.

```yaml
- uses: brevisdev/.github/.github/actions/setup-terraform@main
  with:
    token: ${{ secrets.TF_API_TOKEN }}  # optional, for Terraform Cloud
```

## Reusable Workflows

### deploy-dev-comment

Triggers a dev deployment when someone comments "dev deploy" on a PR. Reacts with a rocket emoji and triggers the repo's `deploy-dev.yaml` workflow.

```yaml
# In consuming repo: .github/workflows/deploy-dev-comment.yaml
name: Dev - deploy comment
on:
  issue_comment:
    types: [created]
jobs:
  deploy:
    uses: brevisdev/.github/.github/workflows/deploy-dev-comment.yaml@main
    secrets: inherit
```

### pr-auto-approve

Auto-approves a PR when it is marked as ready for review. Requires a `PR_REVIEW_PAT` secret with a PAT that has permission to approve PRs.

```yaml
# In consuming repo: .github/workflows/pr-auto-approve.yaml
name: Auto Approve
on:
  pull_request:
    types: [ready_for_review]
jobs:
  auto-approve:
    uses: brevisdev/.github/.github/workflows/pr-auto-approve.yaml@main
    secrets:
      PR_REVIEW_PAT: ${{ secrets.PR_REVIEW_PAT }}
```

## Organization Defaults

- `.github/CODEOWNERS` — Default code owners for all repos
- `.github/dependabot.yml` — Default Dependabot configuration

## Adding a New Composite Action

1. Create a directory under `.github/actions/<action-name>/`
2. Add `action.yaml` with `using: composite`
3. Document usage in this README
4. Update consuming repos to use `brevisdev/.github/.github/actions/<action-name>@main`
