---
title: gRPC
cascade:
  type: docs
---

## Configuration
The gRPC trigger is configurable using the `grpc` type and the following arguments:
| Argument           | Description                               | Optional |
| ------------------ | ----------------------------------------- | -------- |
| `serverAddress`    | gRPC server address or hostname           | No       |
| `descriptorSource` | Source to obtain the gRPC descriptor from | No       |
| `method`           | Name of the gRPC method to call           | No       |
| `metadata`         | Metadata to include in the gRPC call      | Yes      |
| `data`             | gRPC request body                         | Yes      |

### Descriptor source
The `descriptorSource` field specifies the source from which to obtain the gRPC descriptor, which is necessary to make the gRPC call.
**Mtracer** supports `file` and `serverReflection` as sources for the gRPC descriptor, which can be specified through the `type` field within `descriptorSource`.

#### File
If you use `file` as the source for the gRPC descriptor, you need to specify the path to the gRPC descriptor file through the `protoPath` field within `descriptorSource`.

Example:
```yaml
descriptorSource:
    type: "file"
    protoPath: "./greeter.proto"
```

#### Server reflection
If you use `serverReflection` as the source for the gRPC descriptor, you simply need to specify the `serverReflection` type within `descriptorSource`, so that **Mtracer** will use the server reflection feature to obtain the gRPC descriptor directly from the gRPC server specified in the `serverAddress` argument.

Example:
```yaml
descriptorSource:
    type: "serverReflection"
```

### Method
The `method` field specifies the name of the gRPC method to call, which must be specified in the format `package.service.method`, for example `helloworld.Greeter.SayHello`.

### Metadata
The `metadata` field specifies the metadata to include in the gRPC call, which must be specified as key-value pairs within a YAML map.

Example:
```yaml
metadata:
    key1: "value1"
    key2: "value2"
```

### Data
The `data` field specifies the gRPC request body, which must be specified in key-value format within a YAML map. This way, **Mtracer** will automatically convert the YAML map into a gRPC message according to the descriptor it has.

Example starting from a gRPC message defined as follows:
```protobuf
message RollRequest {
    string                    rollerName   = 1;
    google.protobuf.Timestamp rollTime     = 2;
    google.protobuf.Duration  rollDuration = 3;
}
```
This converts to a gRPC request body specified in YAML as follows:
```yaml
data:
    rollerName: "Alice"
    rollTime: "2023-10-01T12:00:00Z"
    rollDuration: "10s"
```
