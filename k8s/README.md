# Kubernetes Deployment

This directory contains Kubernetes manifests organized with Kustomize to deploy the arr-stack media server.

## Structure

```
/kustomize
  /arr
    /base             # Base manifests
      /common         # Shared resources (namespace, configmap, volumes)
      /radarr         # Radarr manifests
      /sonarr         # Sonarr manifests
      /bazarr         # Bazarr manifests
      # ...other services
    /overlays         # Environment-specific overlays
      /dev            # Development environment
      /prod           # Production environment
```

## Deployment Instructions

### Development Environment

To deploy to a development environment:

```bash
# Apply the development overlay
kubectl apply -k kustomize/arr/overlays/dev
```

### Production Environment

To deploy to a production environment:

```bash
# Apply the production overlay
kubectl apply -k kustomize/arr/overlays/prod
```

### Using Flux CD (GitOps)

If you're using Flux CD for GitOps:

1. Install Flux on your cluster (if not already installed)
2. Apply the Flux resources:

```bash
# Add the Git repository source
kubectl apply -f apps/arr-stack-source.yaml

# Apply the Kustomization
kubectl apply -f apps/arr-stack.yaml
```

## Configuration

1. Domain Configuration:
   - Edit the ingress hosts in the overlay patches:
     - Dev: `kustomize/arr/overlays/dev/patch-ingress.yaml`
     - Prod: `kustomize/arr/overlays/prod/patch-ingress.yaml`

2. Resource Limits:
   - Edit resource requests/limits in:
     - Base defaults: In each service's deployment file
     - Production: `kustomize/arr/overlays/prod/patch-resources.yaml`

3. Storage:
   - Edit the PersistentVolumeClaims in `kustomize/arr/base/common/persistent-volumes.yaml`

## Adding New Services

To add a new service:

1. Create a new directory in `kustomize/arr/base/`
2. Add the service manifests (deployment, service, ingress)
3. Create a kustomization.yaml file
4. Add the service to the main kustomization file at `kustomize/arr/base/kustomization.yaml`
5. Add ingress patches in the overlays if needed
