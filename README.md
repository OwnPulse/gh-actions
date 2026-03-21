# OwnPulse Shared GitHub Actions

Reusable composite actions for OwnPulse CI/CD pipelines.

## Actions

| Action | Description |
|--------|-------------|
| `docker-build-push` | Build and push Docker image to GHCR |
| `helm-deploy` | Helm deploy with rollout verification and rollback |
| `coverage-badge` | Generate and deploy coverage badge to badges branch |
| `coverage-comment` | Post/update coverage comment on PRs |
| `smoke-test` | Health check smoke test for Kubernetes services |
| `tofu-plan-comment` | Post/update OpenTofu plan comment on PRs |
| `sops-setup` | Install and configure SOPS with age key |

## Usage

Reference actions as:
```yaml
- uses: OwnPulse/gh-actions/docker-build-push@v1
  with:
    context: web
    image-name: ownpulse-web
    tag: ${{ github.sha }}
```

## Versioning

Actions follow semver. Pin to the major version tag (`@v1`) for automatic compatible updates.
