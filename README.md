# Grafana User Provisioning

This repository contains two Grafana user provisioning setups using Helm:
1. **3 Users Deployment**: A simple setup for provisioning three hardcoded users.
2. **50 Users Deployment**: A dynamic setup for provisioning up to 50 users from a ConfigMap.

---

## **3 Users Deployment**

### Overview
- Hardcoded users are stored in the provisioning script within the ConfigMap.
- Admin credentials and user passwords are stored in a Kubernetes Secret.

### Steps
1. Apply the ConfigMap and Secret:
   - Run the following commands:
     kubectl apply -f 3_users_deployment/provisioning/configmap.yaml
     kubectl apply -f 3_users_deployment/provisioning/secret.yaml

2. Deploy Grafana using the Helm values file:
   - Run the following command:
     helm install grafana grafana/grafana \
       --namespace grafana \
       --create-namespace \
       --values 3_users_deployment/values.yaml

---

## **50 Users Deployment**

### Overview
- User list is stored in a `users.json` format within the `configmap-users.yaml`.
- Passwords are stored in a Kubernetes Secret and referenced dynamically.

### Steps
1. Apply the ConfigMaps and Secret:
   - Run the following commands:
     kubectl apply -f 50_users_deployment/provisioning/configmap-users.yaml
     kubectl apply -f 50_users_deployment/provisioning/configmap-script.yaml
     kubectl apply -f 50_users_deployment/provisioning/secret.yaml

2. Deploy Grafana using the Helm values file:
   - Run the following command:
     helm install grafana grafana/grafana \
       --namespace grafana \
       --create-namespace \
       --values 50_users_deployment/values.yaml

---

## Cleanup

To remove resources, delete the namespace:
- Run the following command:
  kubectl delete namespace grafana

---

## Security Considerations
- All sensitive information (admin credentials and passwords) is stored in Kubernetes Secrets.
- RBAC policies should restrict access to Secrets.
- Ensure Secrets are encrypted at rest and rotated periodically.
