# 22: PostgreSQL Projects

## Real-World Projects from Beginner to Advanced

These projects bridge the gap between learning SQL syntax and building production-grade database systems. Each project is a complete, standalone application schema with realistic data, 20+ queries, stored procedures, triggers, and performance optimization guidance.

---

## Project Index

### Beginner Projects (Weeks 1-3)

| # | Project | Core Skills | Estimated Time |
|---|---------|-------------|----------------|
| 01 | [Library Management System](01_library_management_system.md) | Basic schema, CRUD, JOINs, aggregations, simple triggers | 1 week |
| 02 | [Student Management System](02_student_management_system.md) | Recursive CTEs, weighted GPA, domains, many-to-many | 1 week |
| 03 | [Inventory System](03_inventory_system.md) | Ledger pattern, materialized views, moving average cost, UPSERT | 1 week |

### Intermediate Projects (Weeks 4-7)

| # | Project | Core Skills | Estimated Time |
|---|---------|-------------|----------------|
| 04 | [E-Commerce Backend](04_ecommerce_backend.md) | JSONB, UUID, state machine, LATERAL joins, cohort analysis | 2 weeks |
| 05 | [CRM System](05_crm_system.md) | SCD history, soft deletes, funnel analysis, JSONB custom fields | 2 weeks |
| 06 | [Blogging Platform](06_blogging_platform.md) | FTS, partitioning, generated columns, polymorphic reactions | 2 weeks |

### Advanced Projects (Weeks 8-13)

| # | Project | Core Skills | Estimated Time |
|---|---------|-------------|----------------|
| 07 | [Banking System](07_banking_system.md) | Double-entry ledger, SERIALIZABLE, RLS, fraud detection, interest | 3 weeks |
| 08 | [Ride-Sharing Backend](08_ride_sharing_backend.md) | PostGIS, KNN search, surge pricing, LISTEN/NOTIFY, geofencing | 3 weeks |
| 09 | [Event Streaming Platform](09_event_streaming_platform.md) | Append-only partitioning, funnel analysis, A/B testing, cohort retention | 3 weeks |

---

## How to Use These Projects

### Recommended Learning Path

```
BEGINNER
01 Library  -->  02 Student  -->  03 Inventory
      |                                |
      v                                v
INTERMEDIATE
04 E-Commerce --> 05 CRM --> 06 Blog
      |
      v
ADVANCED
07 Banking --> 08 Ride-Sharing --> 09 Event Platform
```

### For Each Project

1. **Read the overview** — understand what you're building and why.
2. **Draw the ER diagram** on paper before looking at the ASCII version.
3. **Create the schema** from scratch — don't copy-paste. Type it.
4. **Load the sample data** and verify with SELECT queries.
5. **Run queries 1-10** and understand the output.
6. **Implement the stored functions** and test edge cases.
7. **Add the triggers** and verify they fire correctly.
8. **Run EXPLAIN ANALYZE** on the 3 most complex queries.
9. **Pick 2 Extension Challenges** and implement them.

---

## Skills Matrix

| Skill | Project(s) |
|-------|-----------|
| Basic schema design | 01, 02, 03 |
| CHECK / UNIQUE / FK constraints | 01, 02, 03, 04 |
| PL/pgSQL functions | 01, 02, 03, 04, 05, 06 |
| Triggers | All projects |
| Window functions | 01, 02, 03, 04, 05, 06, 07, 08, 09 |
| Recursive CTEs | 02, 05 |
| JSONB | 04, 05, 06, 08, 09 |
| Full-text search | 01, 06 |
| Table partitioning | 06, 07, 08, 09 |
| PostGIS / geospatial | 08 |
| Row-Level Security | 07 |
| SERIALIZABLE transactions | 07 |
| Materialized views | 03, 06, 09 |
| Double-entry bookkeeping | 07 |
| A/B testing | 09 |
| Funnel analysis | 09 |
| Cohort retention | 04, 09 |

---

## Prerequisites

Before starting these projects, ensure you have completed:

- Chapters 01-06: SQL fundamentals through database design
- Chapter 07: Indexes (required for optimization sections)
- Chapter 09: Transactions (required for banking and inventory)
- Chapter 11: Advanced PostgreSQL features (required for projects 06-09)

---

## Setup

```sql
-- Create a dedicated database for projects
CREATE DATABASE pg_projects;

-- Connect and create schemas
\c pg_projects

-- Each project uses its own schema
-- library, sms, inv, shop, crm, blog, bank, rides, analytics

-- Enable required extensions
CREATE EXTENSION IF NOT EXISTS pg_trgm;
CREATE EXTENSION IF NOT EXISTS pgcrypto;
CREATE EXTENSION IF NOT EXISTS postgis;  -- Project 08 only
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
```

---

## Evaluation Criteria

For each project, assess yourself against:

| Criterion | Basic | Proficient | Expert |
|-----------|-------|-----------|--------|
| Schema design | Tables exist | Proper constraints | Optimized + normalized |
| Query correctness | Returns data | Correct results | Edge cases handled |
| Function quality | Works | Error handling | Performance aware |
| Index usage | No indexes | Basic indexes | Partial + covering |
| EXPLAIN usage | Never used | Used once | Used to optimize |
| Extension challenges | 0 completed | 2 completed | 4+ completed |

---

## Connecting to the Interview Prep

Each project maps to common interview question categories:

| Interview Topic | Covered In |
|-----------------|-----------|
| "Design a database for X" | All projects |
| "Write a query for Y analytics" | 04, 05, 09 |
| "How would you handle concurrency?" | 07, 08 |
| "Explain your indexing strategy" | All projects |
| "How would you partition this table?" | 06, 07, 08, 09 |
| "What's your approach to data integrity?" | 07 (banking) |
| "How do you handle soft deletes?" | 05, 06 |
| "Explain your caching strategy" | 03, 06, 09 |
