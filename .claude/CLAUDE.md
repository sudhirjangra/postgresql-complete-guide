# Execution Strategy

Use parallel task execution.

Split work into specialized agents:

1. Curriculum Architect
2. PostgreSQL Fundamentals Writer
3. SQL Interview Writer
4. PostgreSQL Internals Expert
5. Performance Optimization Expert
6. Database Design Expert
7. System Design Expert
8. Company Interview Expert
9. Project Generator
10. Documentation Reviewer

Each agent owns its folder.

Generate files independently in parallel.

After generation:

- Review consistency.
- Fix broken links.
- Generate final indexes.
- Generate learning roadmap.

# Master Prompt: Generate a Complete PostgreSQL Learning & Interview Preparation Repository

## Objective

Create a world-class PostgreSQL learning repository that takes a learner from absolute beginner to advanced database engineer level. The repository must be structured as a professional technical curriculum with extensive theory, hands-on practice, projects, interview preparation, company-specific interview tasks, performance tuning exercises, architecture discussions, and real-world production scenarios.

The output must consist of multiple well-organized Markdown (`.md`) files, grouped into folders with a clear hierarchy, navigation structure, cross-references, learning paths, and progression checkpoints.

The repository should be comprehensive enough that a learner can:

* Learn PostgreSQL from scratch.
* Become job-ready for Backend, Data Engineering, Analytics Engineering, Database Engineering, and Platform Engineering roles.
* Prepare for PostgreSQL-related interviews at Amazon, Google, Microsoft, Meta, Uber, Airbnb, Netflix, LinkedIn, Atlassian, Stripe, DoorDash, Snowflake, Databricks, and similar companies.
* Solve practical SQL and PostgreSQL interview tasks.
* Understand real-world production systems.
* Master PostgreSQL performance optimization.
* Design scalable database architectures.
* Troubleshoot production database issues.
* Crack senior and staff-level database interviews.

---

# Repository Structure

Generate the following structure:

```text
postgresql-complete-guide/

│
├── README.md
├── LEARNING_PATH.md
├── ROADMAP.md
├── GLOSSARY.md
├── INTERVIEW_PREPARATION.md
│
├── 01_Fundamentals/
├── 02_SQL_Basics/
├── 03_Intermediate_SQL/
├── 04_Advanced_SQL/
├── 05_PostgreSQL_Core/
├── 06_Database_Design/
├── 07_Indexes/
├── 08_Query_Optimization/
├── 09_Transactions_Concurrency/
├── 10_PostgreSQL_Internals/
├── 11_Advanced_PostgreSQL/
├── 12_Production_PostgreSQL/
├── 13_Replication_HA/
├── 14_Security/
├── 15_Backup_Recovery/
├── 16_PostgreSQL_For_Data_Engineering/
├── 17_PostgreSQL_For_Backend_Engineers/
├── 18_Architecture_Case_Studies/
├── 19_Interview_Questions/
├── 20_Interview_Tasks/
├── 21_System_Design/
├── 22_Projects/
├── 23_Company_Preparation/
├── 24_Cheat_Sheets/
└── 25_Capstone_Program/
```

---

# Global Content Requirements

Every markdown file must contain:

1. Table of Contents
2. Learning Objectives
3. Theory Section
4. Visual Diagrams (ASCII)
5. Practical Examples
6. PostgreSQL Commands
7. SQL Examples
8. Common Mistakes
9. Best Practices
10. Performance Considerations
11. Interview Questions
12. Interview Answers
13. Hands-on Exercises
14. Solutions
15. Advanced Notes
16. References to related chapters

---

# Beginner Level Coverage

Create detailed chapters covering:

## Database Fundamentals

* What is a database
* Why databases exist
* Types of databases
* Relational databases
* OLTP vs OLAP
* CAP theorem
* ACID
* BASE
* Database architecture

## PostgreSQL Introduction

* History
* Features
* Advantages
* Use cases
* Ecosystem

## Installation

Cover:

* Linux
* Ubuntu
* Debian
* CentOS
* RHEL
* MacOS
* Windows
* Docker
* Kubernetes

Include:

* Installation commands
* Verification steps
* Troubleshooting

---

# SQL Curriculum

Cover every major SQL topic:

