# PostgreSQL Installation on Windows

## Overview

PostgreSQL is an open-source object-relational database management system (ORDBMS).

This document describes the installation process, initial configuration, verification, and first database setup of PostgreSQL Server on Windows.

---

# 1. Download PostgreSQL

Download the PostgreSQL installer from the official PostgreSQL website.

The installer includes:

* PostgreSQL Database Server
* pgAdmin 4
* Command Line Tools
* Additional utilities
---

# 2. Install PostgreSQL

Run the PostgreSQL installer with administrator privileges.

Select the installation directory.

Example:

```text
C:\Program Files\PostgreSQL\16
```


# 3. Select Components

Select required components:

* PostgreSQL Server
* pgAdmin 4
* Command Line Tools
* Stack Builder (Optional)

---

# 4. Configure Data Directory

The Data Directory stores:

* Database files
* Configuration files
* Transaction logs
* System catalogs

Example:

```text
C:\Program Files\PostgreSQL\16\data
```


# 5. Configure Administrator Password

During installation set the password for the default administrator user.

Default user:

```text
Username: postgres
Password: ********
```

# 6. Configure PostgreSQL Port

Default PostgreSQL port:

```text
5432
```

# 7. Verify PostgreSQL Installation

Open Command Prompt:

```bash
psql -U postgres
```

Check PostgreSQL version:

```sql
SELECT version();
```

Example result:

```text
PostgreSQL 16.x
```

# 8. Check Server Configuration

Show configuration file location:

```sql
SHOW config_file;
```

Show data directory:

```sql
SHOW data_directory;
```


# 9. PostgreSQL Service Management

Check PostgreSQL service:

```powershell
Get-Service *postgres*
```

Start service:

```powershell
Start-Service postgresql-x64-16
```

Stop service:

```powershell
Stop-Service postgresql-x64-16
```

Restart service:

```powershell
Restart-Service postgresql-x64-16
```

---

# 10. Connect Using pgAdmin

Create a new server connection:

```text
Host: localhost
Port: 5432
Database: postgres
Username: postgres
Password: ********
```


# 11. Create First Database

Create database:

```sql
CREATE DATABASE testdb;
```

List databases:

```sql
SELECT datname 
FROM pg_database;
```

or:

```sql
\l
```

# 12. Create First User

Create user:

```sql
CREATE USER app_user 
WITH PASSWORD 'StrongPassword123';
```

Check users:

```sql
SELECT usename 
FROM pg_user;
```

---

# 13. Grant Permissions

Grant database access:

```sql
GRANT ALL PRIVILEGES 
ON DATABASE testdb 
TO app_user;
```

Change database owner:

```sql
ALTER DATABASE testdb 
OWNER TO app_user;
```

---

# 14. Test Connection

Connect:

```bash
psql -U app_user -d testdb
```

Check current user:

```sql
SELECT current_user;
```

Check current database:

```sql
SELECT current_database();
```

---

# 15. Initial Health Check

## Active Connections

```sql
SELECT 
    datname,
    count(*)
FROM pg_stat_activity
GROUP BY datname;
```

## Database Size

```sql
SELECT 
    datname,
    pg_size_pretty(pg_database_size(datname)) AS size
FROM pg_database;
```

---

# 16. Initial Configuration Recommendations

After installation:

* Review postgresql.conf
* Review pg_hba.conf
* Configure logging
* Configure backup strategy
* Configure monitoring
* Schedule maintenance tasks

---

# 17. Troubleshooting

## Service Not Started

Check:

* PostgreSQL service status
* Log files
* Port availability

## Authentication Failed

Check:

* Username
* Password
* pg_hba.conf rules

## Connection Failed

Check:

* Firewall
* Port 5432
* Server status

---

# Next Steps

Continue with:

* PostgreSQL Configuration
* User and Role Management
* Backup and Recovery
* Monitoring
* Maintenance
* Performance Tuning
