# High-Performance Next.js Docker Pipeline

This repository provides a production-ready, highly optimized Docker and GitHub Actions CI pipeline template for Next.js applications. It is designed to demonstrate DevOps best practices, including multi-stage builds and automated CI workflows.

## Key Features

* **Multi-Stage Docker Build:** Minimal production image size using `node:20-alpine`.
* **BuildKit Caching:** Accelerated build times by caching `.next/cache` layers using `--mount=type=cache`.
* **Security Focused:** The application runs on a dedicated non-root `appuser` for enhanced security.
* **Automated CI Pipeline:** A GitHub Actions workflow that validates the Docker build on every push and pull request to `main` or `dev` branches.
* **Local Development:** Includes a `docker-compose.yml` for instant local testing with built-in health checks.

## Project Structure

The project follows a clean and isolated directory structure:

```text
.
├── .github/workflows/
│   └── ci-pipeline.yml    # Automated CI workflow
├── docker/
│   ├── Dockerfile         # Optimized Multi-stage Dockerfile
│   └── .dockerignore      # Excludes node_modules and .next from build context
├── docker-compose.yml     # Local orchestration and health checks
├── .env.example           # Template for environment variables
└── .gitignore             # Prevents sensitive files from being committed
Getting Started
1. Prerequisites
Docker & Docker Compose installed.

A Next.js project (with standalone output enabled in next.config.js).

2. Local Setup
Clone the repository and copy the environment template:

Bash

cp .env.example .env
Run the application locally using Docker Compose:

Bash

docker-compose up --build
The application will be available at http://localhost:3000.

3. CI/CD Configuration
The CI pipeline is configured to use GitHub Secrets for environment variables. To enable automated builds, add these to your repository settings under Settings > Secrets and variables > Actions:

API_SERVER: Your backend API URL.

FIREBASE_KEY: Your Firebase API Key.

Technical Highlights
Docker Optimization
The Dockerfile is split into three distinct stages:

deps: Installs only the necessary dependencies.

builder: Compiles the application and utilizes BuildKit caching for .next/cache.

runner: Creates a minimal production image containing only the standalone output and static assets.

CI Efficiency
The ci-pipeline.yml utilizes GitHub Actions' native cache (type=gha) to persist Docker layers between runs. This mechanism ensures that subsequent builds are significantly faster by reusing unchanged layers.

Health Checks
The docker-compose.yml includes a wget based health check. This ensures that the container is fully operational and responding to requests before it is marked as "healthy" in your orchestration environment.

Open-Source for life! Developed by ilker. Feel free to use, modify, and contribute!
