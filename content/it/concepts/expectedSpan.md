---
title: Expected Span
weight: 11
cascade:
  type: docs
---

## Introduzione

Un *expected span* rappresenta una chiamata che ci si aspetta di trovare all'interno della *trace* raccolta in seguito al trigger.

Esempio:
```yaml
spans:
  - serviceName: "grpc-server"
    operationName: "grpc.RollDice"
    spanKind: "server"
    spanStatus: "unset"
    maxDuration: 2s
    minDuration: 1us
  - serviceName: "even-or-odd-service"
    operationName: "even-or-odd-span"
    spanKind: "internal"
    spanStatus: "unset"
```

## Configurazione

All'interno di un *expected span* è possibile specificare diversi parametri per verificare che la *span* raccolta all'interno della *trace* generata abbia determinate caratteristiche.    
Di seguito sono definiti i parametri utilizzabili all'interno di un *expected span*:

| Argomento       | Descrizione                                              | Opzionale |
| --------------- | -------------------------------------------------------- | --------- |
| `serviceName`   | Nome del servizio *OTel* associato alla span prevista    | No        |
| `operationName` | Nome dell'operazione *OTel* associata alla span prevista | Si        |
| `spanKind`      | Tipo *OTel* della span prevista                          | Si        |
| `spanStatus`    | Stato *OTel* della span prevista                         | Si        |
| `maxDuration`   | Durata massima della span prevista                       | Si        |
| `minDuration`   | Durata minima della span prevista                        | Si        |

### SpanKind
Il campo `spanKind` specifica il tipo *OTel* della span prevista, che può essere uno dei seguenti valori:
- `internal`
- `server`
- `client`
- `producer`
- `consumer`
- `unset`/`unspecified`

È consigliata la lettura della [documentazione ufficiale di OpenTelemetry](https://opentelemetry.io/docs/specs/otel/trace/api/#spankind).

### SpanStatus
Il campo `spanStatus` specifica lo stato *OTel* della span prevista, che può essere uno dei seguenti valori:
- `unset`
- `ok`
- `error`

È consigliata la lettura della [documentazione ufficiale di OpenTelemetry](https://opentelemetry.io/docs/concepts/signals/traces/#span-status).

### MaxDuration e MinDuration
I campi `maxDuration` e `minDuration` specificano rispettivamente la durata massima e minima della span prevista, e devono essere espresse con il formato [**Go Duration String**](https://pkg.go.dev/maze.io/x/duration#ParseDuration).