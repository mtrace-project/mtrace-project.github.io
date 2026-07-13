---
title: Trigger
weight: 5
cascade:
  type: docs
---

## Introduction
A *trigger* is a call executed to generate a **trace** within the system under test.
The *trigger* generates an identifier, called `trace_id`, which is injected into the initial call and propagated throughout the call chain. This identifier is then used by the involved services to correlate all internal requests with the initial one. This chain of correlated calls is known as a **trace**, while the individual calls are called **spans**.
For a more in-depth understanding of these concepts, we recommend reading the [OpenTelemetry](https://opentelemetry.io/docs) documentation.

The following sections illustrate the types of *triggers* supported by **Mtracer** and their configurations. If a specific type of trigger is missing from the sections below, it means it is not currently supported by **Mtracer**.

## Generic Trigger Configuration
The trigger can be configured using the following YAML structure:
```yaml
trigger:
  type: "http" # type of trigger to use
  args: # these vary depending on the trigger type used
    ...
```
The `type` field is mandatory and specifies the kind of *trigger* to use, whereas the `args` field varies depending on the chosen *trigger* and contains the properties necessary to configure it.

The **default** *trigger* type is `http`.
