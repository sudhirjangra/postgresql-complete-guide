# PostgreSQL Commands Cheat Sheet

> psql meta-commands, connection strings, pg_dump, pg_restore, pg_ctl, config params.

---

## psql — Connection

### Connection String Formats
```bash
# URI format
psql "postgresql://user:password@host:5432/dbname?sslmode=require"
psql "postgres://user@host/db"

# Flag format
psql -h host -p 5432 -U user -d dbname -W   # -W prompts for password

# Environment variables (no password in command)
export PGHOST=host PGPORT=5432 PGUSER=user PGDATABASE=db PGPASSWORD=pass
psql

# .pgpass file (auto password, no prompt)
# Format: host:port:dbname:user:password
echo "localhost:5432:mydb:myuser:mypass" >> ~/.pgpass
chmod 600 ~/.pgpass
```

### Common psql Flags
| Flag | Purpose |
|---|---|
| `-h host` | Host (default: localhost via UNIX socket) |
| `-p port` | Port (default: 5432) |
| `-U user` | Username |
| `-d dbname` | Database name |
| `-W` | Force password prompt |
| `-w` | Never prompt for password |
| `-c 'SQL'` | Run SQL and exit |
| `-f file.sql` | Run SQL file |
| `-o output.txt` | Write output to file |
| `-A` | Unaligned output (good for CSV-like) |
| `-F ','` | Field separator (with -A) |
| `-t` | Tuples only (no column headers) |
| `-q` | Quiet mode (suppress startup message) |
| `-v var=val` | Set psql variable |
| `--echo-all` | Print all SQL commands |

```bash
# Run query and export as CSV
psql -h host -U user -d db -A -F ',' -c "SELECT * FROM orders" > output.csv

# Run a file
psql -h host -U user -d db -f migration.sql

# Run inline SQL
psql -h host -U user -d db -c "SELECT version();"
```

---

## psql Meta-Commands (Backslash Commands)

### Connection & Session
| Command | Description |
|---|---|
| `\c dbname [user]` | Connect to database |
| `\c -` | Reconnect (same settings) |
| `\conninfo` | Show current connection info |
| `\q` | Quit psql |
| `\!` | Shell command |
| `\! ls` | Execute shell command |
| `\set VAR value` | Set psql variable |
| `\echo :VAR` | Print variable |
| `\timing [on|off]` | Toggle query timing |
| `\pset pager off` | Disable pager (less) |
| `\x [on|off|auto]` | Expanded display (vertical) |
| `\pset format unaligned` | Unaligned output |
| `\pset fieldsep ','` | Set field separator |
| `\o filename` | Redirect output to file |
| `\o` | Stop redirecting output |
| `\i filename` | Execute SQL file |
| `\ir filename` | Execute relative to current file |
| `\e` | Edit query in $EDITOR |

### Information / Describe Commands
| Command | Description |
|---|---|
| `\l` or `\list` | List databases |
| `\l+` | List databases with size |
| `\dn` | List schemas |
| `\dn+` | List schemas with permissions |
| `\dt` | List tables (current schema) |
| `\dt *.*` | List ALL tables in ALL schemas |
| `\dt schema.*` | List tables in specific schema |
| `\dt+ tablename` | Describe table with size/description |
| `\d tablename` | Describe table (columns, indexes, FKs) |
| `\d+ tablename` | Describe table (verbose, storage params) |
| `\di` | List indexes |
| `\di+ indexname` | Index details with size |
| `\ds` | List sequences |
| `\dv` | List views |
| `\dv+ viewname` | View definition |
| `\dm` | List materialized views |
| `\df` | List functions |
| `\df funcname` | Function details |
| `\df+` | Functions with source |
| `\dp tablename` | Table permissions (ACL) |
| `\du` | List roles/users |
| `\du rolename` | Role details |
| `\dg` | List groups |
| `\dS` | List system catalog tables |
| `\dT` | List data types |
| `\dx` | List installed extensions |
| `\db` | List tablespaces |
| `\dC` | List casts |
| `\do` | List operators |
| `\dF` | List text search configurations |
| `\dFd` | List text search dictionaries |
| `\sf funcname` | Show function source |
| `\sv viewname` | Show view source |

