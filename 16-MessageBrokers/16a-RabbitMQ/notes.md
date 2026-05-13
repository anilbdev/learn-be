# 16a — RabbitMQ

## Key Concepts

| Term | Definition |
|------|-----------|
| Message Broker | Middleman between services — decouples producer and consumer |
| Producer | Service that sends a message |
| Exchange | Receives messages from producer, routes them to queues |
| Queue | Buffer that holds messages until consumed |
| Consumer | Service that picks up and processes messages |
| ACK | Acknowledgment sent by consumer after successful processing |
| NACK | Negative acknowledgment — message is re-queued |
| Dead Letter Queue (DLQ) | Queue where repeatedly failed messages are sent |
| Durability | Queues/messages survive broker restarts |
| Prefetch Count | Max unacknowledged messages a consumer can hold at once |
| AMQP | Protocol RabbitMQ implements (Advanced Message Queuing Protocol) |

---

## Exchange Types

| Type | Routing Behavior |
|------|-----------------|
| Direct | Routes to queue with exact matching routing key |
| Fanout | Broadcasts to ALL bound queues (pub/sub) |
| Topic | Pattern matching on routing key (e.g., `order.*`) |
| Headers | Routes based on message header attributes |

---

## How It Works

1. Producer publishes message to an **Exchange**
2. Exchange applies routing rules → sends to matching **Queue(s)**
3. Message sits in queue
4. Consumer picks up message and processes it
5. Consumer sends **ACK** → message deleted from queue
6. If no ACK (crash/failure) → RabbitMQ **re-queues** the message automatically

```
Producer → Exchange → Queue → Consumer
                ↓ (on failure, no ACK)
           Re-queue
```

---

## Competing Consumers vs Pub/Sub

| Pattern | Setup | Behavior |
|---------|-------|----------|
| Competing Consumers | Multiple consumers, one queue | Each message goes to ONE consumer (round-robin) — load balancing |
| Pub/Sub | Fanout exchange, one queue per consumer | Every consumer gets every message — broadcast |

---

## Industry Examples

- **Uber** — ride request queued; driver-matching, pricing, and notification services consume independently
- **Zalando** — order placed → inventory, billing, shipping each process from their own queues
- **General** — email sending, video rendering, log processing, background jobs

---

## When to Use RabbitMQ

- Task queues (background jobs, email, image resize)
- Message must be processed **once then discarded**
- Complex routing needed (topic/fanout patterns)
- Simple, low-latency async messaging
- Smaller scale applications

## When NOT to Use

- Need to replay messages (use Kafka)
- Multiple independent services consuming same stream (use Kafka)
- Need long-term message retention (use Kafka)
- Need synchronous response (use HTTP/gRPC)

---

## Interview Questions

**Q: What happens if a RabbitMQ consumer crashes before ACKing?**
RabbitMQ detects the missing ACK via timeout and re-queues the message for another consumer to pick up.

**Q: What is the difference between Direct and Fanout exchange?**
Direct routes to a queue with an exact matching routing key. Fanout broadcasts to all bound queues regardless of routing key — used for pub/sub.

**Q: If 3 consumers are on the same queue, does each get every message?**
No — each message goes to only one consumer via round-robin (competing consumers). For every consumer to get every message, use a Fanout exchange with a separate queue per consumer.

**Q: What is a Dead Letter Queue?**
A queue where messages are sent after repeated failures (max retries exceeded, TTL expired, or explicitly NACKed). Used for debugging and manual reprocessing.

**Q: When would you use RabbitMQ over Kafka?**
When you need simple task queues, messages should be deleted after processing, or you need complex routing rules. RabbitMQ is simpler to operate for smaller-scale async workloads.
