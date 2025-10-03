# PostgreSQL 18 with Docker Compose

[![Docker](https://img.shields.io/badge/Docker-✔-2496ED?logo=docker&logoColor=white)](https://www.docker.com/) [![Postgres](https://img.shields.io/badge/PostgreSQL-✔-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org/)

PostgreSQL is a powerful, open-source relational database system.  
This repository provides a simple **Docker Compose setup** for PostgreSQL 18.

---

## Purpose

I use PostgreSQL for most of my projects and you probably should too.  
With the new release, **PostgreSQL 18** now supports **native UUIDv7**, which is excellent for creating indexed UUIDs.  

To simplify installation and usage, I prepared this lightweight Docker Compose configuration.

---

## Prerequisites

- A VPS or local machine running Linux (tested on Ubuntu & Arch Linux)  
- Docker and Docker Compose installed  
- A Docker network named `application_network`  
- A PostgreSQL client (CLI or GUI, e.g., psql, DBeaver, TablePlus, etc.)  

---

## Installation & Usage

1. Clone this repository:
   ```bash
   git clone https://github.com/wolewd/postgres.git
   cd postgres
   ```

2. Create an `.env` file:
   ```bash
   touch .env
   ```

3. Copy this into `.env` and change values to your own configuration:
   ```env
   POSTGRES_HOST=localhost
   POSTGRES_USER=your-username
   POSTGRES_PASSWORD=your-password
   POSTGRES_DB=your-database-name
   POSTGRES_PORT=5432
   ```
   Refer to the provided [.env.example](https://github.com/wolewd/postgres/blob/main/.env.example).

4. Start the container:
   ```bash
   docker compose up -d
   ```
5. Connect to the database. Make sure your application or PostgreSQL client is connected to the same `application_network`.
---

## Notes 

- By default, data is stored inside a Docker volume.
- You can modify `docker-compose.yaml` to map data to a local folder if you want direct access. However, it’s generally **not recommended** to manipulate the database files directly outside PostgreSQL client.

---

## Backup Database

### 1. Backup all databases
  Backup
  ```shell
  docker exec -t container-name pg_dumpall -U your-username > backup.sql
  ```

  Restore
  ```shell
  cat backup.sql | docker exec -i container-name psql -U your-username
  ```

### 2. Backup specific databases
  Backup
  ```shell
  docker exec -t container-name pg_dump -U your-username -d your-database > your-backup.sql
  ```

  Restore 
  ```shell
  cat your-backup.sql | docker exec -i container-name psql -U your-username -d your-database
  ```

The backup files will be stored inside this directory. 

---
