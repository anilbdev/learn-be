# 17b — Key-Value Databases (Redis, DynamoDB)

## Key Concepts

| Term | Definition |
|---|---|
| Key-Value DB | Stores data as simple key → value pairs (like a giant hash map) |
| Redis | In-memory key-value store; supports rich data types; blazing fast |
| DynamoDB | AWS managed key-value + document DB; massively scalable |
| TTL (Time To Live) | Automatic expiry on a key after a set time |
| Partition key | The key used to distribute data across nodes (DynamoDB) |
| In-memory | Data stored in RAM, not disk — microsecond read/write |

---

## How It Works

- Every entry is a **key** (unique identifier) mapped to a **value**
- No schema, no joins, no relationships
- Lookup is O(1) — just hash the key and retrieve the value
- Value can be: string, list, set, hash, sorted set (Redis), or any blob (DynamoDB)

```
SET session:user123  "{ userId: 123, role: 'admin' }"
GET session:user123  → "{ userId: 123, role: 'admin' }"
```

---

## Redis vs DynamoDB

| Feature | Redis | DynamoDB |
|---|---|---|
| Storage | In-memory (optional disk persistence) | Disk (SSD), fully managed by AWS |
| Speed | Sub-millisecond (microseconds) | Single-digit millisecond |
| Scale | Vertical + Redis Cluster | Infinite horizontal (AWS managed) |
| Data types | Strings, Lists, Sets, Hashes, Sorted Sets | Key-Value + nested documents |
| Best for | Caching, sessions, rate limiting, leaderboards | High-scale apps, serverless, AWS ecosystem |
| Persistence | Optional (RDB snapshots / AOF logs) | Always persistent |

---

## Common Redis Use Cases

| Use Case | How |
|---|---|
| Session store | Store user session with TTL (auto-expire on logout) |
| Caching | Cache DB query results with short TTL |
| Rate limiting | Increment a counter per user per minute |
| Leaderboards | Sorted sets to rank players by score |
| Pub/Sub | Message passing between services |

---

## Industry Examples

- **Twitter** — Redis for caching timelines and storing user sessions
- **GitHub** — Redis for job queues and rate limiting API calls
- **Snapchat** — Redis for storing ephemeral message metadata
- **Amazon** — DynamoDB for shopping cart (scale: millions of writes/sec on Prime Day)
- **Lyft** — DynamoDB for ride matching and dispatch data
- **Samsung** — DynamoDB for IoT device state storage

---

## Interview Questions

**Q: What is a key-value database and when would you use it?**
A: Stores data as key → value pairs. Use it for caching, session management, or any scenario where you need extremely fast lookups by a known key.

**Q: Why is Redis so fast?**
A: Because it stores all data in RAM. Disk I/O is the bottleneck in most databases — Redis eliminates it entirely, giving sub-millisecond response times.

**Q: What's the difference between Redis and a regular cache?**
A: Redis supports rich data types (lists, sorted sets, hashes) and has persistence options. A plain cache is usually just strings with TTL. Redis can also do pub/sub, queues, and leaderboards — it's a multi-tool.

**Q: When would you choose DynamoDB over Redis?**
A: When you need persistent, infinitely scalable storage at AWS scale. Redis is primarily for caching/ephemeral data; DynamoDB is for durable, primary data storage.

**Q: What is TTL and why is it important in key-value stores?**
A: Time To Live — a countdown after which the key is automatically deleted. Critical for session expiry, cache invalidation, and limiting stale data.
