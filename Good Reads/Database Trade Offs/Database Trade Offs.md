# **Database Tradeoff Document**  
*Comparing MongoDB, DynamoDB, MySQL, PostgreSQL, and Cassandra*  

This document outlines the key tradeoffs between these databases based on **data model, scalability, performance, consistency, and use cases**.  

---

## 1. Data Model & Flexibility

| Database     | Data Model          | Schema Flexibility | Query Language | Best For |
|-------------|--------------------|------------------|--------------|---------|
| **MongoDB** | Document (JSON-like) | Dynamic schema | MongoDB Query Language (MQL) | Semi-structured data, fast iterations |
| **DynamoDB** | Key-Value + Document | Semi-structured (limited schema) | AWS SDK/PartiQL | Simple lookups, serverless apps |
| **MySQL** | Relational (Tables) | Fixed schema (but supports JSON) | SQL | Structured data, transactions |
| **PostgreSQL** | Relational + JSON/NoSQL | Hybrid (structured + JSONB) | SQL (with extensions) | Complex queries, analytics, GIS |
| **Cassandra** | Wide-column (NoSQL) | Schema-flexible (column families) | CQL (SQL-like) | Time-series, high-write scalability |

**Tradeoff:**  
- **Need rigid structure?** → MySQL/PostgreSQL.  
- **Need flexibility?** → MongoDB/Cassandra.  
- **Simple key-value access?** → DynamoDB.  
- **Time-series or wide-column?** → Cassandra.  

---

## 2. Scalability & Performance

| Database     | Horizontal Scaling | Read Performance | Write Performance | Latency |
|-------------|------------------|----------------|-----------------|--------|
| **MongoDB** | ✅ (Sharding) | High (indexed queries) | High (w/ write concerns) | Low-moderate |
| **DynamoDB** | ✅ (Auto-scaling) | Very high (SSD-backed) | Very high (single-digit ms) | Ultra-low |
| **MySQL** | ❌ (Limited, needs replicas) | High (indexed) | Moderate (row-level locks) | Low |
| **PostgreSQL** | ✅ (Logical replication, partitioning) | Very high (MVCC) | High (but slower than MySQL) | Low |
| **Cassandra** | ✅ (Distributed, linear scalability) | High (tunable consistency) | Extremely high (append-optimized) | Low (if tuned) |

**Tradeoff:**  
- **Need auto-scaling?** → DynamoDB/Cassandra.  
- **Need high write throughput?** → Cassandra > DynamoDB > MongoDB.  
- **Need strong read performance?** → PostgreSQL > MongoDB > Cassandra (if indexed).  

---

## 3. Consistency & Transactions

| Database       | Consistency Model               | Transactions               | ACID Compliance           |
| -------------- | ------------------------------- | -------------------------- | ------------------------- |
| **MongoDB**    | Tunable (strong or eventual)    | Multi-document (v4.0+)     | ✅ (with replica sets)     |
| **DynamoDB**   | Tunable (eventual/strong)       | Single-item (no multi-row) | ❌ (only single-item ACID) |
| **MySQL**      | Strong (configurable isolation) | Full multi-row             | ✅                         |
| **PostgreSQL** | Strong (MVCC)                   | Full multi-row + nested    | ✅                         |
| **Cassandra**  | Tunable (eventual → QUORUM)     | No multi-row transactions  | ❌ (AP system)             |

**Tradeoff:**  
- **Need ACID transactions?** → PostgreSQL/MySQL.  
- **Need eventual consistency for global apps?** → Cassandra/DynamoDB.  
- **Balance between NoSQL & transactions?** → MongoDB (4.0+).  

---

## 4. Use Case Recommendations

| Database     | Best For | Worst For |
|-------------|---------|----------|
| **MongoDB** | - Rapid prototyping<br>- Content management<br>- Real-time analytics | - Complex joins<br>- Heavy transactions |
| **DynamoDB** | - Serverless apps<br>- High-scale key-value<br>- Ad tech, gaming | - Complex queries<br>- Multi-item transactions |
| **MySQL** | - Web apps (LAMP stack)<br>- Simple transactions<br>- Read replicas | - Unstructured data<br>- High write contention |
| **PostgreSQL** | - Enterprise apps<br>- GIS/spatial data<br>- Financial systems | - Simple key-value<br>- Ultra-low-latency writes |
| **Cassandra** | - Time-series data (IoT, logs)<br>- High-write scalability<br>- Global distribution | - Strong consistency needs<br>- Complex transactions |

---

## 5. Decision Summary

| **Choose...**  | **If you need...**                                       | **Avoid if...**                             |
| -------------- | -------------------------------------------------------- | ------------------------------------------- |
| **MongoDB**    | Flexible schema, fast iteration, document storage        | Strict transactions, complex joins          |
| **DynamoDB**   | Serverless, auto-scaling, low-latency KV store           | Complex queries, multi-row transactions     |
| **MySQL**      | Simple relational data, high-speed OLTP                  | JSON-heavy, unstructured needs              |
| **PostgreSQL** | Advanced SQL, analytics, ACID compliance                 | Minimalist setup, key-value only            |
| **Cassandra**  | High-write scalability, time-series, global distribution | ACID transactions, low-latency strong reads |

---
### Final Recommendation
- **For structured data + transactions** → PostgreSQL > MySQL.  
- **For flexible schema + scalability** → MongoDB > Cassandra.  
- **For serverless + low-latency KV** → DynamoDB.  
- **For high-write, time-series, or global scale** → Cassandra. 

# Resources
![Youtube|1000](https://www.youtube.com/watch?v=6GebEqt6Ynk)