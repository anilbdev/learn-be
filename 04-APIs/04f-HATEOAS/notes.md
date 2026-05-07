# 04f — HATEOAS

## Key Concepts

| Term | Definition |
|---|---|
| HATEOAS | Hypermedia As The Engine Of Application State — REST constraint where responses include links for available actions |
| Hypermedia | Links embedded in API responses that describe what the client can do next |
| Richardson Maturity Model | 4-level model measuring how "RESTful" an API is (0–3) |
| Level 3 | HATEOAS — highest REST maturity level |

---

## How It Works

1. Client makes a request (e.g. GET `/orders/456`)
2. Server returns data **plus** `_links` — URLs for available next actions
3. Client follows links rather than constructing URLs itself
4. Available links change based on current state (e.g. cancelled order has no `pay` link)

```json
{
  "orderId": "456",
  "status": "pending",
  "_links": {
    "self":   { "href": "/orders/456" },
    "cancel": { "href": "/orders/456/cancel", "method": "DELETE" },
    "pay":    { "href": "/orders/456/pay",    "method": "POST" }
  }
}
```

---

## Richardson Maturity Model

| Level | What It Means | Example |
|---|---|---|
| 0 | One endpoint, RPC-style | `POST /api` for everything |
| 1 | Resources | `/users`, `/orders` |
| 2 | HTTP verbs + resources | `GET /users`, `DELETE /orders/1` |
| 3 | HATEOAS — hypermedia links | Response includes `_links` |

Most real-world APIs are **Level 2**.

---

## Benefits vs Reality

| | Theory | Reality |
|---|---|---|
| Client decoupling | URL changes don't break clients — they follow links | Clients hardcode URLs anyway |
| Discoverability | Clients explore the API dynamically | Clients read docs, not links |
| State awareness | Only valid actions appear as links | Extra server-side logic to generate |

---

## Why It's Rarely Used

- Clients hardcode URLs — the decoupling benefit is rarely realized
- Generating accurate, state-aware links server-side adds complexity
- Most teams get enough value from Level 2 (resources + HTTP verbs)
- No major public API fully implements it (most stop at Level 2)

---

## Interview Questions

**Q: What is HATEOAS?**
A: A REST constraint where API responses include hypermedia links describing available next actions, so clients discover the API dynamically rather than hardcoding URLs.

**Q: What level is HATEOAS on the Richardson Maturity Model?**
A: Level 3 — the highest level of REST maturity.

**Q: Why don't most APIs implement HATEOAS?**
A: Clients hardcode URLs anyway so the decoupling benefit is rarely realized, and generating accurate state-aware links server-side adds complexity with little practical payoff. Most APIs stop at Level 2.

**Q: What is the benefit of HATEOAS in theory?**
A: If server URLs change, clients don't break — they just follow whatever links the response provides, decoupling client from server structure.
