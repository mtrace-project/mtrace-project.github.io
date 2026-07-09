---
title: Trigger
weight: 5
cascade:
  type: docs
---

## Introduzione
Un *trigger* è una chiamata che viene eseguita con lo scopo di generare una **trace** all'interno del sistema soggetto a test.   
Il *trigger* genera un identificativo, chiamato `trace_id`, che viene inserito nella prima chiamata e propagato attraverso la catena di chiamate. Questo identificativo viene poi utilizzato dai servizi coinvolti per correlare tutte le richieste interne alla richiesta iniziale. Questa catena di chiamate correlate prende il nome di **trace**, mentre le singole chiamate prendono il nome di **span**.   
Per uno studio più approfondito di questi concetti, è consigliata la lettura della documentazione di [OpenTelemetry](https://opentelemetry.io/docs).

Nelle sezioni successive, sono illustrati i tipi di *trigger* supportati da **Mtrace** e le loro configurazioni. Se un tipo di trigger richiesto non è presente nelle sezioni sottostanti, significa che attualmente non è supportato da **Mtrace**.

## Configurazione generica del trigger
Il trigger è configurabile attraverso la seguente struttura YAML:
```yaml
trigger:
  type: "http" # tipo di trigger da utilizzare
  args: # variano a seconda del tipo di trigger utilizzato
    ...
```
Il campo `type` è obbligatorio e specifica il tipo di *trigger* da utilizzare, mentre il campo `args` varia a seconda del tipo di *trigger* utilizzato e contiene i campi necessari a configurare il *trigger* stesso.

Il tipo di **default** di *trigger* è `http`.