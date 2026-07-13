---
title: SQL
weight: 5
cascade:
  type: docs
---

## Introduction
The `sql` post-execution command allows you to evaluate expressions in **SQL**. The expression must explicitly return a boolean value (TRUE or FALSE / 1 or 0), indicating whether the check passed or not.   
It is essential to specify the `driverName`, which indicates the type of database you want to connect to, and the `dsn` (Data Source Name), which provides the credentials and information necessary for the connection. For each type of database, it is recommended to consult the official documentation for the correct syntax of the `dsn`.

## Supported Drivers
The drivers supported by **Mtracer** are:
- `pgx`: driver for **PostgreSQL**
- `mysql`: driver for **MySQL**
- `sqlite`: driver for **SQLite**

Additionally, the default value for `driverName` is `pgx`.

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
