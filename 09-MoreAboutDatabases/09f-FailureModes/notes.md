# 09f — Database Failure Modes

## Key Concepts

| Term | Definition |
|---|---|
| Failure Mode | A way a database can fail or degrade in production |
| WAL (Write-Ahead Log) | Changes logged to disk before applied — enables crash recovery |
| Replication Lag | Replica falls behind primary — stale reads possible |
| Deadlock | Two transactions each wait for a lock the other holds — neither can proceed |
| Connection Pool | Fixed set of reusable DB connections — can be exhausted under high load |
| Cascading Failure | One slow query triggers a chain reaction that degrades the whole system |

---

## Failure Modes Summary

| Failure Mode | Cause | Fix |
|---|---|---|
| Hardware failure | Disk/server crash | WAL, replication, backups |
| Replication lag | Replica behind primary | Route critical reads to primary |
| Deadlock | Circular lock dependency | Consistent lock ordering; DB auto-detects and kills one transaction |
| Connection pool exhaustion | Too many concurrent requests | Pool tuning, PgBouncer, query optimization |
| Data corruption | Crash mid-write, disk error | ACID transactions, backups, point-in-time recovery |
| Cascading failure | Slow query blocks others → pool fills → timeouts | Query timeouts, circuit breakers, read replicas |

---

## Deadlock Flow

```
Transaction A: locks Row 1 → waits for Row 2
Transaction B: locks Row 2 → waits for Row 1
→ Neither can proceed → DB detects → kills one → it rolls back
```

---

## Cascading Failure Flow

```
Slow query holds locks
→ Other queries pile up waiting
→ Connection pool exhausts
→ New requests time out
→ Entire service degrades
```

---

## Industry Examples

- **GitHub (2012)** — replication lag caused stale reads after writes; fixed by routing post-write reads to primary
- **Amazon** — circuit breakers prevent one slow DB from cascading into full service outage
- **Stack Overflow** — connection pool exhaustion during traffic spikes; solved with PgBouncer

---

## Interview Questions

| Question | One-Line Answer |
|---|---|
| What is a deadlock? | Two transactions waiting on each other's locks; DB detects and kills one automatically |
| What is replication lag? | Replica falls behind primary — reads immediately after a write may return stale data |
| What is connection pool exhaustion? | All DB connections in use; new requests fail or timeout under high load |
| What is WAL? | Write-Ahead Log — changes written to log before applied; enables crash recovery |
| What is a cascading failure? | One slow query causes lock pile-up → connection exhaustion → full service degradation |
