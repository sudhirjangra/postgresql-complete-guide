# Week 11: Interview Preparation

## Phase 4: Architecture & Career | Week 11 of 12

---

## Week Overview

This week is dedicated to converting your 10 weeks of knowledge into interview performance. You will practice answering questions under time pressure, solve live coding challenges, design systems on a whiteboard, and review all critical concepts. By Friday you are ready for senior PostgreSQL interviews at any tier of company.

**Focus:** The goal is not to memorize answers — it's to speak fluently about databases from deep understanding.

---

## Learning Objectives

By the end of this week, you will be able to:

- Answer any PostgreSQL concept question in under 2 minutes.
- Write SQL queries from scratch under timed conditions.
- Design a database schema live, explaining decisions as you go.
- Debug a slow query using EXPLAIN ANALYZE in a live session.
- Handle adversarial follow-up questions with composure.
- Structure answers using the STAR framework for behavioral questions.
- Assess your own weaknesses and address them before the capstone.

---

## Required Reading

- `19_Interview_Questions/` — All files
- `20_Interview_Tasks/` — All files
- `21_System_Design/` — All files

---

## Daily Schedule

### Monday — SQL Coding Under Time Pressure (90 min)

**Rules for today:** Each query must be solved in under 5 minutes. No looking at notes.

```sql
-- TIMED CHALLENGE SET 1 (5 minutes each)

-- Q1: Given a users table (id, name, signup_date) and an orders table (id, user_id, amount, created_at),
-- find users who signed up in the last 30 days but have NOT placed any orders yet.

-- Q2: From an employee table (id, name, department, salary, manager_id),
-- find the average salary for each department, only for departments where
-- the highest earner makes more than $100,000.

-- Q3: From a sessions table (session_id, user_id, started_at, ended_at),
-- find the longest session per user in the last 7 days.

-- Q4: Given a products table and an order_items table,
-- find products that were ordered in January 2024 but NOT in February 2024.

-- Q5: From a transactions table (id, account_id, type, amount, created_at),
-- calculate the running balance per account ordered by created_at.

-- Q6: Find the top 3 customers by revenue each month for the last 6 months.
-- Expected columns: month, customer_id, revenue, monthly_rank

-- Q7: Write a query to detect duplicate emails in a users table.
-- Return: email, count, list of user IDs that share it.

-- Q8: Given a tree table (id, parent_id, name), find all nodes at depth 3.
```

---

### Tuesday — System Design Interviews (90 min)

**For each system, spend 20 minutes designing the schema and explaining decisions:**

**System 1: URL Shortener**
```
Requirements:
- Shorten long URLs to short codes (e.g., bit.ly/abc123)
- Track click analytics (who, when, where from)
- Custom short codes
- Link expiration
- Rate limiting per user

Design questions:
1. What tables do you need?
2. How do you generate unique short codes efficiently?
3. How do you handle click counting at high volume without locking?
4. How would you partition the analytics table?
5. What indexes are critical for read performance?
```

**System 2: Notification Service**
```
Requirements:
- Send push, email, and SMS notifications to users
- Notification preferences per user per channel
- Batching: send digests instead of individual emails
- Deduplication: don't send same notification twice
- Audit trail of all sent notifications

Design questions:
1. How do you model notification templates?
2. How do you handle the fan-out problem (1 event → millions of notifications)?
3. What's your schema for user preferences?
4. How do you ensure exactly-once delivery at the database level?
5. How would you implement the deduplication check efficiently?
```

---

### Wednesday — Performance Debugging Live (90 min)

**Scenario-based debugging exercises:**

```sql
-- SCENARIO 1: The slow dashboard query
-- A product manager reports the dashboard loads in 25 seconds.
-- Here is the query:

SELECT
    u.name,
    COUNT(o.id) AS order_count,
    SUM(o.total) AS revenue,
    MAX(o.created_at) AS last_order
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE DATE(o.created_at) >= '2024-01-01'
GROUP BY u.id, u.name
ORDER BY revenue DESC
LIMIT 50;

-- Tasks:
-- 1. Run EXPLAIN ANALYZE (simulate it mentally or with test data)
-- 2. Identify at least 3 problems with this query
-- 3. Rewrite it to be faster
-- 4. What indexes would you add?

-- SCENARIO 2: The growing table problem
-- orders table has 500M rows, no partitioning
-- 80% of queries filter on created_at last 30 days
-- VACUUM is falling behind
-- Options to discuss: partition by range, archive old data, summary tables

-- SCENARIO 3: Connection storm
-- After a deploy, the application gets 5,000 connections instantly
-- PostgreSQL max_connections = 200
-- What do you do right now? (kill, PgBouncer, application config)
-- How do you prevent this in the future?

-- SCENARIO 4: Deadlock in production
-- Alert fires: deadlock detected
-- Logs show: process 12345 deadlock on accounts table
-- Walk through: how to diagnose, how to fix, how to prevent

-- SCENARIO 5: Replication lag
-- Replica is 45 minutes behind primary
-- Application reads are going to replica
-- Users are seeing stale data
-- Immediate action? Root cause investigation? Long-term fix?
```

