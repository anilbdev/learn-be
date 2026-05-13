# 18d — Short Polling

## Key Concepts

| Term | Definition |
|------|------------|
| Short Polling | Client sends HTTP requests on a fixed timer interval, regardless of updates |
| Polling interval | Time between requests (e.g. every 3s, every 10s) |
| Empty response | Server responds immediately with no data when nothing has changed |
| Latency | Up to the full interval — data arriving just after a poll waits the full interval |

---

## How It Works

### Request Cycle
1. Client sets a timer (e.g. every 3 seconds)
2. On each tick, sends a standard HTTP GET request
3. Server responds immediately — data if available, empty if not
4. Client processes response or ignores empty one
5. Timer fires again, repeat indefinitely

### Diagram
```
Client: Any updates? → Server: No.
(wait 3s)
Client: Any updates? → Server: No.
(wait 3s)
Client: Any updates? → Server: Yes! Here's the data.
(wait 3s)
Client: Any updates? → Server: No.
```

---

## Problems with Short Polling

| Problem | Explanation |
|---|---|
| Wasted requests | Most responses are empty — bandwidth and compute wasted |
| Latency tied to interval | Data arriving 1s after a poll waits the full interval before client sees it |
| Server load | 1,000 users × every 3s = 333 req/sec even when nothing is happening |
| Battery drain | Constant network activity prevents mobile radio from sleeping |

---

## All Four Techniques — Full Comparison

| Technique | How | Latency | Overhead | Best For |
|---|---|---|---|---|
| Short Polling | Repeated requests on timer | High | Very high | Simple tools, low users, frequent updates |
| Long Polling | Hold request until data ready | Medium | Medium | HTTP-only fallback, no WS support |
| SSE | Persistent HTTP, server streams | Low | Low | Server-to-client feeds, dashboards |
| WebSocket | Persistent, bidirectional | Very low | Very low | Chat, games, live collaboration |

---

## Industry Examples

| Use Case | Example |
|---|---|
| Email clients (legacy) | Webmail polling every 30s for new messages |
| Build status pages | Simple CI dashboards refreshing every 10s |
| IoT sensor dashboards | Basic sensor readings polled every few seconds |
| Legacy banking portals | Transaction history pages that auto-refresh periodically |

---

## When Short Polling is Acceptable

- Updates are very frequent (data arrives every 2-3s anyway — polling matches the rate)
- Very low user count (internal tools, admin dashboards)
- Infrastructure doesn't support persistent connections
- Simplicity is the top priority

**Avoid when:**
- User base is large — server load multiplies fast
- Updates are infrequent — most requests will be empty
- Low latency is required — interval delay is unacceptable
- Mobile clients involved — battery drain is significant

---

## Interview Questions

**Q: What is short polling and what are its drawbacks?**
A: Client sends requests on a fixed timer regardless of updates. Drawbacks: wasted requests, latency tied to interval length, unnecessary server load, battery drain on mobile.

**Q: When is short polling an acceptable choice?**
A: When updates are frequent (so most polls return data), user count is low, or infrastructure doesn't support persistent connections.

**Q: How would you choose between the four real-time techniques?**
A: Bidirectional real-time → WebSocket. Server-only push, persistent → SSE. HTTP-only fallback → Long Polling. Simple, infrequent, low-scale → Short Polling.

**Q: Why does short polling drain mobile battery?**
A: Constant network requests prevent the device radio from entering sleep mode — keeping it active and consuming power continuously.
