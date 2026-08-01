# PostgreSQL Installation on Linux Using Docker

## Overview

This document describes how to run PostgreSQL on Linux using Docker containers.

Docker installation is recommended for:

* Development environments
* Testing environments
* Local labs
* Temporary database instances

For production environments, a native installation is usually preferred unless a container platform is properly managed.

---

# 1. Prerequisites

Requirements:

* Linux server
* Root or sudo access
* Docker installed
* Internet connection

Check Docker installation:

```bash
docker --version
```

Example:

```text
Docker version 27.x
```

---

# 2. Install Docker (Ubuntu Example)

Update packages:

```bash
sudo apt update
```

Install Docker:

```bash
sudo apt install docker.io -y
```

Start Docker service:

```bash
sudo systemctl start docker
```

Enable Docker at startup:

```bash
sudo systemctl enable docker
```

Check Docker status:

```bash
sudo systemctl status docker
```

---

# 3. Download PostgreSQL Docker Image

Download PostgreSQL image:

```bash
docker pull postgres:16
```

Verify image:

```bash
docker images
```

Example:

```text
postgres    16    latest
```

---

# 4. Run PostgreSQL Container

Create PostgreSQL container:

```bash
docker run \
--name postgres-db \
-e POSTGRES_PASSWORD=StrongPassword123 \
-p 5432:5432 \
-d postgres:16
```

Parameters:

| Parameter | Description           |
| --------- | --------------------- |
| --name    | Container name        |
| -e        | Environment variables |
| -p        | Port mapping          |
| -d        | Run in background     |

---

# 5. Check Container Status

List running containers:

```bash
docker ps
```

Example:

```text
postgres-db   postgres:16   Up
```

View logs:

```bash
docker logs postgres-db
```

---

# 6. Connect to PostgreSQL Container

Connect using psql:

```bash
docker exec -it postgres-db psql -U postgres
```

Check version:

```sql
SELECT version();
```

---

# 7. Create Database

Inside PostgreSQL:

```sql
CREATE DATABASE testdb;
```

List databases:

```sql
\l
```

---

# 8. Create User

Create application user:

```sql
CREATE USER app_user
WITH PASSWORD 'StrongPassword123';
```

Grant permissions:

```sql
GRANT ALL PRIVILEGES
ON DATABASE testdb
TO app_user;
```

---

# 9. Persistent Storage

By default, container data can be lost when the container is removed.

Create a Docker volume:

```bash
docker volume create postgres-data
```

Run PostgreSQL with volume:

```bash
docker run \
--name postgres-db \
-e POSTGRES_PASSWORD=StrongPassword123 \
-p 5432:5432 \
-v postgres-data:/var/lib/postgresql/data \
-d postgres:16
```

---

# 10. Container Management

Stop container:

```bash
docker stop postgres-db
```

Start container:

```bash
docker start postgres-db
```

Restart container:

```bash
docker restart postgres-db
```

Remove container:

```bash
docker rm postgres-db
```

---

# 11. Backup PostgreSQL Container

Create backup:

```bash
docker exec postgres-db \
pg_dump -U postgres testdb \
> testdb_backup.sql
```

Restore backup:

```bash
cat testdb_backup.sql | \
docker exec -i postgres-db \
psql -U postgres testdb
```

---

# 12. Troubleshooting

## Container Does Not Start

Check logs:

```bash
docker logs postgres-db
```

---

## Port Conflict

Check port usage:

```bash
ss -lntp | grep 5432
```

---

## Connection Problem

Verify:

```bash
docker ps
```

Check PostgreSQL listener:

```bash
docker exec postgres-db \
pg_isready
```

---

# 13. Best Practices

* Always use persistent volumes.
* Do not store production passwords in plain text.
* Regularly backup container databases.
* Keep Docker images updated.
* Limit container resources in production.
* Monitor container logs.

---

# Next Steps

Continue with:

* PostgreSQL Configuration
* Backup and Recovery
* Monitoring
* Performance Tuning