## DDL

* CREATE
* ALTER
* DROP
* TRUNCATE

## DML

* INSERT
* UPDATE
* DELETE
* MERGE

## DQL

* SELECT
* WHERE
* ORDER BY
* LIMIT
* DISTINCT

## Advanced SQL

* GROUP BY
* HAVING
* Aggregations
* Subqueries
* Correlated subqueries
* EXISTS
* ANY
* ALL

## Joins

* INNER
* LEFT
* RIGHT
* FULL
* CROSS
* SELF JOIN

Include:

* Visual diagrams
* Execution explanations
* Real interview examples

---

# Advanced SQL Coverage

Create dedicated detailed files for:

## Window Functions

Cover:

* ROW_NUMBER
* RANK
* DENSE_RANK
* NTILE
* LAG
* LEAD
* FIRST_VALUE
* LAST_VALUE

Include:

* Business use cases
* Interview tasks
* Company questions

## CTEs

* Simple CTE
* Recursive CTE
* Hierarchical data

## Set Operations

* UNION
* UNION ALL
* INTERSECT
* EXCEPT

## Advanced Aggregation

* GROUPING SETS
* ROLLUP
* CUBE

---

# PostgreSQL Core

Cover:

## Data Types

Every PostgreSQL data type:

* Numeric
* Character
* Boolean
* Date
* Time
* Timestamp
* UUID
* JSON
* JSONB
* XML
* Arrays
* Ranges
* Network types
* Geometric types

For each:

* Use cases
* Storage details
* Performance considerations
* Interview questions

---

# Database Design

Cover:

## Normalization

* 1NF
* 2NF
* 3NF
* BCNF
* 4NF
* 5NF

## Denormalization

## Schema Design

## Entity Relationships

## Modeling

* E-commerce
* Banking
* SaaS
* Social Media
* Logistics
* Healthcare

Include:

* ER diagrams
* Design exercises
* Solutions

---

# Indexing Masterclass

Generate multiple files covering:

## Index Fundamentals

## B-Tree

## Hash Index

## GIN

## GiST

## BRIN

## SP-GiST

## Partial Indexes

## Composite Indexes

## Covering Indexes

## Expression Indexes

For each:

* Internal architecture
* When to use
* When not to use
* Cost analysis
* Interview questions
* Production examples

---

# Query Optimization

Create detailed optimization handbook covering:

## EXPLAIN

## EXPLAIN ANALYZE

## Cost Estimation

## Query Planner

## Statistics

## Parallel Execution

## Query Rewriting

## Anti-patterns

Include:

* Slow query examples
* Optimization walkthroughs
* Production debugging scenarios

---

# Transactions & Concurrency

Cover:

## MVCC

## Isolation Levels

* Read Uncommitted
* Read Committed
* Repeatable Read
* Serializable

## Locks

* Row locks
* Table locks
* Deadlocks

Include:

* Visualization
* Internal flow
* Interview questions

---

# PostgreSQL Internals

Create advanced documentation covering:

## Storage Engine

## Heap Storage

## TOAST

## WAL

## Checkpoints

## Vacuum

## Autovacuum

## Visibility Map

## Free Space Map

## System Catalogs

## Query Executor

Include:

* Internal architecture diagrams
* Deep technical explanations
* Production troubleshooting

---

# Advanced PostgreSQL

Create advanced chapters covering:

## JSONB Mastery

## Full Text Search

## Partitioning

## Materialized Views

## Extensions

Include:

* pg_stat_statements
* PostGIS
* pg_partman
* TimescaleDB
* pgvector

## Logical Decoding

## Foreign Data Wrappers

## LISTEN / NOTIFY

---

# Production PostgreSQL

Generate enterprise-level operational documentation covering:

## Monitoring

## Observability

## Metrics

## Logging

## Capacity Planning

## Maintenance

## Incident Response

## Troubleshooting

Include real production incidents and resolutions.

---

# Replication & High Availability

Cover:

## Streaming Replication

## Logical Replication

## Synchronous Replication

## Asynchronous Replication

## Failover

## Read Replicas

## Patroni

## PgBouncer

## HAProxy

Include architecture diagrams and deployment examples.

---

# Security

Cover:

## Authentication

## Authorization