---

### Thursday — Concept Deep-Dives + Behavioral (60 min)

**Part 1: Fast-fire concept questions (2 minutes each)**

Answer verbally (aloud) without notes:

1. Explain MVCC in 60 seconds.
2. What is a GIN index? Name 3 use cases.
3. What is the purpose of VACUUM?
4. Explain the difference between READ COMMITTED and SERIALIZABLE.
5. How does Patroni prevent split-brain?
6. What is a replication slot and why can it be dangerous?
7. When would you use BRIN instead of B-Tree?
8. What does EXPLAIN ANALYZE tell you that EXPLAIN doesn't?
9. Explain Row-Level Security with a real example.
10. What happens when transaction IDs wrap around?

**Part 2: Behavioral questions (STAR format)**

Prepare 2-3 minute answers for:

1. Tell me about a time you optimized a slow database query. What was your process?
2. Describe a database outage you experienced (or studied). What happened?
3. Tell me about a schema design decision you're proud of and why.
4. Describe a situation where you had to explain a database trade-off to a non-technical stakeholder.
5. Tell me about the most complex SQL query you've written. Walk me through it.

---

### Friday — Full Mock Interview + Gap Analysis (90 min)

**Conduct a full 60-minute mock interview with a peer or record yourself:**

```
Interview Format:
[0-5 min]   Introduce yourself + your PostgreSQL experience
[5-20 min]  SQL coding challenge (live, under pressure)
[20-40 min] System design: "Design the database for a ride-sharing app"
[40-55 min] Concept questions (choose 6 from the week's list)
[55-60 min] Questions for the interviewer (practice asking good questions)

After the mock interview:
- Score yourself on each dimension (SQL, design, concepts, communication)
- List your 3 weakest areas
- Spend Saturday/Sunday on those specific weak areas
```

---

## Complete Interview Question Bank

### Tier 1: Fundamentals (junior - must answer in <60 seconds)

1. What is a primary key?
2. What is the difference between NULL and empty string?
3. What does INNER JOIN do?
4. What is the difference between WHERE and HAVING?
5. How do you find duplicate rows in a table?

### Tier 2: Intermediate (mid-level - 1-2 minutes)

1. Explain window functions with an example.
2. What is a CTE? When would you use it vs. a subquery?
3. What is normalization? Give an example of a 3NF violation.
4. Explain what an index is and how B-Tree works internally.
5. What is MVCC?

### Tier 3: Advanced (senior - 2-5 minutes + follow-ups)

1. You have a 5TB table with no indexes and queries are slow. Walk me through how you'd analyze and fix this.
2. Design a database for a financial ledger with no data loss.
3. Explain all four isolation levels, their anomalies, and when you'd use each.
4. Your PostgreSQL primary goes down in production. What happens next?
5. How would you implement a queue using PostgreSQL?

### Tier 4: Architecture (staff/principal - 10+ minutes)

1. Design a multi-region database architecture for a global e-commerce platform.
2. How would you migrate a 2TB PostgreSQL database with zero downtime?
3. Design a real-time analytics system processing 1 million events per minute.
4. How would you handle a scenario where your replica is used for reads and falls 10 minutes behind?
5. Explain the full lifecycle of a query in PostgreSQL, from parser to executor.

---

## Self-Assessment Checklist

- [ ] I can answer all Tier 1 questions instantly
- [ ] I can answer Tier 2 questions in under 2 minutes
- [ ] I attempted all 8 timed SQL challenges
- [ ] I designed 2 systems on paper without notes
- [ ] I completed a full mock interview
- [ ] I identified my top 3 weak areas
- [ ] I prepared STAR stories for all 5 behavioral questions

---

## Resources

- This repo: `19_Interview_Questions/`, `20_Interview_Tasks/`, `21_System_Design/`
- `23_Company_Preparation/` — Company-specific focus areas
- `24_Cheat_Sheets/` — Quick reference for final review
