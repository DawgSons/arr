# Domain Configuration in the ARR Stack

This document explains the domain configuration system used in the ARR stack Kubernetes deployment.

## Overview

The ARR stack uses Kustomize's variable substitution capabilities to make domain names configurable across different environments without requiring manual edits to each manifest file.

## How It Works

1. **Base Configuration**: A `domain-config` ConfigMap is defined in the base layer with default domain values.

2. **Variable Substitution**: The base kustomization.yaml defines variables that reference this ConfigMap.

3. **Templated Ingress Resources**: Each service's ingress configuration uses these variables with the `$(VARIABLE_NAME)` syntax.

4. **Environment Overrides**: Development and production overlays override the domain names by merging new values into the ConfigMap.

## Changing Domain Names

### For All Services

To change the base domain for all services:

1. Edit the appropriate overlay's kustomization.yaml file:
   - For development: `k8s/kustomize/arr/overlays/dev/kustomization.yaml`
   - For production: `k8s/kustomize/arr/overlays/prod/kustomization.yaml`

2. Update the `BASE_DOMAIN` value:
   ```yaml
   configMapGenerator:
   - name: domain-config
     behavior: merge
     literals:
     - BASE_DOMAIN=your-new-domain.com
     # Update individual FQDNs as needed
     - RADARR_FQDN=radarr.your-new-domain.com
     - SONARR_FQDN=sonarr.your-new-domain.com
     # etc.
   ```

### For Individual Services

To change the domain for a specific service only:

1. Edit the appropriate overlay's kustomization.yaml file
2. Update only the specific FQDN you want to change:
   ```yaml
   configMapGenerator:
   - name: domain-config
     behavior: merge
     literals:
     - RADARR_FQDN=movies.your-domain.com  # Custom subdomain for Radarr
     # Keep other domains as they are
   ```

## Adding New Services

When adding a new service with an ingress resource:

1. Add a new FQDN entry in both the base and overlay configurations
2. Define a new var in the base kustomization.yaml file
3. Use the variable in your ingress manifest with the `$(VARIABLE_NAME)` syntax

## Benefits

- **Environment Isolation**: Development and production use completely different domains.
- **Centralized Configuration**: Domain names are defined in one place per environment.
- **Easy Updates**: Change domains in a single file rather than editing multiple ingress files.
- **Consistent Naming**: Standardized approach for all services.

## Customization Examples

### Custom Subdomains

```yaml
configMapGenerator:
- name: domain-config
  behavior: merge
  literals:
  - RADARR_FQDN=movies.example.com  # Custom subdomain
  - SONARR_FQDN=shows.example.com   # Custom subdomain
```

### Multiple Domains

```yaml
configMapGenerator:
- name: domain-config
  behavior: merge
  literals:
  - RADARR_FQDN=radarr.media-domain.com     # Different domain
  - SONARR_FQDN=sonarr.different-domain.net # Different domain
```
