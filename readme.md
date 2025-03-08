# Open Source Observability Stack - Setup Instructions

This guide will help you set up a complete open-source observability stack using Docker Compose. This stack includes monitoring, logging, tracing, and error tracking capabilities.

## Components Overview

| Component | Purpose | Port |
|-----------|---------|------|
| Prometheus | Metrics collection and storage | 9090 |
| Grafana | Visualization and dashboards | 3000 |
| Loki | Log aggregation | 3100 |
| Promtail | Log collection agent | N/A |
| Tempo | Distributed tracing | 3200, 14250, 6831, 14268, 9411 |
| Alertmanager | Alert handling and notifications | 9093 |

## Directory Structure

Create the following directory structure:

```
observability/
├── docker-compose.yml
├── prometheus/
│   └── prometheus.yml
├── grafana/
│   └── provisioning/
├── loki/
│   └── local-config.yaml
├── promtail/
│   └── config.yml
├── tempo/
│   └── tempo-local.yaml
└── alertmanager/
    └── config.yml
```

## Setup Steps

1. **Create the directory structure**

```bash
mkdir -p observability/{prometheus,grafana/provisioning,loki,promtail,tempo,alertmanager}
```

2. **Copy the configuration files**
   
   Save each configuration file to its respective location as shown in the directory structure above.

3. **Start the stack**

```bash
cd observability
docker-compose up -d
```

4. **Access the services**

- Grafana: http://localhost:3000 (default login: admin/admin)
- Prometheus: http://localhost:9090
- Loki: http://localhost:3100
- Tempo: http://localhost:3200

## Integrating with Your Application

### Logs Integration

For containerized applications, Promtail will automatically collect logs from Docker containers. If needed, add the following volume to your application's service in your main docker-compose.yml:

```yaml
volumes:
  - /var/log:/var/log
```

### Metrics Integration

1. **Expose metrics from your application**:
   - For Java: Use Micrometer
   - For Go: Use Prometheus client libraries
   - For Python: Use prometheus_client
   - For Node.js: Use prom-client

2. **Update Prometheus configuration** to scrape your application:

In `prometheus/prometheus.yml`, add your application to the `scrape_configs` section:

```yaml
- job_name: "your-app-name"
  scrape_interval: 15s
  static_configs:
    - targets: ["your-app-container-name:metrics-port"]
```

### Tracing Integration

1. **Integrate OpenTelemetry in your application code**:
   - Use OpenTelemetry libraries for your programming language
   - Configure the OTLP exporter to send to Tempo at `tempo:4317` (gRPC) or `tempo:4318` (HTTP)

2. **Sample configuration for Java**:

```go

```

## Setting Up Grafana Dashboards

Once the stack is running, go to Grafana (http://localhost:3000) and:

1. **Add data sources**:
   - Add Prometheus: http://prometheus:9090
   - Add Loki: http://loki:3100
   - Add Tempo: http://tempo:3200

2. **Import dashboards**:
   - For infrastructure monitoring: Dashboard ID 1860
   - For Loki logs: Dashboard ID 12019
   - You can find more dashboard templates in the Grafana dashboard library

## Troubleshooting

- **Logs not appearing in Grafana**:
  - Check if Promtail is running: `docker-compose ps promtail`
  - Verify Promtail config has the correct paths
  - Ensure Loki is running and accessible to Promtail

- **Metrics not appearing in Prometheus**:
  - Check targets in Prometheus UI: http://localhost:9090/targets
  - Ensure your application is exposing metrics correctly
  - Verify network connectivity between Prometheus and your app

- **Traces not appearing in Tempo**:
  - Ensure your app is correctly configured to send traces to Tempo
  - Check if Tempo is receiving data at the correct endpoints
