# Datadog Agent Setup Guide

## 1. Create Datadog API and APP Keys

### Log in to Datadog
- Access your Datadog account through the web interface.

### Navigate to Organization Settings
- Go to your profile at the bottom of the dashboard.
- Select **Organization Settings** > **API Keys**.

### Create API Key
1. Click on **New API Key**.
2. Enter a name for the key (e.g., `ecommerce-datadog-nonprod-api-key`).
3. Store the key as a GitHub org secret with the name:  
   **`DATADOG_API_KEY`**

### Create APP Key
1. Click on **New APP Key**.
2. Enter a name for the key (e.g., `ecommerce-datadog-nonprod-app-key`).
3. Store the key as a GitHub org secret with the name:  
   **`DATADOG_APP_KEY`**

---

## 2. Generate Datadog Cluster Agent Token

### Generate a Secure Token
- Use a secure method to generate a random string:
  ```bash
  openssl rand -hex 16
  ```
### Store the Token
- Save the generated token as a GitHub org secret with the name:  
  **`DATADOG_CLUSTER_AGENT_TOKEN`**

---

## 3. Store Keys and Token in Kubernetes Secrets

### Create Kubernetes Secrets
Run the following commands to create secrets for the API key, APP key, and Cluster Agent token in the `monitoring` namespace:

```bash
kubectl create secret generic datadog-api-secret -n monitoring \
  --from-literal=api-key=<YOUR_API_KEY>

kubectl create secret generic datadog-app-secret -n monitoring \
  --from-literal=app-key=<YOUR_APP_KEY>

kubectl create secret generic datadog-cluster-agent-secret -n monitoring \
  --from-literal=token=<CLUSTER_AGENT_TOKEN>
```

## 4. Deploy Datadog Agents (Daemonset and Cluster Agent) Using Helm

### Add Datadog Helm Repository
1. Add the Datadog Helm repository:
   ```bash
   helm repo add datadog https://helm.datadoghq.com
   helm repo update
   ```
### Install Datadog Node Agents
Run the following command to install both Datadog Node Agents (Daemonsets) and the Datadog Cluster Agent:
   ```bash
    helm install datadog-agent datadog/datadog --install -n monitoring \
    --set datadog.apiKeyExistingSecret=datadog-api-secret \
    --set datadog.appKeyExistingSecret=datadog-app-secret \
    --set datadog.clusterAgent.tokenExistingSecret=datadog-cluster-agent-secret
   ```