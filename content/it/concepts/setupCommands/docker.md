---
title: Docker
weight: 2
cascade:
  type: docs
---

Il comando di setup di tipo *docker* consente di eseguire specifiche operazioni sui container Docker, come interrompere o rallentare un container prima dell'esecuzione di un test.

Questo tipo di comando supporta le operazioni descritte di seguito.

## Operazioni sullo stato di un container
Queste operazioni consentono di modificare lo stato di un container Docker, ad esempio per simulare un'interruzione del servizio.

| Operazione         | Descrizione                                                                             |
| ------------------ | --------------------------------------------------------------------------------------- |
| `killContainer`    | Termina l'esecuzione del container senza rimuoverlo, inviando un segnale **SIGKILL**    |
| `stopContainer`    | Interrompe l'esecuzione del container senza rimuoverlo, inviando un segnale **SIGTERM** |
| `startContainer`   | Avvia l'esecuzione del container                                                        |
| `pauseContainer`   | Mette in pausa l'esecuzione del container                                               |
| `unpauseContainer` | Riprende l'esecuzione del container                                                     |

Esempio:
```yaml
setupCommands:
  - type: "docker"
    cmd: "stopContainer"
    args:
      containerId: "container-id|container-name"
```
Tutte le suddette operazioni richiedono un argomento `containerId` che può essere l'identificativo o il nome del container su cui si vuole eseguire l'operazione.

## Esecuzione di comandi all'interno di un container
Questa operazione consente di eseguire un comando all'interno di un container Docker, ad esempio per applicare delle configurazioni specifiche prima dell'esecuzione di un test.   
L'operazione è definita con il nome di `execContainer` e richiede due argomenti: `containerId` e `cmd`, che rappresenta il comando da eseguire all'interno del container.   
Inoltre necessita di un comando di cleanup dello stesso tipo, che consente di eseguire un comando all'interno dello stesso container dopo l'esecuzione del test, ad esempio per ripristinare le configurazioni modificate dal comando di setup.

Esempio:
```yaml
setupCommands:
  - type: "docker"
    cmd: "execContainer"
    args:
      containerId: "container-id|container-name"
      cmd: "echo 'Setup complete!'"
    cleanupCmd:
      cmd: "echo 'Cleanup complete!'"
```

## Avvio container tramite Docker Compose
Questa operazione consente di avviare i container definiti in un dato file di configurazione **Docker Compose** e di attendere che siano tutti in esecuzione o in stato *healthy* prima di procedere con l'esecuzione del test o con i successivi comandi di setup.   
L'operazione è definita con il nome di `composeUp` e richiede un argomento `composePath` che rappresenta il percorso del file di configurazione **Docker Compose** e un argomento opzionale `projectName` che consente di specificare il nome del progetto Docker Compose; in caso contrario, verrà utilizzato il nome della cartella contenente il file di configurazione.

Esempio:
```yaml
setupCommands:
  - type: "docker"
    cmd: "composeUp"
    args:
      composePath: "docker-compose.yaml"
      projectName: "my-project" # argomento opzionale, default: nome della cartella contenente il file di configurazione
```

## Simulazione di problemi di rete all'interno di un container
Queste operazioni consentono di simulare problemi di rete all'interno di un container Docker, ad esempio introducendo latenza o limitando la larghezza di banda disponibile. Tali operazioni richiedono il parametro `containerId` oltre a ulteriori argomenti specifici. 

Inoltre, tramite l'argomento `netInterface` è possibile specificare l'interfaccia di rete virtuale su cui applicare l'operazione; in caso contrario, verrà utilizzata l'interfaccia di default `eth0`.    
Nella maggior parte dei casi, un container Docker senza configurazioni di rete personalizzate utilizza `eth0` come interfaccia predefinita; è comunque possibile verificare il nome dell'interfaccia eseguendo il comando `ip addr` all'interno del container.

{{% callout type="info" %}}
**Mtracer** basa questa funzionalità sull'utilizzo di [`tc`](https://man7.org/linux/man-pages/man8/tc.8.html), uno strumento Linux per la configurazione del sottosistema di rete. Esso permette di applicare regole avanzate di accodamento (`qdisc`), come [`netem`](https://man7.org/linux/man-pages/man8/tc-netem.8.html), all'interfaccia virtuale specificata, al fine di simulare i problemi di rete.
{{% /callout %}}

### Simulazione di ritardi di rete
Per simulare dei ritardi di rete, è possibile utilizzare l'operazione `delayContainer`. Questa operazione richiede un argomento `delay` che specifica la quantità di latenza per il traffico in uscita da applicare al container, ad esempio `100ms` per 100 millisecondi di latenza.   
Il ritardo deve essere specificato in formato [**Go Duration String**](https://pkg.go.dev/maze.io/x/duration#ParseDuration).

Esempio:
```yaml
setupCommands:
  - type: "docker"
    cmd: "delayContainer"
    args:
      containerId: "container-id|container-name"
      delay: "100ms"
      netInterface: "eth0" # argomento opzionale, default "eth0"
```

### Simulazione di perdita di pacchetti
Per simulare la perdita di pacchetti, è possibile utilizzare l'operazione `packetLossContainer`. Questa operazione richiede un argomento `loss` che specifica la percentuale di pacchetti da scartare, ad esempio `10` o `10%` per far perdere il 10% dei pacchetti.

Esempio:
```yaml
setupCommands:
  - type: "docker"
    cmd: "packetLossContainer"
    args:
      containerId: "container-id|container-name"
      loss: "10%"
```

### Simulazione di problemi di rete con qdisc custom
È possibile simulare problemi di rete più complessi utilizzando l'operazione `customQdiscContainer` per eseguire un comando custom all'interno del container che sfrutta `tc` per applicare la configurazione di rete desiderata.   
È importante considerare che il comando indicato nel parametro `qdiscCmd` viene eseguito nel container target mediante `tc qdisc add dev <netInterface> root <qdiscCmd>`; pertanto, sarà possibile applicare le implementazioni di `qdisc` compatibili con `tc` (consultabili [qui](https://man7.org/linux/man-pages/man8/tc.8.html#CLASSLESS_QDISCS)). Dopo aver scelto il tipo di `qdisc`, è fondamentale fare riferimento alla documentazione ufficiale della singola implementazione per conoscere i parametri supportati.

Esempio:
```yaml
setupCommands:
  - type: "docker"
    cmd: "customQdiscContainer"
    args:
      containerId: "container-id|container-name"
      qdiscCmd: "tbf rate 10mbit burst 32kbit latency 400ms"
```

{{% callout type="warning" %}}
**Qdisc** è uno scheduler che gestisce la coda dei pacchetti in **uscita** da un'interfaccia di rete e consente di applicare diverse politiche di gestione del traffico. Tuttavia, si potrebbero notare effetti anche in **entrata** perché per alcuni protocolli di rete, come ad esempio TCP, è necessario un riscontro da parte del destinatario per completare la comunicazione; quindi, se si applica una regola di limitazione della banda in uscita, questa potrebbe influire indirettamente anche sul traffico in entrata.
{{% /callout %}}