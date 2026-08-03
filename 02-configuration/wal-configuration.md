# PostgreSQL WAL Configuration

## Overview

WAL stands for:

Write-Ahead Logging

WAL is a core mechanism in PostgreSQL that ensures data durability, crash recovery, and replication capabilities.

Before PostgreSQL writes changes to the actual data files, it records those changes in WAL.

The main process:

```text
Transaction Change → WAL Record → Commit → Data File Update
```

---

# 1. WAL Architecture

WAL provides several important capabilities:

- Crash Recovery
- Data Durability
- Physical Replication
- Logical Replication
- Point-In-Time Recovery (PITR)

Basic flow:

```text
Database Change → WAL → Disk → Recovery / Replication
```

---

# 2. wal_level

## Purpose

`wal_level` controls how much information PostgreSQL writes to WAL.

Check current value:

```sql
SHOW wal_level;
```

Configuration example:

```conf
wal_level = replica
```

---

## Values

### minimal

Used for:

```text
Crash Recovery
```

Suitable for:

```text
Single Database Server
```

Not suitable for replication.

---

### replica

Used for:

```text
Crash Recovery + Physical Replication
```

Suitable for:

```text
Primary Server → Standby Server
```

---

### logical

Used for:

```text
Crash Recovery + Physical Replication + Logical Replication
```

Suitable for:

```text
Data Integration
Change Data Capture
Logical Replication
```

---

# 3. max_wal_size

## Purpose

Controls the maximum WAL size before PostgreSQL performs a checkpoint.

Check current value:

```sql
SHOW max_wal_size;
```

Configuration example:

```conf
max_wal_size = 4GB
```

---

## Performance Impact

Low value:

```text
Frequent Checkpoints → More Disk I/O → Performance Impact
```

Higher value:

```text
Fewer Checkpoints → Smoother I/O → Better Performance
```

The correct value depends on workload and available storage.

---

# 4. checkpoint_timeout

## Purpose

Defines the maximum time between automatic checkpoints.

Check current value:

```sql
SHOW checkpoint_timeout;
```

Configuration example:

```conf
checkpoint_timeout = 15min
```

---

## Performance Impact

Short interval:

```text
More Checkpoints → Higher I/O Activity
```

Long interval:

```text
Fewer Checkpoints → Longer Recovery Time
```

---

# 5. wal_keep_size

## Purpose

Defines the minimum amount of WAL files retained for standby servers.

Check current value:

```sql
SHOW wal_keep_size;
```

Configuration example:

```conf
wal_keep_size = 1GB
```

---

## Usage

Used mainly in:

```text
Primary Server → Standby Server
```

Insufficient WAL retention may cause standby synchronization problems.

---

# 6. archive_mode

## Purpose

Enables WAL archiving for backup and recovery scenarios.

Check current value:

```sql
SHOW archive_mode;
```

Configuration example:

```conf
archive_mode = on
```

---

# 7. WAL and Recovery

During a crash:

```text
Server Failure → Restart PostgreSQL → Read WAL → Replay Changes → Database Recovery
```

WAL ensures committed transactions are not lost.

---

# 8. WAL and Replication

Physical replication uses WAL records.

Flow:

```text
Primary Database → Generate WAL → Send WAL → Standby Server → Replay WAL
```

---

# 9. Common WAL Problems

## Problem: WAL Files Consume Too Much Disk Space

Possible causes:

- Replication delay
- Long-running transactions
- Incorrect archiving configuration

Investigation:

```text
High WAL Usage → Check Replication Status → Check Long Transactions → Review WAL Settings
```

---

## Problem: Standby Server Cannot Keep Up

Possible causes:

- Network limitation
- High write workload
- Insufficient WAL retention

Investigation:

```text
Replication Delay → Check Network → Check WAL Generation Rate → Adjust Configuration
```

---

# 10. Verification Commands

Check WAL settings:

```sql
SHOW ALL;
```

Check WAL statistics:

```sql
SELECT *
FROM pg_stat_wal;
```

Check replication status:

```sql
SELECT *
FROM pg_stat_replication;
```

---

# 11. Best Practices

- Choose `wal_level` based on requirements.
- Monitor WAL growth.
- Configure enough WAL retention for replication.
- Monitor replication lag.
- Consider recovery requirements before changing WAL settings.
- Document all configuration changes.

---

# Summary

WAL is the foundation of PostgreSQL reliability.

A DBA must understand:

- How WAL protects data
- How WAL supports recovery
- How WAL enables replication
- How WAL configuration affects performance

---

# Next Steps

Continue with:

- Checkpoint Configuration
- Logging Configuration
- Connection Configuration
- Autovacuum Configuration
