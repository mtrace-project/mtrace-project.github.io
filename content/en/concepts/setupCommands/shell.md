---
title: Shell
weight: 1
cascade:
  type: docs
---

The *shell* setup command allows you to run any command-line instruction before and after a test execution, such as executing a *Python* script to populate a database.
If you define a *shell* setup command, it is mandatory to specify another command of the same type to be executed after the test finishes; the latter is called a **cleanup** command.

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
