# 18b — Server-Sent Events (SSE)

## Key Concepts

| Term | Definition |
|------|------------|
| SSE | One-way persistent HTTP connection where server streams data to client |
| Unidirectional | Server → Client only; client cannot send data back over the same connection |
| text/event-stream | The MIME type that signals an SSE connection |
| EventSource | Browser API used to receive SSE events |
| Auto-reconnect | Browser automatically re-establishes SSE connection if it drops |
| Event stream | Newline-delimited text format server uses to push events |

---

## How It Works

### Connection Flow
1. Client sends GET request with `Accept: text/event-stream`
2. Server keeps connection open — does not close after responding
3. Server pushes newline-delimited text events whenever data is available
4. Client processes each event as it arrives
5. If connection drops, browser auto-reconnects

### HTTP Request
```
GET /live-scores HTTP/1.1
Accept: text/event-stream
```

### Server Response Stream
```
data: { "score": "2-1" }

data: { "score": "2-2" }

data: { "score": "3-2" }
```

### Client Code (Browser)
```javascript
const source = new EventSource('/live-scores');
source.onmessage = (event) => {
  console.log(event.data);
};
```

---

## SSE vs WebSocket

| Feature | SSE | WebSocket |
|---|---|---|
| Direction | Server → Client only | Bidirectional |
| Protocol | Plain HTTP | WebSocket (upgrade required) |
| Auto-reconnect | Built in | Must implement manually |
| Data format | Text only | Text or binary |
| Complexity | Simple | More complex |
| Best for | Live feeds, notifications | Chat, games, collaboration |

---

## Industry Examples

| Company | Use Case |
|---|---|
| ChatGPT | Streams tokens word-by-word to browser as model generates them |
| GitHub | CI/CD pipeline progress bars stream status in real time |
| Twitter/X | New tweets pushed to timeline while you're reading |
| News sites | Live election results, sports scores streamed to browser |

---

## When to Use

**Use SSE when:**
- Only the server needs to push data (client just reads)
- Live feeds, dashboards, notifications, progress updates
- You want simplicity — works over plain HTTP/2
- Auto-reconnect is needed without extra code

**Avoid when:**
- Client needs to send data back frequently → use WebSocket
- Binary data needed → use WebSocket
- Very high frequency, ultra-low latency → use WebSocket

---

## Interview Questions

**Q: What is SSE and how does it differ from WebSockets?**
A: SSE is a one-way persistent HTTP connection — server pushes to client only. WebSockets are bidirectional. SSE is simpler, uses plain HTTP, and has auto-reconnect built in.

**Q: How does SSE work under the hood?**
A: Client sends one HTTP GET with `Accept: text/event-stream`. Server keeps the connection open and streams newline-delimited text events whenever data is available.

**Q: Give a real-world example of SSE.**
A: ChatGPT — tokens stream to your browser word by word via SSE as the model generates them, instead of waiting for the full response.

**Q: What is a limitation of SSE?**
A: One-way only — client cannot send data back over the same connection. Also text-only, no binary data support.

**Q: Why choose SSE over WebSocket for a live dashboard?**
A: Dashboard only needs server-to-client updates — no bidirectional communication required. SSE is simpler to implement, works on plain HTTP, and handles reconnection automatically.
