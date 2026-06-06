# Week 1: Setup & SQL Foundations

## Phase 1: Foundations | Week 1 of 12

---

## Week Overview

This week you set up your PostgreSQL environment and learn the core SQL building blocks: how databases and tables are structured, how to insert and query data, how to filter and sort results, and how to use basic functions. By Friday you will have a working PostgreSQL installation, a sample database, and confidence with SELECT, INSERT, UPDATE, DELETE, and WHERE clauses.

**Focus:** Correctness over speed. Get every query right before moving faster.

---

## Learning Objectives

By the end of this week, you will be able to:

- Install PostgreSQL and connect via `psql` and a GUI tool.
- Create databases, schemas, and tables with appropriate data types.
- Insert, update, and delete records safely.
- Write SELECT queries with WHERE, ORDER BY, LIMIT, and OFFSET.
- Use basic functions: `UPPER`, `LOWER`, `LENGTH`, `COALESCE`, `NOW()`, date functions.
- Understand NULL semantics: `IS NULL`, `IS NOT NULL`, `COALESCE`.
- Use `LIKE` and `ILIKE` for pattern matching.
- Export query results to CSV.

---

## Required Reading

- `01_Fundamentals/` — All files
- `02_SQL_Basics/` — Files 01 through 06

---

## Daily Schedule

### Monday — Environment Setup + Database Theory (60 min)

**Topics:**
- What is a relational database and why PostgreSQL?
- ACID properties (Atomicity, Consistency, Isolation, Durability)
- Installing PostgreSQL 16
- Connecting with `psql` and your GUI tool
- `\l`, `\c`, `\dt`, `\d table_name` meta-commands

**Exercises:**
```sql
-- Connect and explore
\l                          -- List all databases
CREATE DATABASE week1_practice;
\c week1_practice
\dt                         -- List tables (empty)

-- First table
CREATE TABLE employees (
    id         SERIAL PRIMARY KEY,
    first_name VARCHAR(100) NOT NULL,
    last_name  VARCHAR(100) NOT NULL,
    email      VARCHAR(200) UNIQUE,
    salary     NUMERIC(10,2),
    hire_date  DATE DEFAULT CURRENT_DATE,
    department VARCHAR(100)
);

\d employees                -- Describe table structure
```

---

### Tuesday — Data Types and INSERT (90 min)

**Topics:**
- PostgreSQL data types: INTEGER, BIGINT, NUMERIC, TEXT, VARCHAR, DATE, TIMESTAMPTZ, BOOLEAN, UUID
- NULL vs. empty string
- INSERT single and multiple rows
- INSERT ... RETURNING
- UPDATE and DELETE safety

**Exercises:**
```sql
-- Insert sample data
INSERT INTO employees (first_name, last_name, email, salary, department)
VALUES
  ('Alice',   'Johnson', 'alice@co.com',  75000, 'Engineering'),
  ('Bob',     'Smith',   'bob@co.com',    65000, 'Marketing'),
  ('Carol',   'Lee',     'carol@co.com',  90000, 'Engineering'),
  ('David',   'Kim',     'david@co.com',  55000, 'HR'),
  ('Emma',    'Garcia',  'emma@co.com',   82000, 'Engineering'),
  ('Frank',   'Wilson',  NULL,            48000, 'Support'),
  ('Grace',   'Chen',    'grace@co.com',  70000, 'Marketing');

-- Safe UPDATE with WHERE
UPDATE employees SET salary = 85000 WHERE email = 'alice@co.com';
UPDATE employees SET department = 'Sales' WHERE department = 'Marketing';

-- DELETE safely
DELETE FROM employees WHERE id = 6 RETURNING *;
```

---

### Wednesday — SELECT, WHERE, ORDER BY (90 min)

**Topics:**
- SELECT column list vs. SELECT *
- WHERE with: =, !=, >, <, >=, <=, BETWEEN, IN, NOT IN
- AND, OR, NOT in WHERE clauses
- ORDER BY (multiple columns, ASC/DESC)
- LIMIT and OFFSET for pagination

