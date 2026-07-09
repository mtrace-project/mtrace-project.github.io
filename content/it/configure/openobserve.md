---
title: Configurazione OpenObserve
cascade:
  type: docs
---

`backend_type="openobserve"`

| Variabile d’ambiente             | Variabile YAML            | Descrizione                                    | Valore di default       |
| -------------------------------- | ------------------------- | ---------------------------------------------- | ----------------------- |
| `MTRACE_OPENOBSERVE_BASE_URL`    | `openobserve.base_url`    | URL di base dell'istanza OpenObserve           | `http://localhost:5080` |
| `MTRACE_OPENOBSERVE_ORG_NAME`    | `openobserve.org_name`    | Nome dell'organizzazione in OpenObserve        | `default`               |
| `MTRACE_OPENOBSERVE_STREAM_NAME` | `openobserve.stream_name` | Nome dello stream (log/traces) di destinazione | `default`               |
| `MTRACE_OPENOBSERVE_USERNAME`    | `openobserve.username`    | Username per l'autenticazione                  | `admin@example.com`     |
| `MTRACE_OPENOBSERVE_PASSWORD`    | `openobserve.password`    | Password per l'autenticazione                  | `admin`                 |

### Esempio in YAML
```yaml
backend_type: "openobserve"
openobserve:
  base_url: "http://localhost:5080"
  org_name: "default"
  stream_name: "default"
  username: "admin@example.com"
  password: "admin"
```