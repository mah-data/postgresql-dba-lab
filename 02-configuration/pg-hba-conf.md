# PostgreSQL Client Authentication Configuration (pg_hba.conf)

## Overview

The `pg_hba.conf` file controls client authentication and access permissions in PostgreSQL.

HBA stands for:

Host-Based Authentication

This file defines:

- Which users can connect
- Which databases they can access
- From which IP addresses connections are allowed
- Which authentication method is used

---

# 1. Location of pg_hba.conf

Find the active authentication file:

```sql
SHOW hba_file;
```

Common locations:

## Linux

```text
/etc/postgresql/<version>/main/pg_hba.conf
```

or

```text
/var/lib/pgsql/<version>/data/pg_hba.conf
```

## Windows

```text
C:\Program Files\PostgreSQL\<version>\data\pg_hba.conf
```

---

# 2. File Structure

Each authentication rule contains five fields:

```text
TYPE   DATABASE   USER   ADDRESS   METHOD
```

Example:

```conf
host    all    all    192.168.1.0/24    scram-sha-256
```

Meaning:

| Field | Description |
|---|---|
| TYPE | Connection type |
| DATABASE | Allowed database |
| USER | Allowed user |
| ADDRESS | Client IP range |
| METHOD | Authentication method |

---

# 3. Connection Types

## Local Connection

Used for connections from the same server:

```conf
local   all   all   peer
```

---

## TCP/IP Connection

Used for remote connections:

```conf
host    all    all    192.168.1.0/24    scram-sha-256
```

---

# 4. Authentication Methods

## trust

No password is required.

Example:

```conf
host all all 192.168.1.10/32 trust
```

Use:

- Temporary testing only

Avoid in production environments.

---

## password

Uses clear-text password authentication.

Example:

```conf
host all all 192.168.1.10/32 password
```

SSL encryption should be used when applying this method.

---

## md5

Password authentication using MD5 encryption.

Example:

```conf
host all all 192.168.1.10/32 md5
```

Older authentication method.

---

## scram-sha-256

Modern password authentication method.

Example:

```conf
host all all 192.168.1.10/32 scram-sha-256
```

Recommended for production environments.

---

# 5. Allow Remote Connection Example

## Step 1: Configure PostgreSQL Listener

Edit:

```text
postgresql.conf
```

Set:

```conf
listen_addresses = '*'
```

---

## Step 2: Add Client Access Rule

Allow client server:

```text
192.168.1.50
```

Configuration:

```conf
host    all    all    192.168.1.50/32    scram-sha-256
```

---

## Step 3: Reload Configuration

```sql
SELECT pg_reload_conf();
```

or:

```bash
sudo systemctl reload postgresql
```

---

# 6. Restrict Database Access

Allow only a specific user:

```conf
host    salesdb    sales_user    192.168.1.0/24    scram-sha-256
```

Meaning:

```text
Database  → salesdb
User      → sales_user
Network   → 192.168.1.x
Method    → scram-sha-256
```

---

# 7. Rule Order

Important:

PostgreSQL uses the first matching rule.

Example:

Incorrect order:

```conf
host    all    all    0.0.0.0/0    reject

host    salesdb    app_user    192.168.1.20/32    scram-sha-256
```

The second rule will never be reached.

Correct order:

```conf
host    salesdb    app_user    192.168.1.20/32    scram-sha-256

host    all    all    0.0.0.0/0    reject
```

Decision Flow:

```text
Connection Request
        ↓
Read pg_hba.conf from top to bottom
        ↓
First Matching Rule
        ↓
Authentication Method Applied
        ↓
Allow or Reject Connection
```

---

# 8. Validate Configuration

Check authentication rules:

```sql
SELECT * FROM pg_hba_file_rules;
```

This helps identify configuration errors before applying changes.

---

# 9. Security Best Practices

Recommended:

- Use `scram-sha-256`
- Avoid `trust` authentication
- Limit IP ranges
- Do not allow unnecessary remote access
- Separate application users from administrators
- Review authentication logs
- Document all authentication rules

---

# 10. Check Active Connections

View connected users:

```sql
SELECT
    usename,
    client_addr,
    datname
FROM pg_stat_activity;
```

---

# 11. Troubleshooting

## Authentication Failed

Check:

- Username
- Password
- pg_hba.conf rules
- Authentication method

---

## Connection Refused

Check:

```bash
ss -lntp | grep 5432
```

Verify:

- listen_addresses
- port
- Firewall rules

---

# Security Checklist

| Item | Recommendation |
|---|---|
| Authentication | scram-sha-256 |
| Remote Access | Limited IPs only |
| Admin Access | Restricted users |
| Passwords | Strong passwords |
| Rules | Documented and reviewed |

---

# Next Steps

Continue with:

- Connection Management
- Memory Configuration
- WAL Configuration
- Logging Configuration
