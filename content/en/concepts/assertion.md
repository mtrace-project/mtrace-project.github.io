---
title: Assertions
weight: 15
cascade:
  type: docs
---

## Introduction
This section describes the assertions in **Mtrace**. Assertions are a tool for performing more specific checks on the collected trace using various assertion languages.   

## Supported assertions
Currently, **Mtrace** supports the assertions detailed in the sections below.   
The default assertion type is `cel`.

### Common Expression Language (CEL)
CEL is an expression language designed to be simple and efficient, allowing you to write boolean expressions to verify whether the collected trace meets certain conditions.

We recommend reading the short [tutorial](https://cel.dev/tutorials/cel-get-started-tutorial) for an overview of CEL's features.

### Example
```yaml
assertions:
  - name: "Check error count and span status"
    type: "cel"
    queries:
      errorCheck: "trace.errorCount == 0"
      spanStatusCheck: "trace.spans.all(s, s.spanStatus == 'unset')"
```

Notice that you can define multiple assertions, and within a single assertion, you can define multiple queries. All queries are evaluated and must return true for the assertion to be considered successful.

### Trace fields
Within CEL expressions, you can access various fields of the collected trace.    
The available fields are defined below:
- `traceId`: the unique identifier of the collected trace
- `startTime`: the trace start timestamp in RFC3339 format
- `endTime`: the trace end timestamp in RFC3339 format
- `duration`: the duration of the trace in seconds. The string represents seconds and must end with the "s" suffix, e.g., `1.5s`
- `spanCount`: the number of spans present in the collected trace
- `errorCount`: the number of spans in an error state present in the collected trace
- `spans`: a list of spans present in the collected trace

{{< callout type="info" >}}
You can perform operations using the `startTime` and `endTime` fields with the `timestamp()` and `duration()` functions. For example, to verify that the trace was generated within a certain time window:
```go
now > timestamp("2026-06-12T08:00:00Z") + duration("2h30m")
```
{{< /callout >}}

### Span fields
Within CEL expressions, you can access various fields of the individual spans within the collected trace.    
The available fields for each span are defined below:
- `s.spanId`: the unique identifier of the span
- `s.parentId`: the identifier of the parent span, if any
- `s.serviceName`: the *OTel* service name associated with the span
- `s.operationName`: the *OTel* operation name associated with the span
- `s.spanKind`: the *OTel* kind of the span
- `s.spanStatus`: the *OTel* status of the span
- `s.startTime`: the start timestamp of the span in RFC3339 format
- `s.endTime`: the end timestamp of the span in RFC3339 format
- `s.duration`: the duration of the span in seconds. The string represents seconds and must end with the "s" suffix, e.g., `1.5s`

{{< callout type="info" >}}
CEL provides several macros that simplify and add functionality to expressions.    
We strongly recommend reading the [official CEL documentation](https://github.com/google/cel-spec/blob/master/doc/langdef.md#macros) to discover all available macros.
{{< /callout >}}
