# Capstone Program: PostgreSQL Mastery

## 12-Week Intensive Program — From SQL Basics to Production Expert

---

## Program Overview

This 12-week capstone program transforms you from a SQL learner into a PostgreSQL expert capable of designing production systems, optimizing performance, and excelling in technical interviews at top-tier technology companies. The program combines structured daily study (1-2 hours/day), hands-on projects, mock interviews, and a final capstone project.

**Total Commitment:** 12 weeks, 1-2 hours/day, 5 days/week  
**Total Hours:** ~120-160 hours  
**Outcome:** Production-ready PostgreSQL proficiency + interview-ready

---

## Prerequisites

Before starting this program, you should be comfortable with:

- Basic programming concepts (variables, loops, conditionals)
- General understanding of what a database is
- Command-line / terminal usage
- Ability to install software on your machine

**Not required:** Prior SQL experience (Week 1 starts from scratch)

---

## Learning Outcomes

Upon completion of this program, you will be able to:

### Technical Skills
- Write complex SQL queries including window functions, CTEs, and subqueries.
- Design normalized, production-quality database schemas.
- Implement PL/pgSQL stored functions and triggers.
- Configure and tune PostgreSQL for production workloads.
- Set up streaming replication and high-availability clusters.
- Diagnose and resolve performance bottlenecks using EXPLAIN ANALYZE.
- Implement security best practices (RLS, roles, encryption).
- Back up and restore PostgreSQL databases.
- Monitor and alert on key database metrics.

### Career Skills
- Answer any PostgreSQL interview question with confidence.
- Design systems under pressure (whiteboard/live coding).
- Communicate database decisions to both technical and non-technical stakeholders.
- Build a portfolio of 9 complete database projects.
- Present a final capstone project to simulated reviewers.

---

## Program Structure

```
PHASE 1: FOUNDATIONS (Weeks 1-2)
├── Week 1: Environment Setup + SQL Basics
└── Week 2: Intermediate SQL (JOINs, Aggregations)

PHASE 2: POSTGRESQL MASTERY (Weeks 3-6)
├── Week 3: Advanced SQL (Window Functions, CTEs)
├── Week 4: PostgreSQL Core (Types, Constraints, JSONB)
├── Week 5: Database Design (Normalization, ER Modeling)
└── Week 6: Indexes & Query Optimization

PHASE 3: INTERNALS & OPERATIONS (Weeks 7-9)
├── Week 7: Transactions, MVCC, Locking
├── Week 8: Advanced Features (Partitioning, FTS, Extensions)
└── Week 9: Production Ops (Monitoring, Security, Backup)

PHASE 4: ARCHITECTURE & CAREER (Weeks 10-12)
├── Week 10: Replication, HA, PgBouncer, Patroni
├── Week 11: Interview Preparation + Mock Interviews
└── Week 12: Capstone Project + Final Assessment
```

---

## Weekly Time Commitment

| Day | Activity | Time |
|-----|----------|------|
| Monday | Reading + Theory | 60 min |
| Tuesday | Hands-on SQL Practice | 90 min |
| Wednesday | Mini-Project / Exercises | 90 min |
| Thursday | Mock Interview Questions | 60 min |
| Friday | Review + Week Prep | 45 min |
| Weekend | Optional: Extension challenges, blog posts | 0-120 min |

---

## Tools Required

```bash
# Core
PostgreSQL 16 (local install)
psql (command-line client)
pg_dump / pg_restore

# Recommended GUI
DBeaver (free, cross-platform)
pgAdmin 4 (free, web-based)
TablePlus (paid, excellent UX)

# Optional but valuable
Docker (for multi-node setups)
git (for tracking your work)
VS Code with PostgreSQL extension
```

---

## Evaluation Criteria

Your progress is evaluated across five dimensions:

### 1. SQL Query Quality (25%)
- Correctness: Does the query return the right results?
- Efficiency: Does it use appropriate indexes and avoid full scans?
- Readability: Is it formatted and commented clearly?
- Edge Cases: Does it handle NULLs, empty sets, and boundary values?

### 2. Schema Design (25%)
- Normalization: Is it in 3NF without unnecessary redundancy?
- Constraints: Are business rules enforced at the database level?
- Naming: Are conventions consistent and meaningful?
- Scalability: Would this design survive 100x data growth?

### 3. Performance Optimization (20%)
- Index selection: Right types for the right query patterns.
- EXPLAIN usage: Can you read and act on execution plans?
- Partitioning: Appropriate use of table partitioning.
- Configuration: Basic understanding of memory and parallelism settings.

### 4. Production Readiness (20%)
- Security: RLS, roles, least-privilege access.
- Monitoring: Key metrics identified and tracked.
- Backup strategy: Demonstrated backup and restore procedures.
- High Availability: Basic replication understanding.

### 5. Communication (10%)
- Interview responses: STAR format, concise, technically accurate.
- Design explanations: Can defend schema choices.
- Trade-off awareness: Understands when to denormalize, when not to.

---

## Grading Scale

| Score | Level | Description |
|-------|-------|-------------|
| 90-100 | Expert | Ready for senior DB engineer or DBA roles |
| 75-89  | Proficient | Ready for mid-level backend/data engineer roles |
| 60-74  | Competent | Ready for junior roles; continue practicing |
| Below 60 | Developing | Repeat relevant weeks before proceeding |

---

## Capstone Project Options

In Week 12, choose ONE of the following:

1. **E-Commerce Platform**: Full-featured multi-tenant shop with payments, inventory, analytics, and RLS.
2. **Analytics Platform**: Time-series event store with funnel analysis, cohort retention, and real-time dashboards.
3. **Financial Ledger**: Double-entry bookkeeping system with auditing, interest calculation, and regulatory reports.
4. **Open Proposal**: Propose your own domain (must be approved by end of Week 10).

---

## Week-by-Week Map to Repository Chapters

| Week | Topic | Repository Chapters |
|------|-------|---------------------|
| 1    | Foundations | 01_Fundamentals, 02_SQL_Basics |
| 2    | SQL Mastery | 03_Intermediate_SQL |
| 3    | Advanced SQL | 04_Advanced_SQL |
| 4    | PG Core | 05_PostgreSQL_Core |
| 5    | DB Design | 06_Database_Design |
| 6    | Indexes | 07_Indexes, 08_Query_Optimization |
| 7    | Transactions | 09_Transactions_Concurrency, 10_PostgreSQL_Internals |
| 8    | Advanced Features | 11_Advanced_PostgreSQL |
| 9    | Production | 12_Production_PostgreSQL, 14_Security, 15_Backup_Recovery |
| 10   | Replication & HA | 13_Replication_HA |
| 11   | Interview Prep | 19_Interview_Questions, 20_Interview_Tasks |
| 12   | Capstone | 22_Projects |

---

## Community and Support

- Track your daily progress in a private journal or repo.
- Post your queries to the `#sql-review` channel for peer feedback.
- Use the `19_Interview_Questions` chapter for self-testing every Friday.
- If you get stuck, re-read the relevant chapter before asking for help.

---

## Success Tips

1. **Type every query** — never copy-paste from the examples. Muscle memory matters.
2. **Break things on purpose** — drop indexes and observe query degradation.
3. **Read EXPLAIN output daily** — make it a habit, not an emergency tool.
4. **Build projects from memory** — after studying, close the notes and rebuild.
5. **Simulate pressure** — time yourself on interview questions. 5 minutes max per answer.
6. **Teach back** — explain concepts in your own words. If you can't, you don't know it.
