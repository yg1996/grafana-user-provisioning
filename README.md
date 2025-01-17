# Grafana User Provisioning During Helm Chart Installation

This setup provisions users during the initial deployment of Grafana using the official Helm Chart. It utilizes `extraInitContainers` to run a script that creates users and assigns roles.

## Project Structure
- `values.yaml`: Custom Helm values to integrate user provisioning.
- `provisioning/configmap.yaml`: Contains the script to provision users.
- `provisioning/secret.yaml`: Stores admin credentials securely.

## Prerequisites
- Kubernetes cluster with Helm installed.
- Access to the Grafana Helm Chart repository.

## Deployment Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yg1996/grafana-user-provisioning.git
   cd grafana-user-provisioning
   ```
2. **Apply ConfigMap and Secret**
   ```bash
   kubectl apply -f provisioning/configmap.yaml
   kubectl apply -f provisioning/secret.yaml
   ```
3. **Deploy Grafana with Custom Values Use the values.yaml file during the Helm Chart installation:**
   ```bash
   helm repo add grafana https://grafana.github.io/helm-charts
   helm install grafana grafana/grafana \
    --namespace grafana \
    --create-namespace \
    --values values.yaml
   ```
2. **Verify User Creation**
   * Log in to Grafana using the admin credentials.
   * Navigate to Configuration > Users to confirm the Viewer, Editor, and Admin users are created.