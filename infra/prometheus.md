## Prometheus

### Why Prometheus?

- Managing hundreds of servers is challenging.
- Errors in a chain of components can appear in the UI, but finding the exact error requires debugging backward.
- Prometheus allows monitoring every service and logging their status during failures.
- Helps identify problems before they occur (e.g., 70% usage alerts).

### Target

**What does it monitor?**

- Nginx Server / Python Server / Database

### Metrics

**What units are monitored?**

- CPU status
- Memory usage
- Request count

---

### Architecture

1. **Prometheus Server**:
   - **Retrieval**: Pulls metrics from services.
   - **Storage**: Stores metrics data.
   - **HTTP Server**: Accepts PromQL queries and returns responses.
2. **Prometheus Web UI / Grafana**:
   - Sends PromQL queries to the HTTP Server and visualizes responses on the Grafana Dashboard.
3. **AlertManager**:
   - **Workflow**:
     - Master → Push → Alert Manager → Trigger → Email/Slack

---

### Metrics Types

- **Counter**: Tracks increasing counts (e.g., number of requests).
- **Gauge**: Tracks values that can increase or decrease (e.g., CPU usage).
- **Histogram**: Tracks size and duration (e.g., response time).

---

### Data Retrieval

- Prometheus retrieves data from the `/metrics` endpoint.
- Accepts only specific formats: Counter, Gauge, Histogram.

### Exporters

Some services directly expose the `/metrics` endpoint. For others:

- **Exporters** act as a mediator between the target and Prometheus.
- **Tasks**:
  1. Fetch metrics from the target.
  2. Update the format.
  3. Expose data at `/metrics`.

Exporters are service-specific and often run as sidecar containers. For example:

- Use `python_exporter` alongside a Python application inside a pod.

---

### Monitoring Custom Applications

- Use **client libraries** to expose a `/metrics` endpoint.

---

### Push System

- Examples: CloudWatch, New Relic.
- Applications/servers push metrics to a centralized platform.
- **Challenges**: High traffic can become a bottleneck.

### PushGateway

- **Short-lived jobs**: Push metrics upon exit.

---

### Configuration File

- **How often**: Frequency of Prometheus scraping data.
- **Rules**: For raising alerts and aggregating metrics.
- **What**: Specifies resources to monitor.

---

### Data Storage

- Prometheus stores data both **locally** and **remotely**.

---

### Disadvantages

- **Scaling challenges**:
  - Increase Prometheus server capacity.
  - Decrease the number of metrics collected.
