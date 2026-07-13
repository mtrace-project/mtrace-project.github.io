---
title: OpenObserve Configuration
cascade:
  type: docs
---

`backend_type="openobserve"`

| Environment Variable              | YAML Variable             | Description                                    | Default Value           |
| --------------------------------- | ------------------------- | ---------------------------------------------- | ----------------------- |
| `MTRACER_OPENOBSERVE_BASE_URL`    | `openobserve.base_url`    | Base URL of the OpenObserve instance           | `http://localhost:5080` |
| `MTRACER_OPENOBSERVE_ORG_NAME`    | `openobserve.org_name`    | Organization name in OpenObserve               | `default`               |
| `MTRACER_OPENOBSERVE_STREAM_NAME` | `openobserve.stream_name` | Destination stream name (log/traces)           | `default`               |
| `MTRACER_OPENOBSERVE_USERNAME`    | `openobserve.username`    | Username for authentication                    | `admin@example.com`     |
| `MTRACER_OPENOBSERVE_PASSWORD`    | `openobserve.password`    | Password for authentication                    | `admin`                 |

### YAML Example
```yaml
backend_type: "openobserve"
openobserve:
  base_url: "http://localhost:5080"
  org_name: "default"
  stream_name: "default"
  username: "admin@example.com"
  password: "admin"
```
