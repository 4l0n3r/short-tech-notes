# Profiles

A profile is a snapshot of your application's resource usage.

## Key Metrics for Java Application Profiling

- **JVM Metrics:** Heap memory usage, garbage collection times, thread count, CPU usage.
- **Application Performance:** Response time, throughput, error rates.
- **Resource Utilization:** CPU, memory, disk usage of the underlying infrastructure.

## Features

- **Continuous Monitoring**
- **Granular Insights**
- **Correlation with Other Data:** Time, environment, filters.
- **Language Support**
- **Filter Options:** Environment, service, version, host.

> **Note:** Ensure your services are tagged properly with `service` and `env`.

<br><br>

# Access

## Authentication

## Authorization - RBAC

- **Custom Roles**
- **Default Roles** [ Official Doc ](https://docs.datadoghq.com/account_management/rbac/?tab=datadogapplication#datadog-default-roles)
  - Read Only Role
  - Standard Role
  - Admin Role

<br><br>

# Datadog Agent Setup Guide

## 1. Create Datadog API and APP Keys

### Log in to Datadog

- Access your Datadog account through the web interface.

### Navigate to Organization Settings

- Go to your profile at the bottom of the dashboard.
- Select **Organization Settings** > **API Keys**.

### Create API Key

1. Click on **New API Key**.
2. Enter a name for the key (e.g., `datadog-nonprod-api-key`).
3. Store the key as a GitHub org secret with the name:  
   **`DATADOG_API_KEY`**

### Create APP Key

1. Click on **New APP Key**.
2. Enter a name for the key (e.g., `datadog-nonprod-app-key`).
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

<br><br>

# How we configured

## Logs

The datadog-agent can be configured to collect container logs directly. The agent will collect logs from `/var/logs`. We can't replicate same with fargate. Because,
  - we can't install datadog-agent as daemonset since fargate won't support it. So you should install the agent as a sidecar container
  - agent can't read the logs from `/var/logs` since In fargate nodes we can't create container with previlege access. So we should have a different approach to read logs. ( fluentbit -> cloudwatch -> lambda_forwarder -> datadog ).

## Traces

## Metrics

<br><br>

# RUM

**Real User Monitoring (RUM)** is a tool used to gather, analyze, and interpret performance data based on the actual interactions of users with a web application.

## Datadog RUM Features

- **User-centric Performance Metrics:** Page load time, response times.
- **Global Performance Monitoring**
- **Real-time Monitoring**
- **Browser-specific Metrics**
- **Error and Frustration Detection**

---

## Doubts

1. Haven't we enabled tracing for AppMesh?
2. Where are we creating alerts and dashboards?  
   _Got the answer!_
3. What are all RUM tools we installed? And how?
4. How is Datadog getting AWS data? How is it authenticated and authorized?
5. What is a webhook exactly?
