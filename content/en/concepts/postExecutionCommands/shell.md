---
title: Shell
weight: 1
cascade:
  type: docs
---

The `shell` post-execution check command allows you to execute terminal commands after a **test** execution to make assertions of any kind by comparing the **exit codes** of the executed commands.   
The operation is simple: the developer specifies the terminal command to execute and optionally the expected **exit code**. If the **exit code** of the executed command does not match the expected one, the post-execution check command fails and consequently the check fails as well.   
The expected **exit code** is optional; if not specified, the default value is `0`.

## Example
```yaml
postExecChecks:
  - name: "Shell command check"
    type: "shell"
    args:
      cmd: "echo 'Hello'"
      expectedExitCode: 0 # Optional, default value is 0
```
