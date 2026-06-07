# 01_Fundamentals — Folder Index

> **PostgreSQL Complete Guide**
> Module 1: Database Fundamentals
> Prerequisites: None | Difficulty: Beginner → Intermediate

---

## Overview

This module builds the conceptual foundation required before writing a single line of SQL. Understanding *why* databases exist, *what* makes PostgreSQL different, and *how* fundamental principles like ACID and CAP apply to real systems will make you a far better engineer than memorizing syntax alone.

---

## Chapters in This Module

| # | File | Topic | Difficulty | Time |
|---|------|--------|------------|------|
| 01 | [01_what_is_a_database.md](./01_what_is_a_database.md) | What is a Database? | Beginner | 20 min |
| 02 | [02_types_of_databases.md](./02_types_of_databases.md) | Types of Databases | Beginner | 25 min |
| 03 | [03_relational_databases.md](./03_relational_databases.md) | Relational Databases | Beginner-Intermediate | 30 min |
| 04 | [04_oltp_vs_olap.md](./04_oltp_vs_olap.md) | OLTP vs. OLAP | Intermediate | 25 min |
| 05 | [05_cap_theorem.md](./05_cap_theorem.md) | CAP Theorem | Intermediate | 25 min |
| 06 | [06_acid_properties.md](./06_acid_properties.md) | ACID Properties | Intermediate | 30 min |
| 07 | [07_base_properties.md](./07_base_properties.md) | BASE Properties | Intermediate | 20 min |
| 08 | [08_postgresql_introduction.md](./08_postgresql_introduction.md) | PostgreSQL Introduction | Beginner | 25 min |
| 09 | [09_installation_guide.md](./09_installation_guide.md) | Installation Guide | Beginner | 45 min |

**Total Estimated Time:** ~3.5 hours

---

## Learning Path

```
Start Here
    |
    v
01_what_is_a_database   --> Understand what a database IS
    |
    v
02_types_of_databases   --> Know the landscape (SQL, NoSQL, NewSQL)
    |
    v
03_relational_databases --> Master the relational model (keys, joins, normalization)
    |
    v
04_oltp_vs_olap         --> Understand workload types and schema design implications
    |
    v
05_cap_theorem          --> Grasp distributed systems trade-offs
    |
    v
06_acid_properties      --> Understand transaction guarantees deeply (critical!)
    |
    v
07_base_properties      --> See the other side: eventual consistency
    |
    v
08_postgresql_introduction --> Meet your database engine
    |
    v
09_installation_guide   --> Get PostgreSQL running on your machine
    |
    v
02_SQL_Basics/ --> Next module: Start writing SQL!
```

---

## Key Concepts Covered

### Database Fundamentals
- What is a database vs. a DBMS vs. a database system
- Problems databases solve over flat-file storage
- The three-schema architecture (external/conceptual/internal)

### Data Models
- Relational, document, key-value, wide-column, graph, time-series
- When to use each type
- Polyglot persistence

### The Relational Model
- Tables, rows, columns, domains
- Primary keys, foreign keys, candidate keys, composite keys
- One-to-one, one-to-many, many-to-many relationships
- First, Second, Third Normal Form
- Referential integrity and constraints

### Workload Types
- OLTP: transactional, normalized, row-oriented
- OLAP: analytical, denormalized, star schema
- ETL pipelines and data warehousing

### Distributed Systems Theory
- CAP Theorem: Consistency, Availability, Partition Tolerance
- PACELC model
- Consistency models: linearizable → eventual
- ACID: Atomicity, Consistency, Isolation, Durability
- BASE: Basically Available, Soft State, Eventually Consistent
- MVCC and transaction isolation levels

### PostgreSQL Specifics
- History (INGRES → POSTGRES → PostgreSQL)
- Process architecture (multi-process, shared memory)
- Key features: JSONB, arrays, extensions, custom types
- Installation on all major platforms
- Post-installation security and configuration

---

## Quick Reference: Key SQL from This Module

```sql
-- Connect to PostgreSQL
psql -U postgres -d mydb

-- Create your first database
CREATE DATABASE learning_db;

-- Create a normalized table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    email       VARCHAR(100) UNIQUE NOT NULL,
    created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Check ACID in action
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- MVCC: Multiple isolation levels
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
  SELECT * FROM products WHERE id = 1;
COMMIT;

-- Check replication lag (distributed systems)
SELECT now() - pg_last_xact_replay_timestamp() AS lag;
```

---

## Interview Topics From This Module

These are commonly asked in database/backend engineering interviews:

1. What problems do databases solve that flat files cannot?
2. What is the relational model? Who invented it?
3. What are the ACID properties?
4. What is the difference between ACID-Consistency and CAP-Consistency?
5. What are the four transaction isolation levels?
6. What read anomalies does each isolation level prevent?
7. What is MVCC and how does it enable high concurrency?
8. What is the CAP theorem?
9. Why can't a distributed system guarantee all three CAP properties?
10. What is the difference between OLTP and OLAP workloads?
11. What is a star schema?
12. What is eventual consistency?
13. What is the difference between BASE and ACID?
14. What makes PostgreSQL different from MySQL?
15. How does PostgreSQL implement durability?

---

## Prerequisites for Next Module

After completing this module, you should be able to:

- Connect to a PostgreSQL instance using `psql`
- Create databases and basic tables
- Understand why transactions matter
- Explain ACID properties in an interview
- Know when to choose PostgreSQL vs. other database types

**Next:** [02_SQL_Basics/README.md](../02_SQL_Basics/README.md) — DDL, DML, SELECT, filtering, functions

---

## Resources

### Official Documentation
- [PostgreSQL Documentation](https://www.postgresql.org/docs/current/)
- [PostgreSQL Wiki](https://wiki.postgresql.org/)

### Books Referenced
- *An Introduction to Database Systems* — C.J. Date
- *PostgreSQL: Up and Running* — Regina Obe & Leo Hsu
- *The Art of PostgreSQL* — Dimitri Fontaine
- *Designing Data-Intensive Applications* — Martin Kleppmann (for CAP/distributed systems)

### Online Tools
- [DB-Engines Ranking](https://db-engines.com/en/ranking)
- [use-the-index-luke.com](https://use-the-index-luke.com) — SQL indexing explained

---

*Module 1 of the PostgreSQL Complete Guide | Author: Sudhir Jangra*
