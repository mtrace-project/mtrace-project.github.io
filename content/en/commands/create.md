---
title: Create test
weight: 5
cascade:
  type: docs
---

The `mtrace create` command allows you to create a new test file with the `.mt.yaml` extension in the specified directory (or in the current directory if none is specified). The generated test file will contain a basic template that developers can modify according to their needs.

## Command execution
```bash
mtrace create [test_name] [flags]
```
The `.mt.yaml` extension will be automatically added to the file name, so there is no need to include it in the command. For example, running `mtrace create test1` will create a file named `test1.mt.yaml`.

## Flags
The `mtrace create` command supports the following flags:
- `--dir` or `-d`: specifies the directory where the test file will be created. If not provided, the file will be created in the current working directory.
- `--trigger-type` or `-t`: specifies the type of *trigger* to use to start the trace. If not provided, **HTTP** will be used as the default trigger type.    
You can view all supported trigger types in the [Supported triggers]({{< ref "../concepts/trigger.md#supported-triggers" >}}) section.
