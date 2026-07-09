---
title: Shell
weight: 1
cascade:
  type: docs
---

The *shell* setup command allows you to run any command-line instruction before and after a test execution, such as running a Python script to populate a database.   
If you specify a *shell* setup command, it is mandatory to specify one of the same type after the test execution, which is called the **cleanup** command.

#### Example
```yaml
setupCommands:
  - type: "shell"
    cmd: "docker compose -f docker-compose.yaml up -d"
    cleanupCmd:
      cmd: "docker compose -f docker-compose.yaml down -v"
  - cmd: "python3 setup.py" # type: "shell" is implicit
    cleanupCmd:
      cmd: "python3 cleanup.py"
```
