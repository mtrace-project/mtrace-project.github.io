---
title: Architettura
weight: 30
prev: "/commands"
showPrevNext: false 
cascade:
  type: docs
---

<!-- 
Link to architecture diagram:
https://drive.google.com/file/d/1bz2YKIqLonzFlI8o2H8lS6YydQYkXG95/view?usp=sharing
-->

Questa pagina ha lo scopo di fornire una panoramica dell'architettura di **Mtracer** e del suo funzionamento interno.

## Container diagram
![Container diagram di Mtracer](../../images/containerDiagram.svg)

**Mtracer** offre due comandi principali: `mtracer create` e `mtracer run`, i quali consentono rispettivamente di creare i test e di eseguirli.

`mtracer create` è un comando che permette di generare un nuovo file di test con estensione `.mt.yaml` all'interno della cartella specificata, fornendo una struttura di base che potrà essere modificata in base alle esigenze dello sviluppatore. Si tratta di un comando di supporto non strettamente necessario per creare un file di test, ma consigliato per evitare errori di sintassi e velocizzare il processo di creazione.

`mtracer run` è il comando che esegue i test passati come argomenti o, in assenza di argomenti, tutti i test presenti nella cartella di lavoro corrente.
Questo comando rappresenta il cuore di **Mtracer**, in quanto consente di eseguire i test e verificare che le *trace* generate dal sistema testato siano conformi a quelle attese, così come definite nei file di test. Inoltre, è l'unico comando che interagisce con l'esterno, interfacciandosi con i seguenti sistemi:
- **Observability backend**: raccoglie e memorizza le *trace* generate dal sistema. **Mtracer** vi si interfaccia per recuperare le *trace* relative all'esecuzione dei test e confrontarle con quelle attese.
- **Sistema soggetto a test**: è l'applicazione testata. **Mtracer** vi interagisce tramite una richiesta iniziale chiamata *trigger*, il cui scopo è generare una *trace* all'interno del sistema stesso.

## Component diagram
In questa sezione, è possibile visualizzare un diagramma più dettagliato del comando `mtracer run`, includendo i suoi componenti interni e le relative interazioni.

![Component diagram di Mtracer](../../images/componentDiagram.svg)

### Run Test Controller
È il componente responsabile di ricevere il comando dalla CLI, interpretare e validare i parametri passati e la configurazione attuale.
Inoltre, orchestra il parsing dei file di test e l'esecuzione dei test stessi, interfacciandosi con il `Test Parser` e il `Test Runner`.

### Parser
Si occupa di leggere e interpretare i file di test, convertendo la loro struttura da formato **YAML** a una struttura dati interna, utilizzabile dal `Run Test Controller` per l'esecuzione.

### Validator
Ha il compito di verificare che la struttura dati interna e i campi generati dal `Parser` siano validi e conformi alle aspettative.
Provvede inoltre ad inserire i **valori di default** nei campi mancanti (ove possibile) e a segnalare eventuali errori di validazione al `Run Test Controller`.

### Trace Test Runner
È l'orchestratore del singolo test: gestisce tutte le fasi necessarie alla sua esecuzione. Si interfaccia con i componenti `Setup Command Executor`, `Trigger Invoker`, `Trace Fetcher`, `Trace Comparator`, `Asserter` e `Cleanup Command Executor`. Ha inoltre il compito di restituire l'esito del test al `Run Test Controller`.

### Setup Command Executor
Esegue i comandi di setup necessari prima dell'esecuzione del test.
I comandi di setup sono opzionali, possono essere definiti all'interno dei file di test e servono a preparare l'ambiente prima dell'invocazione del *trigger* (ad esempio, per avviare un container Docker richiesto dal test).
Per approfondire i comandi di setup e la loro configurazione, consulta la pagina [Comandi di setup]({{< ref "setupCommands" >}}).

### Trigger Invoker
Esegue il *trigger* definito nel file di test. Lo scopo è generare un **traceId** e iniettarlo nel sistema testato tramite una chiamata a un servizio. In questo modo, il **traceId** verrà propagato nel sistema, consentendo in seguito di recuperare la *trace* tramite l'observability backend.
Il *trigger* può essere di diversi tipi, a seconda delle esigenze dello sviluppatore e del sistema in esame.
I tipi di *trigger* supportati da **Mtracer** sono elencati nella pagina [Trigger]({{< ref "../concepts/trigger.md" >}}).

### Trace Fetcher
Si interfaccia con l'**observability backend** per recuperare la *trace* generata dal sistema a seguito dell'esecuzione del *trigger*.
**Mtracer** supporta diversi observability backend per soddisfare le necessità dei vari sistemi e sviluppatori.
I backend supportati sono elencati nella pagina [Configurazione observability backend]({{< ref "configure.md#configurazione-observability-backend" >}}).

### Trace Comparator
Confronta le caratteristiche della *trace* recuperata con quelle della *trace* (o *sub-trace*) attesa definita nel file di test. Restituisce un risultato il più chiaro ed esplicativo possibile in merito alle eventuali differenze riscontrate.

### Asserter
Esegue le asserzioni e le query definite nel file di test. Le asserzioni sono espressioni booleane applicate alla *trace* raccolta; affinché il test sia considerato superato, tutte le asserzioni devono risultare vere.
Sono progettate per offrire un livello di flessibilità e personalizzazione maggiore rispetto alla semplice definizione di una *trace* attesa.
I tipi di asserzioni supportate da **Mtracer** si trovano nella pagina [Asserzioni]({{< ref "../concepts/assertion.md" >}}).

### Cleanup Command Executor
Esegue i comandi di cleanup necessari al termine del test. Questo tipo di comando è opzionale e serve a pulire l'ambiente dopo l'esecuzione dei *setup command*.
Ogni tipo di *comando di setup* possiede un corrispondente *comando di cleanup*, che può essere obbligatorio da definire o già presente di default, a seconda del setup utilizzato.
Questo componente è fondamentale per consentire l'esecuzione ripetuta dei test senza il rischio di compromettere o "sporcare" il sistema testato.

### Post Exec Checker
Al termine dell'esecuzione del test, questo componente ha il compito di eseguire delle asserzioni all'interno del sistema testato. Queste asserzioni sono opzionali e servono a verificare che il sistema sia rimasto in uno stato coerente o sia entrato nello stato corretto dopo l'esecuzione del test.    
È possibile approfondire i dettagli dei *post exec checker* nella pagina [Controlli post esecuzione]({{< ref "../concepts/postExecutionCommands.md" >}}).

### Results Exporter
Si occupa di esportare i risultati dei test in diversi formati, come JSON, Markdown o JUnit. Questa funzionalità consente di esportare i risultati dei test in formati standardizzati, facilitando l'integrazione con altri strumenti o pipeline di analisi dei dati.

### Analytics Exporter
Ha il compito di esportare le statistiche delle *trace* e delle *span* generate dai test eseguiti, consentendo agli sviluppatori di ottenere una panoramica completa delle prestazioni del sistema sotto test ed evidenziando eventuali variazioni o anomalie nei risultati.   
È possibile approfondire la funzionalità nella pagina [Esportazione delle statistiche]({{< ref "../concepts/analyticsExporters.md" >}}).