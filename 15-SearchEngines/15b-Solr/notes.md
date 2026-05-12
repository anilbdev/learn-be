# 15b — Solr

## Key Concepts

| Term | Definition |
|------|-----------|
| **Solr** | Open-source enterprise search platform built on Apache Lucene |
| **Lucene** | The underlying Java search library both Solr and Elasticsearch use |
| **SolrCloud** | Solr's distributed/clustered mode — requires Apache ZooKeeper |
| **ZooKeeper** | Coordination service SolrCloud depends on for cluster management |
| **Faceting** | Grouping search results by categories (e.g., filter by brand, price range) |
| **Text Analysis Pipeline** | Chain of tokenizers and filters that process text before indexing |
| **Schema** | Solr uses explicit XML-based schema to define fields and types |

---

## How It Works

1. Documents submitted to Solr (XML, JSON, CSV)
2. Text passes through **analysis pipeline** — tokenized, filtered, stemmed
3. Processed terms stored in **Lucene inverted index**
4. Search queries analyzed the same way, then matched against index
5. Results returned with optional relevance scoring and facets

---

## Solr vs Elasticsearch

| | Solr | Elasticsearch |
|---|---|---|
| **Founded** | 2004 (older, more mature) | 2010 |
| **Built on** | Apache Lucene | Apache Lucene |
| **Config style** | XML config files | JSON via REST API |
| **Real-time ingestion** | Slower | Faster (~1 second NRT) |
| **Analytics** | Basic | Strong (aggregations + Kibana) |
| **Scaling** | SolrCloud + ZooKeeper (complex) | Built-in, simpler |
| **Ecosystem** | Apache / Hadoop | ELK Stack (ES + Logstash + Kibana) |
| **Best for** | Static data, enterprise docs, complex text pipelines | Real-time search, logs, analytics, e-commerce |
| **Community** | Mature but smaller | Larger, faster growing |

---

## When to Use Solr vs Elasticsearch

**Choose Solr when:**
- Already in Apache / Hadoop ecosystem
- Large volumes of **static or slowly changing** data
- Need complex, fine-grained **text analysis pipelines**
- Maintaining an existing Solr installation (migration cost too high)
- Enterprise document search (legal, government, research, scientific)

**Choose Elasticsearch when:**
- Greenfield project — modern default
- Need near real-time indexing
- Need analytics + dashboards (Kibana)
- E-commerce, log analysis, application search
- Simpler ops (no ZooKeeper dependency)

---

## Industry Examples

| Company | Use Case |
|---------|----------|
| **Apple** | App Store search (used Solr) |
| **Netflix** | Early content search (used Solr, later migrated) |
| **eBay** | Product catalog search |
| **AT&T** | Enterprise document search |
| **NASA** | Scientific document and publication search |

---

## Interview Questions

**Q: What is Solr and how does it relate to Elasticsearch?**
> Both are search platforms built on Apache Lucene. Solr is older and better suited to enterprise/static data scenarios. ES is preferred for real-time search and analytics. They are feature-similar but differ in ops complexity, ecosystem fit, and real-time capability.

**Q: When would you choose Solr over Elasticsearch?**
> When working in an Apache/Hadoop ecosystem, dealing with static document-heavy search (legal, scientific), needing complex text analysis pipelines, or maintaining an existing Solr system where migration cost is not justified.

**Q: What do Solr and Elasticsearch have in common?**
> Both are built on Apache Lucene, both use inverted indexes for full-text search, both support faceting, relevance scoring, and horizontal scaling.

**Q: For a new e-commerce project, Solr or Elasticsearch?**
> Elasticsearch. E-commerce needs near real-time product indexing, fuzzy search (typo tolerance), relevance ranking, and analytics — all areas where ES outperforms Solr. Solr would only be preferred if the team was already in the Hadoop ecosystem or had specific static-data enterprise requirements.
