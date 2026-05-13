# 17f — Graph Databases (Neo4j, AWS Neptune)

## Key Concepts

| Term | Definition |
|------|------------|
| Graph DB | Database where relationships between entities are first-class citizens |
| Node | An entity in the graph (person, product, city) |
| Edge | A relationship between two nodes (KNOWS, BOUGHT, LIVES_IN) |
| Property | Attribute on a node or edge (name: "Anil", since: "2020") |
| Cypher | Neo4j's query language — pattern-based, visually describes graph shapes |
| Gremlin | Graph traversal query language used by AWS Neptune |
| Traversal | Following edges from node to node through the graph |
| Hop | One step along an edge from one node to another |

---

## How It Works

### Graph Structure
```
(Anil) --[KNOWS]--> (Sara) --[KNOWS]--> (Raj)
  |                                        |
[LIVES_IN]                            [WORKS_AT]
  |                                        |
(Mumbai)                             (TechCorp)
```

### Why Graph DBs Are Fast for Relationships
1. Relationships stored as **direct pointers** between nodes
2. Traversal follows pointers — **O(1) per hop**
3. Total graph size doesn't affect hop speed
4. Relational DB alternative: each hop = another JOIN → cost multiplies exponentially

### Cypher Query Example (Neo4j)
```cypher
-- Find friends of Anil who live in Mumbai
MATCH (a:Person {name: "Anil"})-[:KNOWS]->(friend)-[:LIVES_IN]->(c:City {name: "Mumbai"})
RETURN friend.name
```
You write the pattern you want to find — highly readable.

---

## Neo4j vs AWS Neptune

| Feature | Neo4j | AWS Neptune |
|---|---|---|
| Query language | Cypher | Gremlin / SPARQL |
| Hosting | Self-managed or Neo4j Aura (cloud) | Fully managed AWS |
| Best for | Graph-first apps, rich analytics | AWS ecosystem, enterprise |
| Control | More flexibility | Less ops overhead |
| Integration | Standalone | Native AWS (IAM, VPC, CloudWatch) |

---

## Industry Examples

| Company | Use Case |
|---|---|
| LinkedIn | "People You May Know" — traverse connection graph 2 hops out |
| Amazon | Recommendation engine — "users who bought X also bought Y" |
| Google | Knowledge Graph — powers search result relationships and info panels |
| Banks / FinTech | Fraud detection — find rings of accounts sharing same IP, device, phone |
| NASA | Tracks dependencies between systems and components |

---

## When to Use

**Use Graph DBs when data is fundamentally about relationships:**
- Social networks — friend recommendations, connection degrees
- Fraud detection — spotting linked accounts, shared IPs/devices
- Knowledge graphs — entity relationships for search or AI
- Recommendation engines — user-item-behavior graphs
- Network/IT topology — infrastructure dependency mapping

**Avoid when:**
- Simple CRUD operations with minimal relationships
- High-volume transactional workloads (relational DB is better)
- Relationships between entities are rare or shallow

---

## Interview Questions

**Q: Why use a graph DB instead of a relational DB for relationship queries?**
A: Relational DBs use JOINs that get exponentially expensive with each additional hop. Graph DBs store relationships as direct pointers — traversal is O(1) per hop regardless of graph size.

**Q: Give a real-world use case for a graph database.**
A: Fraud detection — banks traverse the graph to find accounts sharing the same phone number, device, or IP address. These fraud rings are invisible in relational queries but trivial to spot as graph patterns.

**Q: What is Cypher?**
A: Neo4j's query language. You write patterns that visually describe the graph shape you're looking for, making relationship queries intuitive to read and write.

**Q: When would you choose AWS Neptune over Neo4j?**
A: When already on AWS and you want a fully managed solution — trading some flexibility for zero infrastructure overhead.

**Q: What's the difference between GraphQL and a Graph Database?**
A: GraphQL is an API query language for fetching structured data from any backend. A Graph Database (like Neo4j) is a storage engine built around nodes and relationships. They are completely unrelated despite the similar name.
