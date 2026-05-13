# 18a — WebSockets

## Key Concepts

| Term | Definition |
|------|------------|
| WebSocket | Persistent, full-duplex, low-latency connection between client and server |
| Full-duplex | Both sides can send and receive simultaneously |
| Persistent | Connection stays open — no new handshake per message |
| HTTP Upgrade | The handshake mechanism that switches an HTTP connection to WebSocket |
| 101 Switching Protocols | HTTP status code confirming the upgrade to WebSocket |
| Close frame | Signal sent by either side to terminate the WebSocket connection |

---

## How It Works

### Connection Lifecycle
1. Client sends HTTP request with `Upgrade: websocket` header
2. Server responds with `101 Switching Protocols`
3. HTTP protocol is replaced by WebSocket protocol on the same TCP connection
4. Both sides can now send messages freely at any time
5. Either side sends a close frame to terminate

### Handshake
```
Client → Server:
GET /chat HTTP/1.1
Upgrade: websocket
Connection: Upgrade

Server → Client:
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
```

### Bidirectional Messaging
```
Client ──────── "Hello!" ────────────► Server
Client ◄──────── "Hi back!" ─────── Server
Client ◄──────── "New message!" ─── Server   ← server-initiated
```

---

## HTTP vs WebSocket

| Feature | HTTP | WebSocket |
|---|---|---|
| Connection | New per request | One persistent connection |
| Direction | Client → Server only | Both directions |
| Overhead | Headers on every request | Headers only on handshake |
| Latency | Higher | Very low |
| Best for | REST APIs, page loads | Real-time, live updates, chat |

---

## Industry Examples

| Company | Use Case |
|---|---|
| Slack | Instant message delivery — server pushes messages as they arrive |
| Uber | Live driver location updates pushed to rider's map every few seconds |
| Robinhood | Real-time stock price feeds — thousands of updates per second |
| Figma | Live collaborative editing — all users see changes simultaneously |
| Multiplayer games | Player positions and actions synced in real time |

---

## When to Use

**Use WebSockets when:**
- Bidirectional real-time communication is needed
- Client also sends data frequently (chat, games, collaborative tools)
- Very low latency is critical
- Binary data (audio, video) needs to be transmitted

**Avoid when:**
- Server only needs to push data → use SSE instead
- Simple request-response pattern → plain HTTP is sufficient

---

## Interview Questions

**Q: What is a WebSocket and how does it differ from HTTP?**
A: WebSocket is a persistent, full-duplex connection. HTTP is request-response — client always initiates. WebSocket allows server to push data anytime after the initial handshake.

**Q: How does a WebSocket connection get established?**
A: Starts as HTTP with `Upgrade: websocket` header. Server responds with `101 Switching Protocols` — HTTP is replaced by WebSocket protocol on the same TCP connection.

**Q: When would you use WebSockets over REST?**
A: When you need real-time, low-latency, bidirectional communication — chat apps, live dashboards, multiplayer games, live location tracking.

**Q: What is full-duplex communication?**
A: Both client and server can send messages simultaneously and independently — neither waits for the other to finish.

**Q: Is the HTTP connection closed when WebSocket is established?**
A: Not exactly — the HTTP protocol is upgraded/replaced by WebSocket protocol on the same underlying TCP connection. The TCP connection persists.
