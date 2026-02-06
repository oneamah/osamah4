# Kubernetes Full Stack Deployment

This repo contains Kubernetes manifests managed with Kustomize overlays and a GitHub Actions workflow to build, push, and deploy images.

## Structure

- manifests/base: shared resources (namespace, database, backend, frontend)
- manifests/overlays/dev: dev overrides
- manifests/overlays/staging: staging overrides
- manifests/overlays/prod: prod overrides
- backend/Dockerfile: NestJS container image
- frontend/Dockerfile: Angular container image
- workflow.yaml: GitHub Actions deployment workflow

## Prerequisites

- kubectl and kustomize (for local apply)
- AWS ECR repositories: my-frontend, my-backend
- GitHub secrets:
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY
  - AWS_ACCOUNT_ID
  - KUBE_CONFIG_DEV
  - KUBE_CONFIG_STAGING
  - KUBE_CONFIG_PROD

## Deploy with GitHub Actions

The workflow builds and pushes images to ECR, updates Kustomize image tags, and applies the selected overlay.

## Local apply

Apply an overlay locally:

```
kubectl apply -k manifests/overlays/dev
```

## Notes

- Backend and frontend probes use /healthz. Update if your services use a different path.
- Resource overrides are defined per environment under overlays.
