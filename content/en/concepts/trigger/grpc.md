---
title: gRPC
cascade:
  type: docs
---

## Configuration
The gRPC trigger is configurable using the `grpc` type and the following arguments:
| Argument           | Description                               | Optional  |
| ------------------ | ----------------------------------------- | --------- |
| `serverAddress`    | gRPC server address or hostname           | No        |
| `descriptorSource` | Source to obtain the gRPC descriptor      | No        |
| `method`           | Name of the gRPC method to call           | No        |
| `metadata`         | Metadata to include in the gRPC call      | Yes       |
| `data`             | Body of the gRPC request                  | Yes       |

### Descriptor source
The `descriptorSource` field specifies the source to obtain the gRPC descriptor, which is necessary to perform the gRPC call.
**Mtrace** supports `file` and `serverReflection` as sources for the gRPC descriptor, specifiable via the `type` field within `descriptorSource`.

#### File
If `file` is used as the source for the gRPC descriptor, you must specify the path to the gRPC descriptor file via the `protoPath` field within `descriptorSource`.

Example:
```yaml
descriptorSource:
    type: "file"
    protoPath: "./greeter.proto"
```

#### Server reflection
If `serverReflection` is used as the source for the gRPC descriptor, you simply need to set the type to `serverReflection` within `descriptorSource`. **Mtrace** will then use the server reflection feature to obtain the gRPC descriptor directly from the gRPC server specified in the `serverAddress` argument.

Example:
```yaml
descriptorSource:
    type: "serverReflection"
```

### Method
The `method` field specifies the name of the gRPC method to call, which must be formatted as `package.service.method`, for example, `helloworld.Greeter.SayHello`.

### Metadata
The `metadata` field specifies the metadata to include in the gRPC call, which must be provided as key-value pairs within a YAML map.

Example:
```yaml
metadata:
    key1: "value1"
    key2: "value2"
```

### Data
The `data` field specifies the body of the gRPC request, which must be provided as key-value pairs within a YAML map. **Mtrace** will then automatically convert the YAML map into a gRPC message according to the available descriptor.

Example, starting from a gRPC message defined as follows:
```protobuf
message RollRequest {
    string                    rollerName   = 1;
    google.protobuf.Timestamp rollTime     = 2;
    google.protobuf.Duration  rollDuration = 3;
}
```
This is converted into a gRPC request body specified in YAML as follows:
```yaml
data:
    rollerName: "Alice"
    rollTime: "2023-10-01T12:00:00Z"
    rollDuration: "10s"
```
