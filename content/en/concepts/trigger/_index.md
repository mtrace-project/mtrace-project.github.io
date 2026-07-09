---
title: Trigger
weight: 5
cascade:
  type: docs
---

## Introduction
A *trigger* is a call executed with the purpose of generating a **trace** within the system under test.   
The *trigger* generates an identifier called `trace_id`, which is injected into the initial call and propagated throughout the call chain. This identifier is then used by the involved services to correlate all internal requests to the initial one. This correlated chain of calls is called a **trace**, while the individual calls are known as **spans**.   
For an in-depth study of these concepts, we recommend reading the [OpenTelemetry documentation](https://opentelemetry.io/docs).

The following sections outline the *trigger* types supported by **Mtrace** along with their configurations. If a tool you require is not listed in the sections below, it means it is not currently supported by **Mtrace**.

## Generic trigger configuration
The trigger is configurable using the following YAML structure:
```yaml
trigger:
  type: "http" # type of trigger to use
  args: # varies depending on the trigger type used
    ...
```
The `type` field is mandatory and specifies the kind of *trigger* to use, while the `args` field varies depending on the *trigger* type and contains the fields needed to configure the *trigger* itself.

The **default** *trigger* type is `http`.
