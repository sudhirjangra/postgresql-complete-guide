# 23 — Company-Specific Interview Preparation

Company-specific PostgreSQL and SQL interview guides for top tech companies.

## Files

| File | Company | Key Focus |
|---|---|---|
| `01_amazon_preparation.md` | Amazon / AWS | LP principles, Aurora PostgreSQL, order management design |
| `02_google_preparation.md` | Google | Query optimization, distributed systems (Spanner), BigQuery |
| `03_microsoft_preparation.md` | Microsoft | Azure PostgreSQL, T-SQL vs PG differences, enterprise features |
| `04_meta_preparation.md` | Meta | Petabyte-scale SQL, Presto, social graph, A/B testing |
| `05_uber_preparation.md` | Uber | PostGIS geospatial (15 examples), H3 hexagons, real-time location |
| `06_netflix_preparation.md` | Netflix | Streaming analytics, A/B testing, recommendation systems |
| `07_general_tech_companies.md` | Multi-company | Airbnb, Stripe, DoorDash, Snowflake, Databricks, LinkedIn, Atlassian |

## Each File Contains

1. Company Profile (culture, DB tech stack, what they value)
2. Interview Process (rounds, format, duration, level expectations)
3. SQL Coding Round (20–25 questions with full solutions)
4. Database Design Tasks (3–5 complete schema designs)
5. System Design with DB Focus (2–3 scenarios)
6. Behavioral Questions (role-specific)
7. Mock Interview Script (full Q&A dialogue)
8. Evaluation Rubric (L3/L4/L5/L6 bar)
9. Red Flags (what tanks candidates)
10. Green Flags (what impresses interviewers)
11. Salary Bands (approximate total comp by level)
12. Preparation Timeline (4-week plan)

## Quick Start by Company

### If interviewing at Amazon
- Focus on Leadership Principles first
- Study Aurora PostgreSQL vs. vanilla PG differences
- Practice LP storytelling with DB metrics

### If interviewing at Google
- Master EXPLAIN ANALYZE and execution plans
- Study Cloud Spanner and BigQuery alongside PostgreSQL
- Practice query optimization patterns

### If interviewing at Microsoft
- Learn Azure PostgreSQL Flexible Server specifics
- Study row-level security, encryption, compliance features
- Practice Azure-flavored system design

### If interviewing at Meta
- Every design must start at "billions of rows"
- Study Presto/Trino SQL for analytics
- Practice social graph SQL patterns

### If interviewing at Uber
- PostGIS is mandatory — practice all 15 geospatial queries
- Understand H3 hexagonal grid indexing
- Practice real-time vs. durable data split patterns

### If interviewing at Netflix
- Study streaming analytics patterns (Kafka, Flink)
- Practice A/B test analysis SQL
- Understand Freedom & Responsibility culture

### If interviewing at Stripe, DoorDash, Atlassian, etc.
- Study the `07_general_tech_companies.md` company-specific sections
- Match SQL style to company domain
- Research the engineering blog before the interview

## Common SQL Patterns Tested Everywhere

```sql
-- Top N per group
SELECT * FROM (...RANK() OVER (PARTITION BY cat ORDER BY val DESC)...) WHERE rnk <= N;

-- Cohort retention
-- Date spine with generate_series + LEFT JOIN for gap filling
-- Running totals and moving averages
-- Session analysis (gap-based grouping)
-- Funnel conversion rates
-- Year-over-year comparison with LAG or date filtering
```

## Salary Ranges Summary (USD, 2024–2025, mid-level/senior)

| Company | Mid-Level Total Comp | Senior Total Comp |
|---|---|---|
| Google L5 | $360K–$570K | $560K–$900K |
| Meta E5 | $390K–$620K | $700K–$1.2M |
| Netflix L5 | $310K–$440K | $440K–$700K |
| Amazon SDE III | $290K–$420K | $450K–$700K |
| Microsoft Sr SDE | $280K–$420K | $440K–$700K |
| Stripe L3 | $300K–$450K | $450K–$650K |
| Snowflake Senior | $380K–$600K | $600K–$950K |
| Uber L5 | $300K–$480K | $500K–$750K |
| Databricks Senior | $400K–$650K | $650K–$950K |
