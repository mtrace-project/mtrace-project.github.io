---
title: Setup Commands
weight: 25
cascade:
  type: docs
---

## Introduction
This section describes the **Mtracer** setup commands. These commands are designed to define how the environment should be prepared before running the tests and how it should be cleaned up afterwards.
In the test files, you can specify an arbitrary number of setup commands, which will be executed sequentially both before and after the test execution.

## Supported Setup Commands
Currently, **Mtracer** supports the setup commands illustrated in the following sections.
Additionally, the default setup command type is `shell`.
