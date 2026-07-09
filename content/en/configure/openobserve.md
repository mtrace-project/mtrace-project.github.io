---
title: OpenObserve Configuration
cascade:
  type: docs
---

`backend_type="openobserve"`

| Environment variable             | YAML variable             | Description                                   | Default value           |
| -------------------------------- | ------------------------- | --------------------------------------------- | ----------------------- |
| `MTRACE_OPENOBSERVE_BASE_URL`    | `openobserve.base_url`    | Base URL of the OpenObserve instance          | `http://localhost:5080` |
| `MTRACE_OPENOBSERVE_ORG_NAME`    | `openobserve.org_name`    | Name of the organization in OpenObserve       | `default`               |
| `MTRACE_OPENOBSERVE_STREAM_NAME` | `openobserve.stream_name` | Name of the destination stream (log/traces)   | `default`               |
| `MTRACE_OPENOBSERVE_USERNAME`    | `openobserve.username`    | Username for authentication                   | `admin@example.com`     |
| `MTRACE_OPENOBSERVE_PASSWORD`    | `openobserve.password`    | Password for authentication                   | `admin`                 |

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