### Formatting
```
\x              -- toggle expanded view (very useful for wide tables)
\x auto         -- auto-switch based on terminal width
\pset null 'NULL' -- show NULLs as 'NULL' instead of blank
\pset border 2  -- enhanced borders (0=none, 1=plain, 2=full)
\a              -- toggle aligned/unaligned
\H              -- toggle HTML output
```

### History & Editing
```
\l              -- list recent commands
\g              -- repeat last query
\g filename     -- send last query output to file
\p              -- print query buffer
\r              -- reset query buffer
\s              -- command history
\s filename     -- save history to file
```

---

## System Views — Quick Reference

### Connection / Activity
```sql
SELECT * FROM pg_stat_activity;              -- active connections
SELECT pid, usename, state, query FROM pg_stat_activity WHERE state != 'idle';
SELECT count(*) FROM pg_stat_activity;       -- connection count
SELECT pg_terminate_backend(pid);            -- kill connection
SELECT pg_cancel_backend(pid);               -- cancel query (softer)
```

### Tables & Indexes
```sql
SELECT * FROM pg_stat_user_tables;           -- table stats (seq scans, row counts, vacuum)
SELECT * FROM pg_stat_user_indexes;          -- index usage
SELECT * FROM pg_statio_user_tables;         -- I/O stats
SELECT * FROM pg_indexes WHERE tablename = 't'; -- indexes on table
SELECT * FROM pg_tables WHERE schemaname = 'public'; -- tables
```

### Locks
```sql
SELECT * FROM pg_locks;                      -- all locks
SELECT * FROM pg_blocking_pids(pid);         -- what's blocking a pid

-- Find lock contention
SELECT
    blocked_locks.pid AS blocked_pid,
    blocking_locks.pid AS blocking_pid,
    blocked_activity.query AS blocked_query,
    blocking_activity.query AS blocking_query
FROM pg_catalog.pg_locks blocked_locks
JOIN pg_catalog.pg_stat_activity blocked_activity ON blocked_activity.pid = blocked_locks.pid
JOIN pg_catalog.pg_locks blocking_locks ON blocking_locks.locktype = blocked_locks.locktype
    AND blocking_locks.pid != blocked_locks.pid
    AND blocking_locks.granted
JOIN pg_catalog.pg_stat_activity blocking_activity ON blocking_activity.pid = blocking_locks.pid
WHERE NOT blocked_locks.granted;
```

### Performance
```sql
SELECT * FROM pg_stat_statements ORDER BY total_exec_time DESC LIMIT 20;   -- slow queries
SELECT * FROM pg_stat_bgwriter;                                              -- checkpoint stats
SELECT * FROM pg_stat_replication;                                           -- replication lag
SELECT now() - pg_last_xact_replay_timestamp() AS replication_lag;         -- lag on replica
```

### Size Queries
```sql
-- Database sizes
SELECT datname, pg_size_pretty(pg_database_size(datname)) FROM pg_database;

-- Table sizes
SELECT tablename, pg_size_pretty(pg_total_relation_size(tablename::regclass)) AS total_size,
       pg_size_pretty(pg_relation_size(tablename::regclass)) AS table_size,
       pg_size_pretty(pg_indexes_size(tablename::regclass)) AS index_size
FROM pg_tables WHERE schemaname = 'public' ORDER BY pg_total_relation_size(tablename::regclass) DESC;

-- Index sizes
SELECT indexname, pg_size_pretty(pg_relation_size(indexname::regclass)) AS size
FROM pg_indexes WHERE schemaname = 'public' ORDER BY pg_relation_size(indexname::regclass) DESC;

-- Unused indexes
SELECT idx.indexname, idx.tablename, i.idx_scan
FROM pg_stat_user_indexes i
JOIN pg_indexes idx ON i.indexrelname = idx.indexname
WHERE i.idx_scan = 0 AND NOT idx.indisunique;
```

---

## pg_dump / pg_restore

