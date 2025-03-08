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

