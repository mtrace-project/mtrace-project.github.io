---
title: Concetti
weight: 10
prev: "/configure"
next: "/commands"
cascade:
  type: docs
---

## Introduzione
Un **trace test**, nell'ambito di **Mtracer**, è un qualsiasi file `mt.yaml` che utilizza il formato definito con l'obiettivo di invocare un'azione all'interno del sistema sotto osservazione e verificarne gli effetti a livello di *trace* raccolte dall'observability backend.

In altre parole, un **trace test** è un test *end-to-end* che ha come focus l'analisi della *trace* generata dalle applicazioni in seguito a una specifica azione, come per esempio una chiamata HTTP o gRPC.

## Struttura di un trace test
Un file di test `mt.yaml` che definisce un **trace test** è composto da diverse sezioni, ognuna con uno scopo specifico.

Esempio di un file `mt.yaml` che definisce un trace test:
```yaml
name: "Dice microservice test gRPC"
description: "Quick test for the dice microservice to verify that it generates the expected trace spans when receiving a gRPC request."
setupCommands:
  - type: "shell"
    cmd: "echo 'Setting up test environment'"
    cleanupCmd:
      cmd: "echo 'Cleaning up test environment'"
trigger:
  type: "grpc"
  args:
    serverAddress: "localhost:5051"
    descriptorSource:
      type: "file"
      protoPath: "dice.proto"
    method: "dice.DiceService.RollDice"
    metadata:
      authorization: "Bearer localToken"
    data:
      rollerName: "Alice"
      rollTime: "2023-10-01T12:00:00Z"
      rollDuration: "10s"
waitBeforeFetch: 10s
timeout: 120s
retryDelay: 1s
lastSpan:
  serviceName: "even-or-odd-service"
  operationName: "even-or-odd-span"
  spanKind: "internal"
  spanStatus: "unset"
expectedProperties:
    spanCount: 7
    errorCount: 0
    maxDuration: 30s
    minDuration: 1s
expectedTraces:
  - ordered: yes
    checker: contains
    spans:
      - serviceName: "grpc-server"
        operationName: "grpc.RollDice"
        spanKind: "server"
        spanStatus: "unset"
        maxDuration: 2s
        minDuration: 1us
      - serviceName: "nats-consumer"
        operationName: "nats-consume"
        spanKind: "consumer"
        spanStatus: "unset"
        maxDuration: 1s
      - serviceName: "dice-service"
        operationName: "dice-server"
        spanKind: "server"
        spanStatus: "unset"
      - serviceName: "even-or-odd-service"
        operationName: "even-or-odd-span"
        spanKind: "internal"
        spanStatus: "unset"
  - ordered: no
    spans:
      - serviceName: "dice-service"
        operationName: "dice-server"
        spanKind: "server"
        spanStatus: "unset"
      - serviceName: "nats-consumer"
        operationName: "nats-consume"
        spanKind: "consumer"
        spanStatus: "unset"
      - serviceName: "even-or-odd-service"
        operationName: "even-or-odd-span"
        spanKind: "internal"
        spanStatus: "unset"
assertions:
  - name: "Check error count and span status"
    type: "cel"
    queries:
      errorCheck: "trace.errorCount == 0"
      spanStatusCheck: "trace.spans.all(s, s.spanStatus == 'unset')"
postExecChecks:
  - name: "Check if there are any rolls for Alice in the database"
    type: "sql"
    args:
      dsn: "postgres://postgres:postgres@localhost:5432/postgres?sslmode=disable"
      query: "(SELECT COUNT(*) FROM rolls WHERE player_name = 'Alice') > 0"
  - name: "Check if the dice service is running"
    type: "shell"
    args:
      cmd: "echo 'Hello'"
      expectedExitCode: 0
```

{{% callout type="warning" %}}
È importante sapere che i percorsi definiti all'interno dei test devono essere relativi alla posizione del file di test stesso, e non alla cartella di lavoro specificata con il flag `--dir` o in cui si esegue il comando.
{{% /callout %}}

### Nome e descrizione del test
La sezione `name` e `description` serve a identificare il test e a fornire una breve descrizione del suo scopo. Queste informazioni sono utili per comprendere rapidamente l'obiettivo del test e per organizzare i test in modo efficace.