### pg_dump — Backup
```bash
# Logical dump (plain SQL)
pg_dump -h host -U user -d dbname -f backup.sql

# Custom format (best for pg_restore, supports parallel restore)
pg_dump -h host -U user -d dbname -Fc -f backup.dump

# Directory format (parallel dump)
pg_dump -h host -U user -d dbname -Fd -j 4 -f backup_dir/

# Compress output
pg_dump -h host -U user -d dbname -Fc -Z 9 -f backup_compressed.dump

# Table-level backup
pg_dump -h host -U user -d dbname -t tablename -f table_backup.sql

# Schema only (no data)
pg_dump -h host -U user -d dbname -s -f schema_only.sql

# Data only (no schema)
pg_dump -h host -U user -d dbname -a -f data_only.sql

# Exclude a table
pg_dump -h host -U user -d dbname -T exclude_table -f backup.sql

# Specific schema
pg_dump -h host -U user -d dbname -n schema_name -f schema_backup.sql
```

### pg_dump Flags Reference
| Flag | Description |
|---|---|
| `-F c` | Custom format (compressed) |
| `-F d` | Directory format |
| `-F t` | Tar format |
| `-F p` | Plain SQL (default) |
| `-j n` | Parallel workers (with -Fd) |
| `-Z n` | Compression level (0-9) |
| `-s` | Schema only |
| `-a` | Data only |
| `-t table` | Specific table |
| `-T table` | Exclude table |
| `-n schema` | Specific schema |
| `-N schema` | Exclude schema |
| `--no-owner` | No ownership commands |
| `--no-privileges` | No GRANT/REVOKE |
| `--column-inserts` | INSERT statements instead of COPY |
| `--if-exists` | Add IF EXISTS to DROP statements |

### pg_restore
```bash
# Restore from custom format
pg_restore -h host -U user -d dbname backup.dump

# Restore with parallel workers
pg_restore -h host -U user -d dbname -j 4 backup.dump

# Restore only schema
pg_restore -h host -U user -d dbname -s backup.dump

# Restore only data
pg_restore -h host -U user -d dbname -a backup.dump

# Restore specific table
pg_restore -h host -U user -d dbname -t tablename backup.dump

# List contents of dump
pg_restore --list backup.dump

# Restore with verbose output
pg_restore -h host -U user -d dbname -v backup.dump

# Restore plain SQL dump
psql -h host -U user -d dbname -f backup.sql
```

### pg_dumpall — Backup All Databases
```bash
pg_dumpall -h host -U postgres -f all_databases.sql   # includes roles and tablespaces
pg_dumpall -h host -U postgres -g -f roles.sql        # roles/globals only
```

---

## pg_ctl — Server Control

```bash
# Start PostgreSQL
pg_ctl -D /var/lib/postgresql/data start
pg_ctl -D /var/lib/postgresql/data start -l logfile

# Stop (modes: smart=wait for clients, fast=disconnect clients, immediate=crash-stop)
pg_ctl -D /var/lib/postgresql/data stop -m fast
pg_ctl -D /var/lib/postgresql/data stop -m smart
pg_ctl -D /var/lib/postgresql/data stop -m immediate  # dangerous

# Restart
pg_ctl -D /var/lib/postgresql/data restart -m fast

# Reload config (no restart needed for most params)
pg_ctl -D /var/lib/postgresql/data reload
-- OR from psql:
SELECT pg_reload_conf();

# Status
pg_ctl -D /var/lib/postgresql/data status

# Initdb (initialize new cluster)
pg_ctl initdb -D /var/lib/postgresql/data

# Promote standby to primary
pg_ctl -D /var/lib/postgresql/data promote
```

### systemd Alternative (common on Linux)
```bash
systemctl start postgresql
systemctl stop postgresql
systemctl restart postgresql
systemctl reload postgresql
systemctl status postgresql
```

---

## psql — Useful Query Patterns

