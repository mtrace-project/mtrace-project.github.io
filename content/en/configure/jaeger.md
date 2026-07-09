---
title: Jaeger Configuration
cascade:
  type: docs
---

`backend_type="jaeger"`

| Environment variable     | YAML variable     | Description                     | Default value            |
| ------------------------ | ----------------- | ------------------------------- | ------------------------ |
| `MTRACE_JAEGER_BASE_URL` | `jaeger.base_url` | Base URL of the Jaeger instance | `http://localhost:16686` |

### YAML Example
```yaml
backend_type: "jaeger"
jaeger:
  base_url: "http://localhost:16686"
```
