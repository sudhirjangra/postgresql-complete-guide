# 24 — Cheat Sheets

Dense, scannable reference cards for PostgreSQL and SQL. Designed for pre-interview review and on-the-job lookup.

## Files

| File | Content | Lines |
|---|---|---|
| `01_sql_syntax_cheatsheet.md` | DDL, DML, DQL, JOINs, aggregations, window functions, CTEs, subqueries, set ops, string/date functions, NULL handling, JSONB, arrays | ~350 |
| `02_postgresql_commands_cheatsheet.md` | psql meta-commands, connection strings, pg_dump/pg_restore flags, pg_ctl, system views, VACUUM, config params | ~380 |
| `03_window_functions_cheatsheet.md` | All window functions with syntax, frame options, 10 common patterns (running totals, gaps/islands, sessions, cohorts) | ~290 |
| `04_index_decision_cheatsheet.md` | ASCII decision tree, index types table, when to use each, partial indexes, composite index rules, gotchas | ~250 |
| `05_explain_cheatsheet.md` | All EXPLAIN node types, cost fields, red flags, green flags, worked example, auto_explain config | ~280 |
| `06_transaction_isolation_cheatsheet.md` | Isolation levels matrix, MVCC internals, locking reference, SKIP LOCKED, advisory locks, two-phase commit | ~270 |
| `07_performance_tuning_cheatsheet.md` | Top 20 postgresql.conf params, OLTP/OLAP configs, formulas (work_mem, shared_buffers), tuning checklist | ~320 |
| `08_interview_quick_reference.md` | 30 most common interview Q&A, one-page answers, 20-point rapid review card | ~310 |

## How to Use

### Before an Interview
1. Read `08_interview_quick_reference.md` — the 30 Q&A
2. Review the 20-point rapid review card at the bottom
3. Skim `03_window_functions_cheatsheet.md` — the Quick Cheat Table at bottom

### Debugging a Slow Query
1. `05_explain_cheatsheet.md` — node types and red flags
2. `04_index_decision_cheatsheet.md` — decide which index to add
3. `07_performance_tuning_cheatsheet.md` — check work_mem, random_page_cost

### During a Schema Design Task
1. `01_sql_syntax_cheatsheet.md` — DDL, constraints, data types
2. `04_index_decision_cheatsheet.md` — choose index types
3. `06_transaction_isolation_cheatsheet.md` — choose isolation level

### Setting Up a PostgreSQL Server
1. `02_postgresql_commands_cheatsheet.md` — all psql commands
2. `07_performance_tuning_cheatsheet.md` — full config template (OLTP or OLAP)

## Top 10 Facts to Remember (Critical for Interviews)

1. `NOT IN` with NULLs in subquery returns **zero rows** — use `NOT EXISTS`
2. `LAST_VALUE` needs `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` to work correctly
3. `random_page_cost = 1.1` for SSD — critical for index usage
4. CTEs are **inlined** in PostgreSQL 12+ (not optimization fences)
5. `VACUUM` ≠ `VACUUM FULL` — FULL rewrites and locks the table
6. `work_mem` is per sort/hash operation, not per session
7. MVCC: readers never block writers, writers never block readers
8. `JSONB` > `JSON` for almost all use cases
9. `EXPLAIN ANALYZE` actually **executes** the query — use `BEGIN/ROLLBACK` for mutations
10. Autovacuum prevents XID wraparound — never disable it
