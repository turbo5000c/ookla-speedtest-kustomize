# Speedtest Tracker Kubernetes Deployment

This repository contains a `kustomize` setup to deploy [Speedtest Tracker](https://github.com/henrywhitaker3/Speedtest-Tracker) in a Kubernetes cluster. The deployment includes a dedicated namespace, ConfigMap, Secret, and resource limits for the application.

## Prerequisites

- A running Kubernetes cluster
- `kubectl` installed and configured
- `kustomize` support (built into `kubectl` since version 1.14)

## Deployment Overview

The deployment consists of:
- **Namespace**: A dedicated namespace for isolation.
- **Deployment**: The main application container.
- **Service**: Exposes the application within the cluster.
- **ConfigMap**: Stores non-sensitive environment variables.
- **Secret**: Stores sensitive database credentials securely.
- **Resource Limits**: Ensures fair resource usage.

## Files

| File               | Description                                                      |
|--------------------|------------------------------------------------------------------|
| `kustomization.yaml` | Entry point for `kustomize`, references all other resources.    |
| `namespace.yaml`     | Defines the `speedtest-tracker` namespace.                     |
| `deployment.yaml`    | Defines the Deployment resource for the application.           |
| `service.yaml`       | Exposes the application as a ClusterIP service.               |
| `configmap.yaml`     | Stores non-sensitive environment variables.                    |
| `secret.yaml`        | Stores sensitive database credentials securely.               |

## Configuration

### ConfigMap (`configmap.yaml`)
Non-sensitive environment variables are stored in the ConfigMap:
- `PUID`: User ID for file permissions.
- `PGID`: Group ID for file permissions.
- `TZ`: Timezone setting.
- `DB_CONNECTION`: Database type (`sqlite`, `pgsql`, or `mysql`).
- Additional configuration options for Speedtest Tracker.

### Secret (`secret.yaml`)
Sensitive database credentials are stored in the Secret:
- `DB_HOST`: Database host.
- `DB_PORT`: Database port.
- `DB_DATABASE`: Database name.
- `DB_USERNAME`: Database username.
- `DB_PASSWORD`: Database password.

### Resource Limits
The deployment enforces resource requests and limits:
- Memory: `128Mi` minimum, `256Mi` maximum.
- CPU: `100m` minimum, `500m` maximum.

Adjust these values in `deployment.yaml` based on your environment.

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo/speedtest-tracker-kustomize.git
   cd speedtest-tracker-kustomize
   ```

2. Copy `secret.yaml.example` to `secret.yaml`:
   ```bash
   cp secret.yaml.example secret.yaml
   ```

3. Replace the placeholder values with your actual secrets.

4. Apply the manifests using `kustomize`:
   ```bash
   kubectl apply -k .
   ```

5. Verify the deployment:
   ```bash
   kubectl get pods -n speedtest-tracker
   kubectl get svc -n speedtest-tracker
   ```

6. Access the application:
   - The service is exposed internally via ClusterIP. Use port-forwarding or update the service type to `NodePort` or `LoadBalancer` for external access.

## Notes

- Replace `/path/to/speedtest-tracker/data` in `deployment.yaml` with the actual path for storing data.
- Update `configmap.yaml` and `secret.yaml` with your specific configuration.
- Use `kubectl edit` or update the manifests directly for any configuration changes.

## Troubleshooting

- Check pod logs:
   ```bash
   kubectl logs -n speedtest-tracker <pod-name>
   ```

- Ensure the database credentials and environment variables are correctly configured.

## Resources

- [Speedtest Tracker Documentation](https://github.com/alexjustesen/speedtest-tracker)
- [Kustomize Documentation](https://kustomize.io/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
