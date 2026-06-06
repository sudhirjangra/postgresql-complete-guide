# 03 — Intermediate SQL

This module builds on SQL Basics with powerful query patterns used daily by data engineers, backend developers, and data analysts.

---

## Contents

| File | Topic | Difficulty |
|------|-------|------------|
| [01_joins_complete.md](01_joins_complete.md) | INNER, LEFT, RIGHT, FULL, CROSS, SELF JOINs | Intermediate |
| [02_aggregations.md](02_aggregations.md) | GROUP BY, HAVING, COUNT, SUM, AVG, MIN, MAX | Intermediate |
| [03_subqueries.md](03_subqueries.md) | Scalar, row, table, correlated subqueries | Intermediate |
| [04_exists_any_all.md](04_exists_any_all.md) | EXISTS, NOT EXISTS, ANY, ALL operators | Intermediate |
| [05_set_operations.md](05_set_operations.md) | UNION, UNION ALL, INTERSECT, EXCEPT | Intermediate |
| [06_case_expressions.md](06_case_expressions.md) | CASE, COALESCE, NULLIF, conditional logic | Intermediate |
| [07_grouping_sets_rollup_cube.md](07_grouping_sets_rollup_cube.md) | GROUPING SETS, ROLLUP, CUBE | Advanced-Intermediate |

---

## Learning Path

```
01_joins_complete          ← Start here — foundation for all multi-table queries
         │
         ▼
02_aggregations            ← GROUP BY and aggregate functions
         │
         ▼
03_subqueries              ← Scalar, row, table, correlated
         │
         ▼
04_exists_any_all          ← Semi-joins and quantified comparisons
         │
         ▼
05_set_operations          ← UNION, INTERSECT, EXCEPT
         │
         ▼
06_case_expressions        ← Conditional logic everywhere
         │
         ▼
07_grouping_sets_rollup_cube ← Multi-level aggregation
```

---

## Prerequisites

- Completed `02_SQL_Basics/` (SELECT, WHERE, ORDER BY, basic functions)
- Comfortable with table relationships and foreign keys

---

## What You Will Be Able to Do

After completing this module:

1. Write complex multi-table JOIN queries confidently
2. Aggregate data at any granularity using GROUP BY and HAVING
3. Use subqueries and CTEs to break complex problems into steps
4. Apply EXISTS/NOT EXISTS for efficient semi-join and anti-join patterns
5. Combine result sets with UNION, INTERSECT, and EXCEPT
6. Implement conditional logic with CASE expressions
7. Build executive-level reports with ROLLUP and CUBE

---

## Key SQL Patterns Covered

### Anti-Join (find rows with no match)
```sql
SELECT * FROM A WHERE NOT EXISTS (SELECT 1 FROM B WHERE B.id = A.id);
```

### Semi-Join (find rows that have a match)
```sql
SELECT * FROM A WHERE EXISTS (SELECT 1 FROM B WHERE B.id = A.id);
```

### Conditional Aggregation (pivot)
```sql
SELECT
    region,
    SUM(amount) FILTER (WHERE product = 'Widget A') AS widget_a,
    SUM(amount) FILTER (WHERE product = 'Widget B') AS widget_b
FROM sales GROUP BY region;
```

### Running Total with ROLLUP
```sql
SELECT region, product, SUM(amount)
FROM sales
GROUP BY ROLLUP(region, product);
```

---

## Interview Topics Covered

- All JOIN types and their behavior with NULLs
- NULL trap in NOT IN vs NOT EXISTS
- Difference between WHERE and HAVING
- Correlated subquery performance (N+1 problem)
- UNION vs UNION ALL performance
- ROLLUP vs CUBE vs GROUPING SETS

---

## Next Module

Proceed to `04_Advanced_SQL/` for:
- Window functions (ROW_NUMBER, RANK, LAG, LEAD)
- CTEs and recursive queries
- LATERAL joins
- Advanced aggregation
