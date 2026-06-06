# 25: PostgreSQL Mastery Capstone Program

## 12-Week Intensive — From SQL Basics to Production Expert

---

## What Is This Program?

The Capstone Program is a structured, 12-week daily study curriculum that takes you from SQL fundamentals to production-grade PostgreSQL expertise. It is the glue that holds together all 24 other chapters in this repository, providing a learning path, daily exercises, mock interviews, and a final project.

**Commitment:** 1-2 hours/day, 5 days/week  
**Outcome:** Interview-ready PostgreSQL mastery + 10-project portfolio

---

## Program Files

| File | Week | Phase | Topic |
|------|------|-------|-------|
| [00_capstone_overview.md](00_capstone_overview.md) | — | Overview | Prerequisites, outcomes, evaluation criteria |
| [01_week1_foundations.md](01_week1_foundations.md) | 1 | Phase 1 | Setup, fundamentals, basic SQL |
| [02_week2_sql_mastery.md](02_week2_sql_mastery.md) | 2 | Phase 1 | JOINs, aggregations, subqueries |
| [03_week3_advanced_sql.md](03_week3_advanced_sql.md) | 3 | Phase 2 | Window functions, CTEs, advanced patterns |
| [04_week4_postgresql_core.md](04_week4_postgresql_core.md) | 4 | Phase 2 | JSONB, PL/pgSQL, custom types, domains |
| [05_week5_database_design.md](05_week5_database_design.md) | 5 | Phase 2 | Normalization, schema design, ER modeling |
| [06_week6_indexes_optimization.md](06_week6_indexes_optimization.md) | 6 | Phase 2 | All index types, EXPLAIN ANALYZE, anti-patterns |
| [07_week7_internals.md](07_week7_internals.md) | 7 | Phase 3 | Transactions, MVCC, locking, UPSERT |
| [08_week8_advanced_features.md](08_week8_advanced_features.md) | 8 | Phase 3 | Partitioning, FTS, pg_trgm, extensions |
| [09_week9_production.md](09_week9_production.md) | 9 | Phase 3 | Monitoring, security, backup/recovery |
| [10_week10_replication_ha.md](10_week10_replication_ha.md) | 10 | Phase 4 | Replication, Patroni, PgBouncer, HA design |
| [11_week11_interview_prep.md](11_week11_interview_prep.md) | 11 | Phase 4 | Mock interviews, coding under pressure |
| [12_week12_capstone_project.md](12_week12_capstone_project.md) | 12 | Phase 4 | Build complete project, peer review, assessment |
| [assessment_rubric.md](assessment_rubric.md) | — | Evaluation | Detailed grading criteria across 5 dimensions |

---

## Learning Phases

```
PHASE 1: FOUNDATIONS (Weeks 1-2) ─────────────────────
  Week 1: Environment setup, data types, basic SELECT
  Week 2: JOINs, aggregations, subqueries, set operations

PHASE 2: POSTGRESQL MASTERY (Weeks 3-6) ──────────────
  Week 3: Window functions, CTEs, advanced patterns
  Week 4: JSONB, PL/pgSQL, domains, generated columns
  Week 5: Normalization, design patterns, ER modeling
  Week 6: B-Tree/GIN/BRIN indexes, EXPLAIN ANALYZE

PHASE 3: INTERNALS & OPERATIONS (Weeks 7-9) ──────────
  Week 7: MVCC, isolation levels, locking, UPSERT
  Week 8: Partitioning, full-text search, extensions
  Week 9: Security, monitoring, backup, configuration

PHASE 4: ARCHITECTURE & CAREER (Weeks 10-12) ─────────
  Week 10: Streaming replication, Patroni, PgBouncer
  Week 11: Interview preparation, mock sessions
  Week 12: Capstone project, peer review, final assessment
```

---

## Repository Chapter Map

| Weeks | Chapters in This Repo |
|-------|----------------------|
| 1-2   | 01_Fundamentals, 02_SQL_Basics, 03_Intermediate_SQL |
| 3     | 04_Advanced_SQL |
| 4     | 05_PostgreSQL_Core |
| 5     | 06_Database_Design |
| 6     | 07_Indexes, 08_Query_Optimization |
| 7     | 09_Transactions_Concurrency, 10_PostgreSQL_Internals |
| 8     | 11_Advanced_PostgreSQL |
| 9     | 12_Production_PostgreSQL, 14_Security, 15_Backup_Recovery |
| 10    | 13_Replication_HA |
| 11    | 19_Interview_Questions, 20_Interview_Tasks, 21_System_Design |
| 12    | 22_Projects |

---

## Skills You Will Have After Completion

### SQL Skills
- Complex window functions (OVER, PARTITION BY, frame clauses)
- Recursive CTEs for hierarchies
- JSONB querying with GIN indexes
- Full-text search with tsvector and tsquery
- Advanced aggregations with FILTER and GROUPING SETS

### PostgreSQL Skills
- PL/pgSQL functions with full error handling
- Trigger design for data integrity
- Table partitioning (range, list, hash)
- Row-Level Security policies
- MVCC and isolation levels

### Operations Skills
- EXPLAIN ANALYZE for query optimization
- Index strategy (all 6 types)
- Streaming and logical replication
- Backup and point-in-time recovery
- Connection pooling with PgBouncer
- High-availability with Patroni

### Career Skills
- Answering any interview question in 2 minutes
- Live schema design under pressure
- Portfolio of 10 complete projects
- Production incident response framework

---

## Evaluation

Your work is evaluated across 5 dimensions (see [assessment_rubric.md](assessment_rubric.md)):

| Dimension | Weight |
|-----------|--------|
| Schema Design | 25% |
| SQL Query Quality | 25% |
| PL/pgSQL Functions/Triggers | 20% |
| Performance Optimization | 20% |
| Documentation/Communication | 10% |

**Score bands:** Expert (90+), Proficient (75-89), Competent (60-74), Developing (<60)

---

## Getting Started

1. Read [00_capstone_overview.md](00_capstone_overview.md) completely.
2. Ensure PostgreSQL 16 is installed and you can connect via `psql`.
3. Create a dedicated database: `CREATE DATABASE capstone;`
4. Open [01_week1_foundations.md](01_week1_foundations.md) and start Day 1.
5. Track your progress using the weekly self-assessment template.
6. Post your completed capstone project to GitHub.

---

## Quick Links by Goal

**I want to pass a technical SQL interview this month:**
→ Weeks 3, 6, 7, 11 are highest priority

**I want to learn PostgreSQL-specific features:**
→ Weeks 4, 7, 8, 10

**I want to understand production database operations:**
→ Weeks 6, 7, 9, 10

**I want to build projects for my portfolio:**
→ Complete all weeks, then 22_Projects/
