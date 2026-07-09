---
title: Documentazione di Mtrace
cascade:
  type: docs
---

Benvenuti nella documentazione di **Mtrace**, uno strumento CLI progettato per creare ed eseguire test *end-to-end* all'interno di un sistema distribuito, raccogliendo le *trace* in formato [OpenTelemetry](https://opentelemetry.io/). Questa documentazione fornisce tutto il necessario per comprendere l'utilizzo, i concetti principali e l'architettura di **Mtrace**.

## Cos'è Mtrace?
**Mtrace** è uno strumento da riga di comando che consente agli sviluppatori di creare test *end-to-end* per sistemi distribuiti, raccogliendo e analizzando le *trace* generate dalle applicazioni.

**Mtrace** ha lo scopo di avviare la **trace** attraverso un **trigger** (come *HTTP* o *gRPC*) e di raccoglierla tramite un **observability backend** (come Jaeger, OpenObserve, ecc.).    
È quindi compito dello sviluppatore assicurarsi che tutte le *trace* vengano inviate all'**observability backend** e configurare **Mtrace** affinché possa raccoglierle correttamente.

Una volta definito come avviare e raccogliere le *trace*, lo sviluppatore può eseguire diversi controlli per verificare che le *trace* rispettino le aspettative.

Inoltre, prima di ogni test è possibile definire dei comandi per preparare l'ambiente e pulirlo al termine.

Infine, è fondamentale specificare che **Mtrace** supporta l'analisi delle *trace* solamente in formato **OTel**; pertanto, qualora si desideri utilizzare un observability backend che non supporta nativamente questo formato, o se le *trace* vengono pubblicate in un formato diverso, è necessario prima convertirle in formato **OTel**.

{{< cards >}}
  {{< card link="install" title="Installazione" icon="download" >}}
  {{< card link="configure" title="Configurazione" icon="adjustments" >}}
  {{< card link="concepts" title="Concetti" icon="light-bulb" >}}
  {{< card link="commands" title="Comandi" icon="terminal" >}}
{{< /cards >}}