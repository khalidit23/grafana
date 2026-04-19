# 📡 MikroTik Monitoring Stack (Grafana + Prometheus + VictoriaMetrics)

![Docker](https://img.shields.io/badge/docker-ready-blue?logo=docker)
![Grafana](https://img.shields.io/badge/Grafana-dashboard-orange?logo=grafana)
![Prometheus](https://img.shields.io/badge/Prometheus-monitoring-red?logo=prometheus)
![VictoriaMetrics](https://img.shields.io/badge/VictoriaMetrics-storage-green)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A complete **Docker-based monitoring stack** for MikroTik devices using:

* 📊 Grafana (Visualization)
* 🔍 Prometheus (Metrics scraping)
* 📡 SNMP Exporter (SNMP → Prometheus)
* 🗄️ VictoriaMetrics (Long-term storage)

---

# 🧠 Architecture Diagram

```
                ┌──────────────┐
                │   MikroTik   │
                │   Router     │
                └──────┬───────┘
                       │ SNMP
                       ▼
              ┌─────────────────┐
              │  SNMP Exporter  │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │   Prometheus    │
              │ (Collector)     │
              └────────┬────────┘
                       │ remote_write
                       ▼
          ┌──────────────────────────┐
          │    VictoriaMetrics       │
          │  (Long-term Storage)     │
          └────────┬─────────────────┘
                   │
                   ▼
              ┌──────────────┐
              │   Grafana    │
              │ Dashboard UI │
              └──────────────┘
```

---

# 📁 Project Structure

```
mikrotik-grafana/
│
├── docker-compose.yml
│
├── prometheus/
│   ├── prometheus.yml
│   └── data/
│
├── snmp_exporter/
│   └── snmp.yml
│
├── victoriametrics-data/
│
└── grafana/
    └── provisioning/
        └── datasources/
            └── victoriametrics.yml
```

---

# 🚀 Quick Start

## 1️⃣ Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/mikrotik-grafana.git
cd mikrotik-grafana
```

---

## 2️⃣ Start Stack

```bash
docker compose up -d
```

---

## 3️⃣ Access Services

| Service         | URL                        |
| --------------- | -------------------------- |
| Grafana         | http://localhost:3000      |
| Prometheus      | http://localhost:9090      |
| VictoriaMetrics | http://localhost:8428      |
| VM UI           | http://localhost:8428/vmui |
| SNMP Exporter   | http://localhost:9116      |

---

# 🔐 Default Login (Grafana)

```
Username: admin
Password: admin
```

---

# 📊 Example Metrics

## CPU Load

```promql
avg(hrProcessorLoad{instance="YOUR_ROUTER_IP"})
```

## RAM Usage %

```promql
(
  hrStorageUsed{hrStorageDescr="main memory"}
/
  hrStorageSize{hrStorageDescr="main memory"}
) * 100
```

## WAN Traffic

```promql
rate(ifHCInOctets{ifName="ether1-WAN"}[5m]) * 8
```

# 🧱 Features

* 📡 MikroTik SNMP Monitoring
* 📊 Real-time + Historical metrics
* 🗄️ 1 Year Data Retention (VictoriaMetrics)
* 📈 Grafana Dashboards
* ⚡ Lightweight Docker Setup
* 🔧 Easy to extend

---

# ⚙️ Configuration

## Prometheus → VictoriaMetrics

```yaml
remote_write:
  - url: http://victoriametrics:8428/api/v1/write
```

---

## VictoriaMetrics Retention

```yaml
--retentionPeriod=12
```

👉 Keeps data for **12 months**

---

# 🎯 Dashboard Layout (Recommended)

## Row 1 (Stat Cards)

* Uptime
* CPU Load
* RAM Usage

## Row 2 (Traffic)

* WAN Traffic
* LAN Traffic

## Row 3 (Advanced)

* WLAN
* VPN / API Traffic

---

# 🔍 How It Works

| Component       | Role              |
| --------------- | ----------------- |
| SNMP Exporter   | Collect SNMP data |
| Prometheus      | Scrape & forward  |
| VictoriaMetrics | Store metrics     |
| Grafana         | Visualize data    |

---

# 🚀 Future Improvements

* 🔔 Alerting (CPU / Link Down)
* 📜 Loki for Logs
* 🌐 Multi-device Monitoring
* 📊 NOC Dashboard Layout
* ☁️ Cloud Storage Backup

---

# 🛠 Troubleshooting

## Prometheus not starting

```bash
docker logs prometheus
```

## Grafana datasource error

* Ensure only **one `isDefault: true`**

---

# 📚 Resources

* https://grafana.com
* https://prometheus.io
* https://docs.victoriametrics.com

---

# 🧾 License

MIT License

---

# ❤️ Author

**Your Name / GitHub**

---

⭐ If this project helped you, please star the repository!
