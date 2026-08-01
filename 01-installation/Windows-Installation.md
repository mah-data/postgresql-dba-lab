# PostgreSQL Installation on Windows

## Overview

This document describes how to install PostgreSQL on Windows using the official PostgreSQL installer.

The Windows installer provides:

* PostgreSQL Database Server
* pgAdmin 4
* Command Line Tools
* Additional utilities

This method is suitable for:

* Windows Server environments
* Development systems
* Testing environments
* Local database labs

---

# 1. Prerequisites

Requirements:

* Windows 10/11 or Windows Server
* Administrator access
* Minimum 2 GB RAM
* Minimum 5 GB free disk space

Verify Windows version:

```powershell
winver
```

---

# 2. Download PostgreSQL Installer

Download PostgreSQL installer from the official PostgreSQL website.

Select:

* Windows operating system
* Required PostgreSQL version

Recommended:

* Use the latest stable release
* Download installer from official source

📷 Screenshot:

```text
images/windows-download-installer.png
```

---

# 3. Run Installation Wizard

Run the installer as Administrator.

Installation steps:

```text
Start Installer
      |
      v
Select Installation Directory
      |
      v
Select Components
      |
      v
Configure Data Directory
      |
      v
Set Password
      |
      v
Configure Port
      |
      v
Install PostgreSQL Service
```

---

# 4. Select Components

Recommended components:

| Component          | Description                   |
| ------------------ | ----------------------------- |
| PostgreSQL Server  | Main database engine          |
| pgAdmin 4          | Graphical administration tool |
| Command Line Tools | psql and database utilities   |
| Stack Builder      | Additional packages           |

📷 Screenshot:

```text
images/windows-components-selection.png
```

---

# 5. Configure Data Directory

The data directory stores:

* Database files
* Configuration files
* Transaction logs

Example:

```text
C:\Program Files\PostgreSQL\16\data
```

Best Practice:

* Avoid storing database files on system drive for production servers
* Use separate storage when possible

---

# 6. Set PostgreSQL Administrator Password

Set password for default database administrator:

```text
User:
postgres

Password:
StrongPassword
```

Important:

* Store password securely
* Do not use simple passwords

---

# 7. Configure PostgreSQL Port

Default PostgreSQL port:

```text
5432
```

Check port availability:

```powershell
netstat -ano | findstr 5432
```

---

# 8. Complete Installation

Finish installation wizard.

PostgreSQL is installed as a Windows Service.

Check service:

```powershell
services.msc
```

Service name example:

```text
postgresql-x64-16
```

📷 Screenshot:

```text
images/windows-service-status.png
```

---

# 9. Verify Installation

Open Command Prompt:

```cmd
psql --version
```

Example:

```text
psql (PostgreSQL) 16.x
```

Connect:

```cmd
psql -U postgres
```

Check version:

```sql
SELECT version();
```

---

# 10. Create Test Database

Create database:

```sql
CREATE DATABASE testdb;
```

Check databases:

```sql
\l
```

---

# 11. Create Application User

Create user:

```sql
CREATE USER app_user
WITH PASSWORD 'StrongPassword123';
```

Grant permission:

```sql
GRANT ALL PRIVILEGES
ON DATABASE testdb
TO app_user;
```

---

# 12. Configure Firewall

Allow PostgreSQL port if remote access is required.

Default port:

```text
5432
```

Check Windows Firewall rules.

---

# 13. Basic Troubleshooting

## PostgreSQL Service Does Not Start

Check:

```powershell
services.msc
```

Review log files:

```text
C:\Program Files\PostgreSQL\16\data\log
```

---

## Connection Failed

Check:

* PostgreSQL service status
* Port 5432
* Firewall rules
* User password
* pg_hba.conf settings

---

## Cannot Find psql Command

Add PostgreSQL binary path:

Example:

```text
C:\Program Files\PostgreSQL\16\bin
```

to Windows Environment Variables.

---

# 14. Best Practices

* Use the latest stable PostgreSQL version.
* Secure the postgres password.
* Enable regular backups.
* Separate database storage for production systems.
* Monitor PostgreSQL logs.
* Keep PostgreSQL updated.

---

# Installation Verification Checklist

| Check                          | Status |
| ------------------------------ | ------ |
| PostgreSQL service running     | ✓      |
| psql command available         | ✓      |
| Database connection successful | ✓      |
| Test database created          | ✓      |
| User created successfully      | ✓      |
| Port configuration verified    | ✓      |

---

# Next Steps

Continue with:

* PostgreSQL Configuration
* Security Hardening
* Backup and Recovery
* Monitoring
* Performance Tuning
