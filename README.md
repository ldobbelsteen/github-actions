# Custom GitHub Actions Workflows

Collection of reusable GitHub Actions workflows.

## Workflows

### ghcr-build-push

Builds a multi-arch (amd64/arm64) container image and pushes it to the GitHub Container Registry (GHCR).

```yaml
name: ci

on: push

permissions:
  contents: read
  packages: write

jobs:
  build:
    uses: ldobbelsteen/github-actions/.github/workflows/ghcr-build-push.yml@v1
```
