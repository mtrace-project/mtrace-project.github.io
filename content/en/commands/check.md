---
title: Syntax check
weight: 15
cascade:
  type: docs
---

The `mtrace check` command allows you to perform a syntax check on one or more test files with the `.mt.yaml` extension. The command verifies the syntax correctness of the files and reports any errors.

## Command execution
```bash
mtrace check [test1.mt.yaml] [test2] ... [flags]
```
You can specify the tests with or without the `.mt.yaml` extension; in both cases, **Mtrace** will recognize the file correctly.

Furthermore, if no test file is specified, the command will perform a syntax check on all files with the `.mt.yaml` extension present in the current working directory and its subdirectories.

## Flags
The `mtrace check` command supports the following flags:
- `--dir` or `-d`: allows you to specify the directory in which to perform the syntax check of the test files. If not specified, the current working directory where the command is executed will be used.
