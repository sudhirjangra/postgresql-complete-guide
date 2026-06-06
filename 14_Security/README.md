# Module 14: PostgreSQL Security

## Overview

This module covers the full security stack for PostgreSQL deployments: authentication, authorization, encryption, auditing, secrets management, and compliance. Each file provides production-grade configurations, real-world patterns, and compliance guidance.

## Prerequisites
- PostgreSQL administration basics (Modules 1-5)
- Basic Linux/networking knowledge
- Understanding of SQL and roles

## Module Contents

| File | Topic | Key Concepts |
|------|-------|--------------|
| [01_authentication_methods.md](01_authentication_methods.md) | pg_hba.conf, auth methods | trust/md5/SCRAM/cert/LDAP |
| [02_roles_and_privileges.md](02_roles_and_privileges.md) | Roles, GRANT, REVOKE | RBAC, group roles, default privileges |
| [03_row_level_security.md](03_row_level_security.md) | RLS policies | 7 policy patterns, tenant isolation |
| [04_ssl_tls.md](04_ssl_tls.md) | SSL/TLS setup | sslmode, certs, mTLS, rotation |
| [05_encryption.md](05_encryption.md) | pgcrypto, column encryption | bcrypt, PGP, symmetric, TDE |
| [06_auditing.md](06_auditing.md) | pgaudit, change history | DDL audit, trigger-based audit |
| [07_secrets_management.md](07_secrets_management.md) | Vault, .pgpass, rotation | Dynamic credentials, cert lifecycle |
| [08_compliance_considerations.md](08_compliance_considerations.md) | GDPR, SOC2, HIPAA | Erasure, PHI, evidence collection |

## Security Layers at a Glance

```
Network:   SSL/TLS (verify-full) ── Firewall ── VPN
Auth:      scram-sha-256 ── LDAP ── Certificate
AuthZ:     RBAC roles ── Column privileges ── RLS
Audit:     pgaudit ── Triggers ── Event triggers
Encrypt:   pgcrypto ── Filesystem ── Backup
Secrets:   Vault ── SecretsManager ── .pgpass
```

## Quick Start: Minimum Security Configuration

```sql
-- 1. Use scram-sha-256 (pg_hba.conf: hostssl ... scram-sha-256)
-- 2. Revoke public schema creation
REVOKE CREATE ON SCHEMA public FROM PUBLIC;
-- 3. Create application roles (not superuser)
CREATE ROLE appuser WITH LOGIN PASSWORD 'Strong!' CONNECTION LIMIT 50;
-- 4. Enable RLS on sensitive tables
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
-- 5. Install pgaudit and audit DDL
```

## Related Modules
- Module 13: Replication & HA (security for replication connections)
- Module 15: Backup & Recovery (backup encryption)
