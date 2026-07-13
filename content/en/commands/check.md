---
title: Syntax check
weight: 15
cascade:
  type: docs
---

The `mtracer check` command allows you to perform a syntax check on one or more test files with the `.mt.yaml` extension. The command verifies the file syntax correctness and reports any errors.

## Command execution
```bash
mtracer check [test1.mt.yaml] [test2] ... [flags]
```
You can specify tests with or without the `.mt.yaml` extension; in both cases, **Mtracer** will correctly recognize the file.

Furthermore, if no test file is specified, the command will perform a syntax check on all files with the `.mt.yaml` extension present in the current working directory and its subdirectories.

## Flags
The `mtracer check` command supports the following flags:
- `--dir` or `-d`: allows you to specify the directory in which to perform the syntax check of the test files. If not specified, the working directory will be the one from which the command is executed.
