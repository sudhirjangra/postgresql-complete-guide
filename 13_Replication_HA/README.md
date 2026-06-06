# Module 13: Replication & High Availability

## Overview

This module covers everything needed to design, implement, and operate a production-grade highly available PostgreSQL system. From the theory of WAL-based replication to complete Patroni cluster setups, HAProxy routing, and failover runbooks.

## Prerequisites
- PostgreSQL administration basics (Modules 1-5)
- SQL proficiency
- Linux system administration
- Basic networking concepts

## Module Contents

| File | Topic | Key Concepts |
|------|-------|--------------|
| [01_replication_overview.md](01_replication_overview.md) | Replication fundamentals | Physical vs logical, sync vs async |
| [02_streaming_replication.md](02_streaming_replication.md) | Physical streaming replication | Full setup, pg_hba.conf, monitoring |
| [03_logical_replication.md](03_logical_replication.md) | Logical replication | Publications, subscriptions, migrations |
| [04_synchronous_replication.md](04_synchronous_replication.md) | Synchronous replication | synchronous_standby_names, quorum |
| [05_failover_procedures.md](05_failover_procedures.md) | Failover and promotion | pg_ctl promote, pg_rewind, split-brain |
| [06_patroni.md](06_patroni.md) | Patroni HA automation | Architecture, patroni.yml, patronictl |
| [07_pgbouncer.md](07_pgbouncer.md) | Connection pooling | Session/txn/statement modes, monitoring |
| [08_haproxy_load_balancing.md](08_haproxy_load_balancing.md) | HAProxy load balancing | Health checks, read routing, Keepalived |
| [09_ha_architecture_patterns.md](09_ha_architecture_patterns.md) | Complete HA architectures | ASCII diagrams, multi-region, capacity |

## Learning Path

```
Beginner:   01 → 02 → 03
Intermediate: 04 → 05 → 07
Advanced:    06 → 08 → 09
```

## Key Architecture at a Glance

```
Apps ──> HAProxy VIP ──> PgBouncer ──> PostgreSQL (Patroni)
                                            ↑
                                       etcd DCS cluster
                                            ↓
                                      Automatic failover
```

## Quick Reference: Which Replication Type?

| Need | Use |
|------|-----|
| HA hot standby | Physical streaming |
| Zero data loss | Synchronous physical |
| Selective table replication | Logical |
| Major version upgrade | Logical |
| Automatic failover | Patroni |
| Connection multiplexing | PgBouncer |
| Read/write routing | HAProxy |

## Related Modules
- Module 14: Security (access control for replication users)
- Module 15: Backup & Recovery (complements HA with point-in-time recovery)
