# Namespace Configuration

This document explains how to configure the namespace for the arr-stack deployment.

## Overview

The namespace for the arr-stack deployment is configured using Kustomize's namespace field in the kustomization.yaml files. This allows you to easily change the namespace for different environments.

## How to Configure

### Base Namespace

The default namespace is set in the base kustomization.yaml:

```yaml
# In k8s/kustomize/arr/base/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: media-server

# ...rest of configuration...
```

### Environment-Specific Namespace

Each environment (dev, prod) can override the namespace:

```yaml
# In k8s/kustomize/arr/overlays/dev/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: media-server-dev

# ...rest of configuration...
```

```yaml
# In k8s/kustomize/arr/overlays/prod/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: media-server-prod

# ...rest of configuration...
```

## How It Works

1. Kustomize automatically sets the namespace for all resources using the value from the `namespace` field
2. The namespace is applied to all resources in the deployment
3. Each overlay can override the base namespace

## Best Practices

- Use environment-specific namespaces to isolate different environments
- Keep namespace names consistent with your organization's naming conventions
- Avoid changing the namespace once the deployment is live, as this may cause issues with existing resources
