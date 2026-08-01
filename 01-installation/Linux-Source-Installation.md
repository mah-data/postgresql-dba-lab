# PostgreSQL Installation on Linux From Source Code

## Overview

This document describes how to install PostgreSQL on Linux by compiling the source code.

Source installation provides full control over:

* Installation location
* Build options
* Extensions
* Custom compilation settings

This method is mainly used for:

* Development environments
* Testing
* Custom PostgreSQL builds

---

# 1. Prerequisites

Requirements:

* Linux server
* Root or sudo access
* Compiler tools
* Required development libraries
* Minimum available disk space

Check operating system:

```bash
cat /etc/os-release
```

---

# 2. Install Required Packages

## Ubuntu / Debian

```bash
sudo apt update

sudo apt install -y \
build-essential \
libreadline-dev \
zlib1g-dev \
flex \
bison \
libssl-dev \
wget
```

---

## RHEL / Rocky Linux / AlmaLinux

```bash
sudo dnf groupinstall "Development Tools" -y

sudo dnf install -y \
readline-devel \
zlib-devel \
openssl-devel \
wget
```

---

# 3. Download PostgreSQL Source Code

Download PostgreSQL source package:

```bash
wget https://ftp.postgresql.org/pub/source/v16.2/postgresql-16.2.tar.gz
```

Extract package:

```bash
tar -xzf postgresql-16.2.tar.gz
```

Move to source directory:

```bash
cd postgresql-16.2
```

---

# 4. Configure Build Options

Configure installation directory:

```bash
./configure \
--prefix=/usr/local/pgsql \
--with-openssl
```

Verify configuration result.

---

# 5. Compile PostgreSQL

Build PostgreSQL:

```bash
make
```

Install:

```bash
sudo make install
```

Verify installation:

```bash
/usr/local/pgsql/bin/postgres --version
```

---

# 6. Create PostgreSQL Operating System User

Create database service user:

```bash
sudo useradd postgres
```

Create data directory:

```bash
sudo mkdir /usr/local/pgsql/data
```

Change ownership:

```bash
sudo chown postgres:postgres \
/usr/local/pgsql/data
```

---

# 7. Initialize Database Cluster

Switch to postgres user:

```bash
sudo -i -u postgres
```

Initialize cluster:

```bash
/usr/local/pgsql/bin/initdb \
-D /usr/local/pgsql/data
```

The command creates:

* System catalogs
* Configuration files
* Default database

---

# 8. Start PostgreSQL Server

Start database server:

```bash
/usr/local/pgsql/bin/pg_ctl \
-D /usr/local/pgsql/data \
start
```

Check status:

```bash
/usr/local/pgsql/bin/pg_ctl \
-D /usr/local/pgsql/data \
status
```

---

# 9. Configure Environment Variables

Add PostgreSQL binaries to PATH:

```bash
export PATH=/usr/local/pgsql/bin:$PATH
```

Permanent configuration:

```bash
echo 'export PATH=/usr/local/pgsql/bin:$PATH' >> ~/.bashrc
```

Reload:

```bash
source ~/.bashrc
```

---

# 10. Connect to PostgreSQL

Connect:

```bash
psql
```

Check version:

```sql
SELECT version();
```

---

# 11. Create Database and User

Create database:

```sql
CREATE DATABASE testdb;
```

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

# 12. Stop PostgreSQL Server

Stop server:

```bash
pg_ctl \
-D /usr/local/pgsql/data \
stop
```

Restart:

```bash
pg_ctl \
-D /usr/local/pgsql/data \
restart
```

---

# 13. Troubleshooting

## Compilation Error

Check:

* Required packages
* Compiler version
* Library dependencies

---

## Server Does Not Start

Check logs:

```bash
cat /usr/local/pgsql/data/log/*
```

Check configuration:

```text
postgresql.conf
pg_hba.conf
```

---

# 14. Best Practices

* Use package installation for normal production deployments.
* Use source installation only when customization is required.
* Document build options.
* Keep source version aligned with security updates.
* Separate PostgreSQL binaries and data directory.

---

# Next Steps

Continue with:

* PostgreSQL Configuration
* Security Hardening
* Backup and Recovery
* Performance Tuning
