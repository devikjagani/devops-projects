# AWS Infrastructure Monitoring with Prometheus and Grafana

## 🎯 Project Overview

Implemented centralized infrastructure monitoring across AWS environments using Prometheus and Grafana.

Built dashboards, alerting systems, and email notifications for proactive infrastructure monitoring.

---

## ✅ Monitoring Coverage

### ⚙️ Infrastructure Metrics

* CPU Usage
* Memory Usage
* Disk Usage
* Network Traffic
* Load Average

### Service Monitoring

* Node Exporter
* pm2 metrics
* Application Availability
* Service Status
* Instance Health

---

## 🚨 Alerting System

Configured alerts for:

* Instance Down
* Service Down
* High CPU Usage
* High Memory Usage
* Low Disk Space

---

## 📧 Notification Channels

* Email Notifications
* Alert Escalation

---

## 📊 Dashboard Features

* Real-Time Monitoring
* Historical Analysis
* Resource Trends
* Infrastructure Health

---

## 🔧 Setup Guide

1. Install node_exporter on every EC2 instance

``` bash
useradd --no-create-home --shell /bin/false node_exporter

wget https://github.com/prometheus/node_exporter/releases/download/v1.8.0/node_exporter-1.8.0.linux-amd64.tar.gz
tar -xvf node_exporter-1.8.0.linux-amd64.tar.gz
cp node_exporter-1.8.0.linux-amd64/node_exporter /usr/local/bin/
chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

---

2. Deploy Prometheus, Alertmanager & Grafana (Docker Compose)

```yaml
# docker-compose.yml
version: "3.8"
services:
  prometheus:
    image: prom/prometheus:latest
    ports: [ "9090:9090" ]
    volumes:
      - ./prometheus:/etc/prometheus
    command:
      - "--config.file=/etc/prometheus/prometheus.yml"

  alertmanager:
    image: prom/alertmanager:latest
    ports: [ "9093:9093" ]
    volumes:
      - ./alertmanager:/etc/alertmanager
    command:
      - "--config.file=/etc/alertmanager/alertmanager.yml"

  grafana:
    image: grafana/grafana:latest
    ports: [ "3000:3000" ]
    volumes:
      - grafana-data:/var/lib/grafana

volumes:
  grafana-data:
```

```bash
docker compose up -d
```

---

## ⚙️ Prometheus Configuration

prometheus/prometheus.yml

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

alerting:
  alertmanagers:
    - static_configs:
        - targets: [ "alertmanager:9093" ]

rule_files:
  - "alert.rules.yml"

scrape_configs:
  - job_name: "node_exporter"
    static_configs:
      - targets:
          - "10.0.1.10:9100"   # EC2 Instance 1
          - "10.0.1.11:9100"   # EC2 Instance 2
          - "10.0.1.12:9100"   # EC2 Instance N
        labels:
          environment: "production"
```

---

## 🚨 Alerting Rules

prometheus/alert.rules.yml

```yaml
groups:
  - name: infrastructure-alerts
    rules:
      - alert: NodeDown
        expr: up{job="node_exporter"} == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Node {{ $labels.instance }} is down"
          description: "{{ $labels.instance }} has been unreachable for more than 1 minute."

      - alert: HighCPUUsage
        expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 85
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage on {{ $labels.instance }}"
          description: "CPU usage above 85% for more than 5 minutes."

      - alert: HighMemoryUsage
        expr: (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100 > 90
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High memory usage on {{ $labels.instance }}"
          description: "Memory usage above 90% for more than 5 minutes."
```

---

## ✅ Benefits

* Reduced downtime
* Faster incident response
* Improved visibility
* Proactive monitoring
