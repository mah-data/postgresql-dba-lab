# PostgreSQL Connection Management

## Overview

PostgreSQL connection management defines how clients connect to the database server and how server resources are allocated for active sessions.

Proper connection management helps to:

* Prevent connection overload
* Improve database performance
* Control resource usage
* Maintain system stability

---

# 1. PostgreSQL Connection Architecture

PostgreSQL uses a process-based connection model.

Architecture:

```text
Client Application
        |
        v
PostgreSQL Connection
        |
        v
Backend Process
        |
        v
Database Session
```

Each client connection creates a separate backend process.

---

# 2. Maximum Connections

Parameter:

```conf
max_connections = 200
```

Purpose:

Defines the maximum number of concurrent client connections.

Check current value:

```sql
SHOW max_connections;
```

Check active connections:

```sql
SELECT count(*)
FROM pg_stat_activity;
```

---

# 3. Current Connection Status

View active sessions:

```sql
SELECT
    pid,
    usename,
    datname,
    client_addr,
    state
FROM pg_stat_activity;
```

Example states:

| State               | Description         |
| ------------------- | ------------------- |
| active              | Query is running    |
| idle                | Connection waiting  |
| idle in transaction | Transaction is open |

---

# 4. Connection Timeout Settings

## statement_timeout

Limits query execution time.

Example:

```conf
statement_timeout = 60s
```

Example:

```sql
SET statement_timeout = '30s';
```

Useful for:

* Preventing long-running queries
* Protecting production systems

---

## idle_in_transaction_session_timeout

Terminates idle transactions.

Example:

```conf
idle_in_transaction_session_timeout = 5min
```

Prevents:

* Locked tables
* Long-running transactions

---

# 5. Connection Limits Per User

PostgreSQL allows limiting connections for users.

Create user with connection limit:

```sql
CREATE USER app_user
WITH PASSWORD 'StrongPassword123'
CONNECTION LIMIT 20;
```

Check:

```sql
SELECT
    rolname,
    rolconnlimit
FROM pg_roles;
```

---

# 6. Connection Limits Per Database

Limit database connections:

```sql
ALTER DATABASE testdb
CONNECTION LIMIT 50;
```

Check:

```sql
SELECT
    datname,
    datconnlimit
FROM pg_database;
```

---

# 7. Terminating Connections

Find sessions:

```sql
SELECT
    pid,
    usename,
    state
FROM pg_stat_activity;
```

Terminate a session:

```sql
SELECT pg_terminate_backend(pid);
```

Example:

```sql
SELECT pg_terminate_backend(12345);
```

Use carefully in production.

---

# 8. Connection Pooling

## Problem

Creating many connections increases:

* Memory usage
* CPU overhead
* Process management cost

---

## Solution

Use a connection pooler.

Common tool:

```text
PgBouncer
```

Architecture:

```text
Application
     |
     v
PgBouncer
     |
     v
PostgreSQL
```

Benefits:

* Reduces database connections
* Improves scalability
* Controls workload

---

# 9. Monitoring Connections

## Active Connections

```sql
SELECT
    datname,
    count(*)
FROM pg_stat_activity
GROUP BY datname;
```

---

## Connections by User

```sql
SELECT
    usename,
    count(*)
FROM pg_stat_activity
GROUP BY usename;
```

---

## Long Running Queries

```sql
SELECT
    pid,
    now() - query_start AS duration,
    query
FROM pg_stat_activity
WHERE state = 'active'
ORDER BY duration DESC;
```

---

# 10. Best Practices

* Do not increase `max_connections` without memory analysis.
* Use connection pooling for high traffic applications.
* Set query timeout limits.
* Monitor idle connections.
* Limit application users.
* Terminate abnormal sessions carefully.

---

# Connection Management Checklist

| Item            | Recommendation              |
| --------------- | --------------------------- |
| max_connections | Size according to resources |
| Timeout         | Configure limits            |
| Pooling         | Use PgBouncer when needed   |
| Monitoring      | Check pg_stat_activity      |
| Security        | Limit user connections      |

---

# Next Steps

Continue with:

* Memory Configuration
* WAL Configuration
* Logging Configuration