```sql
-- List all tables with row count estimate
SELECT schemaname, tablename, n_live_tup AS est_row_count
FROM pg_stat_user_tables ORDER BY n_live_tup DESC;

-- Find tables with no primary key
SELECT tablename FROM pg_tables t
WHERE schemaname = 'public'
  AND NOT EXISTS (SELECT 1 FROM pg_constraint c
                  JOIN pg_class r ON c.conrelid = r.oid
                  WHERE r.relname = t.tablename AND c.contype = 'p');

-- Find long-running queries
SELECT pid, now() - query_start AS duration, query
FROM pg_stat_activity
WHERE state = 'active'
  AND query_start < now() - INTERVAL '5 minutes'
ORDER BY duration DESC;

-- Vacuum and analyze stats
SELECT relname, last_vacuum, last_autovacuum, last_analyze, last_autoanalyze, n_dead_tup
FROM pg_stat_user_tables
ORDER BY n_dead_tup DESC;

-- Bloat estimate
SELECT tablename,
       n_dead_tup,
       n_live_tup,
       ROUND(100.0 * n_dead_tup / NULLIF(n_live_tup + n_dead_tup, 0), 2) AS dead_pct
FROM pg_stat_user_tables
WHERE n_dead_tup > 10000
ORDER BY dead_pct DESC;
```

---

## VACUUM and ANALYZE

```sql
VACUUM tablename;                    -- reclaim dead tuples
VACUUM FULL tablename;               -- reclaim space (locks table, rewrites)
VACUUM ANALYZE tablename;            -- vacuum + update stats
VACUUM VERBOSE tablename;            -- with progress output
ANALYZE tablename;                   -- update planner statistics only
ANALYZE tablename (column1, col2);   -- stats for specific columns

-- Check autovacuum settings
SHOW autovacuum;
SHOW autovacuum_vacuum_threshold;
SHOW autovacuum_vacuum_scale_factor;

-- Disable autovacuum for a table (NOT recommended for production)
ALTER TABLE mytable SET (autovacuum_enabled = false);
```

---

## PostgreSQL Config Parameter Reference (postgresql.conf)

### Memory
| Parameter | Default | Notes |
|---|---|---|
| `shared_buffers` | 128MB | Set to 25% of RAM |
| `effective_cache_size` | 4GB | Set to 75% of RAM (hint for planner) |
| `work_mem` | 4MB | Per sort/hash operation. Multiply by max_connections × 2 for total risk |
| `maintenance_work_mem` | 64MB | For VACUUM, CREATE INDEX. Set to 256MB–1GB |
| `wal_buffers` | -1 (auto) | Default is 1/32 of shared_buffers, max 64MB |
| `temp_buffers` | 8MB | Per session temp tables |

### Write-Ahead Log
| Parameter | Default | Notes |
|---|---|---|
| `wal_level` | replica | Options: minimal, replica, logical |
| `synchronous_commit` | on | Set off for async (10x faster writes, lose last 200ms on crash) |
| `checkpoint_completion_target` | 0.9 | Spread checkpoint I/O |
| `checkpoint_timeout` | 5min | Max time between checkpoints |
| `max_wal_size` | 1GB | WAL size before forcing checkpoint |
| `min_wal_size` | 80MB | Minimum WAL retention |
| `wal_compression` | off | Enable for network-heavy replication |

### Connection
| Parameter | Default | Notes |
|---|---|---|
| `max_connections` | 100 | Each connection ~5–10MB RAM. Use PgBouncer |
| `superuser_reserved_connections` | 3 | Reserved for superuser login when pool is full |
| `idle_in_transaction_session_timeout` | 0 (off) | Set to 60000 (1 min) to kill idle-in-txn |
| `statement_timeout` | 0 (off) | Kill long-running queries automatically |
| `lock_timeout` | 0 (off) | Fail if can't acquire lock within N ms |

### Planner
| Parameter | Default | Notes |
|---|---|---|
| `random_page_cost` | 4.0 | Set to 1.1 for SSDs |
| `seq_page_cost` | 1.0 | Leave at default |
| `effective_io_concurrency` | 1 | Set to 200 for SSDs |
| `default_statistics_target` | 100 | Increase to 200–500 for better estimates on complex queries |
| `enable_parallel_query` | on | Enable parallel scans |
| `max_parallel_workers_per_gather` | 2 | Set to num_CPU_cores / 2 |
| `max_parallel_workers` | 8 | Limit total parallel workers |

