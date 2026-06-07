# 18 — Architecture Case Studies

## Overview

This folder contains six complete real-world database architecture case studies modeled in PostgreSQL, plus a comprehensive interview guide. Each case study includes full schema definitions, ER diagrams, indexing strategies, query patterns, and scaling discussions.

## Files

| File | System | Core Theme |
|------|--------|------------|
| `01_amazon_marketplace_db.md` | Amazon-like Marketplace | Orders, inventory, payments, reviews, multi-seller |
| `02_netflix_streaming_platform.md` | Netflix-like Streaming | Content licensing, watch history, recommendations, billing |
| `03_uber_ridesharing.md` | Uber-like Ridesharing | PostGIS geospatial, real-time location, surge pricing |
| `04_banking_platform.md` | Banking Platform | Double-entry ledger, ACID transfers, fraud detection, compliance |
| `05_saas_product.md` | Multi-Tenant SaaS | Tenant isolation, feature flags, usage metering, RLS |
| `06_messaging_system.md` | Messaging System | LISTEN/NOTIFY, billions of messages, read receipts, presence |
| `07_architecture_interview_guide.md` | Interview Guide | 30+ questions with detailed answers, key numbers, pitfalls |

## Key Concepts Covered

### Schema Design Patterns
- Multi-tenant isolation (Row-Level Security, org_id everywhere)
- Double-entry bookkeeping (banking ledger)
- Event sourcing and audit trails
- Hierarchical data (categories with ltree)
- Polymorphic relationships
- Soft deletes vs. hard deletes

### Indexing Techniques
- B-Tree composite indexes for range + equality
- GIN indexes for full-text search and JSONB
- GiST indexes for geospatial (PostGIS) and exclusion constraints
- Partial indexes for sparse conditions
- INCLUDE columns for index-only scans
- BRIN indexes for append-only time-series

### Scaling Strategies Covered
| Scale Threshold | Technique |
|----------------|-----------|
| > 1GB tables | Proper indexing, VACUUM tuning |
| > 10GB tables | Partitioning (range by date) |
| > 100GB / high write QPS | Read replicas, connection pooling |
| > 1TB | Functional decomposition (separate DBs by domain) |
| > 10TB | Horizontal sharding (Citus, manual) |
| > 100TB | Hybrid storage (PostgreSQL + Cassandra/DynamoDB) |

### PostgreSQL Features Used
- `PARTITION BY RANGE` — time-series data management
- `PostGIS` — geospatial queries and indexing
- `LISTEN/NOTIFY` — real-time push notifications
- `UNLOGGED TABLE` — ephemeral high-write data
- `INSERT ... ON CONFLICT` — upserts and idempotency
- `WITH RECURSIVE` — hierarchical data traversal
- `EXCLUDE USING GIST` — no-overlap constraints
- `Row-Level Security` — multi-tenant data isolation
- `GENERATED ALWAYS AS` — computed columns
- `TSVECTOR` + `GIN` — full-text search
- `pg_notify` — real-time events from triggers

## How to Study This Material

### For Schema Design Interviews
1. Read one case study end-to-end.
2. Close the document and try to draw the ER diagram from memory.
3. Write the CREATE TABLE statements for the 3 most important tables.
4. Identify the top 3 indexing decisions and explain why.
5. Describe at what scale each table needs partitioning.

### For System Design Interviews
1. Start with `07_architecture_interview_guide.md` for the framework.
2. Study the case study closest to the system you're designing.
3. Review `21_System_Design/08_system_design_framework.md` for presentation tips.

### Key Patterns to Memorize
- **Idempotency**: `INSERT ... ON CONFLICT (idempotency_key) DO NOTHING`
- **Optimistic locking**: `WHERE version = $read_version`, check rowcount
- **Consistent locking order**: Lock lower-ID row first to prevent deadlocks
- **Cursor pagination**: `WHERE created_at < $cursor ORDER BY created_at DESC`
- **Quota enforcement**: Redis counter + async PostgreSQL write
- **SKIP LOCKED**: Job queue pattern for parallel workers

## Capacity Estimation Quick Reference

| System | Users | Daily TXs | Annual Storage |
|--------|-------|-----------|----------------|
| Marketplace | 300M | 3M orders | ~8TB hot |
| Streaming | 250M subscribers | 200M watch events | 10TB+ |
| Ridesharing | 100M riders | 15M trips | ~5TB |
| Banking | 10M customers | 500M transactions | ~10TB regulated |
| SaaS | 5M users / 100K orgs | 1B API calls | ~20TB |
| Messaging | 500M users | 60B messages | 4PB/yr (hybrid) |

## Prerequisites

- Chapter 4: Transactions and ACID
- Chapter 5: Table Partitioning
- Chapter 6: Isolation Levels
- Chapter 8: Row-Level Security
- Chapter 9: Full-Text Search
- Chapter 10: LISTEN/NOTIFY
- Chapter 11: PostGIS Extension

## Next Steps

After completing this folder, proceed to:
- `21_System_Design/` — end-to-end system design documents
- Practice the questions in `07_architecture_interview_guide.md`
- Build one of these schemas locally and run the query examples
