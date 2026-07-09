---
title: Configurazione Jaeger
cascade:
  type: docs
---

`backend_type="jaeger"`

| Variabile d’ambiente     | Variabile YAML    | Descrizione                     | Valore di default        |
| ------------------------ | ----------------- | ------------------------------- | ------------------------ |
| `MTRACE_JAEGER_BASE_URL` | `jaeger.base_url` | URL di base dell'istanza Jaeger | `http://localhost:16686` |

### Esempio in YAML
```yaml
backend_type: "jaeger"
jaeger:
  base_url: "http://localhost:16686"
```