**Exercises:**
```sql
-- Basic filtering
SELECT * FROM employees WHERE department = 'Engineering';
SELECT first_name, last_name, salary FROM employees WHERE salary > 70000;
SELECT * FROM employees WHERE salary BETWEEN 60000 AND 85000;
SELECT * FROM employees WHERE department IN ('Engineering', 'Marketing');

-- NULL handling
SELECT * FROM employees WHERE email IS NULL;
SELECT * FROM employees WHERE email IS NOT NULL;

-- Sorting
SELECT * FROM employees ORDER BY salary DESC;
SELECT * FROM employees ORDER BY department, last_name;

-- Pagination
SELECT * FROM employees ORDER BY id LIMIT 3 OFFSET 0;  -- Page 1
SELECT * FROM employees ORDER BY id LIMIT 3 OFFSET 3;  -- Page 2

-- LIKE and ILIKE
SELECT * FROM employees WHERE first_name ILIKE 'a%';
SELECT * FROM employees WHERE email LIKE '%@co.com';
```

---

### Thursday — Functions and Expressions (60 min)

**Topics:**
- String functions: UPPER, LOWER, LENGTH, TRIM, CONCAT, SUBSTRING
- Math functions: ROUND, CEIL, FLOOR, ABS, MOD
- Date functions: NOW(), CURRENT_DATE, DATE_TRUNC, EXTRACT, AGE
- COALESCE and NULLIF
- Aliases with AS

**Exercises:**
```sql
-- String functions
SELECT
    UPPER(first_name) AS first_upper,
    LOWER(last_name)  AS last_lower,
    LENGTH(first_name) AS name_length,
    first_name || ' ' || last_name AS full_name,
    COALESCE(email, 'No email') AS email_display
FROM employees;

-- Math and formatting
SELECT
    first_name,
    salary,
    ROUND(salary * 1.10, 2)  AS salary_with_raise,
    ROUND(salary / 12, 2)    AS monthly_salary
FROM employees;

-- Date functions
SELECT
    first_name,
    hire_date,
    AGE(hire_date)          AS tenure,
    EXTRACT(YEAR FROM hire_date) AS hire_year,
    CURRENT_DATE - hire_date AS days_employed
FROM employees;
```

---

### Friday — Review and Mini-Project (45 min)

**Mini-Project:** Create a simple bookstore schema and load data.

```sql
CREATE TABLE authors (id SERIAL PRIMARY KEY, name VARCHAR(200), country VARCHAR(100));
CREATE TABLE books (
    id           SERIAL PRIMARY KEY,
    title        VARCHAR(400) NOT NULL,
    author_id    INTEGER REFERENCES authors(id),
    price        NUMERIC(8,2),
    published_on DATE,
    in_stock     BOOLEAN DEFAULT TRUE
);

-- Load 5 authors and 10 books (make up realistic data)
-- Write 5 queries covering this week's topics
```

---

## Practice Tasks

1. Write a query that returns all employees sorted by salary descending, showing rank using ROW_NUMBER (preview — we'll cover this in Week 3).
2. Find all employees hired in the last 365 days.
3. List employees where salary is NULL or email is NULL.
4. Concatenate first_name + ' ' + last_name into a "full_name" column.
5. Calculate the annual cost of all employees' salaries combined.
6. List the top 3 highest-paid employees.
7. Find employees whose name starts with a vowel (A, E, I, O, U).
8. Show the percentage difference between each employee's salary and the average salary using a subquery.

---

## Self-Assessment Checklist

- [ ] PostgreSQL is installed and I can connect via `psql`
- [ ] I can create tables with correct data types
- [ ] I understand the difference between NULL and empty string
- [ ] I can write SELECT with WHERE, ORDER BY, LIMIT
- [ ] I know at least 5 string functions and 3 date functions
- [ ] I understand how COALESCE handles NULLs
- [ ] I can safely UPDATE and DELETE rows using WHERE
- [ ] I completed the bookstore mini-project

---

## Mock Interview Questions

1. What is the difference between `WHERE salary = NULL` and `WHERE salary IS NULL`?
2. Explain ACID properties in the context of a banking transaction.
3. What is the difference between VARCHAR and TEXT in PostgreSQL?
4. If I have 1 million rows and no index, how does PostgreSQL find a row by email?
5. What does `COALESCE(NULL, NULL, 5)` return? Why?
6. Write a query that returns the 10th to 20th employees sorted by last name alphabetically.
7. What is the difference between `DELETE FROM employees` and `TRUNCATE employees`?
8. Explain what a PRIMARY KEY is and why tables should have one.

---

## Resources

- Official Docs: https://www.postgresql.org/docs/16/tutorial.html
- This repo: `01_Fundamentals/`, `02_SQL_Basics/`
- Interactive practice: https://pgexercises.com (beginner section)
- `24_Cheat_Sheets/` — Quick reference card
