# PostgreSQL Installation on Linux Using PGDG Repository

## Overview

This document describes the installation of PostgreSQL on Linux using the official PostgreSQL Global Development Group (PGDG) repository.

The PGDG repository provides the latest stable PostgreSQL versions and is recommended for production environments.

Supported operating systems:

* Ubuntu / Debian
* RHEL / Rocky Linux / AlmaLinux

---

# 1. Prerequisites

Before installation, verify:

* Linux server is available
* Sudo or root access
* Internet connection
* Minimum required disk space
* Firewall configuration

Check operating system:

```bash
cat /etc/os-release
```

---

# 2. Installation on Ubuntu / Debian

## Add PostgreSQL Repository

Update system packages:

```bash
sudo apt update
```

Install required packages:

```bash
sudo apt install -y wget ca-certificates gnupg
```

Import PostgreSQL signing key:

```bash
wget -qO - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
sudo gpg --dearmor -o /usr/share/keyrings/postgresql.gpg
```

Add PGDG repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/postgresql.gpg] \
http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" | \
sudo tee /etc/apt/sources.list.d/pgdg.list
```

Update repository information:

```bash
sudo apt update
```

---

## Install PostgreSQL

Example: PostgreSQL 16

```bash
sudo apt install -y postgresql-16 postgresql-client-16
```

Install additional tools:

```bash
sudo apt install -y postgresql-contrib-16
```

---

# 3. Installation on RHEL / Rocky Linux / AlmaLinux

## Add PostgreSQL Repository

Install PGDG repository package:

```bash
sudo dnf install -y \
https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

Disable default PostgreSQL module:

```bash
sudo dnf -qy module disable postgresql
```

---

## Install PostgreSQL Server

```bash
sudo dnf install -y postgresql16-server postgresql16
```

---

## Initialize Database Cluster

```bash
sudo /usr/pgsql-16/bin/postgresql-16-setup initdb
```

---

# 4. Start PostgreSQL Service

Ubuntu / Debian:

```bash
sudo systemctl start postgresql
```

Enable automatic startup:

```bash
sudo systemctl enable postgresql
```

RHEL Based:

```bash
sudo systemctl start postgresql-16
```

Enable service:

```bash
sudo systemctl enable postgresql-16
```

---

# 5. Verify Installation

Check PostgreSQL version:

```bash
psql --version
```

Connect to PostgreSQL:

```bash
sudo -u postgres psql
```

Check database version:

```sql
SELECT version();
```

---

# 6. Initial Database Configuration

## Create Database

```sql
CREATE DATABASE testdb;
```

---

## Create User

```sql
CREATE USER app_user
WITH PASSWORD 'StrongPassword123';
```

---

## Grant Access

```sql
GRANT ALL PRIVILEGES
ON DATABASE testdb
TO app_user;
```

---

# 7. Service Management

Check status:

```bash
systemctl status postgresql
```

Restart service:

```bash
sudo systemctl restart postgresql
```

Reload configuration:

```bash
sudo systemctl reload postgresql
```

---

# 8. Verify Network Listener

Check PostgreSQL port:

```bash
ss -lntp | grep 5432
```

Default PostgreSQL port:

```text
5432
```

---

# 9. Basic Health Check

Active connections:

```sql
SELECT
    datname,
    count(*)
FROM pg_stat_activity
GROUP BY datname;
```

Database size:

```sql
SELECT
    datname,
    pg_size_pretty(pg_database_size(datname))
FROM pg_database;
```

---

# 10. Best Practices

* Use PGDG repository for production installations.
* Keep PostgreSQL packages updated.
* Configure backups before application deployment.
* Review authentication settings.
* Monitor logs regularly.
* Document installation parameters.

---

# Troubleshooting

## PostgreSQL Service Not Running

Check:

```bash
systemctl status postgresql
```

View logs:

```bash
journalctl -u postgresql
```

---

## Connection Failed

Check:

```bash
ss -lntp | grep 5432
```

Review:

```text
postgresql.conf
pg_hba.conf
```

---

# Next Steps

Continue with:

* PostgreSQL Configuration
* Security Hardening
* Backup and Recovery
* Monitoring
* Performance Tuning
