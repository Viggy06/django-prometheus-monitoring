
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

## 🧪 Alert Testing

```bash
stress-ng --cpu 4 --cpu-load 85 --timeout 120s
```

---