### Autovacuum
| Parameter | Default | Notes |
|---|---|---|
| `autovacuum` | on | Never turn off |
| `autovacuum_vacuum_threshold` | 50 | Min dead rows before vacuum |
| `autovacuum_vacuum_scale_factor` | 0.2 | Vacuum when 20% of table is dead |
| `autovacuum_analyze_scale_factor` | 0.1 | Analyze when 10% of table changed |
| `autovacuum_vacuum_cost_delay` | 2ms | Throttle autovacuum I/O impact |
| `autovacuum_max_workers` | 3 | Increase for write-heavy workloads |

### Logging
```conf
log_min_duration_statement = 1000    # Log queries > 1 second
log_checkpoints = on
log_connections = off
log_disconnections = off
log_lock_waits = on
log_temp_files = 0                   # Log all temp files (0 = all)
log_autovacuum_min_duration = 250ms  # Log slow autovacuums
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '
```

---

## Reload vs. Restart Requirements

| Action | Required |
|---|---|
| `pg_reload_conf()` / `pg_ctl reload` | Most config changes (work_mem, logging, autovacuum settings) |
| Server restart | `shared_buffers`, `max_connections`, `wal_level`, `listen_addresses` |
| Super-user SET | Session-level: `SET work_mem = '256MB';` |
| `ALTER SYSTEM` | Writes to postgresql.auto.conf, survives restart |

```sql
-- Check which parameters require restart
SELECT name, setting, unit, context FROM pg_settings WHERE pending_restart = TRUE;

-- Change config safely
ALTER SYSTEM SET work_mem = '64MB';
SELECT pg_reload_conf();  -- or restart if required
```

---

## Extensions — Install and Use

```sql
-- List available extensions
SELECT * FROM pg_available_extensions ORDER BY name;

-- List installed extensions
SELECT * FROM pg_extension;
-- Or: \dx in psql

-- Install
CREATE EXTENSION IF NOT EXISTS pg_trgm;         -- trigram similarity
CREATE EXTENSION IF NOT EXISTS postgis;          -- geospatial
CREATE EXTENSION IF NOT EXISTS pgcrypto;         -- encryption
CREATE EXTENSION IF NOT EXISTS uuid-ossp;        -- UUID generation
CREATE EXTENSION IF NOT EXISTS pg_stat_statements; -- query stats
CREATE EXTENSION IF NOT EXISTS btree_gist;       -- GiST for scalar types
CREATE EXTENSION IF NOT EXISTS ltree;            -- hierarchical labels
CREATE EXTENSION IF NOT EXISTS hstore;           -- key-value pairs
CREATE EXTENSION IF NOT EXISTS tablefunc;        -- crosstab (pivot)
CREATE EXTENSION IF NOT EXISTS timescaledb;      -- time-series
CREATE EXTENSION IF NOT EXISTS citus;            -- horizontal sharding

-- Drop
DROP EXTENSION IF EXISTS extension_name;
```

---

## User / Role Management

```sql
-- Create user
CREATE USER username WITH PASSWORD 'password';
CREATE USER username WITH PASSWORD 'password' CREATEDB;

-- Create role
CREATE ROLE rolename WITH LOGIN PASSWORD 'password';
CREATE ROLE readonly_role;

-- Grant permissions
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly_role;
GRANT SELECT, INSERT, UPDATE ON TABLE orders TO app_user;
GRANT ALL PRIVILEGES ON DATABASE dbname TO admin_user;
GRANT EXECUTE ON FUNCTION func_name TO user;
GRANT CONNECT ON DATABASE dbname TO user;
GRANT USAGE ON SCHEMA schema TO user;

-- Default privileges (for future tables)
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO readonly_role;

-- Assign role to user
GRANT readonly_role TO username;

-- Revoke
REVOKE SELECT ON TABLE orders FROM user;

-- Drop user
DROP USER username;
DROP ROLE rolename;

-- Change password
ALTER USER username WITH PASSWORD 'newpassword';

-- List roles
\du  -- in psql
SELECT * FROM pg_roles;
```
