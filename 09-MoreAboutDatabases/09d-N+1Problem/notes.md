# 09d — The N+1 Problem

## Key Concepts

| Term | Definition |
|---|---|
| N+1 Problem | 1 query fetches a list, then N more queries fire for each item's related data |
| Lazy Loading | Related data fetched on demand — triggers a query each time you access a relationship |
| Eager Loading | Related data fetched upfront in the same query — eliminates N+1 |
| DataLoader | Batching pattern (used in GraphQL) — groups N queries into 1 |

---

## How It Happens

```
Query 1:  SELECT * FROM orders         → returns 100 orders
Query 2:  SELECT * FROM users WHERE id = 1
Query 3:  SELECT * FROM users WHERE id = 2
...
Query 101: SELECT * FROM users WHERE id = 100

Total: 101 queries instead of 1
```

---

## Why ORMs Cause It

- ORMs default to **lazy loading**
- Accessing a relationship inside a loop triggers a new query per iteration

```csharp
// BAD — N+1
var orders = context.Orders.ToList();
foreach (var order in orders) {
    Console.WriteLine(order.User.Name); // new query each iteration
}

// GOOD — eager loading
var orders = context.Orders.Include(o => o.User).ToList(); // 1 query
```

---

## How to Fix

| Fix | How |
|---|---|
| Eager loading | `.Include()` in EF Core |
| Raw SQL JOIN | Write the join yourself |
| DataLoader | Batch queries — used in GraphQL APIs |
| Select only needed fields | Avoid loading full objects unnecessarily |

---

## How to Detect

- Query count grows proportionally with list size
- EF Core query logging — see repeated similar queries
- MiniProfiler — shows query count per request
- SQL Profiler / Datadog APM — spots repeated patterns in production

---

## Industry Examples

- **Shopify** — N+1 is their most common Rails performance issue; use `bullet` gem to detect it
- **GitHub** — uses DataLoader in GraphQL API to batch relationship queries
- **Any list page** — orders + customer names, posts + author info = classic N+1 trap

---

## Interview Questions

| Question | One-Line Answer |
|---|---|
| What is the N+1 problem? | 1 query for a list + N queries for each item's related data — instead of 1 join |
| Why do ORMs cause it? | Lazy loading fetches related data on demand — one query per loop iteration |
| How do you fix it? | Eager loading (`.Include()`), explicit JOIN, or DataLoader batching |
| How do you detect it? | Query logging, MiniProfiler, APM tools — look for repeated similar queries |
| Lazy vs eager loading? | Lazy = on demand (causes N+1); Eager = upfront in same query (prevents N+1) |
