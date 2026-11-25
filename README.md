# Automated Monitoring Stack Lab

A production-grade monitoring lab running **Prometheus**, **Node Exporter**, and **Grafana** orchestrated with **Docker Compose**.

## Architecture Flow Diagram
```text
[ Your Laptop / Hardware ]
          │
          ▼ (Exposes host metrics on port 9100)
   [ Node Exporter ]
          ▲
          │ (Scrapes metrics every 15s - PULL model)
   [  Prometheus   ]
          ▲
          │ (Queries stored metrics via PromQL)
   [    Grafana    ] ──▶ [ Web Browser Dashboard (:3000) ]
```

## How to Start
```bash
docker compose up -d
```
- Node Exporter: http://localhost:9100/metrics
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000

## 5 Key Metrics Picked
1. `node_cpu_seconds_total`: Tracks time CPU spent in idle/user/system modes.
2. `node_memory_MemTotal_bytes`: Total physical RAM.
3. `node_memory_MemAvailable_bytes`: Free usable RAM.
4. `node_filesystem_free_bytes`: Free hard drive space.
5. `node_network_receive_bytes_total`: Inbound network traffic in bytes.

## PromQL Queries
- CPU Used %: `100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)`
- Memory Used %: `(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100`
- Disk Used %: `(1 - (node_filesystem_avail_bytes{mountpoint="/"}/node_filesystem_size_bytes{mountpoint="/"})) * 100`
- Network Received (Bytes/s): `rate(node_network_receive_bytes_total{device!~"lo|veth.*|docker.*"}[5m])`
- System Uptime: `time() - node_boot_time_seconds`

## 5-Minute Explanation for Team Lead
A metric travels from the machine to a Grafana panel in 4 steps:
1. Hardware generates performance values in Linux kernel (`/proc`).
2. Node Exporter reads them and exposes plain text numbers at `:9100/metrics`.
3. Prometheus pulls (scrapes) those numbers every 15s and stores them with timestamps.
4. Grafana queries Prometheus via PromQL and draws real-time visual charts and gauges.
