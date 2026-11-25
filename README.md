# Automated Monitoring Stack Lab

A complete production-grade monitoring lab running **Prometheus**, **Node Exporter**, and **Grafana** orchestrated with **Docker Compose**.

## Architecture
- **Node Exporter** (:9100) -> Exposes host metrics (CPU, Memory, Disk, Network).
- **Prometheus** (:9090) -> Scrapes and stores time-series metrics.
- **Grafana** (:3000) -> Visualizes metrics through pre-provisioned dashboards.
