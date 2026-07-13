---
title: Create test
weight: 5
cascade:
  type: docs
---

The `mtracer create` command allows you to create a new test file with the `.mt.yaml` extension inside the specified directory (or in the current one, if not specified). The created test file will contain a basic structure that can be modified according to the developer's needs.

## Command execution
```bash
mtracer create [test_name] [flags]
```
The `.mt.yaml` extension will be automatically added to the file name, so there is no need to include it in the command. For example, executing the command `mtracer create test1` will create a file named `test1.mt.yaml`.

## Flags
The `mtracer create` command supports the following flags:
- `--dir` or `-d`: allows you to specify the directory in which to create the test file. If not specified, the file will be created in the directory from which the command is executed.
- `--trigger-type` or `-t`: allows you to specify the type of *trigger* to use to start the *trace*. If not specified, **HTTP** will be used as the default type.    
You can view all supported trigger types by checking the [Supported triggers]({{< ref "../concepts/trigger.md#trigger-supportati" >}}) section.
