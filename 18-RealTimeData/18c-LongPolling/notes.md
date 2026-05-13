# 18c — Long Polling

## Key Concepts

| Term | Definition |
|------|------------|
| Long Polling | Client sends request; server holds it open until data is ready or timeout |
| Timeout | If no data arrives within ~30-60s, server sends empty response |
| Simulated push | Long polling mimics server push using standard HTTP request-response |
| Thread blocking | Server must keep a thread/connection alive per waiting client |
| Re-request | After each response (data or timeout), client immediately sends a new request |

---

## How It Works

### Request Cycle
1. Client sends HTTP request to server
2. Server holds the request open — does not respond immediately
3. When new data arrives → server responds with data
4. If timeout reached (e.g. 30s) with no data → server responds empty
5. Client receives response, processes it (or discards empty)
6. Client immediately sends a new request → cycle repeats

### Diagram
```
Client: Any updates? ──────────────────► Server (holds...)
                                         (data arrives)
Client: ◄──────────── Here's the data!─ Server
Client: Any updates? ──────────────────► Server (holds again...)
                                         (timeout, no data)
Client: ◄──────────── (empty response) ─ Server
Client: Any updates? ──────────────────► Server (holds again...)
```

---

## Comparison with Short Polling

| Feature | Short Polling | Long Polling |
|---|---|---|
| Server response | Immediate (empty or data) | Waits until data ready |
| Empty responses | Very frequent | Minimal |
| Latency | Up to full interval | Near-instant when data arrives |
| Server load | Very high (constant requests) | Medium (fewer requests) |
| Complexity | Simple | Medium |

---

## Full Comparison

| Technique | Connection | Latency | Overhead | Use When |
|---|---|---|---|---|
| Short Polling | New per request (timer) | High | Very high | Simple, low-user, frequent updates |
| Long Polling | New per response | Medium | Medium | HTTP-only fallback |
| SSE | Persistent HTTP | Low | Low | Server-to-client feeds |
| WebSocket | Persistent bidirectional | Very low | Very low | Chat, games, collaboration |

---

## Industry Examples

| Company | Use Case |
|---|---|
| Facebook Chat (2008) | Original launch used long polling — held requests up to 30s waiting for messages |
| WhatsApp Web (early) | Used long polling before migrating to WebSockets |
| JIRA / Trello (older) | Board updates via long polling before WebSocket adoption |

---

## When to Use

**Use Long Polling when:**
- WebSockets or SSE are blocked by firewalls or corporate security policies
- Infrastructure only supports standard HTTP (old proxies/load balancers)
- Used as a fallback when WebSocket connection fails
- Near-real-time is needed but persistent connections aren't supported

**Avoid when:**
- WebSockets or SSE are available — they are always more efficient
- High user concurrency — thread blocking becomes a serious bottleneck

---

## Interview Questions

**Q: What is long polling and how does it differ from regular polling?**
A: Regular polling sends requests on a fixed timer regardless of updates. Long polling holds the request open until data is ready — reducing empty responses and improving latency.

**Q: What are the downsides of long polling?**
A: Server must hold many open connections simultaneously — memory and thread overhead. Each response still requires a new HTTP request. Not truly real-time.

**Q: When would you still use long polling today?**
A: As a fallback when WebSockets are blocked by firewalls or proxies, or in environments where only plain HTTP is supported.

**Q: How is long polling different from SSE?**
A: SSE keeps one persistent connection open and streams continuously. Long polling closes and reopens the connection after every response — still request-response underneath.
