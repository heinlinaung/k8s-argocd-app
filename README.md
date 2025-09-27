# Grade Service - Application Repository

This repository contains the **application source code** for the Grade Service, which is part of a GitOps workflow managed by ArgoCD.

## GitOps Architecture Overview

This project follows the **GitOps pattern** with two separate repositories:

```
┌─────────────────────────┐    ┌──────────────────────────┐
│   k8s-argocd-app        │    │  k8s-argocd-deployment   │
│  (This Repository)      │    │                          │
│                         │    │                          │
│  • Application Code     │    │  • Kubernetes Manifests  │
│  • Dockerfile           │    │  • Helm Charts           │
│  • Dependencies         │    │  • ArgoCD Applications   │
└─────────────────────────┘    └──────────────────────────┘
         │                                    │
         │                                    │
         ▼                                    ▼
┌─────────────────────────┐    ┌──────────────────────────┐
│    CI Pipeline          │    │       ArgoCD             │
│                         │    │                          │
│  • Build Docker Image   │───▶│  • Monitors Deployment   │
│  • Push to Registry     │    │    Repository            │
│  • Update Manifests     │    │  • Syncs to Kubernetes   │
└─────────────────────────┘    │  • Manages Rollbacks     │
                               └──────────────────────────┘
```

## GitOps Workflow

### 1. **Development Phase** (This Repository)
- Developers push code changes to this repository
- Contains application source code (`src/app.js`)
- Contains build instructions (`Dockerfile`)
- Contains application dependencies (`package.json`)

### 2. **CI/CD Pipeline**
When code is pushed to this repository:
```bash
# Automatic CI Pipeline Triggers:
1. Build Docker image from source code
2. Tag image with commit SHA or version
3. Push image to container registry
4. Update deployment repository with new image tag
```

### 3. **ArgoCD Sync Process**
- ArgoCD monitors the **k8s-argocd-deployment** repository
- Detects changes in Kubernetes manifests
- Automatically syncs changes to the target cluster
- Ensures desired state matches actual cluster state

### 4. **Deployment Flow**
```
Code Change → CI Build → Image Push → Manifest Update → ArgoCD Sync → Cluster Deployment
```

## Repository Structure

```
k8s-argocd-app/
├── Dockerfile          # Container build instructions
├── README.md          # GitOps documentation (this file)
└── src/
    ├── app.js         # Node.js application
    └── package.json   # Application dependencies
```

## GitOps Benefits

- **🔄 Automated Deployments**: Changes trigger automatic deployment pipeline
- **📋 Declarative Configuration**: Infrastructure as Code approach
- **🔙 Easy Rollbacks**: Git history enables quick rollbacks
- **👁️ Visibility**: Clear audit trail of all changes
- **🔒 Security**: Git-based access control and approval workflows
- **🎯 Consistency**: Same process for all environments

## Development Workflow

1. **Make Changes**: Update application code in this repository
2. **Commit & Push**: Changes trigger CI pipeline automatically
3. **Monitor**: Check ArgoCD dashboard for deployment status
4. **Verify**: Confirm application is running in target environment

## Related Repositories

- **Deployment Repository**: `k8s-argocd-deployment` - Contains Kubernetes manifests and ArgoCD applications
- **Container Registry**: Built images are pushed to your configured registry

## ArgoCD Integration

This application is deployed using ArgoCD with the following configuration:
- **Source**: k8s-argocd-deployment repository
- **Sync Policy**: Automatic (configurable)
- **Self-Heal**: Enabled to maintain desired state
- **Prune**: Removes resources not defined in Git

## Getting Started

1. **Clone this repository** for application development
2. **Make code changes** in the `src/` directory  
3. **Push changes** - CI pipeline handles the rest
4. **Monitor deployment** via ArgoCD UI or CLI

## Application Details

- **Technology**: Node.js with Express framework
- **Port**: 3000
- **Endpoints**: REST API for grade management
- **Base Image**: node:14

---
## Github Action Pipeline test
- v1.0.0

*This repository is part of a GitOps workflow. For deployment configuration, see the `k8s-argocd-deployment` repository.*