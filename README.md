# Building & Publishing Docker Images in CI

## Overview
This repository demonstrates a CI pipeline that automatically builds a Docker image on every push to `main`.

## Pipeline

### 0. Build image in CI
On every push to `main`, a GitHub Actions workflow (`.github/workflows/image.yml`) checks out the repository and builds the Docker image using Docker Buildx. No image is published at this stage, the goal is only to confirm the image builds successfully in a clean CI environment.

**Workflow file:** `.github/workflows/image.yml`

**Example successful run:** [View run](https://github.com/benji3106/holbertonschool-continuous_integrations/actions/runs/33051992693)

## Application
The application (`Dockerfile`, `package.json`, `server.js`) is reused from a previous project (`docker_optimization` : hardened Node.js image with non-root user and healthcheck).