### Setup commands
La sezione `setupCommands` è **opzionale** e consente di definire una serie di comandi da eseguire prima e dopo l'esecuzione del test per preparare e pulire l'ambiente. Questi comandi sono fondamentali per garantire che il test venga eseguito in modo corretto e ripetibile.

Per maggiori dettagli sui comandi di setup, consulta la sezione [Comandi di setup](./setupCommand/).

### Trigger
La sezione `trigger` è **obbligatoria** e definisce l'azione che fa cominciare la *trace* all'interno del sistema sotto test.

Per maggiori dettagli sui trigger, consulta la sezione [Trigger](./trigger/).

### Wait before fetch
Il parametro `waitBeforeFetch` è **opzionale**, esso specifica un intervallo di tempo da attendere tra l'esecuzione del trigger e l'inizio del processo di raccolta del *trace* dall'observability backend. Questo intervallo è importante per garantire che tutti gli *span* generati in seguito al trigger siano correttamente raccolti e analizzati.

Questo campo deve essere espresso con il formato [**Go Duration String**](https://pkg.go.dev/maze.io/x/duration#ParseDuration).    
Il valore di default è `5s`.

### Timeout
Il parametro `timeout` è **opzionale** e specifica il tempo massimo da attendere per la raccolta del *trace* dall'observability backend. Allo scadere di questo tempo, l'ultima *trace* raccolta è utilizzata per le verifiche. Nel caso non sia stata raccolta alcuna *trace* entro il tempo specificato, il test fallisce.   
Il valore di default è `60s`.

### Retry delay
Il parametro `retryDelay` è **opzionale** e specifica l'intervallo di tempo da attendere tra un tentativo di raccolta del *trace* e il successivo. Questo intervallo è importante per evitare di sovraccaricare l'observability backend con richieste di raccolta troppo frequenti.   
Il valore di default è `1s`.


### Last span
La sezione `lastSpan` è **opzionale** e consente di specificare le caratteristiche dell'ultimo *span* atteso nella *trace* generata in seguito al trigger. Questa sezione è utile per capire quando la *trace* è considerabile come completa e pronta per essere analizzata, in questo modo è possibile iniziare a verificare le aspettative sulle *trace* non appena viene raccolto l'ultimo *span* atteso, senza dover attendere necessariamente il timeout.

### Expected trace properties
La sezione `expectedProperties` è **opzionale** e consente di specificare alcune proprietà generali che ci si aspetta di trovare all'interno della *trace* generata in seguito al trigger.    
Qui sotto è possibile trovare una tabella con le proprietà generali che è possibile specificare all'interno di questa sezione:
| Proprietà     | Descrizione                                                                     | Opzionale |
| ------------- | ------------------------------------------------------------------------------- | --------- |
| `spanCount`   | Numero di *span* previste all'interno della *trace* raccolta                    | Si        |
| `errorCount`  | Numero di *span* in stato di errore previste all'interno della *trace* raccolta | Si        |
| `maxDuration` | Durata massima di esecuzione della *trace* raccolta                             | Si        |
| `minDuration` | Durata minima di esecuzione della *trace* raccolta                              | Si        |

### Expected traces
La sezione `expectedTraces` è **opzionale** e definisce le aspettative riguardo alla *trace* generata in seguito al trigger. In questa sezione è possibile specificare una o più *trace* attese, ognuna con le proprie caratteristiche e condizioni da verificare.

Per maggiori dettagli consulta la sezione [Expected Trace](./expectedTrace/).

### Assertions
La sezione `assertions` è **opzionale** e consente di definire una serie di asserzioni da verificare sulla *trace* o le singole *span* raccolte.

Per maggiori dettagli sulle asserzioni, consulta la sezione [Asserzioni](./assertion/).

### Post execution checks
La sezione `postExecChecks` è **opzionale** e consente di definire una serie di controlli da eseguire dopo l'esecuzione del test per verificare che l'ambiente sia stato modificato correttamente in seguito al trigger. Questi controlli sono fondamentali per garantire che il test abbia avuto l'effetto desiderato sul sistema sotto test.

Per maggiori dettagli sui controlli post esecuzione, consulta la sezione [Controlli Post Esecuzione](./postExecutionCommands/).
