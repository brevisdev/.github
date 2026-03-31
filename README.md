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

### setup-terraform

Wraps `hashicorp/setup-terraform`. Defaults to Terraform 1.14.3.

```yaml
- uses: brevisdev/.github/.github/actions/setup-terraform@main
  with:
    token: ${{ secrets.TF_API_TOKEN }}  # optional, for Terraform Cloud
```

## Organization Defaults

- `.github/CODEOWNERS` — Default code owners for all repos
- `.github/dependabot.yml` — Default Dependabot configuration

## Adding a New Composite Action

1. Create a directory under `.github/actions/<action-name>/`
2. Add `action.yaml` with `using: composite`
3. Document usage in this README
4. Update consuming repos to use `brevisdev/.github/.github/actions/<action-name>@main`
