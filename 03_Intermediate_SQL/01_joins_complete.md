# Complete Guide to SQL JOINs in PostgreSQL

---

## Table of Contents
1. [Learning Objectives](#learning-objectives)
2. [Theory: What is a JOIN?](#theory-what-is-a-join)
3. [INNER JOIN](#inner-join)
4. [LEFT JOIN (LEFT OUTER JOIN)](#left-join)
5. [RIGHT JOIN (RIGHT OUTER JOIN)](#right-join)
6. [FULL OUTER JOIN](#full-outer-join)
7. [CROSS JOIN](#cross-join)
8. [SELF JOIN](#self-join)
9. [Multi-table JOINs](#multi-table-joins)
10. [JOIN with Conditions and Filters](#join-with-conditions-and-filters)
11. [ASCII Venn Diagrams](#ascii-venn-diagrams)
12. [Execution Plan Concepts](#execution-plan-concepts)
13. [Common Mistakes](#common-mistakes)
14. [Best Practices](#best-practices)
15. [Performance Considerations](#performance-considerations)
16. [Interview Questions & Answers](#interview-questions--answers)
17. [Hands-on Exercises](#hands-on-exercises)
18. [Advanced Notes](#advanced-notes)
19. [Cross-references](#cross-references)

---

## Learning Objectives

By the end of this module, you will be able to:
- Understand all six types of SQL JOINs and when to use each
- Visualize JOIN behavior using Venn diagrams
- Write complex multi-table JOIN queries
- Identify and avoid common JOIN mistakes
- Optimize JOIN performance in PostgreSQL
- Answer JOIN-related interview questions with confidence

---

## Theory: What is a JOIN?

A JOIN combines rows from two or more tables based on a related column. The result set is a new virtual table formed by matching rows according to the join condition.

### Core Concept: The Cartesian Product

Before filtering, SQL forms a **Cartesian Product** (every row paired with every row). The JOIN condition then filters this to meaningful matches.

```
Table A (3 rows) x Table B (4 rows) = 12 candidate row pairs
JOIN condition filters these 12 pairs down to matching rows
```

### Setup: Sample Tables for All Examples

```sql
-- Drop and recreate for a clean slate
DROP TABLE IF EXISTS orders, customers, products, employees, departments, order_items CASCADE;

-- Customers table
CREATE TABLE customers (
    customer_id   SERIAL PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    city          VARCHAR(50),
    country       VARCHAR(50) DEFAULT 'USA'
);

-- Orders table
CREATE TABLE orders (
    order_id    SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    order_date  DATE NOT NULL,
    total_amount NUMERIC(10,2)
);

-- Products table
CREATE TABLE products (
    product_id   SERIAL PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category     VARCHAR(50),
    price        NUMERIC(10,2)
);

-- Order Items table
CREATE TABLE order_items (
    item_id    SERIAL PRIMARY KEY,
    order_id   INT REFERENCES orders(order_id),
    product_id INT REFERENCES products(product_id),
    quantity   INT,
    unit_price NUMERIC(10,2)
);

-- Employees table
CREATE TABLE employees (
    employee_id   SERIAL PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    manager_id    INT REFERENCES employees(employee_id),
    department_id INT,
    salary        NUMERIC(10,2)
);

-- Departments table
CREATE TABLE departments (
    department_id   SERIAL PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL,
    budget          NUMERIC(12,2)
);

-- Insert sample data
INSERT INTO customers (customer_name, city, country) VALUES
    ('Alice Johnson',  'New York',    'USA'),
    ('Bob Smith',      'Los Angeles', 'USA'),
    ('Carol White',    'Chicago',     'USA'),
    ('David Brown',    'Houston',     'USA'),
    ('Eva Green',      'Phoenix',     'USA'),
    ('Frank Miller',   'Toronto',     'Canada'),
    ('Grace Lee',      'London',      'UK');

INSERT INTO orders (customer_id, order_date, total_amount) VALUES
    (1, '2024-01-15', 250.00),
    (1, '2024-02-20', 180.50),
    (2, '2024-01-22', 320.75),
    (3, '2024-03-10', 95.00),
    (5, '2024-03-15', 410.00),
    (NULL, '2024-04-01', 50.00);   -- orphan order

INSERT INTO products (product_name, category, price) VALUES
    ('Laptop',     'Electronics', 999.99),
    ('Mouse',      'Electronics',  29.99),
    ('Keyboard',   'Electronics',  79.99),
    ('Desk Chair', 'Furniture',   299.99),
    ('Monitor',    'Electronics', 399.99);

INSERT INTO departments (department_name, budget) VALUES
    ('Engineering',  500000),
    ('Marketing',    200000),
    ('Sales',        300000),
    ('HR',           150000);

INSERT INTO employees (employee_name, manager_id, department_id, salary) VALUES
    ('Sarah CEO',    NULL, 1, 200000),
    ('John Eng',     1,    1,  95000),
    ('Mary Eng',     1,    1,  90000),
    ('Tom Sales',    1,    3,  75000),
    ('Lisa Mkt',     1,    2,  70000),
    ('Paul Intern',  2,    1,  45000);
```

---

## INNER JOIN

Returns only rows where the join condition matches in **both** tables.

### ASCII Venn Diagram — INNER JOIN

```
    Table A          Table B
  ___________      ___________
 /           \    /           \
|   A only    |  |   B only    |
|   (excluded)|##|  (excluded) |
|             |##|             |
 \___________/##\___________/
              ^^
         INNER JOIN
       (only overlap)
```

### Syntax

```sql
SELECT columns
FROM   table_a
INNER JOIN table_b ON table_a.key = table_b.key;
-- Note: INNER is optional — JOIN alone defaults to INNER JOIN
```

### Examples

```sql
-- Example 1: Basic INNER JOIN — customers with their orders
SELECT
    c.customer_name,
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_name, o.order_date;

-- Example 2: INNER JOIN with column alias disambiguation
SELECT
    c.customer_id,
    c.customer_name,
    COUNT(o.order_id)    AS total_orders,
    SUM(o.total_amount)  AS lifetime_value
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY lifetime_value DESC;

-- Example 3: Three-table INNER JOIN
SELECT
    c.customer_name,
    o.order_id,
    p.product_name,
    oi.quantity,
    oi.unit_price
FROM customers c
JOIN orders     o  ON c.customer_id  = o.customer_id
JOIN order_items oi ON o.order_id    = oi.order_id
JOIN products   p  ON oi.product_id  = p.product_id
ORDER BY c.customer_name;

-- Example 4: INNER JOIN with WHERE filter
SELECT
    c.customer_name,
    o.order_date,
    o.total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total_amount > 200
  AND o.order_date >= '2024-01-01'
ORDER BY o.total_amount DESC;

-- Example 5: INNER JOIN with LIKE condition
SELECT c.customer_name, o.total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE c.customer_name LIKE 'A%';
```

---

## LEFT JOIN

Returns **all rows from the left table** and matched rows from the right table. Unmatched right-side columns are NULL.

### ASCII Venn Diagram — LEFT JOIN

```
    Table A          Table B
  ___________      ___________
 /###########\    /           \
|###########  |##|  (overlap) |
|### A only ##|  |   B only   |
|###########  |  |  (excluded)|
 \###########/    \___________/
       ^^^
   LEFT JOIN
(entire left table)
```

### Examples

```sql
-- Example 6: LEFT JOIN — all customers, even without orders
SELECT
    c.customer_name,
    c.city,
    o.order_id,
    o.total_amount
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_name;

-- Example 7: Find customers with NO orders (anti-join pattern)
SELECT
    c.customer_id,
    c.customer_name,
    c.city
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;

-- Example 8: LEFT JOIN with COALESCE for NULL replacement
SELECT
    c.customer_name,
    COALESCE(COUNT(o.order_id), 0)   AS order_count,
    COALESCE(SUM(o.total_amount), 0) AS total_spent
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_spent DESC;

-- Example 9: LEFT JOIN across three tables
SELECT
    c.customer_name,
    o.order_id,
    p.product_name
FROM customers c
LEFT JOIN orders o      ON c.customer_id = o.customer_id
LEFT JOIN order_items oi ON o.order_id   = oi.order_id
LEFT JOIN products p    ON oi.product_id = p.product_id;
```

---

## RIGHT JOIN

Returns **all rows from the right table** and matched rows from the left table. Rarely used — a LEFT JOIN with tables swapped is equivalent.

### ASCII Venn Diagram — RIGHT JOIN

```
    Table A          Table B
  ___________      ___________
 /           \    /###########\
|   A only    |##|  (overlap) |
|  (excluded) |  |############|
|             |  |## B only ##|
 \___________/    \###########/
                       ^^^
                   RIGHT JOIN
              (entire right table)
```

### Examples

```sql
-- Example 10: RIGHT JOIN — all orders, even orphan orders
SELECT
    c.customer_name,
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id
ORDER BY o.order_id;

-- Example 11: Find orders with no matching customer (data quality check)
SELECT
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id
WHERE c.customer_id IS NULL;

-- Example 12: Equivalent LEFT JOIN rewrite (preferred style)
SELECT
    c.customer_name,
    o.order_id,
    o.order_date,
    o.total_amount
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
ORDER BY o.order_id;
```

---

## FULL OUTER JOIN

Returns **all rows from both tables**. NULLs appear where there is no match.

### ASCII Venn Diagram — FULL OUTER JOIN

```
    Table A          Table B
  ___________      ___________
 /###########\    /###########\
|###########  |##|  ########## |
|### A only ##|  |## B only ###|
|###########  |  |############|
 \###########/    \###########/
  ^^^^^^^^^^        ^^^^^^^^^^
         FULL OUTER JOIN
       (everything from both)
```

### Examples

```sql
-- Example 13: FULL OUTER JOIN — see all customers and all orders
SELECT
    c.customer_name,
    o.order_id,
    o.total_amount
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_name NULLS LAST;

-- Example 14: Data reconciliation — find rows missing on either side
SELECT
    c.customer_id AS cust_id,
    c.customer_name,
    o.order_id,
    CASE
        WHEN c.customer_id IS NULL THEN 'Orphan Order'
        WHEN o.order_id    IS NULL THEN 'No Orders'
        ELSE 'Matched'
    END AS status
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id
WHERE c.customer_id IS NULL OR o.order_id IS NULL;
```

---

## CROSS JOIN

Returns the **Cartesian Product** — every row from A combined with every row from B. No join condition.

### ASCII Diagram — CROSS JOIN

```
Table A: [1, 2, 3]    Table B: [X, Y]

Result: 3 x 2 = 6 rows
  (1,X), (1,Y)
  (2,X), (2,Y)
  (3,X), (3,Y)
```

### Examples

```sql
-- Example 15: Generate all size-color combinations
CREATE TEMP TABLE sizes  (size  VARCHAR(5));
CREATE TEMP TABLE colors (color VARCHAR(10));
INSERT INTO sizes  VALUES ('S'),('M'),('L'),('XL');
INSERT INTO colors VALUES ('Red'),('Blue'),('Green');

SELECT s.size, c.color
FROM sizes s
CROSS JOIN colors c
ORDER BY s.size, c.color;
-- Returns 12 rows (4 sizes x 3 colors)

-- Example 16: Generate a date series using CROSS JOIN
SELECT
    d.day_offset,
    CURRENT_DATE + d.day_offset AS calendar_date
FROM generate_series(0, 29) AS d(day_offset)
ORDER BY d.day_offset;
```

---

## SELF JOIN

A table joined to **itself**. Used for hierarchical data, finding duplicates, or comparing rows within the same table.

### Examples

```sql
-- Example 17: Employee-Manager hierarchy using SELF JOIN
SELECT
    e.employee_name  AS employee,
    m.employee_name  AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id
ORDER BY m.employee_name NULLS FIRST;

-- Example 18: Find employees earning more than their manager
SELECT
    e.employee_name  AS employee,
    e.salary         AS emp_salary,
    m.employee_name  AS manager,
    m.salary         AS mgr_salary
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;

-- Example 19: Find duplicate customers by name
SELECT
    a.customer_id,
    a.customer_name,
    b.customer_id AS duplicate_id
FROM customers a
JOIN customers b
    ON  a.customer_name = b.customer_name
    AND a.customer_id   < b.customer_id;

-- Example 20: Customer pairs in the same city
SELECT
    a.customer_name AS customer1,
    b.customer_name AS customer2,
    a.city
FROM customers a
JOIN customers b
    ON  a.city        = b.city
    AND a.customer_id < b.customer_id
ORDER BY a.city;
```

---

## Multi-table JOINs

```sql
-- Example 21: 4-table join with aggregation
SELECT
    c.customer_name,
    c.country,
    COUNT(DISTINCT o.order_id)  AS orders,
    COUNT(oi.item_id)           AS line_items,
    SUM(oi.quantity * oi.unit_price) AS revenue
FROM customers  c
JOIN orders     o  ON c.customer_id  = o.customer_id
JOIN order_items oi ON o.order_id   = oi.order_id
JOIN products   p  ON oi.product_id  = p.product_id
WHERE p.category = 'Electronics'
GROUP BY c.customer_name, c.country
ORDER BY revenue DESC;
```

---

## ASCII Venn Diagrams

### All JOIN Types at a Glance

```
INNER JOIN          LEFT JOIN           RIGHT JOIN
  A    B              A    B              A    B
(( ## ))            (((##))             ((##)))
 only##only          all  only           only  all

FULL OUTER JOIN     LEFT ANTI-JOIN      RIGHT ANTI-JOIN
  A    B              A    B              A    B
(((##)))            (((  ))             ((  )))
 all  all            only excluded       excluded only

CROSS JOIN: Every row in A x Every row in B (no condition)
```

---

## Execution Plan Concepts

PostgreSQL uses three physical join algorithms:

```
1. NESTED LOOP JOIN
   ─────────────────
   For each row in outer table, scan inner table.
   Best for: small tables, indexed inner table.
   Cost: O(N * M) worst case, O(N * log M) with index.

   [Outer Table Scan]
       |
       +---> [Index Scan on Inner Table]  (repeated N times)

2. HASH JOIN
   ──────────
   Build a hash table from smaller table, probe with larger.
   Best for: large tables, equality conditions, no useful index.
   Cost: O(N + M) but requires memory for hash table.

   [Seq Scan smaller table] --> [Build Hash Table]
   [Seq Scan larger table]  --> [Probe Hash Table]

3. MERGE JOIN
   ───────────
   Both tables sorted on join key, then merged like merge-sort.
   Best for: both tables already sorted (index scans), range joins.
   Cost: O(N log N + M log M) if not pre-sorted.

   [Sort Table A on key] --> [Merge] <-- [Sort Table B on key]
```

```sql
-- View the actual execution plan
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT c.customer_name, o.total_amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

---

## Common Mistakes

### Mistake 1: Missing JOIN condition (accidental CROSS JOIN)

```sql
-- WRONG: Cartesian product — every customer paired with every order
SELECT c.customer_name, o.order_id
FROM customers c, orders o;          -- old comma syntax, no condition!

-- CORRECT:
SELECT c.customer_name, o.order_id
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

### Mistake 2: Filtering on a LEFT JOIN column in WHERE (converts to INNER)

```sql
-- WRONG: The WHERE clause eliminates NULLs, defeating the LEFT JOIN
SELECT c.customer_name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.total_amount > 100;          -- removes rows where o is NULL

-- CORRECT: Push the filter into the ON clause
SELECT c.customer_name, o.order_id
FROM customers c
LEFT JOIN orders o
    ON  c.customer_id  = o.customer_id
    AND o.total_amount > 100;
```

### Mistake 3: Ambiguous column names

```sql
-- WRONG: Which table does customer_id come from?
SELECT customer_id, order_date FROM customers JOIN orders ON ...;

-- CORRECT: Always qualify column names
SELECT c.customer_id, o.order_date FROM customers c JOIN orders o ON ...;
```

### Mistake 4: Duplicate rows from one-to-many JOINs

```sql
-- If a customer has 3 orders, aggregation on customer side inflates
SELECT c.customer_name, SUM(c.budget_limit)   -- BUG: summed 3 times
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name;

-- CORRECT: Aggregate before joining, or use DISTINCT
SELECT c.customer_name, COUNT(DISTINCT o.order_id) AS orders
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name;
```

---

## Best Practices

1. Always use explicit JOIN syntax (not comma-separated tables).
2. Always qualify column names with table alias when joining.
3. Use meaningful aliases (c for customers, o for orders).
4. Put the smaller/filtered table on the left for INNER JOINs when possible.
5. Index foreign key columns used in JOIN conditions.
6. Prefer LEFT JOIN over RIGHT JOIN for readability (swap table order instead).
7. Filter early — apply WHERE conditions to reduce rows before joining.
8. Use EXPLAIN ANALYZE to verify the join strategy chosen by PostgreSQL.

---

## Performance Considerations

```sql
-- 1. Ensure foreign key columns are indexed
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);

-- 2. Use partial indexes for common filter + join patterns
CREATE INDEX idx_orders_recent
    ON orders(customer_id)
    WHERE order_date >= '2024-01-01';

-- 3. Set work_mem higher for large hash joins (session level)
SET work_mem = '256MB';

-- 4. Use EXPLAIN (ANALYZE, BUFFERS) to diagnose slow joins
EXPLAIN (ANALYZE, BUFFERS)
SELECT c.customer_name, SUM(o.total_amount)
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_name;

-- 5. Avoid functions on join columns (prevents index use)
-- SLOW:
SELECT * FROM orders o JOIN customers c
    ON LOWER(c.email) = LOWER(o.email);
-- FAST: store email already lowercased, or use a functional index
```

---

## Interview Questions & Answers

**Q1: What is the difference between INNER JOIN and LEFT JOIN?**

A: INNER JOIN returns only rows that have a match in both tables. LEFT JOIN returns all rows from the left table plus matching rows from the right; unmatched right-side columns are NULL. Use LEFT JOIN when you need to preserve all records from one side regardless of whether a match exists.

**Q2: How do you find records in Table A that have no match in Table B?**

A: Use a LEFT JOIN anti-join pattern:
```sql
SELECT a.* FROM a LEFT JOIN b ON a.id = b.a_id WHERE b.a_id IS NULL;
```
Or use NOT EXISTS / NOT IN, which may perform differently.

**Q3: What is a CROSS JOIN and when would you use it?**

A: A CROSS JOIN produces the Cartesian product — every row of A combined with every row of B. Use it to generate all combinations (e.g., all product-size-color variants, calendar scaffolding, test data generation).

**Q4: What happens when you add a WHERE clause filter on the right-table column of a LEFT JOIN?**

A: It effectively converts the LEFT JOIN into an INNER JOIN because NULL values (from unmatched left rows) are eliminated by the WHERE condition. To preserve the outer join behavior, move the condition into the ON clause.

**Q5: Explain SELF JOIN with a real-world example.**

A: A SELF JOIN joins a table to itself. Classic use case: employee-manager relationships stored in the same table. You alias the table twice — once as "employee" and once as "manager" — and join on `employee.manager_id = manager.employee_id`.

**Q6: What join algorithm does PostgreSQL use by default for large tables?**

A: PostgreSQL chooses among Nested Loop, Hash Join, and Merge Join based on cost estimates. Hash Join is common for large tables with no useful index on the join key. The planner uses table statistics to decide.

**Q7: How do you write a FULL OUTER JOIN to find unmatched rows on both sides?**

A:
```sql
SELECT * FROM a FULL OUTER JOIN b ON a.id = b.id
WHERE a.id IS NULL OR b.id IS NULL;
```

**Q8: What is the difference between JOIN condition in ON vs WHERE?**

A: For INNER JOIN, the result is identical — the planner sees them the same way. For OUTER JOINs, it matters: the ON clause filters before the outer join is applied (preserving unmatched rows), while WHERE filters after (eliminating unmatched rows).

**Q9: How can JOINs cause row duplication?**

A: A one-to-many relationship causes the "one" side row to appear multiple times — once per matching row on the "many" side. This inflates aggregations. Solutions: aggregate in a subquery before joining, or use COUNT(DISTINCT).

**Q10: What is a lateral join?**

A: A LATERAL join allows a subquery in the FROM clause to reference columns from preceding tables in the FROM list. This enables correlated subqueries in FROM, useful for top-N per group problems.

---

## Hands-on Exercises

### Exercise 1: Customer Order Summary
Write a query that returns every customer's name, total number of orders, and total amount spent. Include customers with zero orders (show 0, not NULL).

```sql
-- Solution:
SELECT
    c.customer_name,
    COUNT(o.order_id)              AS total_orders,
    COALESCE(SUM(o.total_amount), 0) AS total_spent
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_spent DESC;
```

### Exercise 2: Products Never Ordered
Find all products that have never appeared in any order.

```sql
-- Solution:
SELECT p.product_id, p.product_name, p.category
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;
```

### Exercise 3: Manager Salary Report
List each employee, their manager's name, and whether the employee earns more than their manager (Yes/No).

```sql
-- Solution:
SELECT
    e.employee_name,
    COALESCE(m.employee_name, 'No Manager') AS manager,
    e.salary                                AS emp_salary,
    m.salary                                AS mgr_salary,
    CASE WHEN e.salary > m.salary THEN 'Yes' ELSE 'No' END AS earns_more
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id
ORDER BY e.employee_name;
```

### Exercise 4: Data Quality — Orphan Orders
Find all orders where the customer_id does not exist in the customers table.

```sql
-- Solution:
SELECT o.order_id, o.customer_id, o.order_date, o.total_amount
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;
```

### Exercise 5: Top Product per City
For each city, find the product that generated the most revenue.

```sql
-- Solution using LATERAL:
SELECT DISTINCT ON (c.city)
    c.city,
    p.product_name,
    SUM(oi.quantity * oi.unit_price) AS city_revenue
FROM customers c
JOIN orders     o  ON c.customer_id  = o.customer_id
JOIN order_items oi ON o.order_id   = oi.order_id
JOIN products   p  ON oi.product_id  = p.product_id
GROUP BY c.city, p.product_name
ORDER BY c.city, city_revenue DESC;
```

### Exercise 6: CROSS JOIN Calendar
Generate a grid of all months (1-12) for years 2024 and 2025.

```sql
-- Solution:
SELECT y.yr, m.mon
FROM (VALUES (2024),(2025)) AS y(yr)
CROSS JOIN generate_series(1,12) AS m(mon)
ORDER BY y.yr, m.mon;
```

---

## Advanced Notes

### Non-Equi JOINs

JOINs do not have to use equality. You can join on ranges, inequalities, or complex conditions:

```sql
-- Find orders that fall within each customer's "preferred range"
-- (hypothetical: customer has a min/max order amount preference)
SELECT c.customer_name, o.total_amount
FROM customers c
JOIN orders o
    ON  o.customer_id    = c.customer_id
    AND o.total_amount BETWEEN 100 AND 500;

-- Date range join: which promotions were active on each order date?
SELECT o.order_id, pr.promotion_name
FROM orders o
JOIN promotions pr
    ON o.order_date BETWEEN pr.start_date AND pr.end_date;
```

### USING Clause

When both tables share the exact same column name for the join key, use `USING`:

```sql
SELECT customer_name, order_id
FROM customers
JOIN orders USING (customer_id);
-- customer_id appears only once in the result (no ambiguity)
```

### JOIN Order and Query Planner

The order of tables in your SQL does not rigidly determine join order — PostgreSQL's planner re-orders joins for efficiency (up to 8 tables by default, controlled by `join_collapse_limit`). For more than 8 tables, it uses a genetic algorithm.

```sql
-- Force a specific join order (rarely needed)
SET join_collapse_limit = 1;
```

---

## Cross-references

- **02_aggregations.md** — GROUP BY and aggregation after JOINs
- **03_subqueries.md** — Subqueries as an alternative to JOINs
- **04_exists_any_all.md** — EXISTS as an efficient anti-join
- **07_lateral_joins.md** (04_Advanced_SQL) — LATERAL for correlated subqueries in FROM
- **07_Indexes/** — Index strategies to accelerate JOINs
- **08_Query_Optimization/** — EXPLAIN ANALYZE for join plan analysis
