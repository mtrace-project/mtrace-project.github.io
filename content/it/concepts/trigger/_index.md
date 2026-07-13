---
title: Trigger
weight: 5
cascade:
  type: docs
---

## Introduzione
Un *trigger* è una chiamata eseguita allo scopo di generare una **trace** all'interno del sistema soggetto a test.   
Il *trigger* genera un identificativo, chiamato `trace_id`, che viene inserito nella prima chiamata e propagato lungo l'intera catena di chiamate. Questo identificativo viene poi utilizzato dai servizi coinvolti per correlare tutte le richieste interne alla richiesta iniziale. Questa catena di chiamate correlate prende il nome di **trace**, mentre le singole chiamate prendono il nome di **span**.   
Per uno studio più approfondito di questi concetti, si consiglia di consultare la documentazione di [OpenTelemetry](https://opentelemetry.io/docs).

Nelle sezioni successive sono illustrati i tipi di *trigger* supportati da **Mtracer** e le relative configurazioni. Se un tipo di trigger non è presente nelle sezioni sottostanti, significa che attualmente non è supportato da **Mtracer**.

## Configurazione generica del trigger
Il trigger è configurabile attraverso la seguente struttura YAML:
```yaml
trigger:
  type: "http" # tipo di trigger da utilizzare
  args: # variano a seconda del tipo di trigger utilizzato
    ...
```
Il campo `type` è obbligatorio e specifica il tipo di *trigger* da utilizzare, mentre il campo `args` varia a seconda del tipo di *trigger* utilizzato e contiene i parametri necessari alla sua configurazione.

Il tipo di **default** di *trigger* è `http`.