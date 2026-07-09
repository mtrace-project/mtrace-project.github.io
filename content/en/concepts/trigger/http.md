---
title: HTTP
cascade:
  type: docs
---

## Configuration
The HTTP trigger is configurable using the `http` type and the following arguments:
| Argument  | Description                               | Default value     | Optional  |
| --------- | ----------------------------------------- | ----------------- | --------- |
| `url`     | HTTP server URL                           |                   | No        |
| `method`  | HTTP method to use                        | GET               | No        |
| `headers` | Headers to include in the request         |                   | Yes       |
| `body`    | HTTP request body                         |                   | Yes       |

### Headers
The `headers` field specifies the headers to include in the HTTP request, which must be provided as key-value pairs within a YAML map.

Example:
```yaml
headers:
    key1: "value1"
    key2: "value2"
```

### Body
The `body` field specifies the body of the HTTP request, which can be provided as a string.

Example:
```yaml
body: '{"key": "value"}'
```
