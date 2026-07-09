---
title: HTTP
cascade:
  type: docs
---

## Configurazione
Il trigger HTTP è configurabile attraverso il type `http` e i seguenti argomenti:
| Argomento | Descrizione                               | Valore di default | Opzionale |
| --------- | ----------------------------------------- | ----------------- | --------- |
| `url`     | URL del server HTTP                       |                   | No        |
| `method`  | Metodo HTTP da utilizzare                 | GET               | No        |
| `headers` | Intestazioni da includere nella richiesta |                   | Si        |
| `body`    | Corpo della richiesta HTTP                |                   | Si        |

### Headers
Il campo `headers` specifica gli headers da includere nella richiesta HTTP, che devono essere specificate come coppie chiave-valore all'interno di una mappa YAML.

Esempio:
```yaml
headers:
    key1: "value1"
    key2: "value2"
```

### Body
Il campo `body` specifica il corpo della richiesta HTTP, che può essere specificato come stringa.

Esempio:
```yaml
body: '{"key": "value"}'
```