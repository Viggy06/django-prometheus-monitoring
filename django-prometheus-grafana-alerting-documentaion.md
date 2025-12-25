
# 🚀 Django Monitoring & Alerting with Prometheus, Grafana & Alertmanager

## 📌 Project Overview

This project demonstrates a **real-time monitoring and alerting system** for a Django CRUD application using **Prometheus, Grafana, and Alertmanager**

Key highlights:
- Application-level monitoring using `django-prometheus`
- System-level monitoring using Node Exporter
- Centralized metrics collection with Prometheus
- Visualization with Grafana
- Alerting with Alertmanager
- Slack notifications for critical system events

---

## 🏗️ Architecture

Django Application (EC2)
- Django + Gunicorn
- django-prometheus
- Node Exporter

Monitoring Server (EC2)
- Prometheus
- Alertmanager
- Grafana

Alerts are sent to **Slack** using Incoming Webhooks.

---

## 🧩 Tech Stack

| Django | Application |
| django-prometheus | App metrics |
| Node Exporter | System metrics |
| Prometheus | Metrics & alerts |
| Grafana | Dashboards |
| Alertmanager |
| Slack | Notifications |


---

## 📊 Metrics Collected

### System Metrics
- CPU usage
- Memory usage
- Disk usage

### Django Metrics
- HTTP request count
- 2xx / 4xx / 5xx responses
- Request latency

---

## 🚨 Alerts

### High CPU Usage Alert
- Triggered when CPU usage > 80%
- Duration: 1 minute
- Severity: warning
- Notification: Slack

---

## ⚙️ Configuration Files

### prometheus.yml

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

alerting:
  alertmanagers:
    - static_configs:
        - targets:
          - "localhost:9093"

rule_files:
  - "alert_rules.yml"

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "django-instance"
    static_configs:
      - targets: ["<DJANGO_EC2_IP>:8000"]

  - job_name: "node-exporter"
    static_configs:
      - targets:
          - "<DJANGO_EC2_IP>:9100"
```

---

### alert_rules.yml

```yaml
groups:
- name: system-alerts
  rules:
  - alert: HighCPUUsage
    expr: 100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100) > 80
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "High CPU usage detected"
      description: "CPU usage is above 80% on {{ $labels.instance }}"
```

---

### alertmanager.yml

```yaml
global:
  resolve_timeout: 1m

route:
  receiver: "slack-notifications"

receivers:
- name: "slack-notifications"
  slack_configs:
  - api_url: "<SLACK_WEBHOOK_URL>"
    channel: "#prometheus-grafana-01"
    title: "Prometheus Alert"
    text: "CPU HIGH LOAD"
```


## 🧪 Alert Testing

```bash
stress-ng --cpu 4 --cpu-load 85 --timeout 120s
```

---
