# Assessment Rubric: PostgreSQL Mastery Capstone

## Detailed Evaluation Criteria

This rubric is used for the Week 12 final assessment, the capstone project review, and weekly self-evaluation. Each criterion has three performance levels: Developing (1), Proficient (2), and Expert (3).

---

## Dimension 1: Schema Design (25%)

### 1.1 Normalization (33% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Schema is in 3NF. Any denormalization is documented with clear business justification and query evidence. No update anomalies possible. |
| Proficient | 2 | Schema is in 2NF. At most 1-2 transitive dependencies present. No obvious redundancy. |
| Developing | 1 | Multiple normalization violations. Repeated data in multiple tables. Potential for insert/update/delete anomalies. |

### 1.2 Constraints and Data Integrity (33% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | All business rules enforced at DB level. CHECK constraints on all domain-restricted columns. NOT NULL on all required fields. UNIQUE where appropriate. Custom DOMAINs used for reusable validation. Computed columns for derived values. |
| Proficient | 2 | Primary and foreign keys all correct. Most NOT NULL constraints present. CHECK constraints on critical columns. |
| Developing | 1 | Primary keys present but foreign keys missing or incorrect. Few CHECK constraints. Business rules only enforced in application code. |

### 1.3 Naming and Conventions (17% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Consistent snake_case throughout. Tables are plural. Boolean columns use `is_/has_` prefix. Timestamps use `_at` suffix. Foreign keys follow `table_id` pattern. Acronyms expanded. |
| Proficient | 2 | Mostly consistent. Minor inconsistencies in a few table or column names. |
| Developing | 1 | Mixed conventions. CamelCase and snake_case in same schema. Unclear abbreviations. |

### 1.4 Scalability Consideration (17% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Partitioning applied to high-volume tables. Soft deletes on appropriate tables. Audit trail tables present. Denormalized counters documented. Schema would survive 100x data growth. |
| Proficient | 2 | Basic scalability patterns applied. Partitioning considered. No obvious growth-blockers. |
| Developing | 1 | Schema would become unmanageable at 10x growth. Missing partitioning on time-series tables. |

---

## Dimension 2: SQL Query Quality (25%)

### 2.1 Correctness (40% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | All queries return exactly correct results. NULL handling is explicit. Edge cases (empty set, all-NULL input) handled. Queries verified with test data covering boundary conditions. |
| Proficient | 2 | Most queries correct. May have minor NULL-handling gaps. Correct for happy-path scenarios. |
| Developing | 1 | Several queries return incorrect results. NULL semantics misunderstood. JOINs produce cartesian products in some cases. |

### 2.2 Advanced SQL Usage (30% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Window functions used where appropriate (not just WHERE + subquery). Recursive CTEs for hierarchies. FILTER clause for conditional aggregation. LATERAL joins where applicable. No unnecessary subqueries where CTEs are cleaner. |
| Proficient | 2 | Window functions used in at least 3 queries. CTEs used for readability. Basic aggregations correct. |
| Developing | 1 | Only basic SELECT, JOIN, GROUP BY. No window functions. All aggregations with simple subqueries even when cleaner options exist. |

### 2.3 Readability and Documentation (30% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Every query has a comment explaining business context. Complex expressions explained inline. CTE names are self-documenting. Consistent formatting. Alias columns with meaningful names. |
| Proficient | 2 | Most queries commented. Formatting is consistent. CTE names meaningful. |
| Developing | 1 | Queries lack comments. SELECT * used. Magic numbers with no explanation. |

---

## Dimension 3: PL/pgSQL Functions and Triggers (20%)

### 3.1 Function Correctness and Business Logic (40% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Functions implement all specified business rules correctly. Return types are appropriate. Functions tested with valid, invalid, and boundary inputs. Atomic operations use transactions correctly. |
| Proficient | 2 | Functions implement core logic correctly. Most edge cases handled. |
| Developing | 1 | Functions work for happy path only. Errors in edge cases. Logic bugs present. |

### 3.2 Error Handling and Robustness (30% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | All functions raise meaningful EXCEPTION messages with context (e.g., `RAISE EXCEPTION 'Insufficient funds for account %: balance=%, requested=%', account_id, balance, amount`). NULL inputs validated. Foreign key violations handled. Deadlock-safe using consistent lock ordering. |
| Proficient | 2 | Most error conditions raise exceptions. Basic NULL checks. |
| Developing | 1 | Errors surface as cryptic system errors. No explicit error handling. |

### 3.3 Trigger Design (30% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Triggers used for cross-row enforcement, audit logging, and computed column maintenance. Triggers are correctly scoped (BEFORE vs AFTER, ROW vs STATEMENT). Triggers do not duplicate logic already handled by constraints. |
| Proficient | 2 | At least 2 triggers with correct timing and scope. Trigger logic correct. |
| Developing | 1 | Triggers present but wrong timing or scope. Logic errors. |

---

## Dimension 4: Performance Optimization (20%)

