# 09b — ACID Properties

## Key Concepts

| Property | One-Line Definition |
|---|---|
| Atomicity | All or nothing — transaction fully succeeds or fully rolls back |
| Consistency | DB always moves from one valid state to another — constraints never broken |
| Isolation | Concurrent transactions don't interfere or see each other's uncommitted changes |
| Durability | Committed data survives crashes — persisted to disk, not just memory |

---

## How It Works

- **Atomicity:** If any step in a transaction fails → entire transaction rolls back
- **Consistency:** Foreign keys, unique constraints, rules always enforced
- **Isolation:** Transactions behave as if they ran sequentially, even when concurrent
- **Durability:** Data written to disk (via WAL) before confirming commit

---

## Isolation Levels (Low → High Protection)

| Level | What It Allows | Problem It Causes |
|---|---|---|
| Read Uncommitted | Read dirty (uncommitted) data | Dirty reads |
| Read Committed | Only read committed data | Non-repeatable reads |
| Repeatable Read | Same read returns same value always | Phantom reads |
| Serializable | Full isolation — transactions run sequentially | Lowest concurrency, slowest |

> Most databases default to **Read Committed**

---

## Real-World Scenarios

| Scenario | ACID Property |
|---|---|
| Payment fails halfway | Atomicity rolls it back |
| Duplicate email signup blocked | Consistency enforces unique constraint |
| Two users book last seat simultaneously | Isolation ensures only one succeeds |
| Server crash after payment confirmation | Durability persists the committed data |

---

## Industry Examples

- **Banks / PayPal** — every money movement is atomic; no partial transfers
- **Amazon** — order placement (charge + inventory + shipment) wrapped in one transaction
- **Airline systems** — isolation prevents double-booking the same seat

---

## Interview Questions

| Question | One-Line Answer |
|---|---|
| What does ACID stand for? | Atomicity, Consistency, Isolation, Durability |
| What is atomicity? | Transaction fully succeeds or fully rolls back — no partial updates |
| Consistency vs Isolation? | Consistency = data rules enforced; Isolation = concurrent transactions don't interfere |
| Why is durability important? | Committed data survives crashes — written to disk, not just memory |
| What if DB is not ACID compliant? | Risk of partial writes, data corruption, lost updates, dirty reads |
| Name the 4 isolation levels? | Read Uncommitted, Read Committed, Repeatable Read, Serializable |
