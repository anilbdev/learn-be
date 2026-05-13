# 17e — Column Databases (Cassandra, HBase)

## Key Concepts

| Term | Definition |
|------|------------|
| Column DB | Stores and retrieves data by column rather than by row |
| Wide-Column Store | Each row has a unique row key; rows can have different columns |
| Row Key | Unique identifier for each row (like a primary key) |
| Column Family | Logical grouping of related columns within a row |
| CQL | Cassandra Query Language — SQL-like syntax for Cassandra |
| Tunable Consistency | Cassandra lets you choose how many nodes must confirm a read/write |
| LSM Tree | Log-Structured Merge Tree — Cassandra's append-only write structure enabling fast writes |

---

## How It Works

### Wide-Column Store Structure
1. Each row is identified by a **row key**
2. Each row can have **different columns** — no fixed schema required
3. Columns are grouped into **column families**
4. Empty/missing columns take up no space (sparse data is efficient)

```
Row Key: user_123
  → profile:   { name: "Anil", city: "Mumbai" }
  → activity:  { last_login: "2026-05-13", clicks: 500 }

Row Key: user_456
  → profile:   { name: "Sara" }
  → purchases: { item: "laptop" }    ← completely different columns
```

### Why Column Reads Are Fast
- Query only loads columns you request — skips everything else
- In a relational DB, a query for 2 columns still reads the full row
- At billions of rows, this difference is massive

### Cassandra Write Path
1. Write goes to an in-memory structure (Memtable)
2. Also appended to a commit log (for durability)
3. Periodically flushed to disk as SSTables
4. No in-place updates — always appends → extremely fast writes

---

## Cassandra vs HBase

| Feature | Cassandra | HBase |
|---|---|---|
| Architecture | Peer-to-peer (no master node) | Master-slave |
| Consistency | Tunable (eventual by default) | Strong consistency |
| Write speed | Extremely fast (LSM tree) | Fast |
| Ecosystem | Standalone | Hadoop / HDFS ecosystem |
| Best for | High-write, distributed, global apps | Analytics on Hadoop, sparse data |
| Query language | CQL (SQL-like) | Java API / REST |

---

## Industry Examples

| Company | DB | Use Case |
|---|---|---|
| Netflix | Cassandra | Viewing history for 200M+ users — millions of writes/sec during streaming |
| Apple | Cassandra | Stores 10s of billions of rows for iCloud data sync |
| Facebook | HBase | Messages storage — billions of rows with user ID + timestamp as row key |
| LinkedIn | HBase | Activity tracking and analytics on Hadoop infrastructure |
| Uber | Cassandra | Trip and event data — high-write, geographically distributed |

---

## When to Use

**Use Column DBs when:**
- Massive scale — billions of rows
- High write throughput — IoT sensors, logs, events, activity tracking
- Time-range queries — "all events for user X between time A and B"
- Wide, sparse rows — rows with many optional/variable columns

**Avoid when:**
- You need complex joins or ad-hoc queries
- Small dataset — relational DB is simpler and better
- Strong consistency is non-negotiable → prefer HBase over Cassandra

---

## Interview Questions

**Q: What is a wide-column store and how does it differ from a relational DB?**
A: Rows have a key and can have different columns; no fixed schema. Data is grouped by column families. Unlike relational DBs, empty columns take no space and reads only load requested columns.

**Q: Why is Cassandra preferred for high-write workloads?**
A: It uses an append-only LSM tree structure, has no master node bottleneck, and distributes writes across all nodes — enabling millions of writes per second.

**Q: What does "tunable consistency" mean in Cassandra?**
A: You configure how many nodes must confirm a read/write before it's considered successful — trade consistency for speed based on your requirement.

**Q: When would you pick HBase over Cassandra?**
A: When you need strong consistency and are already in the Hadoop ecosystem for big data processing.

**Q: What's the difference between a column DB and a columnar DB?**
A: Columnar DBs (like Redshift, BigQuery) store data column-by-column on disk for analytics. Wide-column stores (Cassandra, HBase) are distributed NoSQL stores with flexible column-per-row schemas — different concept, same word is often confused.