## Roles

## Privileges

## Row Level Security

## Encryption

## SSL

## Secrets Management

## Auditing

Include compliance considerations.

---

# Backup & Recovery

Cover:

## pg_dump

## pg_restore

## Base Backup

## PITR

## WAL Archiving

## Disaster Recovery

Include runbooks and recovery procedures.

---

# PostgreSQL for Data Engineering

Cover:

## ETL

## ELT

## CDC

## Data Warehousing

## Batch Processing

## Incremental Loading

## Analytics Workloads

## PostgreSQL + Airflow

## PostgreSQL + Spark

## PostgreSQL + dbt

---

# PostgreSQL for Backend Engineers

Cover:

## Connection Pooling

## ORM Optimization

## Transactions

## Migrations

## API Design

## Multi-Tenant Architectures

## Caching Strategies

## Scaling Patterns

Include examples using:

* Java
* Spring Boot
* Node.js
* NestJS
* Python
* Django
* FastAPI
* Go

---

# Architecture Case Studies

Create detailed case studies for:

## Amazon-like Marketplace

## Netflix-like Streaming Platform

## Uber-like Ride Sharing

## Banking Platform

## SaaS Product

## Messaging System

For each:

* Database schema
* Scaling challenges
* Optimization strategies
* Interview discussion points

---

# Interview Preparation Section

Generate a dedicated interview track containing:

## Beginner Questions

100+ questions

## Intermediate Questions

200+ questions

## Advanced Questions

300+ questions

## Staff-Level Questions

200+ questions

For every question include:

* Question
* Detailed Answer
* Follow-up Questions
* Interviewer Expectations
* Common Mistakes

---

# SQL Interview Tasks

Create large collections of tasks:

## Easy

100+ problems

## Medium

200+ problems

## Hard

200+ problems

For every task include:

* Problem statement
* Schema
* Sample data
* Solution
* Alternative solution
* Optimization discussion

Include company-style tasks.

---

# Company-Specific Preparation

Create separate folders for:

## Amazon

Cover:

* SQL rounds
* Database rounds
* Leadership principle related database questions
* Realistic interview simulations

## Google

Cover:

* SQL
* Query optimization
* Distributed systems discussions
* Database internals

## Microsoft

Cover:

* Azure PostgreSQL
* Performance optimization
* Architecture design

## Meta

Cover:

* Data-intensive systems
* Scale discussions

## Uber

Cover:

* Geospatial data
* Real-time workloads

## Netflix

Cover:

* Large-scale data systems

For every company provide:

* Frequently asked questions
* Mock interviews
* Coding tasks
* Database design tasks
* Evaluation rubric

---

# System Design Section

Create PostgreSQL-focused system design guides covering:

## URL Shortener

## E-commerce

## Banking

## Ride Sharing

## Social Network

## Chat Application

## Analytics Platform

For every design include:

* Requirements
* Capacity estimation
* Schema design
* Scaling strategy
* Partitioning strategy
* Indexing strategy
* Tradeoffs
* Interview discussion

---

# Projects Section

Create complete project guides:

## Beginner

1. Library Management System
2. Student Management System
3. Inventory System

## Intermediate

1. E-commerce Backend
2. CRM
3. Blogging Platform

## Advanced

1. Banking System
2. Ride Sharing Backend
3. Event Streaming Platform

Include:

* Requirements
* Schema
* SQL
* Implementation
* Optimization
* Testing
* Deployment

---

# Capstone Program

Create a final learning path that combines all knowledge into a structured 12-week program.

Include:

* Weekly goals
* Reading assignments
* Practice tasks
* Mock interviews
* Projects
* Assessments
* Final evaluation

---

# Quality Requirements

The generated content must:

* Be technically accurate.
* Be suitable for self-study.
* Be suitable for interview preparation.
* Contain production-grade knowledge.
* Include practical SQL examples.
* Include PostgreSQL-specific behaviors.
* Include edge cases.
* Include performance implications.
* Include troubleshooting scenarios.
* Include advanced engineering discussions.
* Explain concepts from beginner to expert level.
* Be detailed enough to exceed typical database courses and interview preparation resources.

Output all markdown files with proper naming conventions, cross-links, navigation, and consistent formatting across the repository.
