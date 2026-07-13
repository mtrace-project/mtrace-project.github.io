---
title: Expected Span
weight: 11
cascade:
  type: docs
---

## Introduction

An *expected span* represents a call that is expected to be found within the *trace* collected following the trigger execution.

Example:
```yaml
spans:
  - serviceName: "grpc-server"
    operationName: "grpc.RollDice"
    spanKind: "server"
    spanStatus: "unset"
    maxDuration: 2s
    minDuration: 1us
  - serviceName: "even-or-odd-service"
    operationName: "even-or-odd-span"
    spanKind: "internal"
    spanStatus: "unset"
```

## Configuration

Within an *expected span*, you can specify various parameters to verify that the *span* collected in the generated *trace* exhibits specific characteristics.
Below are the parameters that can be used in an *expected span*:

| Argument        | Description                                              | Optional |
| --------------- | -------------------------------------------------------- | -------- |
| `serviceName`   | The *OTel* service name associated with the expected span | No       |
| `operationName` | The *OTel* operation name associated with the expected span | Yes      |
| `spanKind`      | The *OTel* span kind of the expected span                 | Yes      |
| `spanStatus`    | The *OTel* span status of the expected span               | Yes      |
| `maxDuration`   | Maximum allowed duration of the expected span             | Yes      |
| `minDuration`   | Minimum allowed duration of the expected span             | Yes      |

### SpanKind
The `spanKind` field specifies the *OTel* span kind of the expected span, which can be one of the following values:
- `internal`
- `server`
- `client`
- `producer`
- `consumer`
- `unset`/`unspecified`

We recommend reading the [official OpenTelemetry documentation](https://opentelemetry.io/docs/specs/otel/trace/api/#spankind).

### SpanStatus
The `spanStatus` field specifies the *OTel* span status of the expected span, which can be one of the following values:
- `unset`
- `ok`
- `error`

We recommend reading the [official OpenTelemetry documentation](https://opentelemetry.io/docs/concepts/signals/traces/#span-status).

### MaxDuration and MinDuration
The `maxDuration` and `minDuration` fields specify the maximum and minimum duration of the expected span, respectively. They must be formatted as a [**Go Duration String**](https://pkg.go.dev/maze.io/x/duration#ParseDuration).
