# Custom GitHub Actions Workflows

Collection of reusable GitHub Actions workflows.

## Workflows

### create-release

Reusable workflow that creates a GitHub release. Auto-increments the patch version (e.g. `v1.0.0` -> `v1.0.1`) and generates release notes from commits since the last release. Intended to be called on push to `main`.

```yaml
name: release

on:
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  release:
    uses: ldobbelsteen/github-actions/.github/workflows/create-release.yml@v1
```

### ghcr-build-push

Reusable workflow that builds a multi-arch (amd64/arm64) container image and pushes it to the GitHub Container Registry (GHCR), tagged with the release version and `latest`. Intended to be called on release.

```yaml
name: build-image

on:
  release:
    types: [released]

permissions:
  contents: read
  packages: write

jobs:
  build:
    uses: ldobbelsteen/github-actions/.github/workflows/ghcr-build-push.yml@v1
```

