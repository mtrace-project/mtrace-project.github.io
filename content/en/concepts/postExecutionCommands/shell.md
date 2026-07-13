---
title: Shell
weight: 1
cascade:
  type: docs
---

The `shell` post-execution command allows you to execute terminal commands after a **test** to perform any type of assertion by comparing the **exit codes** of the executed commands.   
It is simple to use: the developer specifies the command to execute and, optionally, the expected **exit code**. If the returned **exit code** does not match the expected one, the post-execution check fails.   
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
