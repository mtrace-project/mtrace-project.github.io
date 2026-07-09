---
title: Run test
weight: 10
cascade:
  type: docs
---

The `mtrace run` command allows you to execute the tests passed as arguments or, if no arguments are provided, all the tests present in the working directory.   
Tests are executed sequentially to prevent one test's configuration from interfering with another.

## Command execution
```bash
mtrace run [test1.mt.yaml] [test2] ... [flags]
```
You can specify the tests with or without the `.mt.yaml` extension; in both cases, **Mtrace** will recognize the correct test file.

Furthermore, if no test is specified, all tests located in the working directory and its subdirectories will be executed.

## Flags
The `mtrace run` command supports the following flags:
- `--dir` or `-d`: specifies the working directory. If not provided, the working directory will be the one where the command is executed.
- `--debug` or `-D`: enables debug mode, displaying more detailed information during the execution of the tests.
