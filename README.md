# Ankra GitOps Examples

This repository demonstrates modern GitOps practices for Kubernetes cluster management using the Ankra platform. It showcases how to structure and manage Kubernetes environments using Ankra's Stack-based architecture, providing reusable and versioned cluster configurations.

## Overview

Ankra is a modern GitOps platform that enables teams to manage Kubernetes clusters through a stack-based approach. This repository contains example cluster configurations that demonstrate:

- **Stack-based Architecture**: Grouping related services and controlling deployment order
- **GitOps Workflow**: Managing cluster state through Git repositories
- **Dependency Management**: Automatic resolution of stack dependencies
- **Reusable Components**: Versioned, shareable cluster configurations

## Repository Structure

```
ankra-gitops-examples/
├── README.md
└── clusters/
    └── monitoring-stack/        # Complete observability solution
        ├── cluster.yaml          # Cluster definition and stack configuration
        ├── add-ons/             # Helm chart configurations
        │   ├── kube-prometheus-stack/
        │   │   └── values.yaml
        │   └── loki-distributed/
        │       └── values.yaml
        └── manifests/           # Raw Kubernetes YAML files
```

## Example: Monitoring Stack

The `monitoring-stack` example demonstrates a complete observability stack for Kubernetes clusters, including:

### Monitoring Stack Components

- **Prometheus**: Metrics collection and alerting
- **Grafana**: Data visualization and dashboards
- **Loki**: Log aggregation and querying
- **Tempo**: Distributed tracing
- **Fluent Bit**: Log shipping and processing
- **Grafana Alloy**: Telemetry data collection

## Key Features Demonstrated

### 1. Stack-based Organization

The cluster configuration uses Ankra's stack concept to organize services:

```yaml
stacks:
  - name: monitoring
    # Monitoring-related services
```

### 2. Dependency Management

Services are deployed in the correct order using parent-child relationships:

```yaml
addons:
  - name: prometheus
    parents:
      - name: prometheus-namespace
        kind: manifest
```

### 3. External File References

Clean separation of concerns using `from_file` references:

```yaml
configuration:
  from_file: add-ons/prometheus/values.yaml
```

### 4. Namespace Management

Proper namespace isolation with dedicated manifest files:

```yaml
manifests:
  - name: prometheus-namespace
    from_file: manifests/prometheus-namespace.yaml
```

## How to Use This Repository

### Prerequisites

1. **Ankra Platform Access**: Sign up at [platform.ankra.app](https://platform.ankra.app)
2. **Git Repository**: Fork or clone this repository
3. **Kubernetes Cluster**: A target cluster registered with Ankra

### Setup Steps

1. **Connect Repository to Ankra**:
   - Update the `git_repository` section in `cluster.yaml`
   - Set your GitHub credentials in Ankra

2. **Customize Configuration**:
   - Modify Helm values in `add-ons/` directories
   - Update ingress hostnames and TLS settings
   - Adjust resource limits and requests

3. **Deploy the Stack**:
   - Use Ankra's WebUI or CLI to deploy the cluster configuration
   - Monitor deployment progress in the Ankra dashboard

### Using Ankra CLI

```bash
# Install Ankra CLI
bash <(curl -sL https://github.com/ankraio/ankra-cli/releases/latest/download/install.sh)

# Set API token
export ANKRA_API_TOKEN=your-token-here

# Select cluster and deploy
ankra select cluster
ankra apply -f cluster.yaml
```

## Configuration Highlights

### Prometheus Stack Configuration

The `kube-prometheus-stack` is configured with:

- **Persistence**: 50GB storage for metrics data
- **Grafana Integration**: Pre-configured dashboards and datasources
- **Service Monitoring**: Automatic service discovery
- **Ingress**: HTTPS access via `monitoring.seferon`

### Loki Distributed Setup

The distributed Loki deployment includes:

- **Gateway**: NGINX proxy with authentication
- **Scalability**: Distributed architecture for high availability
- **Ingress**: Secure log access via `logs.seferon`
- **OAuth2 Integration**: Centralized authentication

## Best Practices Demonstrated

1. **Environment Separation**: Use different cluster directories for dev/staging/prod
2. **Version Control**: All configurations are tracked in Git
3. **Security**: TLS certificates and authentication configured
4. **Monitoring**: Complete observability stack deployment
5. **Scalability**: Distributed services with proper resource management

## Customization

### Adding New Services

1. Create Helm values file in `add-ons/service-name/values.yaml`
2. Add service configuration to `cluster.yaml`
3. Define dependencies and namespaces as needed

### Modifying Existing Services

1. Update Helm values in the respective `add-ons/` directory
2. Commit changes to trigger GitOps sync
3. Monitor deployment in Ankra dashboard

## Integration with Ankra Platform

This repository integrates with Ankra's advanced features:

- **Stack Builder**: Visual management of stacks and dependencies
- **AI Editor**: Intelligent suggestions for configurations
- **WebUI**: Real-time monitoring and management
- **Multi-cluster Support**: Deploy to multiple environments

## Getting Started

1. **Explore the Examples**: Review the monitoring stack configuration
2. **Read the Documentation**: Check out the comprehensive Ankra documentation
3. **Join the Community**: Connect with other users on [Ankra Slack](https://join.slack.com/t/ankra-community/shared_invite/zt-30r96vykz-BGBKQ_W0F_wQdMeklRuVSg)
4. **Get Support**: Contact support at hello@ankra.io

## Learn More

- [Ankra Stacks Guide](https://docs.ankra.io/essentials/stacks)
- [GitOps with Ankra](https://docs.ankra.io/essentials/cluster-gitops-single)
- [Ankra CLI Reference](https://docs.ankra.io/essentials/ankra-cli)

---

This repository demonstrates the power of GitOps with Ankra's stack-based approach to Kubernetes cluster management. Start with the monitoring stack example and customize it for your specific needs!