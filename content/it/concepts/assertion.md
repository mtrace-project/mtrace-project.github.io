---
title: Asserzioni
weight: 15
cascade:
  type: docs
---

## Introduzione
In questa sezione vengono descritte le asserzioni di **Mtrace**. Le asserzioni sono uno strumento per fare verifiche più specifiche sulla *trace* raccolta secondo diversi linguaggi di asserzione.   

## Asserzioni supportate
Attualmente, **Mtrace** supporta le asserzioni presenti nelle sezioni sottostanti.   
Inoltre il tipo di default di asserzione è `cel`.

### Common Expression Language (CEL)
CEL è un linguaggio di espressione progettato per essere semplice ed efficiente, con cui è possibile scrivere delle espressioni booleane per verificare se la *trace* raccolta soddisfa determinate condizioni.

Si consiglia la lettura del breve [tutorial](https://cel.dev/tutorials/cel-get-started-tutorial) per una panoramica sulle funzionalità di CEL.

### Esempio
```yaml
assertions:
  - name: "Check error count and span status"
    type: "cel"
    queries:
      errorCheck: "trace.errorCount == 0"
      spanStatusCheck: "trace.spans.all(s, s.spanStatus == 'unset')"
```

Si noti come si possono definire più asserzioni, ma all'interno di un asserzione è possibile definire più query, che vengono tutte verificate e devono essere tutte vere affinché l'asserzione sia considerata verificata.

### Campi trace
All'interno delle espressioni CEL è possibile accedere a diversi campi della *trace* raccolta.    
In seguito sono definiti i campi disponibili:
- `traceId`: identificativo univoco della *trace* raccolta
- `startTime`: timestamp di inizio della *trace* con standard RFC3339
- `endTime`: timestamp di fine della *trace* con standard RFC3339
- `duration`: durata della *trace* in secondi, la stringa è espressa in secondi e deve avere il suffisso "s", ad esempio `1.5s`
- `spanCount`: numero di *span* presenti all'interno della *trace* raccolta
- `errorCount`: numero di *span* in stato di errore presenti all'interno della *trace* raccolta
- `spans`: lista di *span* presenti all'interno della *trace* raccolta

{{< callout type="info" >}}
È possibile eseguire operazioni con i campi `startTime` e `endTime` utilizzando le funzioni `timestamp()` e `duration()`, ad esempio per verificare che la *trace* sia stata generata entro un certo intervallo di tempo:
```go
now > timestamp("2026-06-12T08:00:00Z") + duration("2h30m")
```
{{< /callout >}}

### Campi span
All'interno delle espressioni CEL è possibile accedere a diversi campi delle *span* presenti all'interno della *trace* raccolta.    
In seguito sono definiti i campi disponibili per ogni *span*:
- `s.spanId`: identificativo univoco della *span*
- `s.parentId`: identificativo della *span* padre, se presente
- `s.serviceName`: nome del servizio *OTel* associato alla *span*
- `s.operationName`: nome dell'operazione *OTel* associata alla *span*
- `s.spanKind`: tipo *OTel* della *span*
- `s.spanStatus`: stato *OTel* della *span*
- `s.startTime`: timestamp di inizio della *span* con standard RFC3339
- `s.endTime`: timestamp di fine della *span* con standard RFC3339
- `s.duration`: durata della *span* in secondi, la stringa è espressa in secondi e deve avere il suffisso "s", ad esempio `1.5s`

{{< callout type="info" >}}
CEL offre diverse macro che consentono di semplificare e aggiungere funzionalità alle espressioni.    
Si consiglia perciò la lettura della [documentazione ufficiale di CEL](https://github.com/google/cel-spec/blob/master/doc/langdef.md#macros) per scoprire tutte le macro disponibili.
{{< /callout >}}