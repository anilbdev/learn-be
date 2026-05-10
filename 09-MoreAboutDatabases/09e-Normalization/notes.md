# 09e — Normalization

## Key Concepts

| Term | Definition |
|---|---|
| Normalization | Organizing a DB to reduce redundancy by splitting data into related tables |
| Denormalization | Intentionally duplicating data to avoid expensive joins — for performance |
| Redundancy | Same data stored in multiple places — leads to inconsistency |
| Data Integrity | Data stays accurate and consistent across the database |
| Transitive Dependency | Non-key column depends on another non-key column (violates 3NF) |
| Partial Dependency | Non-key column depends on part of a composite key (violates 2NF) |

---

## Normal Forms

| Normal Form | Rule | Fixes |
|---|---|---|
| **1NF** | Atomic values — no lists or repeating groups in a cell | Repeating groups |
| **2NF** | No partial dependency on composite key | Column depends on part of key |
| **3NF** | No transitive dependency between non-key columns | Non-key depends on non-key |

---

## Examples

**1NF Violation:**
| OrderID | Products |
|---|---|
| 1 | Laptop, Mouse |

Fix: one row per product.

---

**3NF Violation (Transitive Dependency):**
| OrderID | ZipCode | City |
|---|---|---|
| 1 | 10001 | New York |

City depends on ZipCode, not OrderID.

Fix: move ZipCode + City to a separate ZipCodes table. Keep ZipCode on Orders as foreign key.

---

## Normalization vs Denormalization

| | Normalization | Denormalization |
|---|---|---|
| Goal | Reduce redundancy | Improve read performance |
| Joins | More joins needed | Fewer joins needed |
| Use case | Transactional systems (OLTP) | Reporting / analytics (OLAP) |
| Example | Orders → Users via foreign key | Store customer_name on orders table |

---

## Industry Examples

- **E-commerce DBs** — normalized: Products, Orders, Customers in separate tables
- **Amazon Redshift / BigQuery** — denormalized for fast analytics queries
- **Banking systems** — strictly normalized to avoid data inconsistency

---

## Interview Questions

| Question | One-Line Answer |
|---|---|
| What is normalization? | Organizing a DB to reduce redundancy by splitting data into related tables |
| What is 1NF? | Each column holds atomic values — no lists or repeating groups |
| 2NF vs 3NF? | 2NF removes partial key dependency; 3NF removes transitive dependency between non-key columns |
| What is denormalization? | Intentionally duplicating data to avoid joins — used in read-heavy or analytics systems |
| What problems does normalization solve? | Redundancy, update anomalies, data inconsistency |
