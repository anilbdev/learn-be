# 17a — Document Databases (MongoDB, CouchDB)

## Key Concepts

| Term | Definition |
|---|---|
| Document DB | Stores data as JSON-like documents instead of rows/columns |
| Document | A self-contained JSON/BSON object representing one entity |
| Collection | Group of documents (equivalent to a SQL table) |
| Schema-less | Documents in the same collection can have different fields |
| BSON | Binary JSON — MongoDB's internal storage format |
| Embedded document | Nested object or array inside a document |

---

## How It Works

- **Structure:** Database → Collections → Documents
- Related data is embedded inside one document (no joins needed)
- Each document has a unique `_id` field
- Documents in the same collection can have different shapes (flexible schema)
- Queries filter by field values, support nested field access (e.g., `user.address.city`)

```json
{
  "_id": "u123",
  "name": "Anil",
  "orders": [
    { "product": "Laptop", "amount": 1200 },
    { "product": "Mouse", "amount": 25 }
  ]
}
```

---

## MongoDB vs CouchDB

| Feature | MongoDB | CouchDB |
|---|---|---|
| Query language | MQL (MongoDB Query Language) | MapReduce / Mango |
| API | Proprietary driver | HTTP REST API (native) |
| Replication | Replica sets | Built-in multi-master sync |
| Best for | General purpose, high scale | Offline-first, IoT, edge sync |

---

## When to Use

| Use Document DB | Use SQL Instead |
|---|---|
| Hierarchical / nested data | Highly relational data |
| Schema changes frequently | Stable schema |
| Horizontal scaling needed | Complex joins required |
| Read-heavy, self-contained entities | Strong ACID transactions needed |

---

## Industry Examples

- **eBay** — product catalog (each product has different attributes: shirt has size/color, laptop has RAM/storage)
- **Forbes** — article content with varying metadata per article type
- **Lyft** — trip documents embedding driver info, route, pricing, timestamps
- **IBM** — CouchDB for IoT/edge devices with offline-first sync

---

## Interview Questions

**Q: What is a document database?**
A: Stores data as self-contained JSON-like documents. Related data is embedded in one document, eliminating the need for joins.

**Q: When would you choose MongoDB over PostgreSQL?**
A: When data is hierarchical/nested, schema changes frequently, or you need to scale horizontally. PostgreSQL is better when data is highly relational or strong ACID guarantees are required.

**Q: What is schema-less and what are the trade-offs?**
A: Schema-less means no enforced structure — documents in the same collection can have different fields. Benefit: flexibility. Trade-off: risk of inconsistent data, harder to enforce data integrity.

**Q: How does MongoDB handle relationships?**
A: Two ways — embedding (nest related data inside the document) or referencing (store an ID and look up separately). Embedding is preferred when data is always accessed together.

**Q: What is CouchDB best known for?**
A: Its HTTP-native API and built-in multi-master replication — making it ideal for offline-first and edge/IoT scenarios where devices sync data when back online.
