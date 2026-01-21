# BitAxe Monitor

Prometheus and Grafana monitoring stack for BitAxe Bitcoin miners. Collects metrics from the BitAxe API and provides a pre-configured dashboard for real-time monitoring and historical analysis.

![Dashboard Preview](docs/dashboard.png)

## Features

- **55+ metrics** including hashrate, temperature, power, shares, and network stats
- **3-year data retention** by default
- **Pre-configured Grafana dashboard** with organized panels
- **Auto-discovery** of Prometheus datasource
- **Hot-reload targets** without restarting services

## Quick Start

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/bitaxe-monitor.git
   cd bitaxe-monitor
   ```

2. Configure your environment:

   ```bash
   cp .env.example .env
   cp targets.yml.example targets.yml
   ```

3. Edit `targets.yml` and set your BitAxe IP address:

   ```yaml
   - targets:
       - http://YOUR_BITAXE_IP/api/system/info
     labels:
       name: bitaxe-1
   ```

4. Start the stack:

   ```bash
   docker compose up -d
   ```

5. Open Grafana at `http://localhost:3000` (login: `admin`/`admin`)

## Services

| Service       | Port | Description                                    |
| ------------- | ---- | ---------------------------------------------- |
| Grafana       | 3000 | Dashboards and visualization                   |
| Prometheus    | 9090 | Metrics storage and querying                   |
| JSON Exporter | 7979 | Converts BitAxe JSON API to Prometheus metrics |

## Configuration

All configuration is done through two files:

### `.env` - Ports and Settings

```bash
# Ports
PROMETHEUS_PORT=9090
JSON_EXPORTER_PORT=7979
GRAFANA_PORT=3000

# Grafana
GRAFANA_PASSWORD=admin

# Data retention (e.g., 15d, 6m, 3y)
PROMETHEUS_RETENTION=3y
```

### `targets.yml` - BitAxe Miners

Add your miners here. Prometheus auto-reloads this file every 30 seconds.

```yaml
# Single miner
- targets:
    - http://192.168.1.100/api/system/info
  labels:
    name: bitaxe-1

# Multiple miners with labels
- targets:
    - http://192.168.1.101/api/system/info
    - http://192.168.1.102/api/system/info
  labels:
    location: office
```

## Collected Metrics

| Category    | Metrics                                                 |
| ----------- | ------------------------------------------------------- |
| Hashrate    | Current, 1m/10m/1h averages, expected, error %          |
| Temperature | ASIC, voltage regulator, target                         |
| Power       | Watts, voltage, current, efficiency                     |
| Mining      | Shares accepted/rejected, best difficulty, block height |
| Cooling     | Fan RPM, fan %, auto-fan status                         |
| Network     | WiFi RSSI, pool latency, pool status                    |
| System      | Uptime, free heap, frequency                            |

## Data Storage

Persistent data is stored in:

- `./data/prometheus/` - Time series data
- `./data/grafana/` - Grafana configuration and dashboards

## License

MIT
