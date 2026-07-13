---
title: Jaeger Configuration
cascade:
  type: docs
---

`backend_type="jaeger"`

| Environment Variable      | YAML Variable     | Description                     | Default Value            |
| ------------------------- | ----------------- | ------------------------------- | ------------------------ |
| `MTRACER_JAEGER_BASE_URL` | `jaeger.base_url` | Base URL of the Jaeger instance | `http://localhost:16686` |

### YAML Example
```yaml
backend_type: "jaeger"
jaeger:
  base_url: "http://localhost:16686"
```
