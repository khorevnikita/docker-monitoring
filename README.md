# Monitoring and Logging Stack

This repository contains a Docker Compose configuration for setting up a monitoring and logging stack. The stack includes the following components:

- **Prometheus**: Metrics collection and storage.
- **Alertmanager**: Alerts based on Prometheus metrics.
- **Grafana**: Visualization and dashboards.
- **Pushgateway**: Pushes metrics from ephemeral jobs to Prometheus.
- **Node Exporter**: Exports host-level metrics.
- **cAdvisor**: Container resource usage and performance metrics.
- **Loki**: Centralized logging solution.
- **Promtail**: Log collection and forwarding to Loki.
- **Jaeger**: Distributed tracing.
- **OpenTelemetry Collector**: Collects telemetry data from applications.

---

## **Requirements**
- Docker
- Docker Compose

---

## **Usage**

### 1. Clone the Repository
```bash
git clone https://github.com/khorevnikita/docker-monitoring.git
cd docker-monitoring
```

### 2. Environment Variables
Create a `.env` file in the root directory to configure the following variables:

```env
# Grafana Admin Credentials
GRAFANA_ADMIN_USER=admin
GRAFANA_ADMIN_PASSWORD=admin

# Telegram Integration for Alertmanager
TELEGRAM_BOT_TOKEN=your_telegram_bot_token
TELEGRAM_CHAT_ID=your_telegram_chat_id
```

### 3. Start the Stack
```bash
docker-compose up -d
```

### 4. Access the Services
- **Prometheus**: [http://localhost:10003](http://localhost:10003)
- **Alertmanager**: Integrated with Prometheus; no separate UI.
- **Grafana**: [http://localhost:10001](http://localhost:10001)
    - Default credentials: `admin` / `admin` (or set in `.env` file).
- **Jaeger**: [http://localhost:10002](http://localhost:10002)

---

## **Services Overview**

### 1. **Prometheus**
- **Port**: `10003`
- Collects metrics from exporters and other services.
- Configuration: `./prometheus/prometheus.yml`

### 2. **Alertmanager**
- Handles alerts from Prometheus.
- Configurable for Telegram alerts.
- Configuration: `./alertmanager/config.yml`

### 3. **Grafana**
- **Port**: `10001`
- Dashboards for visualizing metrics and logs.
- Configured to use Prometheus, Loki, and Jaeger as data sources.
- Pre-provisioning available in `./grafana/provisioning`.

### 4. **Pushgateway**
- Used for ephemeral jobs to push metrics.

### 5. **Node Exporter**
- Exports host-level metrics like CPU, memory, and disk usage.

### 6. **cAdvisor**
- Provides container resource usage and performance metrics.

### 7. **Loki**
- Centralized logging solution.
- Configuration: `./loki/config.yaml`

### 8. **Promtail**
- Collects and forwards logs to Loki.
- Configuration: `./promtail/config.yaml`

### 9. **Jaeger**
- **Port**: `10002`
- Distributed tracing system.
- Useful for visualizing and analyzing distributed system transactions.

### 10. **OpenTelemetry Collector**
- Centralized telemetry data collection.
- Configuration: `./otel-collector/config.yaml`

---

## **Volumes**
The stack uses named Docker volumes for persistent storage:
- `prometheus_data`
- `grafana_data`
- `loki_data`
- `jaeger_data`

---

## **Stopping the Stack**
To stop and remove all containers, run:
```bash
docker-compose down
```

---

## **Troubleshooting**

1. **Grafana not loading dashboards?**
    - Check the provisioning directory: `./grafana/provisioning`.

2. **Prometheus not scraping metrics?**
    - Verify the targets in `./prometheus/prometheus.yml`.

3. **Logs not appearing in Grafana (Loki)?**
    - Check Promtail configuration: `./promtail/config.yaml`.

4. **Tracing not working in Jaeger?**
    - Ensure your applications are sending traces to OpenTelemetry Collector on `4317`.

---

## **Future Improvements**
- Add more exporters (e.g., Kafka Exporter, Postgres Exporter).
- Enhance Alertmanager configurations.
- Automate dashboard setup for Grafana.