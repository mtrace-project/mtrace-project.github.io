---
title: Configurazione OpenObserve
cascade:
  type: docs
---

`backend_type="openobserve"`

| Variabile d’ambiente              | Variabile YAML            | Descrizione                                    | Valore di default       |
| --------------------------------- | ------------------------- | ---------------------------------------------- | ----------------------- |
| `MTRACER_OPENOBSERVE_BASE_URL`    | `openobserve.base_url`    | URL di base dell'istanza OpenObserve           | `http://localhost:5080` |
| `MTRACER_OPENOBSERVE_ORG_NAME`    | `openobserve.org_name`    | Nome dell'organizzazione in OpenObserve        | `default`               |
| `MTRACER_OPENOBSERVE_STREAM_NAME` | `openobserve.stream_name` | Nome dello stream (log/traces) di destinazione | `default`               |
| `MTRACER_OPENOBSERVE_USERNAME`    | `openobserve.username`    | Username per l'autenticazione                  | `admin@example.com`     |
| `MTRACER_OPENOBSERVE_PASSWORD`    | `openobserve.password`    | Password per l'autenticazione                  | `admin`                 |

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