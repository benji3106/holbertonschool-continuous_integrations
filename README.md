# Building & Publishing Docker Images in CI

## Overview
This repository demonstrates a CI pipeline that automatically builds a Docker image on every push to `main`.

## Pipeline

### 0. Build image in CI
On every push to `main`, a GitHub Actions workflow (`.github/workflows/image.yml`) checks out the repository and builds the Docker image using Docker Buildx. No image is published at this stage, the goal is only to confirm the image builds successfully in a clean CI environment.

**Workflow file:** `.github/workflows/image.yml`

**Example successful run:** [View run](https://github.com/benji3106/holbertonschool-continuous_integrations/actions/runs/33051992693)

### 1. Publish to a registry
On every push to `main`, the workflow authenticates to GitHub Container Registry (GHCR) using the auto-generated `GITHUB_TOKEN` (no hardcoded credentials), then builds and pushes the image. Each push is tagged with both `latest` and the short commit SHA (`sha-<short_sha>`), so any published image can be traced back to the exact commit that produced it.

**Published image:** [ghcr.io/benji3106/holbertonschool-continuous_integrations](https://github.com/benji3106/holbertonschool-continuous_integrations/pkgs/container/holbertonschool-continuous_integrations)

```bash
docker pull ghcr.io/benji3106/holbertonschool-continuous_integrations:latest
```

### 2. Meaningful tagging strategy
Tags are generated automatically from the Git context using `docker/metadata-action`:
- `latest` : always points to the most recent build on the default branch (`main`)
- `main` (or the branch name) : tracks the branch an image was built from
- `sha-<short_sha>` : traces an image back to the exact commit that produced it
- `<version>` (e.g. `1.0.0`) : generated only when a semver Git tag (`vX.Y.Z`) is pushed, marking an official release

**Example release run:** [View run](https://github.com/benji3106/holbertonschool-continuous_integrations/actions/runs/33056173356)

**Example release image:** [ghcr.io/benji3106/holbertonschool-continuous_integrations:1.0.0](https://github.com/benji3106/holbertonschool-continuous_integrations/pkgs/container/holbertonschool-continuous_integrations)

## Application
The application (`Dockerfile`, `package.json`, `server.js`) is reused from a previous project (`docker_optimization`: hardened Node.js image with non-root user and healthcheck).