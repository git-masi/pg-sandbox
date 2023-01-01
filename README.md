# PG Sandbox

## About

This is a simple collection of files that shows how to get up and running quickly with Postgres and Docker

## Docker

### Start docker containers
`docker compose up -d`

### Stop docker containers
`docker compose down --remove-orphans -v`

### Use psql as super user
`docker exec -it sandbox-db psql -U postgres`

### Use psql as a specific DB and user
`docker exec -it sandbox-db psql -d DB_NAME -U USER_NAME -W`

### pgAdmin host

Instead of "localhost" use "host.docker.internal"

## Initial setup

```sql
CREATE DATABASE test_db OWNER postgres;

-- A role could be a user or a group depending on how it is used
CREATE ROLE test_user LOGIN PASSWORD 'testing123';

-- Creating a new schema is not required but can be useful
CREATE schema IF NOT EXISTS test_schema;

-- Set the new schema to the search path to avoid long chains like `test_schema.some_table_name`
SET search_path TO test_schema;

GRANT USE, CREATE ON test_schema TO test_user;

GRANT SELECT, UPDATE, INSERT ON ALL TABLES IN SCHEMA test_schema TO test_user;

-- The auto increment of `BIGSERIAL` is a "sequence" so this is needed for inserts
GRANT usage ON ALL SEQUENCES IN SCHEMA test_schema TO test_user;

CREATE TABLE IF NOT EXISTS test_table(id BIGSERIAL PRIMARY KEY, val TEXT NOT NULL);
```