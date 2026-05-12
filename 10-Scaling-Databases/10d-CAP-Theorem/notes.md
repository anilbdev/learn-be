# 10d — CAP Theorem

## Key Concepts

| Term | Definition |
|------|------------|
| CAP Theorem | A distributed system can guarantee only 2 of 3: Consistency, Availability, Partition Tolerance |
| Consistency (C) | Every read returns the most recent write — no stale data |
| Availability (A) | Every request gets a response — no timeouts or errors |
| Partition Tolerance (P) | System keeps working even when nodes can't communicate |
| Network Partition | A network failure causing nodes to lose communication |
| Eventual Consistency | Data will become consistent across nodes — but not instantly |
| Tunable Consistency | System lets you choose consistency level per query (e.g. Cassandra) |

---

## How It Works

**P is non-negotiable:**
- Network failures happen in all distributed systems
- You must tolerate partitions — so P is always required
- Real choice: **CP or AP**

```
During a network partition:

CP system → stops responding rather than serve stale data
AP system → keeps responding but may serve stale data
```

---

## CP vs AP

| | CP | AP |
|---|---|---|
| During partition | Refuses requests (unavailable) | Keeps responding (may be stale) |
| Guarantees | Consistency | Availability |
| Use when | Wrong answer worse than no answer | Uptime matters more than accuracy |
| Examples | Zookeeper, HBase, MongoDB (default), etcd | Cassandra, DynamoDB, CouchDB |
| Real use case | Banking, payments, inventory | Shopping cart, social feed, product catalog |

---

## Real Scenario

```
Normal:
[Node A] ←——sync——→ [Node B]

During partition:
[Node A]  ✗  [Node B]  (network cut)

CP choice: Node A stops responding until sync restored ✅ consistent ❌ unavailable
AP choice: Node A keeps responding with potentially old data ❌ stale ✅ available
```

---

## PACELC (Senior-Level Extension)

CAP only covers behavior **during** a partition. PACELC adds:
- **Else (no partition):** tradeoff between **Latency** and **Consistency**
- Stricter consistency = more node coordination = higher latency
- Why Cassandra/DynamoDB offer tunable consistency — pick the tradeoff per query

---

## Industry Examples

- **Amazon DynamoDB** — AP; eventual consistency by default, strong consistency available at higher cost
- **Google Spanner** — CP globally; uses atomic clocks to minimize unavailability window
- **Cassandra (Discord, Netflix)** — AP; tunable consistency per query
- **Zookeeper (Kafka coordination)** — CP; must be consistent for leader election

---

## Interview Questions

**Q: What does CAP theorem state?**
A: A distributed system can guarantee only 2 of: Consistency, Availability, Partition Tolerance.

**Q: Why is Partition Tolerance non-negotiable?**
A: Network failures always happen in distributed systems — you must design for them, making the real choice between C and A.

**Q: When do you choose CP over AP?**
A: When correctness is critical — banking, payments, inventory. A wrong answer is worse than no answer.

**Q: When do you choose AP over CP?**
A: When uptime matters more than perfect accuracy — social feeds, shopping carts, product catalogs. Brief staleness is acceptable.

**Q: What is eventual consistency?**
A: Data will become consistent across all nodes eventually — just not immediately. Used by AP systems like DynamoDB and Cassandra.

**Q: What is PACELC?**
A: An extension of CAP — even without a partition, there's a tradeoff between latency and consistency. Stricter consistency requires more node coordination and increases latency.
