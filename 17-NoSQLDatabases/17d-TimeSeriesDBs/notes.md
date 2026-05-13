# 17d — Time Series Databases (InfluxDB, TimescaleDB)

## Key Concepts

| Term | Definition |
|---|---|
| Time Series DB | Optimized for storing and querying data points indexed by time |
| Time series data | Sequence of values recorded at successive points in time |
| Measurement | The "table" equivalent in InfluxDB (e.g., `cpu_usage`) |
| Tag | Indexed metadata field used for filtering (e.g., `host=server1`) |
| Field | Actual measured value (e.g., `value=87.3`) |
| Retention policy | Rule that auto-deletes data older than X days |
| Downsampling | Aggregating high-resolution data into lower-resolution summaries over time |
| TimescaleDB | Time series extension built on top of PostgreSQL |

---

## How It Works

Every data point has:
```
timestamp | tags (metadata) | fields (values)
2026-05-13T10:00:00Z | host=server1, region=us-east | cpu=87.3, memory=62.1
```

- Data always arrives in time order
- Queries are almost always time-range based: "give me CPU usage for the last 1 hour"
- Old data is often less valuable — retention policies auto-purge it
- Downsampling: store raw data for 7 days, hourly averages for 30 days, daily averages forever

### InfluxDB Write Path:
```
App → writes data point → InfluxDB stores in time-sorted structure
      (with tags indexed for fast filtering)
```

### TimescaleDB:
- Built as a PostgreSQL extension
- Data stored in "hypertables" — automatically partitioned by time chunks
- Full SQL support — familiar syntax, time-series performance

---

## InfluxDB vs TimescaleDB

| Feature | InfluxDB | TimescaleDB |
|---|---|---|
| Base | Purpose-built time series | PostgreSQL extension |
| Query language | Flux / InfluxQL | Standard SQL |
| Schema | Schema-less (tags + fields) | Full relational schema |
| Joins | Limited | Full SQL joins |
| Ecosystem | Native time-series tools | Full PostgreSQL ecosystem |
| Best for | Metrics, IoT telemetry | Time series + relational data combined |

---

## When to Use a Time Series DB

| Good fit | Bad fit |
|---|---|
| Server/app metrics (CPU, memory, latency) | General-purpose data storage |
| IoT sensor readings | Highly relational data |
| Financial tick data (stock prices) | Documents or key-value patterns |
| Application performance monitoring (APM) | |
| Real-time dashboards (Grafana) | |

---

## Industry Examples

- **Tesla** — InfluxDB for real-time vehicle telemetry (speed, battery, temperature from millions of cars)
- **eBay** — InfluxDB for infrastructure monitoring across thousands of servers
- **Grafana Labs** — InfluxDB as default backend for metrics dashboards
- **Timescale** — TimescaleDB used by financial firms for stock tick data with SQL analysis
- **Cisco** — InfluxDB for network device health monitoring

---

## Interview Questions

**Q: What is a time series database and why not just use SQL?**
A: A time series DB is optimized for data indexed by timestamp — writes are always append-only and queries are always time-range based. SQL can do this, but time series DBs compress time-ordered data far more efficiently and have built-in retention and downsampling that would be complex to build in SQL.

**Q: What is downsampling and why is it important?**
A: Compressing high-resolution historical data into lower-resolution summaries (e.g., raw per-second data → hourly averages). Saves storage and keeps old data queryable without keeping every raw point forever.

**Q: What is the difference between a tag and a field in InfluxDB?**
A: Tags are indexed metadata used for filtering (e.g., which server). Fields are the actual measured values (e.g., CPU %). You query by tags, you aggregate fields.

**Q: When would you choose TimescaleDB over InfluxDB?**
A: When you need time series AND relational data together — e.g., joining sensor readings with a relational user/device table. TimescaleDB gives you full SQL on top of time series performance.

**Q: What is a retention policy?**
A: A rule that automatically deletes data older than a defined period. Keeps storage costs under control — raw metrics older than 30 days are usually not needed at full resolution.
