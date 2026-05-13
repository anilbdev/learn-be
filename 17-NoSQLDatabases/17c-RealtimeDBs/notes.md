# 17c — Realtime Databases (Firebase, RethinkDB)

## Key Concepts

| Term | Definition |
|---|---|
| Realtime DB | Database that pushes data changes to clients instantly — no polling needed |
| Firebase Realtime DB | Google's cloud-hosted JSON tree DB with live sync via WebSockets |
| Firestore | Firebase's newer document DB with better querying (successor to Realtime DB) |
| RethinkDB | Open-source document DB with built-in changefeeds (push on data change) |
| Changefeed | A stream that emits events whenever data changes in a table |
| Offline-first | App works offline; syncs changes when connection is restored |

---

## How It Works

**Traditional DB (pull-based):**
```
Client → asks DB → "any new data?" → DB responds
(must poll repeatedly)
```

**Realtime DB (push-based):**
```
Client subscribes → DB watches for changes → pushes update instantly
(WebSocket connection stays open)
```

### Firebase Flow:
1. Client opens WebSocket connection to Firebase
2. Client subscribes to a data path (e.g., `/chats/room1`)
3. Any write to that path instantly propagates to all subscribers
4. Works offline — queues changes, syncs on reconnect

### RethinkDB Flow:
1. Client opens a changefeed query: `r.table('orders').changes()`
2. RethinkDB streams a new event every time a row is inserted/updated/deleted
3. Server pushes; client reacts

---

## Firebase vs RethinkDB

| Feature | Firebase Realtime DB | RethinkDB |
|---|---|---|
| Hosted | Cloud (Google-managed) | Self-hosted (open source) |
| Data model | JSON tree | Documents (like MongoDB) |
| Query power | Limited (path-based) | Full query language (ReQL) |
| Offline support | Built-in (client SDK) | Manual |
| Best for | Mobile/web apps, rapid prototyping | Real-time dashboards, collaborative tools |
| Status | Active (Google) | Maintenance mode (less active) |

---

## When to Use a Realtime DB

| Good fit | Bad fit |
|---|---|
| Chat apps | Heavy analytics / reporting |
| Live dashboards | Large relational datasets |
| Collaborative editing (Google Docs-like) | Batch processing |
| Multiplayer game state | Complex transactions |
| IoT live sensor feeds | |

---

## Industry Examples

- **Duolingo** — Firebase for real-time lesson progress sync across devices
- **Alibaba** — Firebase for live notifications and chat features
- **The New York Times** — Firebase for live election results dashboard
- **Horizon (RethinkDB)** — real-time collaborative tools and chat apps
- **Platzi** — Firebase for live coding classroom features

---

## Interview Questions

**Q: What makes a database "realtime"?**
A: Instead of the client polling for changes, the DB pushes updates to connected clients instantly over a persistent connection (WebSocket). Changes appear in the UI automatically.

**Q: How does Firebase sync data in real time?**
A: Clients open a WebSocket connection and subscribe to a data path. When any client writes to that path, Firebase broadcasts the change to all subscribers immediately.

**Q: What is a changefeed in RethinkDB?**
A: A live query that streams events to the application whenever data in a table is inserted, updated, or deleted. Eliminates the need for polling.

**Q: What are the trade-offs of Firebase Realtime DB?**
A: Easy to set up and great for live sync, but limited querying (you can only filter by path/key, not complex conditions), and data is stored as one large JSON tree which can get messy at scale. Firestore fixes most of this.

**Q: When would you NOT use a realtime database?**
A: When you need complex queries, strong consistency, transactions across entities, or are processing large batch datasets. Traditional SQL or document DBs are better there.
