---
title: Expected Span
weight: 11
cascade:
  type: docs
---

## Introduction

An *expected span* represents a call that you expect to find within the collected trace following a trigger.

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

Within an *expected span*, you can specify various parameters to verify that the span collected within the generated trace has specific characteristics.    
The parameters available within an *expected span* are defined below:
| Argument        | Description                                               | Optional |
| --------------- | --------------------------------------------------------- | -------- |
| `serviceName`   | *OTel* service name associated with the expected span     | No       |
| `operationName` | *OTel* operation name associated with the expected span   | Yes      |
| `spanKind`      | *OTel* kind of the expected span                          | Yes      |
| `spanStatus`    | *OTel* status of the expected span                        | Yes      |
| `maxDuration`   | Maximum duration of the expected span                     | Yes      |
| `minDuration`   | Minimum duration of the expected span                     | Yes      |

### SpanKind
The `spanKind` field specifies the *OTel* kind of the expected span, which can be one of the following values:
- `internal`
- `server`
- `client`
- `producer`
- `consumer`
- `unset`/`unspecified`

We recommend reading the [official OpenTelemetry documentation](https://opentelemetry.io/docs/specs/otel/trace/api/#spankind).

### SpanStatus
The `spanStatus` field specifies the *OTel* status of the expected span, which can be one of the following values:
- `unset`
- `ok`
- `error`

We recommend reading the [official OpenTelemetry documentation](https://opentelemetry.io/docs/concepts/signals/traces/#span-status).

### MaxDuration and MinDuration
The `maxDuration` and `minDuration` fields specify the maximum and minimum duration of the expected span, respectively. They must be formatted as a [**Go Duration String**](https://pkg.go.dev/maze.io/x/duration#ParseDuration).
