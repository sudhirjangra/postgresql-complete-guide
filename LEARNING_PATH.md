# Learning Paths — PostgreSQL Complete Guide

> Choose your path based on your current role, experience level, and goals. Each path is designed to be efficient — you will cover what matters most for your context without wasting time on irrelevant material.

---

## Table of Contents

1. [How to Choose Your Path](#1-how-to-choose-your-path)
2. [Path A — Complete Beginner](#2-path-a--complete-beginner)
3. [Path B — Backend Engineer](#3-path-b--backend-engineer)
4. [Path C — Data Engineer](#4-path-c--data-engineer)
5. [Path D — Analytics Engineer](#5-path-d--analytics-engineer)
6. [Path E — Database Engineer / DBA](#6-path-e--database-engineer--dba)
7. [Path F — Interview-Only Fast Track](#7-path-f--interview-only-fast-track)
8. [Cross-Path Reference Table](#8-cross-path-reference-table)
9. [Chapter Dependency Map](#9-chapter-dependency-map)
10. [Milestone Definitions](#10-milestone-definitions)

---

## 1. How to Choose Your Path

Answer these questions:

**Q1: Have you written SQL before?**
- No → Path A (Complete Beginner)
- Yes, basics only → Go to Q2

**Q2: What is your primary goal?**
- Build production backend services with PostgreSQL → Path B
- Build data pipelines and ETL workflows → Path C
- Build analytics queries and reports → Path D
- Administer, tune, and operate PostgreSQL → Path E
- Pass an interview in the next 3–4 weeks → Path F

**Q3: How much time per week can you commit?**
- 5–10 hours/week → Double the estimated durations
- 10–15 hours/week → Use the given estimates
- 15–20+ hours/week → Reduce estimates by 25%

---

## 2. Path A — Complete Beginner

**Target audience:** No prior SQL or database experience. Starting from absolute zero.

**Estimated duration:** 16–20 weeks at 10 hours/week

**End goal:** Junior-to-mid level PostgreSQL proficiency. Ready for junior backend, junior data, or junior analyst roles.

---

### Phase 1 — Foundations (Weeks 1–3)

**Learning objectives:**
- Understand what relational databases are and why they exist
- Write basic SELECT queries with filtering and sorting
- Understand the client/server model and connect to PostgreSQL

| Week | Chapter | Topic | Hours |
|------|---------|-------|-------|
| 1 | 01_Fundamentals | Relational model, SQL history, psql basics | 10h |
| 2 | 02_SQL_Basics (Part 1) | SELECT, WHERE, ORDER BY, LIMIT, basic functions | 10h |
| 3 | 02_SQL_Basics (Part 2) | JOINs, GROUP BY, HAVING, aggregates | 10h |

**Milestone 1 checkpoint:** Can you write a query that joins three tables, filters by date range, groups by category, and returns the top 5 results? If yes, proceed.

**Mini-project:** Query a sample e-commerce database (provided in `22_Projects/project_01`) to answer 10 business questions.

---

### Phase 2 — Intermediate SQL (Weeks 4–6)

**Learning objectives:**
- Use subqueries and CTEs to break down complex problems
- Apply window functions for ranking and running totals
- Handle NULL values correctly

| Week | Chapter | Topic | Hours |
|------|---------|-------|-------|
| 4 | 03_Intermediate_SQL (Part 1) | Subqueries (scalar, correlated), CTEs | 10h |
| 5 | 03_Intermediate_SQL (Part 2) | Window functions: ROW_NUMBER, RANK, LEAD, LAG | 10h |
| 6 | 03_Intermediate_SQL (Part 3) | Window frames, PARTITION BY, advanced patterns | 10h |

**Milestone 2 checkpoint:** Can you calculate a 7-day rolling average using window functions? Can you write a recursive CTE? If yes, proceed.

**Mini-project:** Build a cohort retention analysis from a user events table.

---

### Phase 3 — Advanced SQL (Week 7)

**Learning objectives:**
- Solve complex analytical problems with advanced SQL patterns
- Write recursive queries for hierarchical data

| Week | Chapter | Topic | Hours |
|------|---------|-------|-------|
| 7 | 04_Advanced_SQL | Recursive CTEs, lateral joins, gaps and islands, pivot | 10h |

---

### Phase 4 — PostgreSQL Core (Weeks 8–9)

**Learning objectives:**
- Understand PostgreSQL's rich type system
- Use JSONB, arrays, and PostgreSQL-specific features
- Manage schemas and extensions

| Week | Chapter | Topic | Hours |
|------|---------|-------|-------|
| 8 | 05_PostgreSQL_Core (Part 1) | Data types, type casting, arrays, JSONB | 10h |
| 9 | 05_PostgreSQL_Core (Part 2) | Extensions, schemas, search_path, pg_catalog | 10h |

---

### Phase 5 — Database Design (Week 10)

**Learning objectives:**
- Design normalized schemas from business requirements
- Apply constraints correctly

| Week | Chapter | Topic | Hours |
|------|---------|-------|-------|
| 10 | 06_Database_Design | Normalization, ERD, constraints, naming | 10h |

**Milestone 3 checkpoint:** Can you design a schema for a simple SaaS application (users, organizations, subscriptions, invoices) with proper normalization and constraints?

---

### Phase 6 — Indexes and Optimization (Weeks 11–13)

**Learning objectives:**
- Understand how B-Tree indexes work
- Read basic EXPLAIN output
- Choose when to add an index

| Week | Chapter | Topic | Hours |
|------|---------|-------|-------|
| 11 | 07_Indexes | B-Tree, partial indexes, expression indexes, GIN | 10h |
| 12 | 08_Query_Optimization (Part 1) | EXPLAIN, EXPLAIN ANALYZE, plan nodes | 10h |
| 13 | 08_Query_Optimization (Part 2) | Statistics, planner settings, fixing slow queries | 10h |

---

### Phase 7 — Transactions and Production Basics (Weeks 14–16)

**Learning objectives:**
- Understand ACID and MVCC
- Know basic isolation levels
- Understand connection pooling

| Week | Chapter | Topic | Hours |
|------|---------|-------|-------|
| 14 | 09_Transactions_Concurrency | ACID, MVCC, isolation levels, locking basics | 10h |
| 15 | 12_Production_PostgreSQL | Config basics, PgBouncer intro, monitoring | 10h |
| 16 | 19_Interview_Questions (Junior section) | Interview prep, Q&A review | 10h |

**Milestone 4 — Graduation checkpoint:** Complete the Junior Interview Questions section in `19_Interview_Questions`. Score at least 80% without notes.

---

### Recommended Resources for Path A

- Chapters 01–09 (core path)
- Chapter 19 (junior questions)
- Chapter 24 (cheat sheets — use throughout)
- Project 1 from `22_Projects`

---

## 3. Path B — Backend Engineer

**Target audience:** Software engineers who build APIs and services. You know programming. You may know basic SQL. You need to use PostgreSQL correctly in production systems.

**Estimated duration:** 8–10 weeks at 10 hours/week

**Prerequisite:** Basic SQL knowledge (can write SELECT, JOIN, GROUP BY). If not, do Path A Phases 1–3 first (3 weeks).

**End goal:** Can design production schemas, write efficient queries, manage connections, handle migrations safely, and debug production database issues.

---

### Week 1 — PostgreSQL Core and Schema Design

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–2 | PostgreSQL data types deep dive | 05_PostgreSQL_Core | 3h |
| 3–4 | JSONB patterns for backend engineers | 05_PostgreSQL_Core, 11_Advanced_PostgreSQL | 3h |
| 5–7 | Schema design for SaaS, multi-tenancy, UUIDs | 06_Database_Design | 4h |

**Focus areas:** When to use JSONB vs normalized, UUID vs serial, schema naming conventions, constraint design.

---

### Week 2 — Indexes for Production

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–2 | B-Tree internals and multi-column ordering | 07_Indexes | 3h |
| 3–4 | GIN for JSONB and arrays, partial indexes | 07_Indexes | 3h |
| 5–7 | Covering indexes, index bloat, CREATE INDEX CONCURRENTLY | 07_Indexes | 4h |

**Focus areas:** Never create an index without understanding why. `CREATE INDEX CONCURRENTLY` for zero-downtime.

---

### Week 3 — Query Optimization

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–3 | EXPLAIN ANALYZE — reading every node type | 08_Query_Optimization | 4h |
| 4–5 | Fixing N+1 in ORM-generated queries | 08_Query_Optimization, 17_Backend_Engineers | 3h |
| 6–7 | pg_stat_statements setup and usage | 08_Query_Optimization | 3h |

---

### Week 4 — Transactions, Locking, and Concurrency

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–2 | ACID guarantees and when they matter | 09_Transactions_Concurrency | 3h |
| 3–4 | MVCC, isolation levels, read phenomena | 09_Transactions_Concurrency | 3h |
| 5–7 | SELECT FOR UPDATE, advisory locks, deadlock prevention | 09_Transactions_Concurrency | 4h |

**Focus areas:** How to implement optimistic locking, how to use advisory locks as a distributed lock, how to avoid deadlocks in application code.

---

### Week 5 — Backend-Specific Patterns

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–2 | Connection pooling: PgBouncer transaction mode | 17_Backend_Engineers | 3h |
| 3–4 | Schema migrations: zero-downtime strategies | 17_Backend_Engineers | 3h |
| 5–7 | LISTEN/NOTIFY, upsert patterns, bulk operations | 17_Backend_Engineers | 4h |

---

### Week 6 — Production Operations

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–2 | postgresql.conf tuning for backend workloads | 12_Production_PostgreSQL | 3h |
| 3–4 | Monitoring: pg_stat_*, slow query log, Grafana | 12_Production_PostgreSQL | 3h |
| 5–7 | Bloat monitoring, VACUUM basics, autovacuum | 12_Production_PostgreSQL, 10_Internals | 4h |

---

### Week 7 — Security

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–3 | Row-level security for multi-tenant apps | 14_Security | 4h |
| 4–5 | Roles, privileges, SECURITY DEFINER | 14_Security | 3h |
| 6–7 | pg_hba.conf, SSL, connection security | 14_Security | 3h |

---

### Week 8 — Architecture and Case Studies

| Day | Topic | Chapter | Hours |
|-----|-------|---------|-------|
| 1–3 | Multi-tenant architecture patterns | 18_Architecture_Case_Studies | 4h |
| 4–5 | Backend system design with PostgreSQL | 21_System_Design | 3h |
| 6–7 | Interview prep for backend roles | 19_Interview_Questions | 3h |

**Backend Engineer Milestones:**
- [ ] Can implement a zero-downtime migration adding an index to a 100M-row table
- [ ] Can explain why `SELECT FOR UPDATE` prevents double-booking
- [ ] Can configure PgBouncer in transaction mode
- [ ] Can read `EXPLAIN ANALYZE` output and identify the bottleneck
- [ ] Can implement row-level security for a multi-tenant application

---

## 4. Path C — Data Engineer

**Target audience:** Engineers who build data pipelines, ETL/ELT workflows, and data infrastructure.

**Estimated duration:** 8–10 weeks at 10 hours/week

**Prerequisite:** Intermediate SQL (can write CTEs and subqueries). Python or Spark experience helpful.

**End goal:** Can design and operate a PostgreSQL-based data platform with partitioning, CDC, FDWs, and efficient bulk operations.

---

### Week 1 — Advanced SQL for Data Engineering

| Topic | Chapter | Hours |
|-------|---------|-------|
| Window functions for analytics | 03_Intermediate_SQL | 5h |
| Recursive CTEs for hierarchical data | 04_Advanced_SQL | 5h |

---

### Week 2 — PostgreSQL Data Types for Data Engineering

| Topic | Chapter | Hours |
|-------|---------|-------|
| Arrays, ranges, JSONB for semi-structured data | 05_PostgreSQL_Core | 4h |
| Full-text search for pipeline metadata | 11_Advanced_PostgreSQL | 3h |
| Custom types and domains | 11_Advanced_PostgreSQL | 3h |

---

### Week 3 — Query Optimization for Analytical Queries

| Topic | Chapter | Hours |
|-------|---------|-------|
| Reading EXPLAIN for large analytical queries | 08_Query_Optimization | 4h |
| Parallel query configuration | 08_Query_Optimization | 3h |
| Statistics and query planner for analytics | 08_Query_Optimization | 3h |

---

### Week 4 — Partitioning

| Topic | Chapter | Hours |
|-------|---------|-------|
| Range partitioning for time-series data | 16_Data_Engineering | 4h |
| List and hash partitioning | 16_Data_Engineering | 3h |
| Partition pruning, maintenance, and gotchas | 16_Data_Engineering | 3h |

**Milestone:** Design a partitioned event table for 1 billion rows. Verify partition pruning in `EXPLAIN`.

---

### Week 5 — Bulk Loading and ETL Patterns

| Topic | Chapter | Hours |
|-------|---------|-------|
| COPY command for bulk loading | 16_Data_Engineering | 3h |
| UNLOGGED tables and staging patterns | 16_Data_Engineering | 3h |
| INSERT ... ON CONFLICT for upserts | 16_Data_Engineering | 2h |
| Incremental loading strategies | 16_Data_Engineering | 2h |

---

### Week 6 — Change Data Capture

| Topic | Chapter | Hours |
|-------|---------|-------|
| Logical replication as CDC | 11_Advanced_PostgreSQL | 3h |
| Publications and subscriptions | 13_Replication_HA | 3h |
| Debezium integration with PostgreSQL | 16_Data_Engineering | 4h |

---

### Week 7 — Foreign Data Wrappers and Federated Queries

| Topic | Chapter | Hours |
|-------|---------|-------|
| postgres_fdw setup and query pushdown | 11_Advanced_PostgreSQL | 4h |
| file_fdw for CSV ingestion | 11_Advanced_PostgreSQL | 2h |
| Performance and limitations of FDWs | 11_Advanced_PostgreSQL | 2h |
| Federated query patterns | 16_Data_Engineering | 2h |

---

### Week 8 — Production Data Platform

| Topic | Chapter | Hours |
|-------|---------|-------|
| Monitoring data platform health | 12_Production_PostgreSQL | 3h |
| VACUUM and maintenance for large tables | 12_Production_PostgreSQL | 3h |
| dbt integration with PostgreSQL | 16_Data_Engineering | 4h |

---

### Week 9 — Internals Relevant to Data Engineering

| Topic | Chapter | Hours |
|-------|---------|-------|
| WAL and how it enables CDC | 10_PostgreSQL_Internals | 3h |
| TOAST and large column storage | 10_PostgreSQL_Internals | 2h |
| Bloat in high-update tables | 10_PostgreSQL_Internals | 2h |
| Checkpoint and WAL settings for bulk loads | 12_Production_PostgreSQL | 3h |

**Data Engineer Milestones:**
- [ ] Can design and implement a partitioned table with automated partition creation
- [ ] Can set up logical replication as a CDC source
- [ ] Can implement a bulk load that processes 10M rows/minute with COPY
- [ ] Can query data across two PostgreSQL instances with FDW
- [ ] Can build an incremental dbt model on a partitioned table

---

## 5. Path D — Analytics Engineer

**Target audience:** Analytics engineers, BI developers, data analysts who write complex SQL and build data models.

**Estimated duration:** 6–8 weeks at 10 hours/week

**Prerequisite:** Basic SQL. Can write SELECT, JOIN, GROUP BY.

**End goal:** Can write world-class analytical SQL, understand query performance, build maintainable data models, and contribute meaningfully to data infrastructure decisions.

---

### Week 1 — Intermediate SQL Mastery

| Topic | Chapter | Hours |
|-------|---------|-------|
| CTEs — composability and performance | 03_Intermediate_SQL | 4h |
| Window functions — all frame types | 03_Intermediate_SQL | 6h |

**Drill:** Write 20 window function queries. Track time. Target: < 3 minutes per query.

---

### Week 2 — Advanced SQL Patterns

| Topic | Chapter | Hours |
|-------|---------|-------|
| Conditional aggregation (FILTER, CASE-WHEN) | 04_Advanced_SQL | 3h |
| Pivot and unpivot | 04_Advanced_SQL | 2h |
| Gaps and islands problems | 04_Advanced_SQL | 2h |
| Date and time patterns for analytics | 04_Advanced_SQL | 3h |

---

### Week 3 — PostgreSQL for Analytics

| Topic | Chapter | Hours |
|-------|---------|-------|
| PostgreSQL-specific aggregate functions | 05_PostgreSQL_Core | 3h |
| JSONB for semi-structured event data | 05_PostgreSQL_Core | 3h |
| Full-text search for search analytics | 11_Advanced_PostgreSQL | 4h |

---

### Week 4 — Query Performance

| Topic | Chapter | Hours |
|-------|---------|-------|
| Reading EXPLAIN ANALYZE for analytical queries | 08_Query_Optimization | 5h |
| Index selection for analytics | 07_Indexes | 5h |

**Focus:** Understand hash join vs merge join vs nested loop. Know when each is appropriate.

---

### Week 5 — Data Modeling for Analytics

| Topic | Chapter | Hours |
|-------|---------|-------|
| Dimensional modeling in PostgreSQL | 06_Database_Design | 3h |
| Materialized views — creation and refresh | 11_Advanced_PostgreSQL | 3h |
| Partitioning for analytical workloads | 16_Data_Engineering | 4h |

---

### Week 6 — dbt and Analytics Tooling

| Topic | Chapter | Hours |
|-------|---------|-------|
| dbt models on PostgreSQL | 16_Data_Engineering | 4h |
| Incremental materialization strategies | 16_Data_Engineering | 3h |
| Monitoring query performance | 12_Production_PostgreSQL | 3h |

---

### Week 7 — Interview Preparation

| Topic | Chapter | Hours |
|-------|---------|-------|
| SQL interview question patterns | 19_Interview_Questions | 4h |
| Analytical SQL tasks | 20_Interview_Tasks | 6h |

**Analytics Engineer Milestones:**
- [ ] Can write a cohort retention analysis using window functions
- [ ] Can build a date spine using generate_series and cross joins
- [ ] Can explain why a materialized view refresh is blocking queries
- [ ] Can read `EXPLAIN ANALYZE` and identify if an index is being used
- [ ] Can implement a slowly changing dimension in PostgreSQL

---

## 6. Path E — Database Engineer / DBA

**Target audience:** Database administrators, infrastructure engineers, SREs who are responsible for operating and scaling PostgreSQL in production.

**Estimated duration:** 12–14 weeks at 10 hours/week

**Prerequisite:** Comfortable with SQL. Some Linux administration experience.

**End goal:** Can design, deploy, operate, tune, secure, and recover production PostgreSQL systems at scale. Ready for senior/staff DBA roles.

---

### Phase 1 — Deep SQL and PostgreSQL Fundamentals (Weeks 1–2)

| Week | Topic | Chapter |
|------|-------|---------|
| 1 | Advanced SQL — all patterns | 03, 04 |
| 2 | PostgreSQL core, types, extensions | 05 |

---

### Phase 2 — Schema Design and Indexing (Weeks 3–4)

| Week | Topic | Chapter |
|------|-------|---------|
| 3 | Schema design, normalization, constraints | 06 |
| 4 | All index types — internals and selection | 07 |

**Milestone:** Can audit an existing schema and produce a written report of normalization issues, missing indexes, and redundant indexes.

---

### Phase 3 — Query Optimization (Weeks 5–6)

| Week | Topic | Chapter |
|------|-------|---------|
| 5 | Query planner, statistics, EXPLAIN deep dive | 08 |
| 6 | pg_stat_statements, auto_explain, slow query workflow | 08, 12 |

**Milestone:** Given a slow query, produce a written optimization plan including: current plan analysis, root cause, proposed fix, before/after timing.

---

### Phase 4 — Transactions and Internals (Weeks 7–8)

| Week | Topic | Chapter |
|------|-------|---------|
| 7 | MVCC, isolation levels, locking, deadlocks | 09 |
| 8 | WAL, heap, TOAST, page structure, shared buffers | 10 |

**Milestone:** Can draw the lifecycle of a single-row UPDATE from client to WAL to shared buffer to heap page to visibility map. Can explain xmin/xmax semantics.

---

### Phase 5 — VACUUM and Autovacuum (Weeks 9)

| Week | Topic | Chapter |
|------|-------|---------|
| 9 | VACUUM internals, autovacuum tuning, bloat management | 10, 12 |

**Milestone:** Can identify a table with autovacuum lag, diagnose the cause, and implement a fix. Can calculate the autovacuum trigger threshold.

---

### Phase 6 — Replication and HA (Weeks 10–11)

| Week | Topic | Chapter |
|------|-------|---------|
| 10 | Streaming replication, physical standby, replication slots | 13 |
| 11 | Logical replication, Patroni, PgBouncer HA setup | 13 |

**Milestone:** Can deploy a Patroni cluster with three nodes (primary + two standbys) using etcd as DCS. Can perform a controlled switchover without data loss.

---

### Phase 7 — Backup, Recovery, and Security (Weeks 12–13)

| Week | Topic | Chapter |
|------|-------|---------|
| 12 | PITR, pgBackRest, backup verification | 15 |
| 13 | RLS, pgaudit, roles, pg_hba.conf hardening | 14 |

---

### Phase 8 — Interview Preparation (Week 14)

| Week | Topic | Chapter |
|------|-------|---------|
| 14 | Senior/Staff interview Q&A, system design | 19, 21 |

**Database Engineer Milestones:**
- [ ] Can explain MVCC without notes
- [ ] Can read a full `EXPLAIN (ANALYZE, BUFFERS)` output
- [ ] Can configure autovacuum per-table overrides
- [ ] Can set up streaming replication from scratch
- [ ] Can perform a PITR restore and verify data integrity
- [ ] Can implement row-level security for a multi-tenant schema
- [ ] Can deploy PgBouncer in transaction mode
- [ ] Can design a Patroni HA cluster in a system design interview

---

## 7. Path F — Interview-Only Fast Track

**Target audience:** Engineers with existing SQL/PostgreSQL experience who need to pass an interview in 3–4 weeks.

**Estimated duration:** 3–4 weeks at 15 hours/week

**Prerequisite:** Intermediate SQL proficiency. Some PostgreSQL experience.

**End goal:** Pass technical interview rounds at mid-to-senior level at any target company.

---

### Week 1 — SQL Skills Refresh and Gap-Filling

| Day | Focus | Chapter | Hours/Day |
|-----|-------|---------|-----------|
| 1 | Window functions — all patterns | 03_Intermediate_SQL | 3h |
| 2 | CTEs, recursive CTEs | 03, 04 | 3h |
| 3 | Gaps and islands, pivot patterns | 04_Advanced_SQL | 3h |
| 4 | EXPLAIN ANALYZE — read any plan | 08_Query_Optimization | 3h |
| 5 | Index selection and design | 07_Indexes | 3h |
| 6–7 | SQL problem sets: 30 problems timed | 20_Interview_Tasks | 5h |

**Day 7 checkpoint:** Complete the "Mid-Level SQL" section of `20_Interview_Tasks`. Target: solve 80% correctly without hints.

---

### Week 2 — PostgreSQL Internals and Concepts

| Day | Focus | Chapter | Hours/Day |
|-----|-------|---------|-----------|
| 1 | MVCC, ACID, isolation levels | 09_Transactions_Concurrency | 3h |
| 2 | WAL, checkpoint, crash recovery | 10_PostgreSQL_Internals | 3h |
| 3 | VACUUM, autovacuum, bloat | 10, 12 | 3h |
| 4 | Replication: streaming + logical | 13_Replication_HA | 3h |
| 5 | Backup: PITR concepts | 15_Backup_Recovery | 2h |
| 6 | Security: RLS, roles, pg_hba.conf | 14_Security | 2h |
| 7 | Practice explaining internals out loud | 19_Interview_Questions | 3h |

---

### Week 3 — System Design and Company Prep

| Day | Focus | Chapter | Hours/Day |
|-----|-------|---------|-----------|
| 1 | System design framework for DB-heavy systems | 21_System_Design | 3h |
| 2 | HA design with Patroni | 18, 21 | 3h |
| 3 | Sharding and partitioning design | 18, 21 | 3h |
| 4 | Company-specific prep (your target) | 23_Company_Preparation | 4h |
| 5 | Behavioral questions for DB engineers | INTERVIEW_PREPARATION.md | 2h |
| 6–7 | Mock interview simulation (full rounds) | 20_Interview_Tasks | 5h |

---

### Week 4 — Final Polish

| Day | Focus | Hours |
|-----|-------|-------|
| 1 | Review all red/yellow flashcard items | 2h |
| 2 | Senior question bank — all topics | 3h |
| 3 | System design mock: design a HA DB for a ride-share app | 2h |
| 4 | Company-specific research + prep | 2h |
| 5 | Light review, cheat sheets, rest | 1h |
| 6–7 | Mock interviews with a friend | 4h |

**Fast Track Milestones:**
- [ ] Can answer "Explain MVCC" in 3 minutes without notes
- [ ] Can read any EXPLAIN ANALYZE output and identify the problem
- [ ] Can write a window function query in < 2 minutes
- [ ] Can design a HA PostgreSQL system on a whiteboard in 15 minutes
- [ ] Completed 50+ timed SQL problems

---

## 8. Cross-Path Reference Table

| Chapter | Beginner | Backend | Data Eng | Analytics | DBA | Fast Track |
|---------|----------|---------|----------|-----------|-----|------------|
| 01_Fundamentals | Core | Skip | Skip | Skip | Skip | Skip |
| 02_SQL_Basics | Core | Skip | Skip | Core | Skip | Skim |
| 03_Intermediate_SQL | Core | Skim | Core | Core | Skim | Core |
| 04_Advanced_SQL | Core | Optional | Core | Core | Optional | Core |
| 05_PostgreSQL_Core | Core | Core | Core | Core | Core | Skim |
| 06_Database_Design | Core | Core | Optional | Optional | Core | Skim |
| 07_Indexes | Core | Core | Optional | Core | Core | Core |
| 08_Query_Optimization | Core | Core | Core | Core | Core | Core |
| 09_Transactions_Concurrency | Core | Core | Core | Optional | Core | Core |
| 10_PostgreSQL_Internals | Optional | Optional | Core | Skip | Core | Core |
| 11_Advanced_PostgreSQL | Optional | Optional | Core | Core | Core | Skim |
| 12_Production_PostgreSQL | Optional | Core | Core | Optional | Core | Skim |
| 13_Replication_HA | Skip | Optional | Core | Skip | Core | Core |
| 14_Security | Skip | Core | Optional | Skip | Core | Skim |
| 15_Backup_Recovery | Skip | Optional | Optional | Skip | Core | Skim |
| 16_Data_Engineering | Skip | Optional | Core | Core | Optional | Skip |
| 17_Backend_Engineers | Skip | Core | Skip | Skip | Optional | Skim |
| 18_Architecture | Skip | Core | Optional | Optional | Core | Core |
| 19_Interview_Questions | Core | Core | Core | Core | Core | Core |
| 20_Interview_Tasks | Core | Core | Core | Core | Core | Core |
| 21_System_Design | Skip | Core | Optional | Skip | Core | Core |
| 22_Projects | Core | Core | Core | Core | Core | Skip |
| 23_Company_Preparation | Skip | Optional | Optional | Optional | Optional | Core |
| 24_Cheat_Sheets | All | All | All | All | All | Core |
| 25_Capstone | Optional | Optional | Optional | Optional | Core | Skip |

Legend: **Core** = must complete, **Optional** = do if time allows, **Skim** = read headings only, **Skip** = not relevant

---

## 9. Chapter Dependency Map

Some chapters have strict prerequisite chains. Do not skip prerequisites.

```
01_Fundamentals
    └── 02_SQL_Basics
            ├── 03_Intermediate_SQL
            │       └── 04_Advanced_SQL
            └── 05_PostgreSQL_Core
                    ├── 06_Database_Design
                    │       └── (informs all schema work)
                    ├── 07_Indexes
                    │       └── 08_Query_Optimization
                    ├── 09_Transactions_Concurrency
                    │       └── 10_PostgreSQL_Internals
                    │               ├── 12_Production_PostgreSQL
                    │               └── 13_Replication_HA
                    │                       └── 15_Backup_Recovery
                    └── 11_Advanced_PostgreSQL
                            └── 16_Data_Engineering

14_Security ← requires 05, 06, 09
17_Backend_Engineers ← requires 05, 06, 07, 08, 09
18_Architecture ← requires 06, 09, 13
21_System_Design ← requires 13, 18
22_Projects ← requires relevant preceding chapters
23_Company_Preparation ← requires 19, 20, 21
```

---

## 10. Milestone Definitions

These milestone definitions are used across all paths. Each milestone represents a concrete skill checkpoint.

### Milestone 1 — SQL Foundations
**Can:** Write a multi-table JOIN with GROUP BY, filtering, and HAVING. Handle NULLs correctly. Use all aggregate functions.
**Chapter:** 02_SQL_Basics
**Test:** Complete the SQL Basics problem set in `20_Interview_Tasks` with 80% accuracy.

### Milestone 2 — SQL Practitioner
**Can:** Write CTEs, subqueries, and window functions. Read a basic EXPLAIN output.
**Chapter:** 03_Intermediate_SQL
**Test:** Complete the Intermediate SQL section of `20_Interview_Tasks` without hints.

### Milestone 3 — PostgreSQL Developer
**Can:** Design a normalized schema, choose indexes correctly, explain MVCC.
**Chapters:** 05–09
**Test:** Complete the Mid-Level question bank in `19_Interview_Questions` with 70% accuracy.

### Milestone 4 — Senior PostgreSQL Engineer
**Can:** Configure a production instance, read EXPLAIN ANALYZE, tune autovacuum, implement RLS, explain WAL.
**Chapters:** 10–15
**Test:** Complete the Senior question bank in `19_Interview_Questions` with 70% accuracy. Complete Project 3 or 4 in `22_Projects`.

### Milestone 5 — Staff/Principal PostgreSQL Engineer
**Can:** Design a Patroni HA cluster, implement PITR, design a sharding strategy, lead a system design interview.
**Chapters:** All
**Test:** Complete the Capstone in `25_Capstone_Program`. Complete a mock system design interview with a peer.
