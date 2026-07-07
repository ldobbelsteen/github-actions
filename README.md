# Custom GitHub Actions Workflows

Collection of reusable GitHub Actions workflows.

## Workflows

### ghcr-build-push

Builds a multi-arch (amd64/arm64) container image and pushes it to the GitHub Container Registry (GHCR). Triggered automatically when a release is published; tags the image with the release version and `latest`.
