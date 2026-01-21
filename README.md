# BitAxe Monitor

Prometheus and Grafana monitoring stack for BitAxe Bitcoin miners. Collects metrics from the BitAxe API and provides a pre-configured dashboard for real-time monitoring and historical analysis.

![Dashboard Preview](docs/dashboard.png)

## Features

- **55+ metrics** including hashrate, temperature, power, shares, and network stats
- **3-year data retention** by default
- **Pre-configured Grafana dashboard** with organized panels
- **Auto-discovery** of Prometheus datasource

## Quick Start

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/bitaxe-monitor.git
   cd bitaxe-monitor
   ```

2. Edit `prometheus.yml` and set your BitAxe IP address:
   ```yaml
   - targets:
       - http://YOUR_BITAXE_IP/api/system/info
   ```

3. Start the stack:
   ```bash
   docker compose up -d
   ```

4. Open Grafana at `http://localhost:3010` (login: `admin`/`admin`)

## Services

| Service | Port | Description |
|---------|------|-------------|
| Grafana | 3010 | Dashboards and visualization |
| Prometheus | 9090 | Metrics storage and querying |
| JSON Exporter | 7979 | Converts BitAxe JSON API to Prometheus metrics |

## Collected Metrics

| Category | Metrics |
|----------|---------|
| Hashrate | Current, 1m/10m/1h averages, expected, error % |
| Temperature | ASIC, voltage regulator, target |
| Power | Watts, voltage, current, efficiency |
| Mining | Shares accepted/rejected, best difficulty, block height |
| Cooling | Fan RPM, fan %, auto-fan status |
| Network | WiFi RSSI, pool latency, pool status |
| System | Uptime, free heap, frequency |

## Configuration

### Data Retention

Edit `docker-compose.yml` to change retention period:
```yaml
- '--storage.tsdb.retention.time=3y'
```

### Multiple BitAxe Devices

Add additional targets in `prometheus.yml`:
```yaml
- targets:
    - http://10.1.0.85/api/system/info
    - http://10.1.0.86/api/system/info
    - http://10.1.0.87/api/system/info
```

### Grafana Password

Change the default password in `docker-compose.yml`:
```yaml
- GF_SECURITY_ADMIN_PASSWORD=your_secure_password
```

## Data Storage

Persistent data is stored in:
- `./data/prometheus/` - Time series data
- `./data/grafana/` - Grafana configuration and dashboards

## License

MIT
