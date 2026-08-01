# PostgreSQL Installation on Linux Using Default Repository

## Overview

This document describes the installation of PostgreSQL using the default package repository provided by the Linux distribution.

This method is simple and suitable for:

* Development environments
* Test environments
* Learning environments

For production environments, the official PGDG repository is usually recommended.

---

# 1. Prerequisites

Before installation:

* Linux operating system
* Root or sudo privileges
* Internet access
* Available disk space

Check Linux distribution:

```bash
cat /etc/os-release
```

---

# 2. Installation on Ubuntu / Debian

## Update Package List

```bash
sudo apt update
```

---

## Install PostgreSQL

```bash
sudo apt install postgresql postgresql-contrib -y
```

Installed components:

* PostgreSQL Server
* Client tools
* Additional extensions

---

## Check Installed Version

```bash
psql --version
```

Example:

```text
psql (PostgreSQL) 14.x
```

---

# 3. Installation on RHEL / Rocky Linux / AlmaLinux

## Install PostgreSQL Packages

```bash
sudo dnf install postgresql-server postgresql-contrib -y
```

---

## Initialize Database Cluster

```bash
sudo postgresql-setup --initdb
```

---

# 4. Start PostgreSQL Service

Start service:

```bash
sudo systemctl start postgresql
```

Enable startup after reboot:

```bash
sudo systemctl enable postgresql
```

Check status:

```bash
sudo systemctl status postgresql
```

---

# 5. Connect to PostgreSQL

Switch to PostgreSQL administrator:

```bash
sudo -i -u postgres
```

Open PostgreSQL shell:

```bash
psql
```

---

# 6. Verify Installation

Check version:

```sql
SELECT version();
```

Check configuration file:

```sql
SHOW config_file;
```

Check data directory:

```sql
SHOW data_directory;
```

---

# 7. Create Database

Create a sample database:

```sql
CREATE DATABASE testdb;
```

List databases:

```sql
\l
```

---

# 8. Create Database User

Create user:

```sql
CREATE USER app_user
WITH PASSWORD 'StrongPassword123';
```

Grant privileges:

```sql
GRANT ALL PRIVILEGES
ON DATABASE testdb
TO app_user;
```

---

# 9. Service Management

Restart PostgreSQL:

```bash
sudo systemctl restart postgresql
```

Stop PostgreSQL:

```bash
sudo systemctl stop postgresql
```

Reload configuration:

```bash
sudo systemctl reload postgresql
```

---

# 10. Check PostgreSQL Port

Default port:

```text
5432
```

Check listener:

```bash
ss -lntp | grep 5432
```

---

# 11. Troubleshooting

## Service Does Not Start

Check service:

```bash
systemctl status postgresql
```

Check logs:

```bash
journalctl -u postgresql
```

---

## Authentication Error

Check:

```text
pg_hba.conf
```

Verify:

* Username
* Password
* Authentication method

---

## Connection Error

Check:

* PostgreSQL service
* Firewall rules
* Port 5432
* Network configuration

---

# 12. Best Practices

* Use PGDG repository for production systems.
* Keep packages updated.
* Configure backups before using production data.
* Review PostgreSQL configuration files.
* Monitor database logs.

---

# Next Steps

Continue with:

* PostgreSQL Configuration
* User and Role Management
* Backup and Recovery
* Monitoring
