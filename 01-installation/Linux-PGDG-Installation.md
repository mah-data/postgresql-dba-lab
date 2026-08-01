# PostgreSQL Installation on Linux (PGDG Repository)

## Overview

This guide explains how to install PostgreSQL on Linux using the official PostgreSQL Global Development Group (PGDG) repository. 
This is the recommended method for production environments because it provides the latest stable releases and simplifies updates.

---

## Prerequisites

* Ubuntu 22.04+ or Debian 12+
* Sudo privileges
* Internet connection
* At least 2 GB RAM
* 10 GB free disk space

---

## Step 1 – Update the System

```bash
sudo apt update
sudo apt upgrade -y


## Step 2 – Add the PostgreSQL Repository

Import the PostgreSQL signing key:

```bash
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
sudo gpg --dearmor -o /usr/share/keyrings/postgresql.gpg
```

Add the PGDG repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/postgresql.gpg] \
https://apt.postgresql.org/pub/repos/apt \
$(lsb_release -cs)-pgdg main" | \
sudo tee /etc/apt/sources.list.d/pgdg.list
```

Update package information:

```bash
sudo apt update
```


## Step 3 – Install PostgreSQL

Install PostgreSQL 16:

```bash
sudo apt install postgresql-16 postgresql-client-16 -y
```

Verify the installation:

```bash
psql --version
```

## Step 4 – Check the Service

Verify the PostgreSQL service:

```bash
sudo systemctl status postgresql
```

Start the service if needed:

```bash
sudo systemctl start postgresql
```

Enable automatic startup:

```bash
sudo systemctl enable postgresql
```

## Step 5 – Connect to PostgreSQL

Switch to the PostgreSQL administrator account:

```bash
sudo -u postgres psql
```

Verify the version:

```sql
SELECT version();
```

---

## Step 6 – Create a Database

```sql
CREATE DATABASE companydb;
```

List all databases:

```sql
\l
```

---

## Step 7 – Create a User

```sql
CREATE USER dbadmin
WITH PASSWORD 'StrongPassword123';
```

Grant privileges:

```sql
GRANT ALL PRIVILEGES
ON DATABASE companydb
TO dbadmin;
```

---

## Step 8 – Verify Configuration

Display the configuration file:

```sql
SHOW config_file;
```

Display the data directory:

```sql
SHOW data_directory;
```

---

## Step 9 – Health Check

Show active sessions:

```sql
SELECT datname, COUNT(*)
FROM pg_stat_activity
GROUP BY datname;
```

Show database sizes:

```sql
SELECT datname,
       pg_size_pretty(pg_database_size(datname)) AS size
FROM pg_database;
```

---

## Troubleshooting

| Issue                  | Solution                                   |
| ---------------------- | ------------------------------------------ |
| Service is not running | Check `systemctl status postgresql`        |
| Authentication failed  | Verify user credentials and `pg_hba.conf`  |
| Port conflict          | Check if port `5432` is already in use     |
| Repository error       | Verify the PGDG repository and signing key |

---

## Best Practices

* Install PostgreSQL from the official PGDG repository.
* Keep PostgreSQL updated with security patches.
* Use strong passwords for all database users.
* Configure regular backups immediately after installation.
* Monitor PostgreSQL logs and service status.

---

## Images

* `images/linux-update-system.png`
* `images/add-pgdg-repository.png`
* `images/install-postgresql.png`
* `images/systemctl-status.png`

---

## References

* PostgreSQL Official Documentation
* PostgreSQL Wiki
