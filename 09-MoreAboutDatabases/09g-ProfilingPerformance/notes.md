# 09g — Profiling Database Performance

## Key Concepts

| Term | Definition |
|---|---|
| Profiling | Identifying slow or inefficient queries to fix before they cause production issues |
| EXPLAIN / EXPLAIN ANALYZE | SQL command that shows how the DB executes a query and where time is spent |
| Seq Scan | Sequential (full table) scan — reads every row — bad on large tables |
| Index Scan | Uses an index to find rows directly — fast |
| Slow Query Log | DB logs queries exceeding a time threshold — used to find worst offenders |
| APM | Application Performance Monitoring — continuously tracks query performance in production |

---

## The Fix Workflow

1. Identify slow query via slow query log or APM alert
2. Run `EXPLAIN ANALYZE` on it
3. Look for Seq Scans, missing indexes, expensive joins
4. Add index / rewrite query / add eager loading
5. Run `EXPLAIN ANALYZE` again to confirm improvement
6. Deploy and monitor

---

## What to Look For in EXPLAIN Output

| Symptom | Problem |
|---|---|
| `Seq Scan` on large table | Missing index |
| High row count scanned | Query not selective enough |
| Many repeated similar queries | N+1 problem |
| High cost on join step | Inefficient join / missing index on join column |

---

## Profiling Tools

| Tool | Used For |
|---|---|
| `EXPLAIN ANALYZE` | Query execution plan — built into PostgreSQL/MySQL |
| Slow Query Log | DB-level logging of queries over threshold |
| **MiniProfiler** | .NET — query count and duration per request |
| **EF Core Query Logging** | Logs all generated SQL to console/file |
| **Application Insights** | Microsoft/Azure APM — tracks slow queries in production |
| **New Relic / Datadog** | Production APM — alerts on slow queries across all services |
| **pgAdmin** | Visual query analysis for PostgreSQL |

---

## APM vs Manual Profiling

| | Manual (EXPLAIN) | APM Tools |
|---|---|---|
| When | On-demand investigation | Continuous, always-on |
| How | You run it manually | Automatically monitors all queries |
| Best for | Diagnosing a specific slow query | Catching regressions in production |

---

## Industry Examples

- **Shopify** — Datadog APM catches N+1 and slow queries before they reach customers
- **GitHub** — runs EXPLAIN on every slow query alert; indexes are first line of fix
- **Stack Overflow** — MiniProfiler built by their team for per-request query tracking in .NET

---

## Interview Questions

| Question | One-Line Answer |
|---|---|
| How do you find slow queries in production? | Slow query log + APM tools like Datadog or Application Insights |
| What does EXPLAIN ANALYZE tell you? | Execution plan — indexes used, rows scanned, time per step |
| What is a Seq Scan and why is it bad? | Full table scan — reads every row — very slow on large tables, usually means missing index |
| What tools have you used for DB profiling? | EF Core query logging and MiniProfiler in .NET |
| What is APM? | Application Performance Monitoring — continuously tracks query performance and latency in production |
