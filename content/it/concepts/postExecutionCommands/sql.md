---
title: SQL
weight: 5
cascade:
  type: docs
---

## Introduzione
Il comando di controllo post-esecuzione `sql` consente di valutare espressioni in linguaggio **SQL**. L'espressione deve restituire esplicitamente un valore booleano (TRUE o FALSE / 1 o 0), che indica se il controllo ha avuto esito positivo.   
È fondamentale specificare il `driverName`, che indica il tipo di database a cui ci si vuole connettere, e il `dsn` (Data Source Name), che fornisce le informazioni necessarie per la connessione. Per ogni tipo di database, si consiglia di consultare la documentazione ufficiale per verificare la sintassi corretta del `dsn`.

## Driver supportati
I driver supportati da **Mtracer** sono i seguenti:
- `pgx`: driver per **PostgreSQL**
- `mysql`: driver per **MySQL**
- `sqlite`: driver per **SQLite**

Inoltre, il valore di default del `driverName` è `pgx`.

## Esempio
```yaml
postExecChecks:
  - name: "Check if there are any rolls for Alice in the database"
    type: "sql"
    args:
      driverName: "pgx" # Valore di default è "pgx"
      dsn: "postgres://postgres:postgres@localhost:5432/postgres?sslmode=disable"
      query: "(SELECT COUNT(*) FROM rolls WHERE player_name = 'Alice') > 0"
```