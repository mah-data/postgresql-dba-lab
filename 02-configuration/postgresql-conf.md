# PostgreSQL Configuration: postgresql.conf

## Overview

The `postgresql.conf` file is the main configuration file of PostgreSQL.

It controls server behavior including:

* Connection settings
* Memory management
* Query performance
* Logging
* WAL and checkpoint behavior
* Resource utilization

After changing parameters, PostgreSQL may require a reload or restart depending on the parameter type.

---

# 1. Configuration File Location

Find the configuration file:

```sql
SHOW config_file;
```

Find data directory:

```sql
SHOW data_directory;
```

Common locations:

## Linux

```text
/etc/postgresql/<version>/main/postgresql.conf
```

or

```text
/var/lib/pgsql/<version>/data/postgresql.conf
```

## Windows

```text
C:\Program Files\PostgreSQL\<version>\data\postgresql.conf
```

---

# 2. Configuration Structure

The main sections:

```text
Connection Settings → Memory Settings → WAL Configuration → Logging → Query Planner → Autovacuum
```

---

# 3. Connection Configuration

## Listen Addresses

Controls network interfaces PostgreSQL listens on.

Default:

```conf
listen_addresses = 'localhost'
```

Remote access example:

```conf
listen_addresses = '*'
```

---

## Port

Default PostgreSQL port:

```conf
port = 5432
```

Check current port:

```sql
SHOW port;
```

---

## Maximum Connections

Controls maximum client connections.

Example:

```conf
max_connections = 200
```

Check:

```sql
SHOW max_connections;
```

Important:

Increasing connections requires more memory.

---

# 4. Memory Configuration

## shared_buffers

Main memory area used by PostgreSQL.
`shared_buffers` is not the first parameter a DBA should increase. 
Memory tuning must be based on workload analysis and performance measurements.

Example:

```conf
shared_buffers = 2GB
```

Check:

```sql
SHOW shared_buffers;
```

---

## work_mem

Memory used for query operations:

* Sort
* Hash Join
* Aggregation

Example:

```conf
work_mem = 64MB
```

---

## maintenance_work_mem

Memory for maintenance operations:

* VACUUM
* CREATE INDEX
* ALTER TABLE

Example:

```conf
maintenance_work_mem = 512MB
```

---

# 5. WAL Configuration

WAL (Write Ahead Logging) provides:

* Data durability
* Crash recovery
* Replication support

---

## wal_level

Controls WAL information level.

Example:

```conf
wal_level = replica
```

Options:

```text
minimal
replica
logical
```

---

## WAL Size

Example:

```conf
max_wal_size = 4GB
```

Controls WAL growth before checkpoint.

---

# 6. Checkpoint Configuration

Checkpoints write modified data from memory to disk.

Important parameters:

```conf
checkpoint_timeout = 15min

checkpoint_completion_target = 0.9
```

Benefits:

* Reduce I/O spikes
* Improve performance

---

# 7. Logging Configuration

Enable logging:

```conf
logging_collector = on
```

Log directory:

```conf
log_directory = 'log'
```

Log filename:

```conf
log_filename = 'postgresql-%Y-%m-%d.log'
```

---

## Slow Query Logging

Enable slow query monitoring:

```conf
log_min_duration_statement = 1000
```

Meaning:

Queries slower than 1000 milliseconds are logged.

---

# 8. Apply Configuration Changes

## Reload Configuration

For reloadable parameters:

```bash
sudo systemctl reload postgresql
```

or:

```sql
SELECT pg_reload_conf();
```

---

## Restart PostgreSQL

Required for some parameters:

```bash
sudo systemctl restart postgresql
```

---

# 9. Check Current Settings

Show all parameters:

```sql
SHOW ALL;
```

Check specific parameter:

```sql
SHOW shared_buffers;
```

---

# 10. Best Practices

* Never change configuration parameters without measuring the impact.
* Backup configuration before changes.
* Change one parameter at a time.
* Document every modification.
* Monitor performance after changes.
* Test configuration changes before production deployment.

---

# Configuration Checklist

| Parameter                  | Purpose                  |
| -------------------------- | ------------------------ |
| listen_addresses           | Network access           |
| port                       | Connection port          |
| max_connections            | Client limit             |
| shared_buffers             | Database cache           |
| work_mem                   | Query memory             |
| wal_level                  | Recovery and replication |
| logging_collector          | Log management           |
| log_min_duration_statement | Slow query monitoring    |

---

# Next Steps

Continue with:

* pg_hba.conf Security Configuration
* Connection Configuration
* Memory Configuration
* WAL Configuration

