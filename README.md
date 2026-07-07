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
    uses: ldobbelsteen/github-actions/.github/workflows/create-release.yml@main
    secrets: inherit
```

Releases created with the default `GITHUB_TOKEN` do not trigger downstream workflows (GitHub's anti-recursion rule). To chain `create-release` -> `build-image`, create a fine-grained PAT and add it as a repository secret named `RELEASE_TOKEN` in each consuming repo.

1. Go to **GitHub -> Settings -> Developer settings -> Personal access tokens -> Fine-grained tokens -> Generate new token**
2. **Resource owner:** your user or organization
3. **Repository access:** select the consuming repo (or "All repositories")
4. **Permissions -> Repository permissions:**
   - **Contents:** Read and write
5. Generate and copy the token
6. In the consuming repo: **Settings -> Secrets and variables -> Actions -> New repository secret**
   - **Name:** `RELEASE_TOKEN`
   - **Value:** the token from step 5

The `release` caller passes `secrets: inherit`, so `create-release.yml` picks up `RELEASE_TOKEN` automatically. If unset, it falls back to `GITHUB_TOKEN` (releases still get created, but `build-image` won't trigger).

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
    uses: ldobbelsteen/github-actions/.github/workflows/ghcr-build-push.yml@main
```

