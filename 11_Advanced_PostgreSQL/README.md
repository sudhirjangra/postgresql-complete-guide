# Module 11: Advanced PostgreSQL

## Overview

This module covers the features that distinguish PostgreSQL from other relational databases and from basic SQL usage. Every topic is production-relevant: engineers at companies like Shopify, GitLab, Stripe, and Notion rely on these features for their core data infrastructure. Completing this module unlocks the ability to design sophisticated PostgreSQL-native solutions that outperform comparable application-level implementations.

## Prerequisites
- Strong SQL foundations (Modules 1–4)
- PostgreSQL Core familiarity (Module 5)
- Database Design (Module 6)
- Indexes and Query Optimization (Modules 7–8)
- Transactions and Concurrency (Module 9)
- PostgreSQL Internals (Module 10)

## Module Contents

| File | Topic | Key Concepts |
|------|-------|--------------|
| [01_jsonb_mastery.md](01_jsonb_mastery.md) | JSONB deep dive | Operators, GIN indexes, JSONPath, jsonb_set |
| [02_full_text_search.md](02_full_text_search.md) | Full-text search | tsvector, tsquery, GIN/GiST, ranking |
| [03_partitioning.md](03_partitioning.md) | Table partitioning | Range, list, hash; partition pruning; pg_partman |
| [04_materialized_views.md](04_materialized_views.md) | Materialized views | REFRESH CONCURRENTLY, incremental refresh, patterns |
| [05_extensions_overview.md](05_extensions_overview.md) | Extensions ecosystem | pg_stat_statements, uuid-ossp, PostGIS, TimescaleDB |
| [06_logical_decoding.md](06_logical_decoding.md) | Logical decoding | Replication slots, pg_logical, wal2json, Debezium |
| [07_foreign_data_wrappers.md](07_foreign_data_wrappers.md) | Foreign Data Wrappers | postgres_fdw, file_fdw, import foreign schema |
| [08_listen_notify.md](08_listen_notify.md) | LISTEN / NOTIFY | Async messaging, pubsub patterns, real-time apps |
| [09_stored_procedures_functions.md](09_stored_procedures_functions.md) | Stored procedures & functions | PL/pgSQL, volatility, SECURITY DEFINER, cursors |
| [10_triggers_and_rules.md](10_triggers_and_rules.md) | Triggers and rules | BEFORE/AFTER/INSTEAD OF, audit patterns, row versioning |

## Learning Path

```
Foundation:    01 (JSONB) → 02 (FTS) → 03 (Partitioning)
Intermediate:  04 (MatViews) → 05 (Extensions) → 09 (Functions)
Advanced:      06 (Logical Decoding) → 07 (FDW) → 08 (Listen/Notify) → 10 (Triggers)
```

## Module Architecture at a Glance

```
PostgreSQL Advanced Feature Stack
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
QUERY LAYER      │  JSONB operators  │  Full-text search  │  FDW queries
─────────────────┼───────────────────┼────────────────────┼───────────────
COMPUTE LAYER    │  Stored functions │  Triggers          │  Rules
─────────────────┼───────────────────┼────────────────────┼───────────────
STORAGE LAYER    │  Partitioned tables│  Materialized views│  TOAST / JSONB
─────────────────┼───────────────────┼────────────────────┼───────────────
CHANGE CAPTURE   │  Logical decoding │  LISTEN/NOTIFY     │  Audit triggers
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Quick Reference: Feature Selection Guide

| Use Case | Feature |
|----------|---------|
| Semi-structured data, flexible schemas | JSONB |
| Search by keywords with ranking | Full-text search |
| Very large tables (time series, logs, events) | Partitioning |
| Pre-computed expensive aggregations | Materialized views |
| Streaming changes to Kafka / Debezium | Logical decoding |
| Querying remote databases transparently | Foreign Data Wrappers |
| Real-time push notifications from DB | LISTEN / NOTIFY |
| Reusable business logic in the DB | Stored functions |
| Automatic DML side effects | Triggers |
| Audit logging and row versioning | Triggers |

## Key Skills Gained

After completing this module you will be able to:
- Design schemas that blend relational and document (JSONB) columns effectively
- Build full-text search with PostgreSQL without an external search engine
- Partition multi-billion-row tables for query pruning and maintenance efficiency
- Implement audit logging, optimistic locking, and soft-delete using triggers
- Stream database changes to external systems via logical decoding
- Use FDWs to query Postgres, CSV files, and other data sources in a single SQL statement
- Build real-time notification systems using LISTEN/NOTIFY

## Related Modules
- **Module 12: Production PostgreSQL** — Operating these features in production (monitoring, tuning)
- **Module 13: Replication & HA** — Logical decoding is the foundation of logical replication
- **Module 15: Backup & Recovery** — Logical decoding slots must be managed to avoid WAL accumulation
- **Module 16: PostgreSQL for Data Engineering** — FDWs, triggers, and logical decoding power ETL pipelines
