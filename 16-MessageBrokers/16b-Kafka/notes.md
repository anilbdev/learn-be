# 16b — Kafka

## Key Concepts

| Term | Definition |
|------|-----------|
| Kafka | Distributed event streaming platform — append-only log, not a queue |
| Topic | Named stream of events (e.g., `orders`, `user-events`) |
| Partition | A topic split into ordered, immutable segments for parallelism |
| Offset | Position of a message within a partition — consumers track this themselves |
| Producer | Writes events to a topic |
| Consumer | Reads events from a topic, manages its own offset |
| Consumer Group | Group of consumers sharing partitions — each partition assigned to one consumer |
| Broker | A Kafka server; multiple brokers = cluster |
| Retention Period | How long messages are kept (e.g., 7 days) — not deleted after consumption |
| Rebalance | Redistribution of partitions when a consumer joins or leaves a group |

---

## How It Works

1. Producer sends event to a **Topic**
2. Kafka appends it to a **Partition** (immutable log, gets an Offset)
3. Consumer reads from its assigned partition, tracks its own **Offset**
4. Message is **NOT deleted** after reading — retained until expiry
5. Any consumer can **replay** from any offset at any time

```
Producer → Topic
              ├── Partition 0: [msg1, msg4, msg7...]
              ├── Partition 1: [msg2, msg5, msg8...]
              └── Partition 2: [msg3, msg6, msg9...]
                      ↓
              Consumer Group (one consumer per partition)
```

---

## Consumer Groups & Partitions

| Scenario | Behavior |
|----------|----------|
| Consumers < Partitions | Some consumers handle multiple partitions |
| Consumers = Partitions | Ideal — one consumer per partition |
| Consumers > Partitions | Extra consumers sit idle (on standby) |

- Multiple **independent consumer groups** can read the same topic simultaneously
- Each group tracks its own offset — no interference between groups

---

## Kafka vs RabbitMQ

| | RabbitMQ | Kafka |
|--|----------|-------|
| Model | Message Queue | Event Log |
| After consumption | Message deleted | Message retained |
| Offset tracking | Broker tracks | Consumer tracks |
| Replay | Not possible | Yes — reset offset |
| Routing | Rich (Direct/Fanout/Topic) | Topic + partition key |
| Use case | Task queues, background jobs | Streaming, audit logs, analytics |
| Complexity | Lower | Higher |

---

## Industry Examples

- **LinkedIn** — invented Kafka; handles 7 trillion messages/day for activity tracking
- **Netflix** — streams playback events (pause, buffer, skip) for real-time recommendations
- **Uber** — driver location updates flow through Kafka to surge pricing engine
- **Airbnb** — audit log of every booking event; replay to rebuild derived datasets

---

## When to Use Kafka

- Need **message replay** (reprocessing, bug recovery)
- Multiple independent services consuming the **same event stream**
- **Audit logs** and event sourcing
- High-throughput real-time analytics pipelines
- Long-term event retention needed

## When NOT to Use

- Simple background jobs (RabbitMQ is simpler)
- Need complex message routing (RabbitMQ handles better)
- Small-scale application (Kafka is operationally heavy)
- Need synchronous request-response (use HTTP/gRPC)

---

## Interview Questions

**Q: What is a Kafka offset and why does it matter?**
The position of a message within a partition. Consumers track their own offset, giving them full control over where to read — enabling replay, independent progress per consumer group, and reprocessing after failures.

**Q: What happens if a Kafka consumer in a group goes down?**
Kafka detects it via heartbeat timeout and triggers a rebalance — that consumer's partitions are redistributed among the remaining active consumers in the group.

**Q: Can two consumer groups read the same Kafka topic?**
Yes. Each consumer group maintains its own offset independently. A billing service and an analytics service can both consume the same `orders` topic at their own pace without interference.

**Q: If a topic has 4 partitions and a consumer group has 6 consumers, what happens?**
Only 4 consumers actively read (one per partition). The other 2 sit idle and act as standby — they take over if an active consumer fails.

**Q: Give a real scenario where Kafka's replay saves the day.**
An analytics service had a bug and processed 3 days of events incorrectly. With Kafka, reset the offset back 3 days, replay through the fixed service, and rebuild correct data. With RabbitMQ, those messages would be gone permanently.

**Q: What is the difference between Kafka and RabbitMQ?**
RabbitMQ is a message queue — messages deleted after ACK, broker controls routing, best for task distribution. Kafka is an event log — messages retained, consumers track their own position, best for streaming, replay, and multiple independent consumers on the same data.