### 4.1 Index Strategy (50% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Correct index type for each query pattern (B-Tree, GIN, BRIN, partial). Covering indexes for index-only scans. Partial indexes for filtered queries. No redundant or unused indexes. Index sizes documented. |
| Proficient | 2 | B-Tree indexes on all foreign keys and common filter columns. At least 1 partial index. |
| Developing | 1 | Few or no indexes beyond primary key. Foreign keys not indexed. |

### 4.2 EXPLAIN ANALYZE Usage (30% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | EXPLAIN ANALYZE run on all major queries. Before/after comparison documented for optimizations. Can explain every node in execution plan. Buffer usage analyzed with BUFFERS option. |
| Proficient | 2 | EXPLAIN ANALYZE used for at least 3 queries. Index usage verified. Seq scans investigated. |
| Developing | 1 | EXPLAIN never used or used without understanding output. |

### 4.3 Materialized Views and Caching (20% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Materialized views for expensive aggregation queries. Refresh strategy documented. CONCURRENTLY used where appropriate. Partial refresh considered. |
| Proficient | 2 | At least 1 materialized view with index and refresh plan. |
| Developing | 1 | No materialized views. Expensive queries run on every request. |

---

## Dimension 5: Documentation and Communication (10%)

### 5.1 Code Comments and README (50% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Every table has a COMMENT ON TABLE. Complex columns commented. README explains architecture, design decisions, and how to run the project. Architecture Decision Records for non-obvious choices. |
| Proficient | 2 | Most tables commented. README present with setup instructions. |
| Developing | 1 | No comments. No README. Code is not self-documenting. |

### 5.2 Verbal Explanation (50% of dimension)

| Level | Score | Description |
|-------|-------|-------------|
| Expert | 3 | Can explain any design decision without notes. Proactively discusses trade-offs. Acknowledges what they would do differently. Anticipates follow-up questions. |
| Proficient | 2 | Can explain most decisions clearly. Some notes needed. Handles follow-ups well. |
| Developing | 1 | Struggles to explain decisions. Cannot articulate trade-offs. |

---

## Scoring Summary

Calculate your score for each dimension:

```
Dimension 1 (Schema Design, 25%)
   1.1 Normalization:        ___ / 3 × 0.33 × 25 = ___
   1.2 Constraints:          ___ / 3 × 0.33 × 25 = ___
   1.3 Naming:               ___ / 3 × 0.17 × 25 = ___
   1.4 Scalability:          ___ / 3 × 0.17 × 25 = ___
   Subtotal:                                         ___/ 25

Dimension 2 (Queries, 25%)
   2.1 Correctness:          ___ / 3 × 0.40 × 25 = ___
   2.2 Advanced SQL:         ___ / 3 × 0.30 × 25 = ___
   2.3 Readability:          ___ / 3 × 0.30 × 25 = ___
   Subtotal:                                         ___/ 25

Dimension 3 (Functions, 20%)
   3.1 Business Logic:       ___ / 3 × 0.40 × 20 = ___
   3.2 Error Handling:       ___ / 3 × 0.30 × 20 = ___
   3.3 Triggers:             ___ / 3 × 0.30 × 20 = ___
   Subtotal:                                         ___/ 20

Dimension 4 (Performance, 20%)
   4.1 Indexes:              ___ / 3 × 0.50 × 20 = ___
   4.2 EXPLAIN:              ___ / 3 × 0.30 × 20 = ___
   4.3 Materialized Views:   ___ / 3 × 0.20 × 20 = ___
   Subtotal:                                         ___/ 20

Dimension 5 (Documentation, 10%)
   5.1 Comments/README:      ___ / 3 × 0.50 × 10 = ___
   5.2 Verbal:               ___ / 3 × 0.50 × 10 = ___
   Subtotal:                                         ___/ 10

TOTAL:                                               ___/ 100
```

---

## Performance Bands

| Score | Band | Career Readiness |
|-------|------|-----------------|
| 90-100 | Expert | Senior DB engineer / DBA roles |
| 75-89  | Proficient | Mid-level backend/data engineering |
| 60-74  | Competent | Junior roles with continued growth |
| 45-59  | Developing | Review Weeks 5-8 before applying |
| Below 45 | Foundational | Return to Week 1 and repeat the program |

---

## Weekly Self-Assessment Template

Use this at the end of each week to track your progress:

```
Week: ___  Date: ___

What I learned this week (3 things):
1.
2.
3.

What I'm still confused about (1-2 things):
1.
2.

Evidence of learning (SQL I wrote, project I built):


Score on this week's mock interview questions: ___/10

Goal for next week:
```

---

## Revision Tracker

Mark each dimension when you reach "Proficient" and again when you reach "Expert":

| Dimension | Proficient Date | Expert Date |
|-----------|----------------|-------------|
| Schema Design | | |
| SQL Query Quality | | |
| PL/pgSQL Functions | | |
| Performance | | |
| Documentation | | |

When all five dimensions show an Expert date, you have completed the program.
