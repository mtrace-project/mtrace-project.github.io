---
title: Setup commands
weight: 25
cascade:
  type: docs
---

## Introduction
This section describes the setup commands in **Mtrace**. These commands are designed to define how to prepare the test environment before the test execution and how to clean it up afterwards.    
Within test files, you can specify any number of setup commands, which will be executed sequentially before and after the test execution.

## Supported setup commands
Currently, **Mtrace** supports the setup commands detailed in the following sections.   
Furthermore, the default setup command type is `shell`.
