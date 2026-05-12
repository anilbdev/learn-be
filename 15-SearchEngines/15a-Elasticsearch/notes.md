# 15a — Elasticsearch

## Key Concepts

| Term | Definition |
|------|-----------|
| **Elasticsearch** | Distributed search and analytics engine built on Apache Lucene |
| **Document** | JSON object stored in ES — equivalent to a row in a DB |
| **Index** | Collection of documents — like a DB table, optimized for search |
| **Inverted Index** | Data structure mapping each word → list of document IDs containing it |
| **Shard** | A partition/slice of an index distributed across nodes for scalability |
| **Replica** | A copy of a shard on a different node — for fault tolerance and read throughput |
| **Node** | A single server/machine in the ES cluster |
| **Near Real-Time (NRT)** | New documents become searchable within ~1 second |
| **BM25** | Relevance ranking algorithm ES uses to score and sort results |
| **Fuzzy Search** | Matches near-misspellings — "sneekers" finds "sneakers" |

---

## How It Works

1. Document is submitted to ES via REST API
2. ES tokenizes the text — splits into individual words/terms
3. Each term is added to the **inverted index**: `word → [doc1, doc5, doc12]`
4. On search, ES looks up the query term in the inverted index (no full scan)
5. Matching documents are retrieved and **scored by relevance** (BM25)
6. Results returned ranked by score, within ~1 second

### Inverted Index Example
```
"payment" → [doc1, doc100, doc150]
"failure" → [doc3, doc100, doc200]

Search "payment failure" → intersection → [doc100]
```

### Shard vs Node vs Replica
```
Cluster
├── Node A
│   ├── Shard 1 (primary)
│   └── Shard 2 (replica of Shard 2 on Node B)
└── Node B
    ├── Shard 2 (primary)
    └── Shard 1 (replica of Shard 1 on Node A)
```
- **Node** = the machine
- **Shard** = partition of the index data
- **Replica** = copy of a shard on a different node

---

## Why ES Over SQL Full-Text Search

| | SQL `LIKE '%term%'` | Elasticsearch |
|---|---|---|
| Speed | Full table scan — slow | Inverted index lookup — fast |
| Relevance ranking | No | Yes (BM25) |
| Fuzzy matching | No | Yes |
| Scalability | Scale whole DB | Horizontal via shards |
| Analytics | Limited | Strong aggregations |

---

## Industry Examples

| Company | Use Case |
|---------|----------|
| **GitHub** | Search repos, files, issues across millions of records |
| **Netflix** | Content discovery — search titles, tags, metadata |
| **Shopify** | Product search with relevance ranking |
| **Uber** | Driver/trip log search and analytics |
| **Stack Overflow** | Full-text search with relevance ranking |
| **Wikipedia** | Search across millions of articles |

---

## Common Pattern: Postgres + Elasticsearch

- Postgres = source of truth (relational data, transactions)
- Elasticsearch = search layer (synced from Postgres)
- Data flows: Postgres → sync pipeline → ES index
- Used by Shopify, GitHub, most large e-commerce platforms

---

## Interview Questions

**Q: What is Elasticsearch and when would you use it over SQL?**
> Use ES when you need full-text search, relevance ranking, or fast querying across large unstructured text. SQL is better for relational data with exact lookups and transactions.

**Q: What is an inverted index?**
> A data structure that maps each word to the list of documents containing it. Instead of scanning every document, ES looks up the word in this pre-built map — making search near-instant.

**Q: What is the difference between a shard and a replica?**
> A shard is a partition of an index — ES splits data across shards on different nodes for horizontal scalability. A replica is a copy of a shard on a different node, providing fault tolerance and additional read throughput.

**Q: What does near real-time mean in Elasticsearch?**
> Documents become searchable within ~1 second of being indexed, due to how Lucene refreshes its internal segments. Not instant, but fast enough for most use cases.

**Q: Why add Elasticsearch on top of Postgres instead of using Postgres full-text search?**
> Postgres full-text search lacks relevance ranking, fuzzy matching, and doesn't scale independently. ES provides BM25 scoring, typo tolerance, and horizontal scaling via shards — Postgres remains the source of truth while ES handles search.
