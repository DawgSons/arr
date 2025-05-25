# ARR Media Server Stack

A collection of media server applications (Radarr, Sonarr, etc.) deployable via Docker Compose or Kubernetes.

## Components

- 🎬 **Radarr**: Movie management
- 📺 **Sonarr**: TV show management
- 🔤 **Bazarr**: Subtitle management
- 📥 **SABnzbd**: Usenet downloader
- 🔍 **Prowlarr**: Indexer manager
- 🎵 **YTPlaylistarr**: YouTube playlist downloader
- 🖥️ **Jellyfin**: Media server
- � **Heimdall**: Application dashboard

## Deployment Options

### Docker Compose

For simple deployments, use Docker Compose:

```bash
cd docker
docker-compose up -d
```

### Kubernetes

For Kubernetes deployments using kustomize:

1. Development environment:
```bash
kubectl apply -k k8s/kustomize/arr/overlays/dev
```

2. Production environment:
```bash
kubectl apply -k k8s/kustomize/arr/overlays/prod
```

3. Using Flux CD (GitOps):
```bash
kubectl apply -f k8s/apps/arr-stack-source.yaml
kubectl apply -f k8s/apps/arr-stack.yaml
```

## File Structure

```
/docker               # Docker Compose deployment
  docker-compose.yml  # Main Docker Compose configuration
  ytplaylistarr/      # Custom YouTube playlist downloader

/k8s                  # Kubernetes deployment
  /apps               # Flux CD application definitions
  /kustomize          # Kustomize manifests
    /arr              # Main application stack
      /base           # Base manifests
        /common       # Shared resources (namespace, configmap, volumes)
        /radarr       # Radarr manifests
        /sonarr       # Sonarr manifests
        /bazarr       # Bazarr manifests
        # ...other services
      /overlays       # Environment-specific overlays
        /dev          # Development environment
        /prod         # Production environment

/.github              # GitHub related files
  /Todo.md            # Project task list
  /CopilotTodo.md     # Copilot task management config
```

## Configuration

### Docker Compose

Edit the `docker-compose.yml` file to adjust:
- Volume mounts
- Environment variables
- Port mappings

### Kubernetes

1. Common config: `k8s/kustomize/arr/base/common/configmap.yaml`
2. Environment-specific configurations in the overlay directories

## Requirements

### Docker Compose
- Docker
- Docker Compose

### Kubernetes
- Kubernetes cluster (k3s, k8s, etc)
- kubectl
- kustomize
- Optional: Flux CD for GitOps

## License

MIT