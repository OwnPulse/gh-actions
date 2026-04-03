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
| `tailscale-cluster-connect` | Connect to a k3s cluster over Tailscale with kubectl and helm |

## Usage

Reference actions as:
```yaml
- uses: OwnPulse/gh-actions/docker-build-push@v1
  with:
    context: web
    image-name: ownpulse-web
    tag: ${{ github.sha }}
```

### `tailscale-cluster-connect`

Connects to a k3s cluster over Tailscale. Handles the full bootstrap sequence:
Tailscale OAuth login, peer discovery, kubeconfig fetch via SSH, server URL
rewrite, kubectl and helm installation, and connectivity verification.

```yaml
- uses: OwnPulse/gh-actions/tailscale-cluster-connect@v1
  with:
    oauth-client-id: ${{ secrets.TS_OAUTH_CLIENT_ID }}
    oauth-secret: ${{ secrets.TS_OAUTH_SECRET }}
    ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

After this step, `kubectl` and `helm` are installed and `KUBECONFIG` points to
the cluster. All subsequent steps in the job can use them directly.

**Inputs:**

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `oauth-client-id` | yes | | Tailscale OAuth client ID |
| `oauth-secret` | yes | | Tailscale OAuth client secret |
| `ssh-private-key` | yes | | SSH ed25519 private key for the k3s node |
| `tags` | no | `tag:ci-runner` | Tailscale ACL tags |
| `hostname` | no | `ownpulse-us` | Tailscale hostname of the k3s node |
| `peer-discovery-timeout` | no | `60` | Seconds to wait for peer discovery |
| `install-helm` | no | `true` | Whether to install Helm |
| `helm-version` | no | `v4.1.3` | Helm version to install |

**Outputs:**

| Output | Description |
|--------|-------------|
| `peer-ip` | Tailscale IPv4 address of the k3s node |
| `kubeconfig` | Path to the generated kubeconfig file |

## Versioning

Actions follow semver. Pin to the major version tag (`@v1`) for automatic compatible updates.
