# 10b — Data Replication

## Key Concepts

| Term | Definition |
|------|------------|
| Replication | Continuously copying data from one DB server to one or more others |
| Primary (Master) | The server that accepts all writes |
| Replica (Slave) | Copy of the primary; serves read requests |
| Replication Lag | Delay between a write on primary and it appearing on replica |
| Failover | Promoting a replica to primary when the primary goes down |
| Async Replication | Write confirmed before replica saves — faster, small data loss risk |
| Sync Replication | Write confirmed only after replica also saves — slower, no data loss |

---

## How It Works

**Primary-Replica (most common):**
1. Client sends write → goes to Primary only
2. Primary saves the data
3. Primary streams changes to Replica(s)
4. Reads can be served by any Replica

**Replication lag problem:**
1. User updates profile → write hits Primary ✅
2. User immediately reads profile → hits Replica ❌ (replica not caught up yet)
3. User sees stale data → thinks update failed

---

## Types of Replication

| Type | Description | Used When |
|------|-------------|-----------|
| Primary-Replica | One writer, many readers | Most web apps |
| Primary-Primary | Both nodes accept writes | Multi-region writes needed |
| Sync | Replica confirms before write ack | Banking, payments |
| Async | Write ack before replica catches up | Social media, most web apps |

---

## Replication Lag Solutions

- **Read-your-own-writes** — route user's reads to primary right after their write
- **Sticky sessions** — always route a specific user to the same replica
- **Wait for replica** — only read from replica after it confirms the latest write

---

## Industry Examples

- **GitHub** — primary-replica; writes to primary, reads spread globally across replicas
- **Netflix** — replicates across AWS regions; if us-east-1 fails, eu-west-1 takes over
- **Twitter** — replication lag caused users to not see their own tweet for ~1 second in early days
- **Amazon RDS** — managed read replicas with automatic failover to promoted replica

---

## Interview Questions

**Q: What is replication and why do we use it?**
A: Copying data to multiple servers for high availability (failover) and read scalability (spread read load).

**Q: What is replication lag?**
A: The delay between a write on the primary and it appearing on replicas — can cause users to read stale data right after writing.

**Q: Sync vs async replication — when do you use each?**
A: Sync for critical data (banking) — safer but slower. Async for most web apps — faster but brief data loss risk on primary crash.

**Q: What is the difference between replication and sharding?**
A: Replication = same data on multiple servers (read scale + availability). Sharding = different data on each server (write scale + storage scale).

**Q: How do you solve the read-your-own-writes problem?**
A: Route the user's reads to the primary temporarily after a write, or use sticky sessions to always hit the same replica.
