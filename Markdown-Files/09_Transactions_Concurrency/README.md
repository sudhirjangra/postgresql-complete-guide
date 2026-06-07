# 09 Transactions & Concurrency

Understanding transactions and concurrency is what separates a developer who uses PostgreSQL from one who can trust it. This folder covers everything from basic ACID guarantees through MVCC internals, isolation levels, locking strategies, deadlock handling, and PostgreSQL's advanced Serializable Snapshot Isolation.

---

## Contents

| File | Topic | Difficulty | Est. Time |
|---|---|---|---|
| 01_transaction_basics.md | BEGIN/COMMIT/ROLLBACK/SAVEPOINT, ACID | Beginner | 1h |
| 02_mvcc.md | xmin/xmax, tuple versions, snapshot isolation | Intermediate | 2h |
| 03_isolation_levels.md | READ COMMITTED, REPEATABLE READ, SERIALIZABLE — anomalies | Intermediate | 2h |
| 04_locks.md | Row locks, table locks, advisory locks, lock matrix | Intermediate | 2h |
| 05_deadlocks.md | Detection, prevention, pg_locks investigation | Intermediate | 1.5h |
| 06_optimistic_vs_pessimistic.md | FOR UPDATE, version columns, SKIP LOCKED | Intermediate | 1.5h |
| 07_serializable_snapshot_isolation.md | SSI, predicate locks, write skew, retry | Advanced | 2h |
| 08_transaction_interview_guide.md | 40 Q&A: ACID → MVCC → locks → SSI | All levels | 3h |

---

## Learning Path

1. **01_transaction_basics** — start here even if you know transactions; covers PostgreSQL specifics
2. **02_mvcc** — essential foundation; everything else builds on MVCC
3. **03_isolation_levels** — what each level actually prevents (with PostgreSQL-specific behavior)
4. **04_locks** — understand lock modes before reading about deadlocks
5. **05_deadlocks** — detection, investigation, prevention
6. **06_optimistic_vs_pessimistic** — application patterns (SKIP LOCKED, version columns)
7. **07_serializable_snapshot_isolation** — advanced; read after mastering previous files
8. **08_transaction_interview_guide** — interview prep, review after completing all files

---

## Prerequisites

- Basic SQL: SELECT, INSERT, UPDATE, DELETE
- Understand what a transaction is (can commit or rollback)
- Helpful: read 10_PostgreSQL_Internals/02_heap_storage.md for MVCC tuple storage details

---

## Key Concepts Covered

- **ACID**: Atomicity, Consistency, Isolation, Durability in PostgreSQL context
- **MVCC**: Multi-Version Concurrency Control — how reads never block writes
- **xmin/xmax**: Transaction ID fields that control tuple visibility
- **Snapshot**: Point-in-time view of the database a transaction operates on
- **Isolation levels**: Four levels, three anomalies (dirty read, non-repeatable read, phantom)
- **Write skew**: Anomaly that only SERIALIZABLE prevents
- **Row locks**: FOR UPDATE, FOR SHARE, FOR NO KEY UPDATE, FOR KEY SHARE
- **Table locks**: ACCESS SHARE through ACCESS EXCLUSIVE — 8 modes
- **Advisory locks**: Application-level locks using pg_advisory_lock
- **Deadlock**: Cycle in lock wait graph — PostgreSQL detects and breaks it
- **SKIP LOCKED**: Job queue pattern — skip rows locked by other workers
- **Optimistic locking**: Version column pattern for low-contention scenarios
- **SSI**: Serializable Snapshot Isolation — detects rw-anti-dependency cycles

---

## Related Folders

- **10_PostgreSQL_Internals** — deep dive into xmin/xmax storage, tuple structure, visibility rules
- **08_Query_Optimization** — lock waits visible in EXPLAIN, query plan interaction with locks
- **12_Production_PostgreSQL** — incident runbooks for lock waits, deadlocks in production
- **17_PostgreSQL_For_Backend_Engineers** — application-level transaction management (Python, Java, Node, Go)
