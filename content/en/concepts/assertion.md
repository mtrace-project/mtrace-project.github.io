---
title: Assertions
weight: 15
cascade:
  type: docs
---

## Introduction
This section describes **Mtracer** assertions. Assertions are a tool that allows you to perform more specific verifications on the collected *trace* using different assertion languages.   

## Supported Assertions
Currently, **Mtracer** supports the assertions detailed in the sections below.   
Additionally, the default assertion type is `cel`.

### Common Expression Language (CEL)
CEL is an expression language designed to be simple and efficient, allowing you to write boolean expressions to verify if the collected *trace* meets certain conditions.

It is recommended to read the short [tutorial](https://cel.dev/tutorials/cel-get-started-tutorial) for an overview of CEL's capabilities.

### Example
```yaml
assertions:
  - name: "Check error count and span status"
    type: "cel"
    queries:
      errorCheck: "trace.errorCount == 0"
      spanStatusCheck: "trace.spans.all(s, s.spanStatus == 'unset')"
```

Note that you can define multiple assertions and, within each one, multiple queries. All queries are evaluated and must be true for the assertion to be considered verified.

### Trace Fields
Within CEL expressions, you can access various fields of the collected *trace*.    
The following fields are available:
- `traceId`: unique identifier of the collected *trace*
- `startTime`: start timestamp of the *trace* in RFC3339 format
- `endTime`: end timestamp of the *trace* in RFC3339 format
- `duration`: duration of the *trace* in seconds; the string is expressed in seconds and must have the "s" suffix, e.g., `1.5s`
- `spanCount`: number of *spans* present in the collected *trace*
- `errorCount`: number of *spans* with an error status present in the collected *trace*
- `spans`: list of *spans* present in the collected *trace*

{{< callout type="info" >}}
You can perform operations with the `startTime` and `endTime` fields using the `timestamp()` and `duration()` functions, for example, to verify that the *trace* was generated within a certain time window:
```go
now > timestamp("2026-06-12T08:00:00Z") + duration("2h30m")
```
{{< /callout >}}

### Span Fields
Within CEL expressions, you can access various fields of the *spans* present in the collected *trace*.    
The following fields are available for each *span*:
- `s.spanId`: unique identifier of the *span*
- `s.parentId`: identifier of the parent *span*, if any
- `s.serviceName`: *OTel* service name associated with the *span*
- `s.operationName`: *OTel* operation name associated with the *span*
- `s.spanKind`: *OTel* kind of the *span*
- `s.spanStatus`: *OTel* status of the *span*
- `s.startTime`: start timestamp of the *span* in RFC3339 format
- `s.endTime`: end timestamp of the *span* in RFC3339 format
- `s.duration`: duration of the *span* in seconds; the string is expressed in seconds and must have the "s" suffix, e.g., `1.5s`
- `s.attributes`: map of attributes associated with the *span*, with string keys and any type for values. This field is useful for running assertions on specific attributes of one or more *spans*, for example:
```yaml
assertions:
  - name: "Check db system"
    queries:
      dbSystemCheck: "trace.spans[3].attributes['db.system'] == 'postgresql'"
```

{{< callout type="info" >}}
CEL offers several macros that simplify and add functionality to expressions.    
It is therefore recommended to read the [official CEL documentation](https://github.com/google/cel-spec/blob/master/doc/langdef.md#macros) to discover all available macros.
{{< /callout >}}
