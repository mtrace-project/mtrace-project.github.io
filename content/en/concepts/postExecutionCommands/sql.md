---
title: SQL
weight: 5
cascade:
  type: docs
---

## Introduction
The `sql` post-execution check command allows you to execute boolean expressions in **SQL** language. The expression must explicitly return a boolean value (TRUE or FALSE / 1 or 0), which indicates whether the check passed or not.   
It is essential to specify the `driverName`, which indicates the type of database you want to connect to, and the `dsn` (Data Source Name), which specifies the necessary information to connect to the database. For each database type, we recommend consulting the official documentation for the correct syntax of the `dsn`.

## Supported drivers
The drivers supported by **Mtrace** are the following:
- `pgx`: driver for **PostgreSQL**
- `mysql`: driver for **MySQL**
- `sqlite`: driver for **SQLite**

Furthermore, the default value for the `driverName` is `pgx`.

## Example
```yaml
postExecChecks:
  - name: "Check if there are any rolls for Alice in the database"
    type: "sql"
    args:
      driverName: "pgx" # Default value is "pgx"
      dsn: "postgres://postgres:postgres@localhost:5432/postgres?sslmode=disable"
      query: "(SELECT COUNT(*) FROM rolls WHERE player_name = 'Alice') > 0"
```
