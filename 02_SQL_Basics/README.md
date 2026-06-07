# 02_SQL_Basics — Module Overview

> **PostgreSQL Complete Guide**
> Module 02 of 08 | Difficulty: Beginner → Beginner-Intermediate | Total Estimated Reading Time: ~5 hours

---

## Table of Contents

- [What This Module Covers](#what-this-module-covers)
- [File List and Descriptions](#file-list-and-descriptions)
- [Learning Objectives](#learning-objectives)
- [Prerequisites](#prerequisites)
- [How to Use This Module](#how-to-use-this-module)
- [Quick Reference — SQL Command Categories](#quick-reference--sql-command-categories)
- [Module Dependency Map](#module-dependency-map)
- [Next Steps](#next-steps)

---

## What This Module Covers

This module establishes the **complete foundation of SQL in PostgreSQL**. You will go from writing your first `CREATE TABLE` to mastering filtering operators, NULL semantics, and the full suite of built-in scalar functions for strings, numbers, and dates.

By the end of this module you will be able to design table schemas, load data, query it with precision, and transform it entirely within SQL — no application code required.

---

## File List and Descriptions

| # | File | Topics | Difficulty | Read Time |
|---|---|---|---|---|
| 01 | [01_ddl_commands.md](01_ddl_commands.md) | CREATE, ALTER, DROP, TRUNCATE, schemas, indexes, sequences | Beginner | 35 min |
| 02 | [02_dml_commands.md](02_dml_commands.md) | INSERT, UPDATE, DELETE, MERGE, RETURNING, upserts | Beginner | 35 min |
| 03 | [03_select_basics.md](03_select_basics.md) | SELECT, WHERE, ORDER BY, LIMIT, OFFSET, DISTINCT, DISTINCT ON | Beginner | 40 min |
| 04 | [04_filtering_and_operators.md](04_filtering_and_operators.md) | Comparison operators, AND/OR/NOT, BETWEEN, IN, LIKE, ILIKE, SIMILAR TO, regex `~` | Beginner–Int. | 40 min |
| 05 | [05_null_handling.md](05_null_handling.md) | NULL semantics, IS NULL, IS DISTINCT FROM, COALESCE, NULLIF, three-valued logic | Beginner–Int. | 35 min |
| 06 | [06_string_functions.md](06_string_functions.md) | UPPER/LOWER, TRIM, LENGTH, SUBSTRING, POSITION, REPLACE, CONCAT, FORMAT, LPAD, RPAD, REGEXP_REPLACE, REGEXP_MATCH | Beginner–Int. | 45 min |
| 07 | [07_numeric_functions.md](07_numeric_functions.md) | ROUND, CEIL, FLOOR, TRUNC, ABS, MOD, POWER, SQRT, RANDOM, GREATEST, LEAST, type casting | Beginner–Int. | 35 min |
| 08 | [08_date_time_functions.md](08_date_time_functions.md) | NOW, CURRENT_DATE, EXTRACT, DATE_PART, DATE_TRUNC, AGE, MAKE_DATE, TO_CHAR, TO_DATE, INTERVAL arithmetic, AT TIME ZONE | Beginner–Int. | 50 min |

---

## Learning Objectives

After completing this module, you will be able to:

1. **Define and modify database structures** — create tables with constraints, add/drop columns, create indexes and sequences, safely truncate or drop objects.
2. **Manipulate data** — insert single and bulk rows, update with joins and subqueries, delete safely with RETURNING, implement conflict-handling upserts.
3. **Query with confidence** — write SELECT statements with precise filtering, multi-column sorting, DISTINCT deduplication, and cursor-style pagination.
4. **Master filtering** — use every PostgreSQL comparison and pattern-matching operator correctly, including the NULL-safe operators and full POSIX regex.
5. **Handle NULL correctly** — understand three-valued logic, use IS NULL / IS DISTINCT FROM, apply COALESCE and NULLIF, and reason about NULL in joins and aggregations.
6. **Transform text** — apply the full suite of PostgreSQL string functions for cleaning, splitting, extracting, replacing, and formatting text data.
7. **Do math in SQL** — apply rounding, modulo, power, random sampling, and safe division functions, and understand integer vs float vs NUMERIC arithmetic.
8. **Work with dates and times** — store, query, calculate, and format dates and timestamps correctly, including time zone handling and interval arithmetic.

---

## Prerequisites

Before starting this module you should:

- Have PostgreSQL installed and be able to connect with `psql` or a GUI client (covered in `01_Introduction/`)
- Understand the concept of a relational database, tables, rows, and columns (covered in `01_Introduction/02_relational_model.md`)
- Be comfortable running SQL statements in your PostgreSQL environment

No prior programming experience is required. Each file builds on the previous one within the module.

---

## How to Use This Module

### Recommended Order

Follow the files in numbered order (01 → 08). Each file cross-references the ones before it.

### Hands-on Practice

Every file includes a **Setup** section with CREATE TABLE and INSERT statements you can run directly in PostgreSQL. Run the setup, work through the SQL Examples section interactively, then attempt the Exercises before reading the Solutions.

### Building a Practice Database

By the time you finish all 8 files, you will have worked with these practice tables:

```
departments     -- DDL chapter setup
employees       -- DDL chapter setup
products        -- SELECT basics, filtering, string functions
orders          -- multiple chapters
contacts        -- string functions
employees_demo  -- NULL handling
financial_data  -- numeric functions
events          -- date/time functions
```

Consider combining them into a single practice schema so you can write cross-table queries as you proceed into Module 03.

---

## Quick Reference — SQL Command Categories

```
DDL (Data Definition)     DML (Data Manipulation)   Scalar Functions
-----------------------   -----------------------   ----------------
CREATE TABLE              INSERT                    UPPER / LOWER
ALTER TABLE               UPDATE                    TRIM / LENGTH
DROP TABLE                DELETE                    SUBSTRING / POSITION
TRUNCATE TABLE            MERGE                     CONCAT / FORMAT
CREATE INDEX              SELECT (read)             ROUND / TRUNC
CREATE SEQUENCE                                     ABS / MOD / POWER
                          Filtering Predicates      NOW / EXTRACT
TCL (Transactions)        -------------------------  DATE_TRUNC / AGE
-----------------------   WHERE col = val           COALESCE / NULLIF
BEGIN                     WHERE col BETWEEN a AND b
COMMIT                    WHERE col IN (list)
ROLLBACK                  WHERE col LIKE 'pattern'
SAVEPOINT                 WHERE col ~ 'regex'
                          WHERE col IS NULL
```

---

## Module Dependency Map

```
01_Introduction/
        |
        v
02_SQL_Basics/  <--- YOU ARE HERE
   01_ddl_commands.md
   02_dml_commands.md
   03_select_basics.md
   04_filtering_and_operators.md
   05_null_handling.md
   06_string_functions.md
   07_numeric_functions.md
   08_date_time_functions.md
        |
        v
03_Intermediate_SQL/
   01_joins.md
   02_aggregations.md
   03_subqueries.md
   04_window_functions.md
   05_ctes.md
   ...
```

Every topic in Module 03 (Intermediate SQL) assumes mastery of this module. Joins require SELECT and WHERE; aggregations require GROUP BY and ORDER BY; window functions require all of the above.

---

## Next Steps

When you have completed all 8 files in this module:

1. **Test yourself** — open a blank SQL editor and write a query that filters, joins, groups, sorts, and formats output without referring to notes.
2. **Move to Module 03** — [03_Intermediate_SQL/README.md](../03_Intermediate_SQL/README.md) covers JOINs, GROUP BY, subqueries, CTEs, and window functions.
3. **Practice projects** — apply this module's skills to a real dataset (Northwind, AdventureWorks, or a public dataset from Kaggle or data.gov).

---

*Module 02 is part of the [PostgreSQL Complete Guide](../README.md).*
